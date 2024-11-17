# **Tip 5 – Khuyến nghị về hiệu suất của kiến trúc MySQL**

---

## **Thực trạng phổ biến**
- **Quan sát thực tế**:
  - Phần lớn các công ty chỉ sử dụng **một máy chủ MySQL** để xử lý cả tác vụ **đọc** và **ghi**.
  - Các tác vụ đọc bao gồm:
    - Báo cáo hàng ngày, hàng tháng, hàng năm.
    - Các truy vấn tạo ra kết quả phân tích lớn.
  - Khi công ty phát triển, áp lực lên cơ sở dữ liệu MySQL tăng lên:
    - Giới hạn tài nguyên vật lý (RAM, CPU, Disk).
    - Các truy vấn không được tối ưu hóa gây quá tải.

---

## **Những điều cần cân nhắc khi công ty phát triển nhanh**
Để duy trì hiệu suất MySQL, cần tập trung vào ba khía cạnh chính:

### **1. Tối ưu hóa truy vấn**
- **Lý do**:
  - Các truy vấn không tối ưu là nguyên nhân chính khiến MySQL chậm.
- **Giải pháp**:
  - Tối ưu hóa truy vấn bằng cách:
    - Sử dụng chỉ mục hiệu quả (Compound Index).
    - Kiểm tra và tối ưu kế hoạch thực thi truy vấn với **EXPLAIN**.
    - Tránh quét toàn bộ bảng (FULL SCAN).

---

### **2. Phân tán kiến trúc MySQL với Replication**
- **Replication là gì?**
  - Là quá trình tạo các **bản sao (replicas)** của cơ sở dữ liệu chính (primary server).
  - Các bản sao được sử dụng để xử lý các truy vấn đọc, giảm tải cho máy chủ chính.

- **Chiến lược**:
  - Sử dụng các **replicas** để chạy báo cáo và truy vấn phân tích.
  - Mỗi replica được chuyên môn hóa cho các mục tiêu cụ thể (ví dụ: báo cáo hàng tháng, hàng năm).

---

### **3. Nâng cấp tài nguyên phần cứng**
- **Khi nào cần nâng cấp?**
  - Nếu đã tối ưu hóa truy vấn và sử dụng replication, nhưng máy chủ chính vẫn chậm, cần xem xét việc nâng cấp phần cứng.

- **Tài nguyên cần nâng cấp**:
  - **RAM**: Tăng bộ nhớ để cải thiện khả năng lưu trữ bộ đệm (InnoDB buffer pool).
  - **CPU**: Sử dụng bộ xử lý nhanh hơn hoặc nhiều nhân hơn để xử lý các tác vụ đa luồng.
  - **Disk**: Thay thế ổ cứng HDD bằng SSD hoặc NVMe để tăng tốc độ đọc/ghi dữ liệu.

---

## **Kết luận**
- **Tối ưu hóa truy vấn** luôn là bước đầu tiên và quan trọng nhất để cải thiện hiệu suất.
- **Phân tán kiến trúc MySQL** bằng replication giúp giảm tải cho máy chủ chính, đặc biệt khi có các báo cáo lớn.
- **Nâng cấp phần cứng** là giải pháp cuối cùng khi tất cả các biện pháp trên đã được triển khai nhưng hiệu suất vẫn chưa đạt yêu cầu.
