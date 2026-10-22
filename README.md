# Klass-C-Frame
Web tạo khung cho các bài truyền thông của Klass-C Frame

## Tải về bộ ảnh cuối (đồng bộ giữa thiết bị)

Mỗi lần bấm "Tải media này" / "Tải tất cả", bộ thành phẩm được lưu lên Supabase Storage (bucket `klassc-exports`, chỉ giữ bộ mới nhất). Nút "Tải về bộ ảnh cuối" lấy lại bộ đó trên bất kỳ thiết bị nào đã nhập cùng Supabase URL + anon key.

Chạy một lần trong Supabase → SQL Editor:

```sql
insert into storage.buckets (id, name, public)
values ('klassc-exports', 'klassc-exports', false)
on conflict (id) do nothing;

create policy "klassc exports anon"
on storage.objects for all to anon
using (bucket_id = 'klassc-exports')
with check (bucket_id = 'klassc-exports');
```

Lưu ý: ai có anon key đều đọc/xóa được bucket này.
