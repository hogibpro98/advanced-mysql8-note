# **Partition Pruning trong MySQL**
// Pruning mean vietnamese is "cắt tỉa", nên nói tóm lại là kỹ thuật cắt tỉa các phân vùng không cần thiết trong quá trình truy vấn dữ liệu.

---

## **Partition Pruning là gì?**
- Partition Pruning là một trong những kỹ thuật tối ưu hóa nổi tiếng nhất trong MySQL.

---

## **Partition Pruning là gì?**
- Partition Pruning là một trong những kỹ thuật tối ưu hóa nổi tiếng nhất.
- Nguyên tắc cơ bản: **"Không phân tích các phân vùng không chứa giá trị tương ứng"**.
- Mục tiêu: Giảm lượng dữ liệu MySQL cần quét bằng cách chỉ truy vấn trong các phân vùng liên quan.

---

## **Ví dụ Partition Pruning**
### **Tạo bảng phân vùng `employees`**
```sql
CREATE TABLE employees (
    id INT NOT NULL,
    firstname VARCHAR(50),
    lastname VARCHAR(50),
    job_id INT NOT NULL,
    resto_id INT NOT NULL,
    PRIMARY KEY (id, job_id)
)
PARTITION BY RANGE (job_id) (
    PARTITION p0 VALUES LESS THAN (5),
    PARTITION p1 VALUES LESS THAN (10),
    PARTITION p2 VALUES LESS THAN (15),
    PARTITION p3 VALUES LESS THAN (20)
);

-- Truy vấn SELECT
SELECT firstname, lastname, job_id, resto_id
FROM employees
WHERE job_id > 14 AND job_id < 17;

-- Phân vùng cần quét:
-- Truy vấn chỉ cần kiểm tra các phân vùng p2 và p3, vì các giá trị từ 15 đến 16 nằm trong hai phân vùng này.
-- Các phân vùng khác (p0, p1) sẽ bị loại bỏ (prune).
```
---
## Điều kiện sử dụng Partition Pruning
### Partition Pruning có thể được áp dụng nếu điều kiện WHERE phù hợp với các mẫu sau:
```sql
-- Cột phân vùng bằng một hằng số:

partition_column = constant
-- Ví dụ:
WHERE job_id = 15

-- Cột phân vùng nằm trong một tập giá trị:
WHERE partition_column IN (c1, c2, ..., cN)

-- Ví dụ:
WHERE job_id IN (5, 10, 15)
```
## Hỗ trợ trong các câu lệnh
### Partition Pruning hoạt động với hầu hết các loại truy vấn, bao gồm:

- DELETE: Xóa dữ liệu chỉ trong phân vùng liên quan.
- UPDATE: Cập nhật dữ liệu trong phân vùng cụ thể.
- INSERT: Chèn dữ liệu tự động vào phân vùng phù hợp.
- JOIN: Tối ưu hóa các phép JOIN khi một bảng được phân vùng.
### Tóm tắt
- Partition Pruning là một kỹ thuật mạnh mẽ để giảm lượng dữ liệu cần quét.
- Hiệu quả khi sử dụng với điều kiện WHERE cụ thể để giới hạn các phân vùng liên quan.
- Áp dụng được cho nhiều loại truy vấn như SELECT, DELETE, UPDATE, và JOIN.