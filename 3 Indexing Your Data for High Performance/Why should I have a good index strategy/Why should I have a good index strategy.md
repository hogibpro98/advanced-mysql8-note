# Tại sao tôi cần một chiến lược chỉ mục tốt?

### **Tầm quan trọng của chiến lược chỉ mục**
- Lập chỉ mục dữ liệu là một chủ đề rất rộng, khó để biết nên làm gì và không nên làm gì khi phát triển chiến lược chỉ mục.
- **Mục tiêu chính**: Cải thiện thời gian phản hồi của các yêu cầu hoặc ứng dụng.
- **Thách thức**: Lập chỉ mục dữ liệu là một **bài toán cân bằng**, vì:
  - Mỗi chỉ mục là một bảng được quản lý bởi hệ thống.
  - Việc **thêm hoặc sửa đổi dữ liệu** trong bảng sẽ yêu cầu cập nhật các chỉ mục, điều này có thể làm chậm hiệu suất cập nhật.

---

## **Các điểm cần cân nhắc khi lập chiến lược chỉ mục**

### **1. Tạo khóa chính (Primary Key)**
- Thường là một cột duy nhất, ví dụ: cột kết thúc bằng `id`.

### **2. Dự đoán các cột thường xuyên được truy vấn**
- Các cột trong các câu lệnh:
  - **WHERE**
  - **GROUP BY**
  - **HAVING**
  - **ORDER BY**

### **3. Lập chỉ mục các cột dùng với các hàm tổng hợp**
- Các hàm như:
  - **SUM()**, **COUNT()**, **MIN()**, **MAX()**, **AVG()**.
- **Lợi ích**: Cải thiện hiệu suất xử lý truy vấn.

### **4. Không nên tạo quá nhiều chỉ mục**
- Quá nhiều chỉ mục sẽ làm giảm hiệu suất tổng thể của MySQL:
  - Tăng chi phí cập nhật và chèn dữ liệu.
  - Gây quá tải hệ thống khi duy trì các chỉ mục.

### **5. Dự đoán các khóa thứ cấp (Secondary Key)**
- Tạo các chỉ mục duy nhất (**Unique Index**) cho các khóa thường được sử dụng trong câu **JOIN**:
  - Ví dụ: các cột kết thúc bằng `_id`.

### **6. Các ứng viên tốt cho chỉ mục**
- **Username** hoặc **Email**: Các cột này thường được truy vấn trong các ứng dụng.
- **UUID**: Một số ứng dụng thường sử dụng các URL hoặc UUID làm khóa.

---

## **Khi nào không nên tạo chỉ mục**

### **1. Bảng nhỏ hoặc bảng tĩnh**
- Đối với các bảng nhỏ hoặc bảng dữ liệu tĩnh:
  - **Quét toàn bộ bảng (Full Table Scan)** có thể hiệu quả hơn so với sử dụng chỉ mục.
  - **Nếu bảng có khoảng 100 - 10,000 dòng,** thì việc quét toàn bộ bảng (Full Table Scan) có thể nhanh hơn việc sử dụng chỉ mục.
  - **Tại sao quét toàn bộ bảng hiệu quả hơn với bảng nhỏ?** => Khi toàn bộ bảng đủ nhỏ để nằm trong bộ nhớ RAM, việc quét toàn bộ bảng sẽ nhanh hơn so với tìm kiếm thông qua chỉ mục, vì không có quá nhiều dữ liệu để đọc
- **Lý do**:
  - Quét toàn bộ bảng sẽ nhanh hơn nếu hầu hết các bản ghi trong bảng được truy vấn.

### **2. Tỷ lệ chỉ mục trên kích thước bảng quá lớn**
- Nếu chỉ mục lớn so với kích thước bảng:
  - Chỉ mục sẽ không giảm đáng kể không gian vật lý cần đọc.
  - Chỉ mục trở nên kém hiệu quả.

### **3. Bảng có ít cột và phần lớn bản ghi được truy xuất**
- Nếu một bảng nhỏ và phần lớn dữ liệu thường xuyên được truy vấn:
  - Chỉ mục sẽ không giúp giảm việc quét bảng.
  - Không nên tạo chỉ mục.

### **4. Không xóa khóa chính hoặc khóa ngoại**
- **Lưu ý quan trọng**:
  - Xóa khóa chính (**Primary Key**) hoặc khóa ngoại (**Foreign Key**) có thể gây ra hậu quả không mong muốn.

---

## **Tóm tắt**
- Chỉ nên tạo chỉ mục trên **một tỷ lệ nhỏ các cột** của bảng.
- Tránh tạo chỉ mục cho các bảng nhỏ hoặc các bảng mà việc quét toàn bộ là đủ nhanh.
- Xem xét tỷ lệ kích thước chỉ mục-bảng để đảm bảo chỉ mục thực sự mang lại lợi ích.
- **Lời khuyên chung**:
  - Lập chỉ mục thông minh sẽ cải thiện hiệu suất truy vấn, nhưng việc lạm dụng chỉ mục có thể làm giảm hiệu suất hệ thống tổng thể.
