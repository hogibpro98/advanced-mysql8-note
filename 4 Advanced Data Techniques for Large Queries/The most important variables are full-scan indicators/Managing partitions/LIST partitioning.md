# **Phân vùng LIST trong MySQL**

---

## **Định nghĩa**
- **Phân vùng LIST** tương tự như **phân vùng RANGE**, nhưng dựa trên các giá trị cụ thể của cột thay vì phạm vi giá trị.
- **Khác biệt chính**:
  - **RANGE**: Dựa trên các khoảng giá trị (ví dụ: 1-10, 11-20).
  - **LIST**: Dựa trên danh sách các giá trị cụ thể (ví dụ: 1, 2, 3).

---

## **Cách hoạt động**
- Sử dụng tùy chọn **`PARTITION BY LIST (expression)`**, trong đó:
  - **`expression`**: Một giá trị cột hoặc biểu thức dựa trên cột.
  - **`VALUES IN (values_list)`**: Danh sách các giá trị phân biệt được xác định bởi dấu phẩy.

---

## **Cú pháp cơ bản**
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
) 
PARTITION BY LIST (column_name) (
    PARTITION partition_name VALUES IN (value1, value2, ...),
    PARTITION partition_name VALUES IN (value3, value4, ...)
);
```
---
### Ví dụ
#### Tình huống
- 17 nhà hàng được chia thành 3 nhượng quyền thương mại (franchises):
- Franchise 1: resto_id = 1, 2, 3.
- Franchise 2: resto_id = 4, 5, 6.
- Franchise 3: resto_id = 7, 8, 9.
- Tạo bảng với phân vùng LIST
```sql
CREATE TABLE restaurants (
    id INT NOT NULL AUTO_INCREMENT,
    resto_id INT NOT NULL,
    name VARCHAR(50) NOT NULL,
    location VARCHAR(50) NOT NULL,
    PRIMAY KEY (resto_id)
) PARTITION BY LIST (resto_id) (
    PARTITION franchise1 VALUES IN (1, 2, 3),\
    PARTITION franchise2 VALUES IN (4, 5, 6),\
    PARTITION franchise3 VALUES IN (7, 8, 9)  
);

```
#### Ưu điểm
- Quản lý dễ dàng: Có thể thêm hoặc xóa dữ liệu từ các phân vùng cụ thể dễ dàng hơn.
- Xóa dữ liệu nhanh chóng:
- Ví dụ: Nếu toàn bộ nhà hàng trong vùng pR3 được bán cho công ty khác, bạn có thể xóa nhanh dữ liệu:
sql
Copy code
ALTER TABLE employees TRUNCATE PARTITION pR3;
Hiệu quả hơn so với:
```sql
Copy code
DELETE FROM employees WHERE resto_id IN (7, 8, 9);
```
#### Kết luận
Phân vùng LIST lý tưởng cho các bảng có dữ liệu được phân nhóm dựa trên các giá trị cụ thể.
Dễ dàng mở rộng hoặc bảo trì bằng cách thêm, xóa, hoặc quản lý các phân vùng riêng lẻ.
Copy code