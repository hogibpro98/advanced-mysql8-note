### But how do we evaluate the costs for each operation? Here's how.

![alt text](<the list of default values.png>)


Từ MySQL 5.7, chi phí cho mỗi thao tác được đánh giá để chọn kế hoạch hiệu quả nhất. Trong MySQL 8.0, Oracle bổ sung tính năng tối ưu hóa dựa trên tỷ lệ chỉ mục trong InnoDB Buffer Pool, cải thiện hiệu suất đáng kể.

# Tính Chi Phí (Cost) cho Truy Vấn `SELECT * FROM events`

## Mô tả truy vấn
- Truy vấn: `SELECT * FROM events;`
- Bảng: `events`
- Số lượng dòng: 10 triệu
- MySQL sẽ thực hiện **quét toàn bộ bảng** (full table scan) do không có điều kiện lọc được chỉ định.

---

## Các yếu tố ảnh hưởng đến chi phí (Cost)
### 1. **I/O Đọc Khối Dữ Liệu (I/O Block Read Cost)** 
[show more](#how_to_save_ram)
- Khi quét toàn bộ bảng, MySQL sẽ đọc tất cả các khối dữ liệu của bảng từ đĩa hoặc bộ nhớ.
- **Chi phí mỗi lần đọc khối**:
  - Từ đĩa: `io_block_read_cost`
  - Từ bộ nhớ: `memory_block_read_cost`

### 2. **Chi phí Đánh Giá Hàng (Row Evaluation Cost)**
- Mỗi hàng sẽ được đánh giá khi MySQL quét qua từng hàng trong bảng.
- **Chi phí mỗi hàng**: `row_evaluate_cost`.

---

## Ước tính chi phí dựa trên bảng chi phí

### **Giả định**
1. Bảng `events` lưu trên **đĩa** (không đủ bộ nhớ để lưu toàn bộ bảng).
2. Mỗi **khối dữ liệu** chứa khoảng **100 hàng** (con số này có thể khác tùy vào cấu trúc bảng và dung lượng bộ nhớ).
3. Tham khảo chi phí từ bảng:
   - `io_block_read_cost`: **1** (chi phí đọc một khối từ đĩa).
   - `row_evaluate_cost`: **0.2** (chi phí đánh giá mỗi hàng).

---

## **Tính toán chi phí**

### **1. Chi phí đọc khối (I/O Read Cost)**
- Tổng số khối cần đọc:
  \[
  \text{Số khối} = \frac{\text{Tổng số hàng}}{\text{Số hàng mỗi khối}} = \frac{10,000,000}{100} = 100,000
  \]
- Tổng chi phí đọc từ đĩa:
  \[
  100,000 \times 1 = 100,000
  \]

### **2. Chi phí đánh giá hàng (Row Evaluation Cost)**
- Tổng chi phí đánh giá hàng:
  \[
  \text{Tổng số hàng} \times \text{row\_evaluate\_cost} = 10,000,000 \times 0.2 = 2,000,000
  \]

---

## **Tổng chi phí ước tính**
\[
\text{Tổng chi phí} = \text{Chi phí đọc khối} + \text{Chi phí đánh giá hàng}
\]
\[
\text{Tổng chi phí} = 100,000 + 2,000,000 = 2,100,000
\]

---

## Nhận xét
- Với tổng chi phí ước tính **2,100,000**, đây là một truy vấn có chi phí rất cao vì MySQL phải quét toàn bộ bảng `events` với 10 triệu dòng.
- **Giải pháp**: Để tối ưu hóa:
  - Sử dụng các **điều kiện lọc** (WHERE clause) để giảm lượng dữ liệu cần quét.
  - Tạo **chỉ mục phù hợp** để tránh quét toàn bộ bảng.

# 1. Bảng được lưu trên đĩa (Disk Storage) là gì? <a name = "how_to_save_ram"></a>

Trong MySQL, dữ liệu thường được lưu trữ trên **đĩa** trong các file vật lý, không phải tất cả dữ liệu của bảng đều được lưu trữ trong **bộ nhớ RAM**. 

### Tại sao bảng lớn không thể lưu trọn trong RAM? 
- Khi bạn có một bảng lớn như bảng `events` với 10 triệu dòng, toàn bộ bảng này thường không thể được lưu trong bộ nhớ RAM do giới hạn về dung lượng bộ nhớ.
- Khi MySQL thực hiện truy vấn trên bảng `events`, nó sẽ:
  - Đọc dữ liệu từ các file trên **đĩa** (lưu trữ vật lý).
  - Chỉ tải những phần cần thiết vào **bộ nhớ (RAM)** để xử lý.
  - Sử dụng lại dữ liệu trong bộ nhớ nếu chúng chưa bị ghi đè (gọi là **bộ đệm - buffer**).

### Nhược điểm:
- Với bảng lớn, MySQL không thể tải toàn bộ bảng vào bộ nhớ cùng một lúc.
- Vì vậy, MySQL sẽ phải **liên tục truy cập vào đĩa để đọc dữ liệu**, gây tốn kém về thời gian và chi phí I/O (Input/Output).

### Ví dụ:
- Khi bạn thực hiện truy vấn `SELECT * FROM events;`, MySQL cần đọc toàn bộ dữ liệu của bảng `events`.
- Nếu bảng `events` quá lớn, MySQL phải đọc từ **đĩa nhiều lần** vì bộ nhớ không đủ để lưu toàn bộ bảng cùng một lúc.

---

# 2. Khối dữ liệu (Data Block) là gì?

### Định nghĩa:
Khối dữ liệu (data block) là **đơn vị lưu trữ cơ bản** mà MySQL sử dụng để quản lý dữ liệu trong bảng. 

### Cách thức lưu trữ:
- Khi MySQL lưu dữ liệu vào đĩa, nó không lưu từng dòng một mà lưu thành từng **khối (block)** có kích thước cố định.
- Mỗi khối chứa **nhiều dòng dữ liệu**, giúp tối ưu hóa việc đọc/ghi dữ liệu.

### Tính toán số lượng dòng trong một khối:
- Giả sử kích thước khối dữ liệu là **16KB** (16,384 bytes).
- Một dòng dữ liệu trong bảng chiếm **160 bytes**.
- Số lượng dòng trong mỗi khối:
  \[
  \text{Số dòng mỗi khối} = \frac{16,384}{160} \approx 102 \text{ dòng}.
  \]

### Cách đọc dữ liệu:
- Khi MySQL cần đọc dữ liệu từ đĩa, nó sẽ đọc **từng khối một**, thay vì đọc từng dòng riêng lẻ.
- Điều này giúp tối ưu hóa hiệu suất I/O vì mỗi lần đọc, MySQL có thể lấy **nhiều dòng dữ liệu cùng một lúc**.

### Ví dụ:
- Bảng `events` có **10 triệu dòng**.
- Nếu mỗi khối chứa khoảng **100 dòng**, thì:
  \[
  \text{Số khối dữ liệu} = \frac{10,000,000}{100} = 100,000 \text{ khối}.
  \]
- Khi bạn thực hiện truy vấn quét toàn bộ bảng (như `SELECT * FROM events;`), MySQL sẽ đọc **100,000 khối** dữ liệu từ đĩa để lấy toàn bộ nội dung của bảng `events`.

---

# Tóm tắt

### **Bảng lưu trên đĩa**:
- Dữ liệu bảng được lưu trong **file trên đĩa cứng**.
- Khi cần, dữ liệu chỉ được tải một phần vào **RAM**.
- Toàn bộ bảng lớn không thể được lưu trọn trong RAM.

### **Khối dữ liệu**:
- Đơn vị lưu trữ cơ bản trên đĩa.
- Mỗi khối chứa nhiều dòng dữ liệu.
- MySQL đọc dữ liệu từ đĩa theo từng **khối** để tối ưu hóa tốc độ truy cập.

### **Nhược điểm**:
- Việc đọc nhiều khối từ đĩa (thay vì từ bộ nhớ) gây tốn kém chi phí I/O.
- Đó là lý do tại sao việc **tối ưu hóa truy vấn** để tránh quét toàn bộ bảng là rất quan trọng.

# Tóm lại về số cost trong MySQL

Số **cost** trong MySQL không nhằm xác định một ngưỡng "an toàn", mà được sử dụng để:

1. **Giúp MySQL chọn kế hoạch thực thi tốt nhất**:
   - MySQL so sánh các kế hoạch thực thi dựa trên cost và chọn kế hoạch có cost thấp nhất.

2. **Hỗ trợ người dùng phân tích và tối ưu hóa truy vấn**:
   - Cost cung cấp thông tin để xác định vấn đề và tối ưu hóa truy vấn.

3. **So sánh hiệu quả của các kế hoạch truy vấn khác nhau**:
   - Người dùng có thể đánh giá sự cải thiện của truy vấn bằng cách so sánh cost trước và sau tối ưu hóa.

4. **Dự đoán hiệu suất để tránh quá tải**:
   - Cost cao có thể cảnh báo về truy vấn nặng, giúp người dùng hoặc hệ thống có biện pháp tối ưu hóa.

---

### **Lưu ý**
- Không có một ngưỡng cost "cố định" được coi là an toàn.
- Việc giảm **cost** luôn đồng nghĩa với hiệu suất tốt hơn, đặc biệt khi hệ thống phải xử lý nhiều truy vấn đồng thời.
