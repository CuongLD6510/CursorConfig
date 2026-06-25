# Create User Guide (HDSD)

## Description

Phân tích giao diện web (menu, Views, JavaScript, thông báo) và tạo hoặc cập nhật tài liệu **Hướng dẫn sử dụng (HDSD)** dành cho người dùng cuối — theo cấu trúc chương số La Mã, mục đánh số và placeholder ảnh trong skill (`.cursor/skills/create-user-guide/template.md`). Không mô tả kỹ thuật (API, bảng DB, tên hàm).

## How to use

```
/create-user-guide [tên dự án hoặc app]
/create-user-guide [tên dự án] [màn hình hoặc module gợi ý]
/create-user-guide @Views/...
/create-user-guide update
```

- **tên dự án hoặc app**: Dùng cho tên file output `HDSD_{Tên}.md` (ví dụ `MyWebApp` → `Docs/HDSD/HDSD_MyWebApp.md`).
- **màn hình hoặc module gợi ý** (tuỳ chọn): Chỉ viết/cập nhật các chương liên quan (ví dụ `Cài đặt kho, Báo cáo hóa đơn`).
- **@file**: Tag View, Layout hoặc file JS — suy ra module và màn hình từ code.
- **update**: Cập nhật file HDSD hiện có trong `Docs/HDSD/` theo thay đổi UI; giữ chương không đổi trừ khi user yêu cầu viết lại toàn bộ.

## Steps

1. **Đọc skill và mẫu**: Làm theo `.cursor/skills/create-user-guide/SKILL.md` và `.cursor/skills/create-user-guide/template.md` (format chương, mục con, placeholder ảnh, checklist chất lượng).
2. **Xác định phạm vi**: Từ tham số hoặc file được tag — danh sách màn hình trên menu, tên hiển thị, đường dẫn View/JS tương ứng.
3. **Khám phá codebase**: Đọc `_Layout.cshtml` (menu trái), từng View và JS của màn hình — trích nhãn nút, ô nhập, combobox, thông báo SweetAlert/toast, tên file export, giá trị mặc định, trường bắt buộc.
4. **Phân loại màn hình**: Danh mục/CRUD (master data) hay Báo cáo/tra cứu — áp dụng khung mục tương ứng trong skill.
5. **Tạo hoặc cập nhật** `Docs/HDSD/HDSD_{Tên}.md`:
   - Chương **I. GIỚI THIỆU**, **II. MỤC LỤC**
   - Mỗi màn hình menu = một chương (số La Mã, tiêu đề IN HOA)
   - Chương **LƯU Ý KHI SỬ DỤNG** và **NHẬT KÝ THAY ĐỔI** ở cuối
6. **Ngôn ngữ**: Toàn bộ nội dung HDSD và phản hồi cho user khi chạy command này bằng **tiếng Việt**.
7. **Kết thúc**: In tóm tắt đường dẫn file, số chương đã viết/cập nhật, và nhắc user dán ảnh chụp màn hình vào các placeholder.

## When to use

- Cần HDSD cho website MVC/jQuery/Bootstrap mới hoặc sau khi đổi UI.
- Cần đồng bộ tài liệu vận hành với menu và luồng thao tác thực tế trên màn hình.
- User nhắc **hướng dẫn sử dụng**, **HDSD**, **user guide**, **tài liệu người dùng**.

## Examples

**Ví dụ 1 — Toàn bộ ứng dụng**  
User: `/create-user-guide MyWebApp`  
Agent: Đọc menu và tất cả View, tạo `Docs/HDSD/HDSD_MyWebApp.md` với đủ chương (Giới thiệu, Mục lục, từng màn hình, Lưu ý, Nhật ký).

**Ví dụ 2 — Một vài màn hình**  
User: `/create-user-guide MyWebApp Danh mục kho, Báo cáo doanh thu`  
Agent: Chỉ viết/cập nhật các chương tương ứng và điều chỉnh mục lục + nhật ký.

**Ví dụ 3 — Từ View được tag**  
User: `/create-user-guide @Views/Warehouse/Index.cshtml`  
Agent: Suy ra màn hình từ View, cập nhật chương tương ứng trong file HDSD của project hiện tại.

**Ví dụ 4 — Cập nhật sau đổi UI**  
User: `/create-user-guide update`  
Agent: Tìm `Docs/HDSD/HDSD_*.md`, so sánh với code hiện tại, cập nhật phần thay đổi, thêm dòng vào NHẬT KÝ THAY ĐỔI.

## Important notes

- **Không** ghi tên API, controller, bảng SQL, tên biến code trong HDSD.
- **Có** dùng đúng nhãn UI trên màn hình (in đậm, trong dấu ngoặc kép khi là nút/nhãn).
- Mỗi bước minh hoạ quan trọng: chèn `(vùng trống — dán ảnh chụp màn hình tại đây)` và caption `**Hình {số chương}.{thứ tự} — …**`.
- Mẫu cấu trúc nằm trong bundle: `.cursor/skills/create-user-guide/template.md` — **không** phụ thuộc file HDSD của dự án cụ thể.
- Nếu project đã có `Docs/HDSD/HDSD_*.md`, đọc file đó khi **update** để giữ phong cách và nhật ký; khi **tạo mới** thì theo skill + template.
- Đường dẫn output mặc định: **`Docs/HDSD/`** — chỉ đổi khi user yêu cầu rõ.

## Related

- **Skill**: `.cursor/skills/create-user-guide/SKILL.md`
- **Template**: `.cursor/skills/create-user-guide/template.md`
