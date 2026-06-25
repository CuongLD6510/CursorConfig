---
name: create-user-guide
description: Generates or updates end-user guides (HDSD) in Vietnamese from web UI code (Views, Layout, JavaScript), using Roman-numeral chapters, numbered sections, and screenshot placeholders per template.md. Use when the user invokes /create-user-guide or asks for user guides, HDSD, hướng dẫn sử dụng, or operator documentation for a web app.
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
| Danh sách màn hình | Chỉ viết/cập nhật các chương đó + mục lục + nhật ký |
| Tag `@View` / `@Layout` / JS | Suy ra màn hình tương ứng |
| `update` | Tìm `Docs/HDSD/HDSD_*.md` **trong project hiện tại**, diff với UI, patch có chọn lọc |

Tạo thư mục `Docs/HDSD/` nếu chưa có.

### Mẫu vs file HDSD trong project

- **Tạo mới:** theo [template.md](template.md) + quy tắc trong skill này.
- **Cập nhật (`update`):** đọc `Docs/HDSD/HDSD_*.md` **của project đang làm** để giữ phong cách và nhật ký; chỉ áp dụng thay đổi từ code UI.
- Bundle `.cursor` **không** chứa mẫu gắn với một dự án cụ thể.

## Khám phá codebase

Đọc theo thứ tự ưu tiên:

1. **Menu** — `Views/Shared/_Layout.cshtml` (hoặc partial menu): tên mục, thứ tự, link màn hình.
2. **View** — `.cshtml` của từng màn hình: tiêu đề trang, label form, placeholder combobox (`-- Tất cả --`, …).
3. **JavaScript** — file script của màn hình: nút, validation, SweetAlert/alert text, tên file export, auto-load khi mở trang, giá trị mặc định (ngày, loại, …).
4. **Không** đưa vào HDSD: route API, tên controller, model, tên bảng DB.

Ghi chú ngắn trong thinking nếu thiếu thông tin — dùng placeholder `[cần xác nhận với user]` thay vì đoán nhãn UI.

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

Liệt kê chương III trở đi (và VIII, IX) — mỗi chương một dòng in đậm tên chương.

### Chương III, IV, … — Mỗi màn hình menu một chương

Tiêu đề chương = tên chức năng (thường trùng menu). Số La Mã tăng liên tục.

#### Khung A — Màn hình danh mục / CRUD

Áp dụng khi có bảng danh sách + Thêm/Sửa/Xóa:

| ## | Nội dung |
|----|----------|
| 1 | **Tổng quan màn hình** — cách mở từ menu; bố cục trái/phải |
| 2 | **Tìm kiếm danh sách** — bộ lọc + **"Tìm kiếm"**; auto-load nếu có |
| 3 | **Ý nghĩa các cột trên bảng** — liệt kê từng cột |
| 4 | **Lọc nhanh trên từng cột** — ô dưới header |
| 5 | **Thêm mới** — nút, form, trường bắt buộc, phụ thuộc combobox |
| 6 | **Ví dụ** — một kịch bản điền form cụ thể (bộ phận + giá trị mẫu) |
| 7+ | Thông báo lỗi/empty VTT, lưu thành công, validation — theo text thực trong JS |
| … | **Chỉnh sửa**, **Xóa** (xác nhận INACTIVE nếu có), **Xuất Excel** + pattern tên file |

Bỏ mục không tồn tại trên UI (ví dụ không có Xuất Excel thì không viết).

#### Khung B — Màn hình báo cáo / tra cứu

| ## | Nội dung |
|----|----------|
| 1 | **Tổng quan màn hình** |
| 2 | **Bộ lọc tìm kiếm** — từng điều kiện + giá trị mặc định khi mở trang |
| 3 | **Thực hiện tìm kiếm** — bước + loading **"Đang tìm kiếm..."** nếu có |
| 4 | **Ý nghĩa bảng kết quả** — cột + dòng **TỔNG CỘNG** nếu có |
| 5 | **Lọc nhanh trên từng cột** |
| 6 | **Xuất file báo cáo** — **"Xuất Excel"**, loading, pattern tên file |
| 7+ | **Ví dụ** — 1–2 kịch bản (theo tháng, theo mã, …) |

### Chương cuối (trước nhật ký): LƯU Ý KHI SỬ DỤNG

Gom các tình huống chung:

- Báo cáo không có dữ liệu — checklist gợi ý (mở rộng ngày, bỏ lọc, kiểm tra danh mục, loại kho, …).
- Thông báo validation khi lưu danh mục.
- Mã trùng / lỗi nghiệp vụ thường gặp.
- Không tải được dữ liệu / xuất Excel — thử lại, liên hệ hỗ trợ.

Mỗi mục có placeholder ảnh nếu có hộp thoại điển hình.

### NHẬT KÝ THAY ĐỔI

```markdown
# IX. NHẬT KÝ THAY ĐỔI

- **YYYY-MM-DD:** Mô tả ngắn (khởi tạo / cập nhật chương X / …).
```

Khi **update**: thêm dòng mới, không xóa lịch sử cũ.

## Cập nhật file có sẵn

- Giữ chương không liên quan nguyên vẹn.
- Cập nhật **II. MỤC LỤC** nếu thêm/bớt chương.
- Đánh lại số hình trong chương sửa nếu thêm/bớt mục.
- Không viết lại toàn bộ trừ khi user yêu cầu.

## Checklist trước khi hoàn thành

- [ ] Mọi mục menu chính đều có chương (hoặc nằm trong phạm vi user yêu cầu).
- [ ] Nhãn nút/field khớp code UI (đã đọc View/JS).
- [ ] Không lộ chi tiết kỹ thuật (API, DB, code).
- [ ] Đủ placeholder ảnh + caption `Hình N.M`.
- [ ] Có **LƯU Ý KHI SỬ DỤNG** và **NHẬT KÝ THAY ĐỔI**.
- [ ] Phản hồi user bằng tiếng Việt; nhắc dán screenshot vào placeholder.

## Output

- Đường dẫn: `Docs/HDSD/HDSD_{Tên}.md`
- Tóm tắt một dòng: path, số chương, create vs update.

## Related

- **Command**: `.cursor/commands/create-user-guide.md` — user gọi `/create-user-guide`.
- **Template**: `.cursor/skills/create-user-guide/template.md` — khung HDSD portable (đọc trước khi viết).
