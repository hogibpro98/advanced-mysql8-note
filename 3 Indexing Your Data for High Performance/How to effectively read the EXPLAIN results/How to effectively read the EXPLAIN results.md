# Các cột trong EXPLAIN PLAN của MySQL 8.0

Dưới đây là giải thích về các cột trong **EXPLAIN PLAN** của MySQL 8.0:

| **Cột**         | **Mô tả**                                                                                              |
|------------------|-------------------------------------------------------------------------------------------------------|
| **id**          | Xác định thứ tự của truy vấn hoặc câu lệnh phụ (subquery).                                              |
| **select_type** | Loại truy vấn, ví dụ: **SIMPLE**, **PRIMARY**, **SUBQUERY**, **DERIVED**, **UNION**, v.v.              |
| **table**       | Tên bảng hoặc đối tượng đang được tham chiếu trong truy vấn.                                           |
| **partitions**  | Danh sách các phân vùng được sử dụng trong truy vấn.                                                   |
| **type**        | Loại truy cập dữ liệu, từ tốt nhất đến kém hiệu quả nhất: **system**, **const**, **eq_ref**, **ref**, **range**, **index**, **ALL**. |
| **possible_keys** | Chỉ mục có thể được sử dụng cho truy vấn (nếu có).                                                  |
| **key**         | Chỉ mục thực sự được MySQL chọn để sử dụng.                                                           |
| **key_len**     | Độ dài của chỉ mục được sử dụng.                                                                      |
| **ref**         | Cột hoặc hằng số được sử dụng để so sánh với chỉ mục.                                                 |
| **rows**        | Số lượng hàng mà MySQL ước tính cần đọc để thực hiện truy vấn.                                         |
| **filtered**    | Tỷ lệ (phần trăm) các hàng được giữ lại sau khi áp dụng các điều kiện.                                |
| **Extra**       | Thông tin bổ sung về cách MySQL xử lý truy vấn, ví dụ: **Using where**, **Using index**, **Using filesort**, v.v. |

---

## **Lưu ý:**
- Việc hiểu và phân tích các cột này rất quan trọng để tối ưu hóa hiệu suất của truy vấn.
- Các giá trị như **rows** và **type** là chỉ báo quan trọng về hiệu suất và có thể cho thấy cơ hội cải thiện truy vấn.

# **Giải thích chi tiết cột `type` (JSON name: access_type)**

## **Mục đích**
- Cột `type` trong EXPLAIN chỉ ra loại truy cập hoặc **JOIN** được sử dụng trong truy vấn.
- Các loại truy cập không hiển thị theo thứ tự, mà được phân loại theo hiệu suất của loại truy cập.

---

## **Các loại `type` và ý nghĩa**

| **Loại**           | **Mô tả**                                                                                                                                                               | **Ví dụ**                                                                                                                                                 |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **system**         | Dùng cho các bảng hệ thống, chứa một bản ghi duy nhất.                                                                                                                 |                                                                                                                                                           |
| **const**          | - Rất nhanh, chỉ đọc bản ghi một lần.<br>- Áp dụng khi điều kiện lọc là một giá trị cụ thể.                                                                            | ```sql SELECT * FROM clients WHERE ID=1; ```                                                                                                              |
| **eq_ref**         | - Loại tốt nhất ngoài **const**.<br>- Được sử dụng cho tất cả các chỉ mục khả dụng, chỉ đọc một bản ghi phù hợp từ các bảng đã được liên kết trước đó.                   | ```sql SELECT * FROM clients, invoices WHERE clients.ID = invoices.clientID; ```                                                                          |
| **ref**            | - Truy cập tất cả các bản ghi từ bảng mà có thể tìm thấy trong chỉ mục.<br>- Hiệu suất tối ưu nếu **ref** được dùng với một cột được lập chỉ mục.                        | ```sql SELECT * FROM clients WHERE ID=1; ```                                                                                                              |
| **fulltext**       | Dùng riêng cho các tìm kiếm fulltext trên một chỉ mục fulltext.                                                                                                       |                                                                                                                                                           |
| **ref_or_null**    | - Tương tự như **ref**, nhưng MySQL sẽ làm thêm công việc để tìm các bản ghi có giá trị NULL.                                                                          | ```sql SELECT * FROM clients WHERE ID=1 OR last_billing IS NULL; ```                                                                                      |
| **index_merge**    | - MySQL sử dụng tối ưu hóa **Index Merge**.<br>- Cột `key_len` sẽ chứa danh sách các giá trị lớn hơn vì nhiều chỉ mục được sử dụng.                                      |                                                                                                                                                           |
| **unique_subquery**| Thay thế **eq_ref** khi sử dụng giá trị trong hàm **IN()** của truy vấn con.                                                                                           | ```sql SELECT 10 IN (SELECT ID FROM clients WHERE ID < 100000); ```                                                                                       |
| **index_subquery** | Tương tự **unique_subquery**, nhưng được dùng cho các giá trị không duy nhất.                                                                                        |                                                                                                                                                           |
| **range**          | - Sử dụng cho các so sánh giữa hai giá trị.<br>- EXPLAIN sẽ chỉ ra chỉ mục nào được sử dụng.                                                                           | ```sql SELECT * FROM clients WHERE ID BETWEEN 1000 AND 2000; ```                                                                                          |
| **index**          | - Truy cập toàn bộ chỉ mục (không phải bảng).<br>- Nếu tất cả các điều kiện khớp với chỉ mục, cột `EXTRA` sẽ cung cấp thêm thông tin.                                   |                                                                                                                                                           |
| **all**            | - **Không tốt cho tối ưu hóa**.<br>- MySQL thực hiện quét toàn bộ bảng và không sử dụng bất kỳ chỉ mục nào.                                                            |                                                                                                                                                           |

