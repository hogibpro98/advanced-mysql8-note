### Làm thế nào để tổ chức các cột trong chỉ mục để đạt hiệu suất tốt?

# **Chỉ mục Compound: Tóm lược**

---

## **1. Chỉ mục Compound là gì?**
- **Chỉ mục Compound** là loại chỉ mục bao gồm **nhiều cột** được định nghĩa cùng nhau.
- Cho phép MySQL sử dụng nhiều cột trong một truy vấn để tăng hiệu suất tìm kiếm, lọc dữ liệu, hoặc thực hiện các phép **JOIN**.

---

## **2. Khi nào dùng chỉ mục Compound?**
- Khi truy vấn thường xuyên sử dụng **nhiều cột trong mệnh đề WHERE**.
- Khi cần tối ưu hóa truy vấn kết hợp nhiều cột trong **JOIN**, **GROUP BY**, hoặc **ORDER BY**.
- Khi các cột có thứ tự logic trong cách truy vấn:
  - **Cột đầu tiên** (MostSelective): Lọc nhiều nhất.
  - Các cột tiếp theo giảm dần mức độ chọn lọc.

---

## **3. Cần chú ý điều gì khi dùng chỉ mục Compound?**
1. **Thứ tự cột rất quan trọng**:
   - Chỉ mục sẽ hiệu quả nếu truy vấn tuân theo đúng thứ tự các cột trong định nghĩa chỉ mục từ trái sang phải.
   - Nếu bỏ qua một cột ở giữa, MySQL sẽ không sử dụng được chỉ mục.

2. **Sử dụng cột bên trái trong truy vấn**:
   - Chỉ mục hỗ trợ các cột từ trái sang phải. Ví dụ:
     - `index(col_A, col_B, col_C)` có thể hỗ trợ:
       - `WHERE col_A = value`
       - `WHERE col_A = value AND col_B = value`
     - Nhưng không hỗ trợ nếu bỏ qua `col_B`:
       - `WHERE col_A = value AND col_C = value`

3. **Không nên tạo quá nhiều chỉ mục Compound**:
   - Dễ gây lãng phí tài nguyên và làm chậm các thao tác ghi như **INSERT**, **UPDATE**, **DELETE**.

---

## **4. Ví dụ thực tế**

### **Cấu trúc bảng**
```sql
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL
);

CREATE INDEX idx_customer_order ON orders(customer_id, order_date);

-- Truy vấn sử dụng cả hai cột theo thứ tự
-- Lợi ích: MySQL sử dụng idx_customer_order để lọc customer_id và order_date, giúp truy vấn nhanh hơn.
SELECT * 
FROM orders 
WHERE customer_id = 123 
  AND order_date BETWEEN '2023-01-01' AND '2023-12-31';

-- Truy vấn không tối ưu - Bỏ qua cột ở giữa
SELECT * 
FROM orders 
WHERE customer_id = 123 AND total_amount > 1000;
-- Hệ quả: MySQL sẽ không sử dụng chỉ mục idx_customer_order, dẫn đến quét toàn bộ bảng (full scan).

-- Công thức đặt trường từ trái quá phải
SELECT 
    count(distinct user_id)/count(1) as o_user_id, 
    count(distinct event_name)/count(1) as o_event_name, 
    count(distinct time)/count(1) as o_time 
FROM events;

COUNT(DISTINCT user_id):

-- Đếm số lượng giá trị duy nhất (distinct) trong cột user_id.
-- Ví dụ: Nếu cột user_id có giá trị [1, 2, 2, 3], thì COUNT(DISTINCT user_id) sẽ trả về 3.
-- COUNT(1):

-- Đếm tổng số bản ghi trong bảng events.
-- COUNT(1) tương đương với COUNT(*) (cách MySQL tối ưu hóa giúp đếm nhanh tất cả bản ghi).
-- COUNT(DISTINCT user_id)/COUNT(1) (as o_user_id):

-- Tính tỷ lệ giá trị duy nhất trong cột user_id so với tổng số bản ghi.

-- user_id = 1 | event_name = 0 | time = 0.0002 => index(user_id, time, event_name)
```