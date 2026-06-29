---
name: create-user-guide
description: Generates or updates end-user guides (HDSD) in Vietnamese from web UI code (Views, Layout, JavaScript), using Roman-numeral chapters, numbered sections, and screenshot placeholders per template.md. Focuses on screen overview and special operations; skips standard search/filter steps. Use when the user invokes /create-user-guide or asks for user guides, HDSD, hướng dẫn sử dụng, or operator documentation for a web app.
disable-model-invocation: true
---
# Create User Guide (HDSD)

Tạo hoặc cập nhật `Docs/HDSD/HDSD_{Tên}.md` — tài liệu cho **người dùng cuối**, không cho developer.

**Mẫu cấu trúc (portable):** [template.md](template.md) trong cùng thư mục skill — dùng cho mọi dự án, không phụ thuộc file HDSD của project cụ thể.

### Output path convention

| Artifact | Path | Note |
|----------|------|------|
| HDSD (this skill) | `Docs/HDSD/HDSD_{Tên}.md` | Legacy ASP.NET convention — capital `Docs` |
| DB diagram | `docs/db_diagrams/` | lowercase `docs` |
| Flowchart | `docs/{module}/flowchart.md` | lowercase `docs` |
| API docs | `docs/APIs/` | lowercase `docs` |

Do not change HDSD to `docs/` unless the target project already uses lowercase — match existing project layout on `update`.

## Resolve scope

| Input user | Hành động |
|------------|-----------|
| Tên app/project | File output `Docs/HDSD/HDSD_{Tên}.md` |
| Danh sách màn hình | Chỉ viết/cập nhật các chương đó + mục lục |
| Tag `@View` / `@Layout` / JS | Suy ra màn hình tương ứng |
| `update` | Tìm `Docs/HDSD/HDSD_*.md` **trong project hiện tại**, diff với UI, patch có chọn lọc |

Tạo thư mục `Docs/HDSD/` nếu chưa có.

### Mẫu vs file HDSD trong project

- **Tạo mới:** theo [template.md](template.md) + quy tắc trong skill này.
- **Cập nhật (`update`):** đọc `Docs/HDSD/HDSD_*.md` **của project đang làm** để giữ phong cách; chỉ áp dụng thay đổi từ code UI; gỡ chương **NHẬT KÝ THAY ĐỔI** và các mục tìm kiếm/lọc cơ bản nếu file cũ còn.
- Bundle `.cursor` **không** chứa mẫu gắn với một dự án cụ thể.

## Khám phá codebase

Đọc theo thứ tự ưu tiên:

1. **Menu** — `Views/Shared/_Layout.cshtml` (hoặc partial menu): tên mục, thứ tự, link màn hình.
2. **View** — `.cshtml` của từng màn hình: tiêu đề trang, label form, placeholder combobox (`-- Tất cả --`, …).
3. **JavaScript** — file script của màn hình: nút, validation, SweetAlert/alert text, tên file export, auto-load khi mở trang, giá trị mặc định (ngày, loại, …).
4. **Không** đưa vào HDSD: route API, tên controller, model, tên bảng DB.

Ghi chú ngắn trong thinking nếu thiếu thông tin — dùng placeholder `[cần xác nhận với user]` thay vì đoán nhãn UI.

## Phạm vi nội dung mỗi màn hình

Mỗi chương màn hình **ngắn gọn**, tập trung vào điều người dùng cần biết mà không suy ra được từ màn hình khác.

### Viết

- **Tổng quan** — cách mở từ menu; bố cục chính (trái/phải, tab, panel).
- **Ý nghĩa cột / trường** — khi tên cột hoặc nhãn không tự giải thích được nghiệp vụ.
- **Chức năng và thao tác đặc biệt** — thêm/sửa/xóa có quy tắc riêng; phụ thuộc combobox; validation hoặc thông báo lỗi đặc thù; xuất file (pattern tên file); auto-load; giá trị mặc định bất thường; luồng nhiều bước; nút/hành động chỉ có trên màn hình này.
- **Bộ lọc / tìm kiếm** — **chỉ khi** có điểm khác biệt so với pattern chuẩn của hệ thống (ví dụ: bắt buộc chọn kho trước khi tìm; mặc định tháng hiện tại; điều kiện phụ thuộc lẫn nhau). Gộp ngắn trong tổng quan hoặc một mục riêng nếu cần.

