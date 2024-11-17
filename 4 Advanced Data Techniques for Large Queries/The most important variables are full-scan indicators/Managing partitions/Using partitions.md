# **Sử dụng phân vùng trong MySQL**

---

## **Lý do phổ biến cho việc sử dụng phân vùng**
- Một trong những lý do phổ biến nhất để sử dụng phân vùng là **phân chia dữ liệu theo ngày tháng**.
- Một số hệ quản trị cơ sở dữ liệu (RDBMS) hỗ trợ phân vùng dựa trên ngày tháng một cách rõ ràng.
- Trong MySQL 8.0, mặc dù không có **phân vùng trực tiếp theo ngày tháng**, nhưng bạn có thể dễ dàng tạo phân vùng dựa trên các cột kiểu **DATE**, **TIME**, hoặc **DATETIME**.

---

## **Ví dụ phân vùng theo ngày tháng**
### **Tạo bảng với phân vùng theo khoảng ngày tháng**
- Dữ liệu giao dịch được lưu theo từng tháng.

```sql
CREATE TABLE transactions (
    id INT NOT NULL,
    transaction_date DATE NOT NULL,
    amount DECIMAL(10, 2),
    PRIMARY KEY (id, transaction_date)
)
PARTITION BY RANGE (YEAR(transaction_date), MONTH(transaction_date)) (
    PARTITION pJan VALUES LESS THAN (2023, 2),   -- Tháng 1 năm 2023
    PARTITION pFeb VALUES LESS THAN (2023, 3),   -- Tháng 2 năm 2023
    PARTITION pMar VALUES LESS THAN (2023, 4),   -- Tháng 3 năm 2023
    PARTITION pApr VALUES LESS THAN MAXVALUE     -- Các tháng còn lại
);
