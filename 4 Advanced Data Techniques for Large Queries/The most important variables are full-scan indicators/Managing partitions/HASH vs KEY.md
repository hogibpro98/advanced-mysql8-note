# **So sánh phân vùng HASH và KEY trong MySQL**

| **Tiêu chí**                | **Phân vùng HASH**                                     | **Phân vùng KEY**                                      |
|-----------------------------|-------------------------------------------------------|-------------------------------------------------------|
| **Hàm băm**                 | Do người dùng định nghĩa (custom hash function).      | Do MySQL cung cấp (built-in hash function).           |
| **Cột tham chiếu**          | Biểu thức hoặc cột được chỉ định bởi người dùng.       | Danh sách cột hoặc tự động sử dụng khóa chính (Primary Key). |
| **Mặc định cột phân vùng**  | Không có mặc định, người dùng phải cung cấp biểu thức.| Sử dụng khóa chính của bảng nếu không chỉ định.       |
| **Linh hoạt trong định nghĩa** | Người dùng có thể tự do tạo biểu thức phức tạp.        | Chỉ hỗ trợ danh sách tên cột đơn giản.                |
| **Yêu cầu khóa chính**      | Không yêu cầu bảng phải có khóa chính.                | Yêu cầu sử dụng một phần hoặc toàn bộ khóa chính nếu bảng có khóa chính. |
| **Cách tính phân vùng**     | Sử dụng biểu thức băm do người dùng chỉ định.          | MySQL tự động tính toán phân vùng dựa trên hàm băm nội bộ. |
| **Cú pháp**                 | `PARTITION BY HASH (expression)`                      | `PARTITION BY KEY (column_name)`                     |
| **Phân phối dữ liệu**       | Phụ thuộc vào biểu thức băm do người dùng định nghĩa. | Tự động phân phối đồng đều dựa trên hàm băm MySQL.    |
| **Độ phức tạp**             | Có thể phức tạp hơn nếu biểu thức băm phức tạp.        | Đơn giản hơn, không cần định nghĩa hàm băm.           |
| **Ứng dụng phù hợp**        | Khi cần tùy chỉnh cách phân phối dữ liệu.             | Khi muốn tự động hóa và tối ưu hóa việc phân phối dữ liệu. |

---

### **Tóm tắt**
- **Phân vùng HASH**:
  - Phù hợp khi cần **tùy chỉnh** cách dữ liệu được phân phối dựa trên biểu thức cụ thể.
  - Linh hoạt, nhưng đòi hỏi người dùng phải định nghĩa biểu thức băm.

- **Phân vùng KEY**:
  - Phù hợp khi muốn **đơn giản hóa** việc phân vùng mà không cần định nghĩa biểu thức.
  - Thích hợp với bảng có **khóa chính** và cần phân phối dữ liệu đồng đều.
