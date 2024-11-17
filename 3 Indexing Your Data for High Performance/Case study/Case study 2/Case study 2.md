# **How to display and analyze a table structure versus the EXPLAIN plan tool Tối ưu hóa truy vấn: Các bước cần thực hiện**

---

## **Các bước để tối ưu hóa truy vấn**

1. **Chạy kế hoạch thực thi (EXPLAIN)**
   - Thêm từ khóa **EXPLAIN** trước truy vấn của bạn để xem kế hoạch thực thi của MySQL.

2. **Quan sát kết quả từ EXPLAIN**
   - Tìm kiếm các phần trong cột **ACCESS_TYPE** có giá trị **ALL** hoặc các dấu hiệu khác cho thấy truy vấn cần được tối ưu hóa.

3. **Phân tích cấu trúc bảng**
   - Sử dụng lệnh:
     ```sql
     SHOW CREATE TABLE [tablename]\G
     ```
   - Đặt câu hỏi:
     1. **Bảng có khóa chính (Primary Key) không?**
     2. **Các cột trong mệnh đề `WHERE`, `GROUP BY`, `ORDER BY`, hoặc `HAVING` đã có chỉ mục chưa?**
        - Nếu chưa, bạn cần xác định:
          - Có cần tạo chỉ mục **Compound** bao gồm các cột trong `WHERE`, `GROUP BY`, và `ORDER BY` không?
          - Hay chỉ cần một chỉ mục đơn giản cho cột trong mệnh đề `WHERE`?

4. **Sửa đổi truy vấn nếu cần**
   - Điều chỉnh truy vấn của bạn để sử dụng các chỉ mục đã tạo.

5. **Lặp lại quy trình nếu cần**
   - Thông thường, các bước 1 đến 3 là đủ. Tuy nhiên, với các truy vấn phức tạp, bạn có thể cần lặp lại quá trình này để đạt hiệu suất tối ưu.

---

## **Những điều đã học trong phần này**
- Cách chạy kế hoạch thực thi với **EXPLAIN**.
- Cách hiển thị cấu trúc bảng để hỗ trợ tối ưu hóa truy vấn.
- Cách so sánh cấu trúc bảng với kết quả từ **EXPLAIN**.

---

## **Phần tiếp theo**
- Bạn sẽ học cách tổ chức các cột trong chiến lược lập chỉ mục để đạt hiệu suất cao nhất.