### Bỏ qua (không viết mục riêng)

- Thao tác **tìm kiếm**, **lọc**, **lọc nhanh trên cột** theo pattern chuẩn (chọn điều kiện → nhấn **"Tìm kiếm"**; ô lọc dưới header cột) — trừ khi có quy tắc đặc biệt như trên.
- Các bước CRUD/xuất Excel **lặp lại y hệt** màn hình danh mục khác — chỉ nhắc khác biệt (trường bắt buộc, validation, tên file export, …).
- Mục **NHẬT KÝ THAY ĐỔI** ở cuối tài liệu — **không** tạo mới; khi update file cũ thì xóa chương đó.

## Quy tắc định dạng

### Chương (cấp 1)

`# [SỐ LA MÃ]. [TIÊU ĐỀ IN HOA]`

Ví dụ: `# III. CÀI ĐẶT KHO`, `# VIII. LƯU Ý KHI SỬ DỤNG`

Giữa các chương lớn: dòng `---` (horizontal rule).

### Mục con (trong chương)

`## 1. Tổng quan màn hình`, `## 2. Tìm kiếm…` — đánh số tuần tự **trong từng chương**, bắt đầu từ 1.

Không dùng `###` trừ khi thật sự cần phân cấp sâu (hiếm trong HDSD mẫu).

### Văn phong

- Tiếng Việt, ngắn gọn, thao tác từng bước.
- Nút / nhãn / tiêu đề trên UI: **in đậm**; nhãn hiển thị trên màn hình thêm dấu ngoặc kép: **"Tìm kiếm"**.
- Giá trị ví dụ người dùng nhập: không in đậm (ví dụ: KT-WH01).
- Tuỳ chọn: `(Tuỳ chọn)` ở đầu bước.
- Trường bắt buộc: ghi **bắt buộc** hoặc dấu * nếu UI có.

### Placeholder ảnh

Sau mỗi mục cần minh hoạ trực quan:

```markdown
(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình {N}.{M} — Mô tả ngắn bằng tiếng Việt**
```

- `{N}` = số thứ tự chương (III → 3, IV → 4, …).
- `{M}` = thứ tự hình trong chương, tăng dần.
- Không chèn link ảnh giả; để trống cho user dán sau.

## Cấu trúc bắt buộc toàn tài liệu

### I. GIỚI THIỆU

- Tên hệ thống (từ Layout/title).
- Liệt kê **đánh số** các màn hình chính trên menu trái — mỗi dòng: **Tên menu** — mô tả một câu.
- Câu kết: tài liệu hướng dẫn thao tác chi tiết từng màn hình.

### II. MỤC LỤC

Liệt kê chương III trở đi (và chương **LƯU Ý KHI SỬ DỤNG** cuối) — mỗi chương một dòng in đậm tên chương.

### Chương III, IV, … — Mỗi màn hình menu một chương

Tiêu đề chương = tên chức năng (thường trùng menu). Số La Mã tăng liên tục.

#### Khung A — Màn hình danh mục / CRUD

Áp dụng khi có bảng danh sách + Thêm/Sửa/Xóa. **Chỉ thêm mục khi nội dung thật sự cần thiết** — không lấp đầy theo bảng dưới nếu màn hình không có điểm đặc biệt.

