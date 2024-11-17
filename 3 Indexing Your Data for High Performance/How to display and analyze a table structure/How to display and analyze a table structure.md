# Hiển thị và phân tích cấu trúc bảng trong MySQL

## **1. Tầm quan trọng của việc hiểu cấu trúc bảng**
- Lệnh SQL rất đơn giản và dễ học, nhưng:
  - Hiệu suất của các truy vấn và chức năng cơ sở dữ liệu có thể khác nhau.
  - Khi lượng dữ liệu tăng lên, tối ưu hóa truy vấn trở nên quan trọng hơn.
  - Điều này đặc biệt đúng nếu cơ sở dữ liệu của bạn hỗ trợ một trang web có lưu lượng truy cập lớn.

- Trong bối cảnh tối ưu hóa truy vấn:
  - **Hiểu cấu trúc bảng** là rất quan trọng.
  - Nhờ việc hiển thị cấu trúc bảng, bạn có thể:
    - Diễn giải kế hoạch thực thi (**EXPLAIN PLAN**).
    - Tham chiếu và tìm ra giải pháp cho các vấn đề về hiệu suất truy vấn.

---

## **2. Những thông tin cấu trúc bảng cung cấp**
- Cấu trúc bảng cung cấp câu chuyện về những khía cạnh quan trọng sau:
  1. Bảng được sử dụng hoặc sẽ được sử dụng như thế nào?
  2. Loại dữ liệu được lưu trữ trong bảng là gì?
  3. Các cột nào đã được lập chỉ mục?
  4. Bảng có sử dụng **triggers** không?
  5. Có **khóa chính (Primary Key)** hoặc **khóa thứ cấp (Secondary Key)** không?
  6. Có nhận xét (**comments**) nào giúp hiểu thêm về các loại dữ liệu cột không?

---

## **3. Lệnh hiển thị cấu trúc bảng**
Để xem cấu trúc và lịch sử của bảng, sử dụng lệnh:
```sql
SHOW CREATE TABLE [table_name]\G
