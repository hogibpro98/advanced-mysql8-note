# **Phân vùng HASH trong MySQL**

---

## **Định nghĩa**
- **Phân vùng HASH**: Chọn phân vùng dựa trên giá trị được trả về từ một biểu thức do người dùng định nghĩa trên các cột của bản ghi được thêm vào bảng.
- **Biểu thức**: Là một biểu thức trả về giá trị **nguyên** (integer).

---

## **Cách sử dụng**
- Sử dụng mệnh đề **`PARTITION BY HASH`** trong câu lệnh **`CREATE TABLE`**.
- Cần xác định:
  - **Cột** hoặc **biểu thức** dùng để tính giá trị băm.
  - Số lượng phân vùng (**PARTITIONS**).

---

## **Cú pháp cơ bản**
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
) 
PARTITION BY HASH (expression) 
PARTITIONS num_partitions;
```
### Ví dụ
- Tình huống
- Bảng: Lưu thông tin nhân viên nhà hàng.
- Cột: resto_id được sử dụng để chia dữ liệu vào các phân vùng dựa trên giá trị băm.
- Tạo bảng với phân vùng HASH
# **Phân vùng HASH trong MySQL**

---

## **Định nghĩa**
- **Phân vùng HASH**: Chọn phân vùng dựa trên giá trị được trả về từ một biểu thức do người dùng định nghĩa trên các cột của bản ghi được thêm vào bảng.
- **Biểu thức**: Là một biểu thức trả về giá trị **nguyên** (integer).

---

## **Cách sử dụng**
- Sử dụng mệnh đề **`PARTITION BY HASH`** trong câu lệnh **`CREATE TABLE`**.
- Cần xác định:
  - **Cột** hoặc **biểu thức** dùng để tính giá trị băm.
  - Số lượng phân vùng (**PARTITIONS**).
```
---

## **Cú pháp cơ bản**
```sql
CREATE TABLE employees (
    id INT NOT NULL,
    name VARCHAR(50),
    resto_id INT NOT NULL,
    PRIMARY KEY (id, resto_id)
)
PARTITION BY HASH (resto_id) 
PARTITIONS 4 (bội số của 4 vì CPU/vCPU: 4 lõi là thấp nhất);
ví dụ:
- Với RAM 16GB và 4 lõi CPU, số phân vùng tối ưu thường là:
- 4, 8, 12, 16 phân vùng (bội số của CPU).

```
## Ý nghĩa:
- Dữ liệu sẽ được chia đều vào 4 phân vùng dựa trên giá trị băm của cột resto_id.
### Lưu ý
- Số lượng phân vùng:

- Nếu không chỉ định số lượng phân vùng (PARTITIONS), MySQL mặc định sẽ tạo 1 phân vùng.
- Nếu chỉ dùng PARTITIONS mà không kèm số lượng, sẽ gây lỗi cú pháp.
### Tính đơn giản:

- Lệnh HASH dễ sử dụng và hiệu quả cho việc phân chia dữ liệu đồng đều dựa trên giá trị băm.
