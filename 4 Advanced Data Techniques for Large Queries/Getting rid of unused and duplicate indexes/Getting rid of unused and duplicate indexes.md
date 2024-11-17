# **Loại bỏ các chỉ mục không sử dụng và trùng lặp**

---

## **Tầm quan trọng của việc loại bỏ chỉ mục không cần thiết**
- **Chỉ mục** có thể tạo ra sự khác biệt lớn trong hiệu suất của cơ sở dữ liệu:
  - **Chỉ mục tốt**: Tăng tốc độ truy vấn.
  - **Chỉ mục không cần thiết hoặc trùng lặp**: Làm chậm các hoạt động ghi dữ liệu và tăng chi phí bảo trì.
- Việc bảo trì định kỳ giúp xác định và loại bỏ các chỉ mục không còn hữu ích hoặc bị trùng lặp.

---

## **Giới thiệu schema SYS**
- Từ MySQL **5.7.7**, schema mới có tên **SYS** được giới thiệu.
- **SYS schema** cung cấp công cụ để:
  - Hỗ trợ các DBA và nhà phát triển đọc dữ liệu từ **performance schema**.
  - Hỗ trợ các trường hợp tối ưu hóa và chẩn đoán.

---

## **Các đối tượng trong SYS schema**
### **1. Views**
- Tóm tắt dữ liệu hiệu suất schema dưới dạng tiện lợi và dễ hiểu hơn.
- Giúp người dùng nhanh chóng xác định các chỉ mục cần tối ưu.

### **2. Stored Procedures và Functions**
- Thực hiện các hoạt động như:
  - **Cấu hình performance schema**.
  - **Tạo báo cáo chẩn đoán**.
- Hỗ trợ tối ưu hóa cơ sở dữ liệu.

---

## **Ứng dụng thực tế**
- **Tìm kiếm chỉ mục không sử dụng**:
  - Sử dụng **SYS schema** để xác định các chỉ mục không được tham chiếu trong các truy vấn gần đây.
- **Xác định chỉ mục trùng lặp**:
  - Phân tích cấu trúc bảng và truy vấn để phát hiện các chỉ mục có phạm vi tương tự.

---

## **Lợi ích của SYS schema**
- Dễ dàng chẩn đoán và tối ưu hóa cơ sở dữ liệu.
- Giảm thời gian và công sức trong việc bảo trì chỉ mục.
- Cải thiện hiệu suất tổng thể của MySQL.

