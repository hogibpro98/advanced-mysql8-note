# **Techniques**

## **Sử dụng MySQL hiệu quả với các tham số tối ưu hóa**
Ngoài việc sử dụng các tính năng đúng cách, có một số tham số có thể được điều chỉnh dựa trên các trường hợp sử dụng MySQL. Dưới đây là các thảo luận về khả năng của MySQL khi xử lý cơ sở dữ liệu lớn và các nguyên nhân gây suy giảm hiệu suất.

---

## **MySQL có thể thực hiện truy vấn trên hàng tỷ dòng dữ liệu không?**
- MySQL có khả năng thực hiện truy vấn trên hàng tỷ dòng dữ liệu, tuy nhiên điều này phụ thuộc vào kiểu truy vấn được thực hiện:
  - **Truy vấn đọc sử dụng khóa chính:** Nếu truy vấn chỉ lọc theo khóa chính (ví dụ: `WHERE primary_key = value`), hiệu suất rất tốt.
  - **Truy vấn với phạm vi (`RANGE`), LIKE, hoặc REGEX:** Hiệu suất sẽ giảm do cần quét thêm dữ liệu.
  - **Truy vấn trên các cột không được lập chỉ mục:** Dẫn đến **quét toàn bộ bảng** (full table scan), làm giảm hiệu suất nghiêm trọng.

---

## **InnoDB có phù hợp cho cơ sở dữ liệu hàng tỷ dòng không?**
- InnoDB là lựa chọn phù hợp cho cơ sở dữ liệu lớn, tuy nhiên, điều này phụ thuộc vào kích thước hàng trung bình:
  - **Kích thước hàng trung bình ~1 KB:** InnoDB là lựa chọn tốt cho cơ sở dữ liệu lớn.
  - **Kích thước hàng trung bình ~10 MB:** Hiệu suất có thể giảm do tải trọng lớn.

---

## **Kích thước tối đa của MySQL trước khi hiệu suất giảm?**
- Hiệu suất của MySQL không chỉ phụ thuộc vào kích thước cơ sở dữ liệu mà còn vào:
  - **Thiết kế schema:** Schema đơn giản, tối ưu hóa sẽ hỗ trợ cơ sở dữ liệu có hơn 1 tỷ dòng hoạt động hiệu quả.
  - **Độ phức tạp của truy vấn:** Truy vấn phức tạp, không tối ưu hóa có thể làm giảm hiệu suất ngay cả với cơ sở dữ liệu nhỏ.

---

## **Tại sao MySQL chậm với các bảng lớn?**
- Các lý do chính bao gồm:
  - **Truy vấn trên các cột không được lập chỉ mục:** Gây ra quét toàn bộ bảng, tiêu tốn tài nguyên.
  - **Quét phạm vi (`RANGE`):** Yêu cầu xử lý dữ liệu tương tự như các cột không có chỉ mục.

---

## **MySQL có phải là giải pháp tốt nhất để xử lý blobs không?**
- Phụ thuộc vào loại blob và cách sử dụng:
  - **TINYBLOB/BLOB:** Hiệu quả với hàng triệu dòng và tần suất sử dụng cao.
  - **MEDIUMBLOB/LONGBLOB:** Hiệu suất giảm khi xử lý hàng triệu dòng với tần suất cao.
- **Các thao tác trên bảng:** Với bảng chứa nhiều blob lớn, các thao tác như `OPTIMIZE` hoặc `ALTER` sẽ có thời gian thực thi lâu hơn.

**Tham khảo thêm:** [Yêu cầu lưu trữ dữ liệu MySQL](https://dev.mysql.com/doc/refman/8.0/en/storage-requirements.html).

