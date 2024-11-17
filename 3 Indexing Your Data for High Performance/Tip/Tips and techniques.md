# **Mẹo và kỹ thuật tối ưu hóa chỉ mục**

---

## **Quy tắc chung cho việc tạo chỉ mục**
Dưới đây là 5 quy tắc phổ biến để xác định khi nào nên tạo chỉ mục. Bạn có thể trả lời "có" cho các câu hỏi sau:

1. **Bạn có thể lập chỉ mục cho mỗi khóa chính (Primary Key) không?**
   - **Ý nghĩa**: Luôn lập chỉ mục cho khóa chính để đảm bảo tìm kiếm nhanh và duy nhất.

2. **Bạn có thể lập chỉ mục cho mỗi khóa ngoại (Foreign Key) hoặc khóa thứ cấp (Secondary Key) không?**
   - **Ý nghĩa**: Hỗ trợ các phép JOIN giữa các bảng bằng cách lập chỉ mục các cột liên quan.

3. **Bạn có thể lập chỉ mục cho mỗi cột được sử dụng trong câu lệnh JOIN không?**
   - **Ý nghĩa**: Cải thiện hiệu suất khi thực hiện truy vấn JOIN giữa các bảng.

4. **Bạn có thể lập chỉ mục cho mỗi cột trong mệnh đề WHERE không?**
   - **Ý nghĩa**: Chỉ mục giúp lọc dữ liệu nhanh hơn trong điều kiện WHERE.

5. **Bạn có thể lập chỉ mục cho mỗi cột trong GROUP BY và ORDER BY không?**
   - **Ý nghĩa**: Chỉ mục giảm thời gian sắp xếp và nhóm dữ liệu.

6. **Bạn có định tạo báo cáo từ bảng này không?**
   - **Ý nghĩa**: Báo cáo thường yêu cầu lọc và sắp xếp dữ liệu nhanh, do đó chỉ mục rất quan trọng.

---

## **Khi nào cần sử dụng chỉ mục**
- **Trường hợp cần thiết**:
  - Khi sử dụng các mệnh đề **WHERE**, **GROUP BY**, **ORDER BY**, hoặc **JOIN**.
  - Khi bảng có **số lượng bản ghi lớn**. Chỉ mục chỉ thực sự hiệu quả khi bảng chứa nhiều dữ liệu.

- **Trường hợp không cần thiết**:
  - Nếu bảng rất nhỏ (Nếu bảng có khoảng 100 - 10,000 dòng), việc quét toàn bộ bảng (**full table scan**) có thể hiệu quả hơn so với sử dụng chỉ mục.

---

## **Sử dụng chỉ mục COMPOUND**
- **Ưu tiên chỉ mục COMPOUND** trong các truy vấn phức tạp.
- **Lợi ích**:
  - Giảm số lượng chỉ mục cần thiết.
  - Cải thiện hiệu suất bằng cách sử dụng một chỉ mục duy nhất cho nhiều cột trong mệnh đề **WHERE**, **GROUP BY**, hoặc **ORDER BY**.

---

## **Kết luận**
- Chỉ mục là công cụ mạnh mẽ để tối ưu hóa hiệu suất truy vấn, nhưng chúng cần được sử dụng đúng cách.
- Với các bảng nhỏ, chỉ mục có thể không cần thiết, nhưng với bảng lớn và truy vấn phức tạp, **chỉ mục COMPOUND** thường là lựa chọn tối ưu.