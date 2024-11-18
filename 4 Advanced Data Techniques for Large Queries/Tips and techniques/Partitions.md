# Phân vùng (Partitions)

## **Một số lưu ý**
1. **Không nên sử dụng phân vùng (PARTITIONS)** nếu bạn chưa hiểu rõ nó sẽ mang lại lợi ích gì cho mình.
2. Chỉ sử dụng phân vùng khi bảng của bạn có hơn **một triệu bản ghi**.
3. **Không sử dụng nhiều hơn 50 phân vùng** cho một bảng; nếu không, hiệu suất sẽ bị ảnh hưởng.
4. **Phân vùng RANGE** là phương pháp hữu ích và phù hợp nhất trong hầu hết các trường hợp; nó cũng dễ quản lý.
5. **SUB-PARTITIONS** (phân vùng phụ) không thực sự cần thiết, trừ những trường hợp đặc biệt.
6. **Khóa phân vùng (Partition Key)** không nên là cột đầu tiên trong một chỉ mục; nếu không, việc bảo trì sẽ ảnh hưởng đến hiệu suất.

## **Kỹ thuật**
1. **Phân vùng không phải là giải pháp cho mọi vấn đề hiệu suất.** 
   - Phân vùng chia một bảng thành nhiều bảng nhỏ hơn, nhưng kích thước của bảng hiếm khi là nguyên nhân gây ra vấn đề hiệu suất.
   - Thông thường, thời gian đọc/ghi (I/O) và chỉ mục mới là các yếu tố ảnh hưởng lớn nhất.
2. Trong **MySQL 8.0**, từ điển dữ liệu (data dictionary) đã loại bỏ giới hạn sử dụng 50 phân vùng.

## **Trường hợp sử dụng phân vùng**
- **Trường hợp 1:** Lệnh `DROP PARTITION` nhanh hơn nhiều so với `DELETE` khi xử lý một lượng lớn bản ghi.
- **Trường hợp 2:** Nếu chỉ mục của một bảng quá lớn để lưu trữ trong bộ nhớ đệm (cache), thì việc sử dụng chỉ mục của từng phân vùng sẽ hiệu quả hơn. 
  - Phân vùng có thể giữ toàn bộ chỉ mục trong RAM, từ đó tránh được nhiều hoạt động đọc/ghi (disk I/O).
