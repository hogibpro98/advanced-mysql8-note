# **Phân vùng một bảng (Partitioning a Table)**

---

## **Định nghĩa**
- **Phân vùng bảng (Partitioning a Table)** là cách MySQL chia dữ liệu thực tế của một bảng thành nhiều bảng riêng biệt.
- **Lưu ý quan trọng**: 
  - Dù dữ liệu được chia thành các bảng nhỏ (partitions), MySQL vẫn xử lý chúng như **một bảng duy nhất** ở cấp độ SQL.

---
# An overview of partitioning

# **Tổng quan về phân vùng trong MySQL 8.0**

---

## **Phân vùng là gì?**
- Khi phân vùng, điều quan trọng là phải xác định **khóa phân vùng (partition key)**.
- **Mục tiêu**:
  - Đảm bảo các truy vấn như **SELECT, UPDATE, DELETE** sử dụng khóa phân vùng trong **WHERE clause** để truy vấn chỉ tập trung vào phân vùng liên quan, từ đó tăng hiệu suất.

---

## **Thực hành tốt nhất**
1. **Sử dụng khóa phân vùng với khóa chính**:
   - Thêm khóa phân vùng vào khóa chính (**PRIMARY KEY**) với tính năng tự động tăng (**AUTO_INCREMENT**).
   - Ví dụ: 
     ```sql
     PRIMARY KEY (customer_id, id)
     ```
   - **Lưu ý**: 
     - Nếu không thiết kế tốt khóa chính (ví dụ: không chọn các cột nhỏ và hiệu quả), điều này có thể làm tăng kích thước của các chỉ mục thứ cấp (**secondary indexes**).

---

## **Các kiểu phân vùng**
1. **Phân vùng theo RANGE (Range Partitioning)**:
   - **Phù hợp**: Khi dữ liệu được nhóm lại thành các nhóm ID đã biết trong từng phân vùng.
   - **Ưu điểm**: 
     - Dễ truy vấn dữ liệu dựa trên ID phân vùng đã biết.

2. **Phân vùng theo HASH (Hash Partitioning)**:
   - **Phù hợp**: Khi cần cân bằng tải trên bảng.
   - **Ưu điểm**:
     - Cho phép ghi dữ liệu đồng thời vào các phân vùng.

---

## **Hỗ trợ phân vùng trong MySQL 8.0**
- Từ MySQL 8.0, bạn có thể sử dụng phân vùng trên bảng **InnoDB**.
- Điều này mở rộng khả năng tối ưu hóa hiệu suất trên các bảng lớn bằng cách phân vùng dữ liệu.

---
# **Các loại phân vùng trong MySQL**

---

## **Tại sao cần phân vùng?**
- **Phân vùng (Partitioning)** giúp:
  - **Tăng hiệu suất**: Truy vấn chỉ cần truy cập một phần nhỏ dữ liệu, thay vì toàn bộ bảng.
  - **Dễ quản lý và bảo trì**: Tổ chức và chia nhỏ các bảng, chỉ mục, và dữ liệu.
  - **Giảm chi phí lưu trữ**: Tối ưu hóa việc lưu trữ lượng dữ liệu lớn.

---

## **Phân loại phân vùng trong MySQL**

### **1. Phân vùng theo chiều ngang (Horizontal Partitioning)**
- **Định nghĩa**:
  - Dữ liệu được chia theo hàng, tạo ra các phần độc lập.
- **Ứng dụng**:
  - Khi cần xử lý dữ liệu lớn và phân chia dựa trên một tiêu chí cụ thể, như **ngày** hoặc **ID người dùng**.

### **2. Phân vùng theo chiều dọc (Vertical Partitioning)**
- **Định nghĩa**:
  - Dữ liệu được chia theo cột, tách dữ liệu thành các bảng nhỏ hơn với các tập hợp cột khác nhau.
- **Ứng dụng**:
  - Khi muốn tối ưu hóa việc truy cập cột cụ thể trong một bảng lớn.

---

## **Các loại phân vùng có sẵn trong MySQL 8.0**

### **1. Phân vùng theo RANGE**
- **Mô tả**:
  - Dữ liệu được chia thành các dải giá trị liên tục.
- **Ví dụ**:
  - Dữ liệu từ `ID 1-1000` trong một phân vùng, từ `1001-2000` trong phân vùng khác.

### **2. Phân vùng theo LIST**
- **Mô tả**:
  - Dữ liệu được chia dựa trên danh sách các giá trị cụ thể.
- **Ví dụ**:
  - Các bản ghi từ khu vực `USA`, `EU`, `ASIA` được lưu trong các phân vùng riêng.

### **3. Phân vùng theo HASH**
- **Mô tả**:
  - Dữ liệu được phân chia dựa trên giá trị băm của một cột hoặc một biểu thức.
- **Ứng dụng**:
  - Cân bằng tải trên các phân vùng.

### **4. Phân vùng theo KEY**
- **Mô tả**:
  - Tương tự như phân vùng HASH nhưng dựa trên thuật toán mặc định của MySQL.
- **Ứng dụng**:
  - Dễ dàng hơn so với việc tự định nghĩa hàm băm.

---

## **Kết luận**
- MySQL 8.0 hỗ trợ nhiều loại phân vùng giúp tối ưu hóa hiệu suất truy vấn và quản lý dữ liệu lớn.
- Lựa chọn loại phân vùng phù hợp phụ thuộc vào cách tổ chức dữ liệu và yêu cầu truy vấn thực tế.

# **Phân vùng dữ liệu theo chiều ngang (Horizontal Partitioning)**

---

## **Định nghĩa**
- **Phân vùng theo chiều ngang** là phương pháp chia các bản ghi trong bảng thành nhiều phân vùng nhỏ hơn.
- **Đặc điểm**:
  - Tất cả các cột trong bảng vẫn tồn tại trong mỗi phân vùng.
  - Mỗi phân vùng có thể được quản lý riêng lẻ hoặc như một tập hợp.

---

## **Ví dụ thực tế**
- **Tình huống**:
  - Một bảng chứa thông tin giao dịch (**transactions**) như dữ liệu đăng ký trên trang web trong suốt một năm.
- **Giải pháp phân vùng**:
  - Phân vùng dữ liệu theo **tháng**:
    - Chia bảng thành **12 phân vùng**, mỗi phân vùng chứa dữ liệu của một tháng.

---

## **Lợi ích**
1. **Hiệu suất cao hơn**:
   - Truy vấn sẽ chỉ cần truy cập vào phân vùng liên quan, thay vì toàn bộ bảng.
2. **Dễ bảo trì**:
   - Có thể quản lý từng phân vùng riêng biệt (ví dụ: xóa phân vùng cũ khi hết hạn).
3. **Tăng khả năng mở rộng**:
   - Hỗ trợ lưu trữ và xử lý hiệu quả lượng dữ liệu lớn.

