# Hướng dẫn authoring DB Diagram

Template chuẩn (portable, nằm trong `.cursor`): [document-template.md](document-template.md)

## Naming file

| Loại | Pattern | Ví dụ |
| ---- | ------- | ----- |
| Toàn module | `db_diagram_{module}.md` | `db_diagram_warehouse.md` |
| Loại phiếu / subtype | `db_diagram_{TYPE_CODE}.md` | `db_diagram_INBOUND_TICKET.md` |
| Submodule | `db_diagram_{feature}.md` | `db_diagram_stock_transfer.md` |

**Output:** `docs/db_diagrams/` trong project đang làm việc.

## Cấu trúc bắt buộc

Xem đầy đủ skeleton trong [document-template.md](document-template.md). Tóm tắt:

1. `## 1. Mục tiêu tổng quan` — nhóm nghiệp vụ + nguyên tắc thiết kế
2. `# 1A. Ma trận ... ↔ bảng` — tra cứu nhanh **trước** chi tiết schema
3. `# 2..N. Nhóm bảng` — mỗi bảng: cột chính, cách dùng, FK
4. `# N. Luồng dữ liệu chính` — bước đánh số
5. `# N+1. Tổng hợp danh sách bảng`
6. `# N+2. Sơ đồ quan hệ rút gọn` (text tree)
7. `# N+3. Script SQL (chạy một lần)` — **một** khối SQL duy nhất: CREATE bảng mới + ALTER bảng cũ + seed tùy chọn

## Quy tắc Script SQL (mục cuối)

1. **Một script duy nhất** trong tài liệu — không tách file migration riêng trừ khi user yêu cầu.
2. **Idempotent:** `IF OBJECT_ID ... IS NULL` (CREATE), `IF COL_LENGTH ... IS NULL` (ALTER ADD).
3. **Thứ tự:** bảng cha → con; `GO` giữa batch; FK sau khi bảng tham chiếu đã tồn tại.
4. **ALTER bảng cũ:** kiểm tra `OBJECT_ID` tồn tại trước khi `ALTER`; backfill → `ALTER COLUMN NOT NULL` nếu cần.
5. **Khớp schema:** cột/kiểu/constraint trong SQL phải trùng mục §2+ (bảng mô tả).
6. **Seed:** chỉ khi module cần template master mặc định; bọc `IF NOT EXISTS`.

## Quy tắc nội dung

1. **Mỗi bảng** có: tên, cột chính (bảng 2 cột), mục "Cách dùng", FK nếu quan trọng.
2. **Giai đoạn** thống nhất: `Template` | `Phiếu` | `Master` | `Kết quả` | `Dùng chung`.
3. **Ma trận §1A** là phần tra cứu nhanh — ưu tiên làm trước phần chi tiết §2+.
4. **Luồng dữ liệu**: bước đánh số, thứ tự insert/snapshot rõ ràng.
5. **Sơ đồ text**: dùng `└──`, `├──`.
6. **Liên kết**: `[text](db_diagram_xxx.md)` cho tài liệu cùng thư mục output.
7. **Thiếu bằng chứng**: `*(cần xác nhận)*` hoặc `*(chưa triển khai)*`.

## Mapping source → tài liệu

| Nguồn codebase | Ghi vào tài liệu |
| -------------- | ---------------- |
| Views / pages | Màn hình template / phiếu |
| Controller actions (`fn*`, API routes) | Luồng CRUD, thao tác nghiệp vụ |
| Models / services | Bảng, SP, quy tắc nghiệp vụ |
| Client scripts | Grid, cột động, validation UI |
| `TBL_*` trong code/SQL | Schema sections §2+ |

## Checklist trước khi hoàn tất

- [ ] §1 mục tiêu + nguyên tắc thiết kế
- [ ] §1A ma trận tra cứu đầy đủ entity trong phạm vi
- [ ] Mỗi bảng trong phạm vi có mục riêng hoặc được gộp có lý do
- [ ] Luồng dữ liệu chính (tạo template, tạo phiếu, nhập kết quả, …)
- [ ] Bảng tổng hợp + sơ đồ quan hệ rút gọn
- [ ] **§Script SQL:** một khối SQL idempotent — CREATE bảng mới + ALTER bảng cũ, khớp §2+
- [ ] Tên file đúng convention, nằm trong `docs/db_diagrams/`