| ## | Nội dung | Ghi chú |
|----|----------|---------|
| 1 | **Tổng quan màn hình** — cách mở từ menu; bố cục; nhắc ngắn auto-load nếu có | Bắt buộc |
| 2 | **Ý nghĩa các cột trên bảng** — liệt kê cột cần giải thích nghiệp vụ | Bỏ nếu tên cột đã rõ |
| 3 | **Thêm mới** — trường bắt buộc, phụ thuộc combobox, quy tắc riêng | Chỉ khác biệt so với CRUD chuẩn |
| 4 | **Chỉnh sửa / Xóa** — xác nhận, INACTIVE, ràng buộc đặc thù | Chỉ khi khác màn hình khác |
| 5 | **Xuất Excel** — pattern tên file, điều kiện trước khi xuất | Chỉ khi có nút xuất |
| 6+ | Thông báo lỗi/validation đặc thù; **Ví dụ** kịch bản điền form khi phức tạp | Theo text thực trong JS |
| (tuỳ chọn) | **Điều kiện lọc đặc biệt** — giá trị mặc định, phụ thuộc, bắt buộc | **Không** viết mục tìm kiếm/lọc chuẩn |

**Không** viết mục riêng: Tìm kiếm danh sách, Lọc nhanh trên từng cột — trừ khi có quy tắc đặc biệt (gộp vào tổng quan hoặc mục điều kiện lọc đặc biệt).

#### Khung B — Màn hình báo cáo / tra cứu

| ## | Nội dung | Ghi chú |
|----|----------|---------|
| 1 | **Tổng quan màn hình** — cách mở; bố cục; **mặc định khi mở trang** nếu có trong JS | Bắt buộc |
| 2 | **Ý nghĩa bảng kết quả** — cột cần giải thích; dòng **TỔNG CỘNG** nếu có | Bỏ cột đã rõ nghĩa |
| 3 | **Xuất file báo cáo** — **"Xuất Excel"**, pattern tên file, điều kiện đặc biệt | Chỉ khi có xuất |
| 4+ | **Điều kiện lọc đặc biệt**; **Ví dụ** 1–2 kịch bản khi logic phức tạp | Không viết bước tìm kiếm chuẩn |

**Không** viết mục riêng: Bộ lọc tìm kiếm (từng điều kiện thông thường), Thực hiện tìm kiếm, Lọc nhanh trên từng cột — trừ khi có quy tắc đặc biệt (gộp vào tổng quan hoặc mục điều kiện lọc đặc biệt).

### Chương cuối: LƯU Ý KHI SỬ DỤNG

Gom các tình huống chung:

- Báo cáo không có dữ liệu — checklist gợi ý (mở rộng ngày, bỏ lọc, kiểm tra danh mục, loại kho, …).
- Thông báo validation khi lưu danh mục.
- Mã trùng / lỗi nghiệp vụ thường gặp.
- Không tải được dữ liệu / xuất Excel — thử lại, liên hệ hỗ trợ.

Mỗi mục có placeholder ảnh nếu có hộp thoại điển hình.

## Cập nhật file có sẵn

- Giữ chương không liên quan nguyên vẹn (có thể rút gọn mục tìm kiếm/lọc cơ bản và xóa chương Nhật ký nếu user chưa yêu cầu giữ).
- Cập nhật **II. MỤC LỤC** nếu thêm/bớt chương.
- Đánh lại số hình trong chương sửa nếu thêm/bớt mục.
- Không viết lại toàn bộ trừ khi user yêu cầu.

## Checklist trước khi hoàn thành

- [ ] Mọi mục menu chính đều có chương (hoặc nằm trong phạm vi user yêu cầu).
- [ ] Nhãn nút/field khớp code UI (đã đọc View/JS).
- [ ] Không lộ chi tiết kỹ thuật (API, DB, code).
- [ ] Đủ placeholder ảnh + caption `Hình N.M`.
- [ ] Mỗi chương màn hình tập trung tổng quan + thao tác đặc biệt; không lặp tìm kiếm/lọc chuẩn.
- [ ] Có **LƯU Ý KHI SỬ DỤNG**; **không** có chương **NHẬT KÝ THAY ĐỔI**.
- [ ] Phản hồi user bằng tiếng Việt; nhắc dán screenshot vào placeholder.

## Output

- Đường dẫn: `Docs/HDSD/HDSD_{Tên}.md`
- Tóm tắt một dòng: path, số chương, create vs update.

## Related

- **Command**: `.cursor/commands/create-user-guide.md` — user gọi `/create-user-guide`.
- **Template**: `.cursor/skills/create-user-guide/template.md` — khung HDSD portable (đọc trước khi viết).
