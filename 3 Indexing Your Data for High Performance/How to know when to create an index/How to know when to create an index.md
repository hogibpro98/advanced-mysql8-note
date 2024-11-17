# Khi nào cần tạo một chỉ mục?

## **1. Chỉ mục trong cơ sở dữ liệu là gì?**
- Chỉ mục trong cơ sở dữ liệu giống như phần **mục lục** ở cuối một cuốn sách:
  - Là một phần riêng biệt, được sắp xếp, giúp tra cứu nhanh thông tin mà không phải duyệt qua từng trang.
- **Ý nghĩa**:
  - Chỉ mục là một danh sách có thứ tự của các thông tin liên quan, giúp tăng tốc tìm kiếm các bản ghi phù hợp với tiêu chí.
  - Chỉ mục ngăn chặn việc quét toàn bộ bảng (**full table scan**) khi tìm kiếm hoặc sắp xếp dữ liệu.

---

## **2. Khi nào nên tạo chỉ mục?**
Bạn nên tạo chỉ mục nếu:
- Có thông tin cần tìm kiếm thường xuyên và yêu cầu phản hồi nhanh.
- Cần sắp xếp (**ORDER BY**) hoặc nhóm dữ liệu (**GROUP BY**) trên một cột.

> **Lưu ý**: Nếu bạn muốn kết hợp nhiều cột trong một chỉ mục, các truy vấn phải có cùng số lượng cột và thứ tự trong các câu lệnh **WHERE**.

---

## **3. 5 nguyên tắc chung để tạo chỉ mục**
Hãy trả lời **"Có"** cho những câu hỏi sau để biết khi nào cần tạo chỉ mục:
1. Có nên lập chỉ mục cho mỗi **khóa chính (Primary Key)** không?
2. Có nên lập chỉ mục cho mỗi **khóa ngoại (Foreign Key)** hoặc **khóa thứ cấp (Secondary Key)** không?
3. Có nên lập chỉ mục cho mỗi cột được sử dụng trong câu lệnh **JOIN** không?
4. Có nên lập chỉ mục cho mỗi cột trong câu lệnh **WHERE** không?
5. Có nên lập chỉ mục cho mỗi cột trong câu lệnh **GROUP BY** hoặc **ORDER BY** không?

---

## **4. Khi nào không cần chỉ mục?**
- **Bảng nhỏ**:
  - Nếu bảng có rất ít bản ghi (ví dụ: vài nhân viên), việc sử dụng chỉ mục không hiệu quả so với việc để MySQL quét toàn bộ bảng.

- **Tác động đến các lệnh khác**:
  - Chỉ mục có thể làm chậm các lệnh **INSERT**, **UPDATE**, và **DELETE** vì cần cập nhật chỉ mục sau mỗi thao tác.

---

## **5. Chỉ mục nhiều cột**
- Trong nhiều trường hợp, một chỉ mục loại nhiều cột (**multi-column index**) sẽ hiệu quả hơn so với việc tạo nhiều chỉ mục cho từng cột.
- Tuy nhiên, cần cân nhắc kỹ khi thiết kế chiến lược chỉ mục, vì chúng có thể ảnh hưởng đến các lệnh SQL khác.

---

## **6. Ví dụ cấu trúc bảng đơn giản**
```sql
CREATE TABLE employees (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT
);
