# **Unused Indexes (Chỉ mục không sử dụng)**

---

## **Khái niệm**
- **Unused Indexes** là những chỉ mục không được sử dụng trong các truy vấn.
- Chúng làm tăng chi phí bảo trì mà không mang lại lợi ích về hiệu suất.

---

## **Tìm kiếm chỉ mục không sử dụng**
- Sử dụng view **`schema_unused_indexes`** trong **SYS schema** để xác định chỉ mục không được sử dụng.
- View này dựa trên:
```sql
  select * from sys.schema_unused_indexes;
-- This view is based on the performance_schema.table_io_waits_summary_by_index_usage table, which requires the activation of the performance schema, and of the events_waits_current
-- variable and other dependencies, such as wait, io, table, and sql. Please note that
primary indexes (keys) will be ignored.
```
---
## Kích hoạt Performance Schema
- Nếu chưa bật, chạy các lệnh sau:

```sql
SET GLOBAL performance_schema = ON;
UPDATE performance_schema.setup_instruments 
SET ENABLED = 'YES' WHERE NAME LIKE 'wait / io / table / sql / handler';

UPDATE performance_schema.setup_consumers 
SET ENABLED = 'YES' WHERE NAME LIKE 'events_waits_current';
```
---
## Trước khi xóa chỉ mục
- Hãy cân nhắc các yếu tố sau trước khi xóa chỉ mục:

### Nhiệm vụ hàng tuần:

- Quan sát ít nhất 1 tuần để kiểm tra xem chỉ mục có được sử dụng không.
### Báo cáo hàng tháng:

- Đợi ít nhất 1 tháng để đảm bảo chỉ mục không cần thiết cho các báo cáo định kỳ.
### Ứng dụng dừng hoạt động:

- Tìm hiểu xem có ứng dụng nào trước đây sử dụng chỉ mục này nhưng hiện tại đang dừng không.