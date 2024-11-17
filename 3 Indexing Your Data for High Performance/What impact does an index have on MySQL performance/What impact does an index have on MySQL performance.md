# Chỉ mục ảnh hưởng như thế nào đến hiệu suất của MySQL?

## **1. Ảnh hưởng đến hiệu suất ghi dữ liệu**
- Việc thêm chỉ mục có thể tối ưu hóa truy vấn, nhưng cũng gây ảnh hưởng tiêu cực đến hiệu suất ghi dữ liệu (**INSERT**, **UPDATE**, **DELETE**).
- **Ví dụ**:
  - Trong bảng `music_album`, khi thêm nhiều chỉ mục hơn, tốc độ chèn dữ liệu bị giảm **bốn lần** so với định nghĩa ban đầu với ít chỉ mục hơn.
  - Điều này minh họa rõ ràng rằng chỉ mục bổ sung có thể làm suy giảm hiệu suất ghi.
![alt text](<image.png>)
![alt text](<ex3.png>)
---

## **2. Nhiệm vụ tối ưu hóa chỉ mục**
- Một trong những nhiệm vụ tối ưu hóa là **xóa các chỉ mục trùng lặp**:
  - **Chỉ mục trùng lặp hoàn toàn**:
    - Các chỉ mục giống hệt nhau.
  - **Chỉ mục là tập con của chỉ mục khác**:
    - Nếu một chỉ mục nằm ở **phần bên trái** của một chỉ mục khác, nó là trùng lặp và không được sử dụng.
    - **Ví dụ**: Trong bảng `music_album`, chỉ mục `music_country_id` là trùng lặp vì đã được bao hàm trong chỉ mục `idx2`.
![alt text](<ex2.png>)
---

## **Tóm tắt**
- **Chỉ mục giúp tăng hiệu suất truy vấn đọc**, nhưng làm giảm hiệu suất ghi dữ liệu.
- **Xóa các chỉ mục trùng lặp** là một chiến lược tối ưu hóa cần thiết để giảm chi phí bảo trì và cải thiện hiệu suất tổng thể.
- Luôn cân nhắc giữa lợi ích và chi phí khi thêm chỉ mục vào bảng.
