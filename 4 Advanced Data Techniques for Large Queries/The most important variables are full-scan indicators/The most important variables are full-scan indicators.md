# **Managing High Load on MySQL During Peak Seasons**

---

## **Tầm quan trọng của kiểm tra hiệu suất**
- **Thách thức**: Khi mùa cao điểm đến, bạn cần quản lý tải trên cơ sở dữ liệu MySQL sao cho không làm giảm hiệu suất.
- **Giải pháp không phù hợp**: Kiểm tra áp lực (**stress tests**) trực tiếp trên môi trường sản xuất là không an toàn.
- **Giải pháp thay thế**: Sử dụng các biến quan trọng trong MySQL để phân tích và dự đoán tải, chẳng hạn:
  - **Select_scan**.
  - **Select_full_join**.

---

## **Các biến quan trọng trong MySQL**

### **1. Select_scan**
- **Ý nghĩa**:
  - Biến này hiển thị số lần quét toàn bộ bảng (**full scan**) đã được thực hiện kể từ lần khởi động MySQL gần nhất.
- **Tìm hiểu**:
  - Sử dụng lệnh `SHOW GLOBAL STATUS` để xem giá trị của **Select_scan**.
  - **Lưu ý**: Biến này được đặt lại về 0 sau mỗi lần khởi động lại MySQL.

### **2. Select_full_join**
- **Ý nghĩa**:
  - Hiển thị số lần MySQL thực hiện **join không có chỉ mục**.
- **Tìm hiểu**:
  - Số lượng nhỏ của **Select_full_join** cho thấy việc tối ưu hóa join có thể bỏ qua để tập trung vào các yếu tố khác.

---

## **Khi nào cần quan tâm đến chỉ mục?**
- **Bảng nhỏ**:
  - Quét toàn bộ bảng (**full scan**) trên các bảng nhỏ(< 10000 bản ghi) thường nhanh, vì vậy bạn có thể bỏ qua chỉ mục khi tải nhẹ.
- **Tăng tải hoặc dữ liệu**:
  - Khi tải hoặc lượng dữ liệu tăng lên, cần kiểm tra lại và thêm chỉ mục nếu cần thiết.

---

## **Lời khuyên thực tế**
1. **Kiểm tra trước khi triển khai**:
   - Bất kỳ tính năng mới nào được thêm vào ứng dụng hoặc website nên được kiểm tra với lượng dữ liệu tương đương với môi trường sản xuất.
2. **Chuẩn bị trước chỉ mục**:
   - Tạo chỉ mục trên môi trường sản xuất trước khi triển khai để tránh ảnh hưởng đến hiệu suất khi tính năng hoạt động.

---

