```markdown
# **Tip 2 – Năm quy tắc chung để tối ưu hóa truy vấn**

---

## **Các bước tối ưu hóa truy vấn**

1. **Chạy kế hoạch thực thi (EXPLAIN)**
   - Thêm từ khóa **EXPLAIN** trước truy vấn để xem kế hoạch thực thi mà MySQL sẽ sử dụng.

2. **Quan sát kết quả từ EXPLAIN**
   - Tìm kiếm các phần trong cột **ACCESS_TYPE** có giá trị **ALL**.
   - Dấu hiệu **ALL** trong ACCESS_TYPE cho thấy MySQL đang thực hiện **FULL TABLE SCAN** (quét toàn bộ bảng), điều này cần được chú ý.

3. **Phân tích cấu trúc bảng**
   - Sử dụng lệnh:
     ```sql
     SHOW CREATE TABLE [tablename]\G
     ```
   - Trả lời các câu hỏi sau:
     1. **Bảng có khóa chính (Primary Key) không?**
     2. **Có chỉ mục cho các cột được sử dụng trong WHERE, GROUP BY, ORDER BY, hoặc HAVING không?**
     3. **Nếu câu trả lời là "không" cho câu hỏi (2)**:
        - Có cần một **COMPOUND index** để bao gồm các cột trong WHERE, GROUP BY, ORDER BY không?
        - Hay chỉ cần một chỉ mục đơn giản cho cột trong mệnh đề WHERE?

4. **Sửa đổi truy vấn nếu cần**
   - Điều chỉnh truy vấn để tận dụng các chỉ mục đã tạo.

5. **Lặp lại các bước**
   - Thông thường, các bước từ 1 đến 3 là đủ.
   - Tuy nhiên, nếu truy vấn phức tạp, bạn có thể cần lặp lại quá trình này để tối ưu hóa thêm.

---

## **Lưu ý quan trọng**
- **Chỉ mục phù hợp giúp giảm thiểu chi phí truy vấn**:
  - Đảm bảo rằng các cột trong mệnh đề **WHERE**, **GROUP BY**, và **ORDER BY** đều có chỉ mục.
  - **COMPOUND index** đặc biệt hiệu quả với các truy vấn kết hợp nhiều cột.

- **Lặp lại quy trình nếu cần thiết**:
  - Với các truy vấn phức tạp, việc thử nghiệm và tối ưu hóa nhiều lần có thể cần thiết để đạt được hiệu suất tối đa.

---

## **Kết luận**
- Sử dụng **EXPLAIN** để kiểm tra kế hoạch thực thi và xác định các vấn đề.
- Đảm bảo các chỉ mục được tạo trên các cột cần thiết trong truy vấn.
- Với các bảng lớn hoặc truy vấn phức tạp, lặp lại quy trình cho đến khi đạt hiệu suất tối ưu.
```