---

## **Những loại cần tránh**
- **all**: Quét toàn bộ bảng, không sử dụng chỉ mục.
- **Using filesort** hoặc **Using temporary**: Những chỉ báo này trong cột `EXTRA` cho thấy cần tối ưu hóa thêm.

---

## **Kết luận**
- Loại **const** và **eq_ref** là tốt nhất về mặt hiệu suất.
- Tránh loại **all** trong các truy vấn lớn vì nó làm giảm đáng kể hiệu suất.
- Hiểu rõ cột `type` là bước quan trọng để tối ưu hóa truy vấn MySQL.


# Thông tin bổ sung từ EXPLAIN - Cột EXTRA

## **Tầm quan trọng của cột EXTRA**
- **EXTRA** là một trong những cột quan trọng trong kết quả của **EXPLAIN**.
- Cột này cung cấp thông tin bổ sung về cách MySQL hỗ trợ truy vấn của bạn.

---

## **Lưu ý nhanh**
- Để tăng tốc độ truy vấn, hãy kiểm tra cột **EXTRA** và chú ý đến các thông báo như:
  - **Using Filesort**
  - **Using Temporary Table**
- Đây là các thông báo cần được tối ưu hóa.

---

## **Danh sách các thông báo quan trọng trong cột EXTRA**

| **Thông báo**                    | **Ý nghĩa**                                                                                                               |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| **Full scan on NULL key**         | Hiển thị khi MySQL không thể truy cập một chỉ mục cho truy vấn con (subquery).                                            |
| **Impossible HAVING**             | Câu lệnh **HAVING** không thể chọn bất kỳ bản ghi nào.                                                                     |
| **Impossible WHERE**              | Tương tự **HAVING**, MySQL không thể tìm thấy bản ghi thỏa mãn điều kiện **WHERE**.                                        |
| **Not exists**                    | Tối ưu hóa **LEFT JOIN**: Nếu MySQL tìm thấy một dòng phù hợp, nó sẽ không quét phần còn lại của bảng liên quan.           |
| **Using filesort**                | MySQL cần thực hiện thêm công việc để sắp xếp bản ghi theo thứ tự yêu cầu.                                                  |
| **Using index**                   | MySQL sử dụng một chỉ mục để thực thi truy vấn.                                                                            |
| **Using index condition**         | MySQL làm việc với bảng và các tuple của chỉ mục để đọc bản ghi thông qua chỉ mục.                                         |
| **Using join buffer**             | MySQL sử dụng **join_buffer** để lưu các kết quả JOIN vào bộ nhớ. Nếu không đủ bộ nhớ, MySQL sẽ dùng ổ đĩa, làm giảm hiệu suất. |
| **Using temporary**               | Hiển thị khi MySQL sử dụng bảng tạm để thực hiện **GROUP BY** hoặc **SORT BY**. Nếu thiếu RAM, bảng tạm sẽ được lưu trên đĩa. |

---

## **Ví dụ về tối ưu hóa với LEFT JOIN**
- **Truy vấn**:
  ```sql
    explain SELECT *  FROM events  LEFT JOIN address  ON events.id = address.user_id  WHERE events.id>10;
