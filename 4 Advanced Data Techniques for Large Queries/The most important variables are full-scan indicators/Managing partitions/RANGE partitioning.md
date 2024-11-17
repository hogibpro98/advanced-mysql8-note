# **Phân vùng RANGE**

---

## **Định nghĩa**
- **Phân vùng RANGE** chia các bản ghi trong bảng dựa trên giá trị của các cột nằm trong một phạm vi nhất định.
- Các phân vùng RANGE được tổ chức theo khoảng giá trị, đảm bảo các giá trị không bị trùng lặp và được thiết lập bằng toán tử **`VALUES LESS THAN`**.

---

## **Cú pháp cơ bản**
Cú pháp để tạo phân vùng với **RANGE** như sau:
```sql
PARTITION BY RANGE (column_name) (
    PARTITION partition_name VALUES LESS THAN (value),
    PARTITION partition_name VALUES LESS THAN (value),
    ...
);
```
# **Ví dụ về phân vùng RANGE**

## **Tình huống**
- **Bảng**: Lưu thông tin nhân viên cho một chuỗi nhà hàng pizza gồm 30 chi nhánh, được đánh số từ **1 đến 30**.
- **Yêu cầu**: Phân vùng bảng theo khoảng giá trị của cột **`resto_id`**.

---

## **Cách tổ chức phân vùng**
- Phân vùng bảng theo **`resto_id`** dựa trên các khoảng giá trị cụ thể.
```sql
CREATE TABLE employees (
    id INT NOT NULL,
    name VARCHAR(50) NOT NULL,
    resto_id INT NOT NULL,
    hire_date DATE NOT NULL
)
PARTITION BY RANGE (resto_id) (
    PARTITION p1 VALUES LESS THAN (10),
    PARTITION p2 VALUES LESS THAN (20),
    PARTITION p3 VALUES LESS THAN (30)
);
```

# **Ví dụ: Phân vùng RANGE và xử lý giá trị vượt quá**

---

## **Tình huống**
1. **Cách phân vùng ban đầu**:
   - Các nhân viên thuộc nhà hàng từ **1 đến 5** được lưu trong phân vùng **p0**.
   - Các nhân viên thuộc nhà hàng từ **6 đến 10** được lưu trong phân vùng **p1**, và tiếp tục như vậy đến **p4**.

2. **Ví dụ về thêm bản ghi**:
   - Một bản ghi mới có giá trị sau:
     ```sql
     (53, 'Sia', 'lalonde', '1999-06-24', NULL, 14)
     ```
   - Dữ liệu này sẽ được thêm vào phân vùng **p2**, vì `resto_id = 14`.

3. **Vấn đề khi thêm dữ liệu vượt phạm vi**:
   - Nếu nhà hàng thứ 31 được thêm (resto_id = 31), MySQL sẽ gặp lỗi vì không có quy tắc phân vùng nào áp dụng cho giá trị lớn hơn 30.

---

## **Giải pháp**
1. **Sử dụng `VALUES LESS THAN` với MAXVALUE**:
   - Thêm phân vùng xử lý các giá trị vượt quá:
     ```sql
     PARTITION p5 VALUES LESS THAN (MAXVALUE)
     ```
   - **MAXVALUE**: Đại diện cho giá trị **INTEGER lớn nhất**, đảm bảo các giá trị lớn hơn đều được lưu vào phân vùng **p5**.

2. **Cập nhật phân vùng bằng `ALTER TABLE`**:
   - Nếu cần thêm phân vùng mới cho các nhà hàng tương lai:
     ```sql
     ALTER TABLE employees
     ADD PARTITION (
         PARTITION p6 VALUES LESS THAN (40)
     );
     ```

---

## **Kết luận**
- **Phân vùng RANGE** yêu cầu quản lý cẩn thận để tránh lỗi khi dữ liệu vượt quá phạm vi được định nghĩa.
- **MAXVALUE** hoặc **ALTER TABLE** là những cách hiệu quả để mở rộng phân vùng cho dữ liệu trong tương lai.

# **Phân vùng RANGE với cột TIMESTAMP trong MySQL 8.0**

---

## **Tính năng**
- Trong MySQL 8.0, bạn có thể sử dụng kiểu phân vùng **RANGE** dựa trên giá trị của cột **TIMESTAMP**.
- Sử dụng hàm **UNIX_TIMESTAMP()** để chuyển giá trị **TIMESTAMP** thành số nguyên, giúp phân vùng hiệu quả hơn.

---

## **Ví dụ**
Phân vùng bảng dựa trên giá trị thời gian:
```sql
CREATE TABLE events (
    event_id INT NOT NULL,
    event_name VARCHAR(50),
    event_time TIMESTAMP NOT NULL
)
PARTITION BY RANGE (UNIX_TIMESTAMP(event_time)) (
    PARTITION p0 VALUES LESS THAN (UNIX_TIMESTAMP('2023-01-01 00:00:00')),
    PARTITION p1 VALUES LESS THAN (UNIX_TIMESTAMP('2023-07-01 00:00:00')),
    PARTITION p2 VALUES LESS THAN (MAXVALUE)
);
```

### Giải thích
#### Phân vùng theo thời gian:

- p0: Lưu các sự kiện trước ngày 2023-01-01.
- p1: Lưu các sự kiện từ ngày 2023-01-01 đến trước ngày 2023-07-01.
- p2: Lưu các sự kiện sau ngày 2023-07-01.
#### UNIX_TIMESTAMP():

- Chuyển giá trị TIMESTAMP thành số nguyên dạng UNIX, thuận tiện cho việc xác định phạm vi giá trị trong phân vùng.

