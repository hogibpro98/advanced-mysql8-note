# **Nghiên cứu tình huống kỹ thuật: Tối ưu hóa MySQL**

---

## **Tình huống**
Một công ty chuyên về thẻ thông minh gặp phải các vấn đề nghiêm trọng về hiệu suất MySQL trong quá trình mở rộng kinh doanh.

---

## **Các vấn đề gặp phải**
1. **Sử dụng CPU lên tới 5,000%**.
2. **40-50% dung lượng ổ đĩa** được sử dụng cho các bảng tạm thời (**temporary tables**) của MySQL.
3. **Truy vấn bị xếp chồng** lên nhau.
4. **Xuất hiện tình trạng deadlocks**.

Những vấn đề này thường gặp ở các công ty phát triển nhanh, với đội ngũ kỹ thuật quá tải và không biết cách giải quyết hiệu quả.

---

## **Các bước thực hiện và kết quả**

### **1. Thay đổi cấu hình cơ bản**
- **Hành động**: Thay đổi một số tham số quan trọng trong cấu hình MySQL.
- **Kết quả**:
  - Hiệu suất cải thiện **30%**.
  - **CPU giảm xuống còn 3,500%**.
  - **40-50%** ổ đĩa vẫn được sử dụng cho bảng tạm.
  - Truy vấn vẫn bị xếp chồng và tình trạng deadlocks vẫn xảy ra.

---

### **2. Nâng cấp RAM**
- **Hành động**: Tăng RAM từ **16 GB lên 32 GB**.
- **Kết quả**:
  - Hiệu suất cải thiện thêm **15%**.
  - **CPU giảm xuống còn 2,700%**.
  - **40-50%** ổ đĩa vẫn được sử dụng cho bảng tạm.
  - Truy vấn vẫn bị xếp chồng và tình trạng deadlocks vẫn xảy ra.

---

### **3. Phân tích và tối ưu hóa truy vấn chậm**
- **Hành động**:
  1. Xác định **3 truy vấn chậm nhất**.
  2. Chạy **EXPLAIN** để phân tích kế hoạch thực thi.
  3. Phát hiện **thiếu chỉ mục**, bao gồm:
     - **Hai chỉ mục đơn cột (single-column indexes)** trên các cột duy nhất.
     - **Một chỉ mục COMPOUND** trên bảng chính.
- **Kết quả**:
  - Sử dụng CPU giảm xuống **30%**.
  - Sử dụng ổ đĩa I/O giảm xuống **45%**.
  - **Không còn truy vấn bị xếp chồng**.
  - **Thời gian phản hồi nhanh**.
  - **Không còn deadlocks**.

---

## **Bài học rút ra**

### **1. Tối ưu hóa truy vấn chậm trước tiên**
- Phân tích truy vấn với **EXPLAIN**.
- Thêm các chỉ mục phù hợp, đặc biệt là **COMPOUND index**.

### **2. Hạn chế tăng tài nguyên phần cứng**
- Nâng cấp phần cứng chỉ nên thực hiện sau khi tối ưu hóa truy vấn. Việc tăng tài nguyên mà không tối ưu truy vấn chỉ mang lại cải thiện tạm thời.

### **3. Tách biệt MySQL và ứng dụng**
- Tránh chạy MySQL và máy chủ ứng dụng (như Apache) trên cùng một máy chủ vì cả hai đều tiêu tốn nhiều RAM và CPU.

---

## **Kết luận**
- Tối ưu hóa truy vấn, đặc biệt là thêm chỉ mục phù hợp, là bước đầu tiên và quan trọng nhất để cải thiện hiệu suất MySQL.
- Chỉ khi các truy vấn đã được tối ưu hóa mà vẫn gặp vấn đề, mới cần xem xét nâng cấp phần cứng hoặc thay đổi cấu hình máy chủ.
