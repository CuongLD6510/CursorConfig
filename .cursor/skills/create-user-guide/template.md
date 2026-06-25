# HDSD — Mẫu cấu trúc (portable)

File này nằm trong bundle `.cursor`, dùng làm mẫu khi viết HDSD cho **bất kỳ dự án nào**. Không tham chiếu file HDSD cụ thể của một project.

## Khung toàn tài liệu

```markdown
# I. GIỚI THIỆU

Website **{Tên hệ thống}** gồm {N} màn hình chức năng chính. Mở từng màn hình bằng cách nhấn tên mục tương ứng trên **menu bên trái**:

1. **{Tên menu 1}** — {mô tả một câu}
2. **{Tên menu 2}** — {mô tả một câu}

Tài liệu dưới đây hướng dẫn thao tác chi tiết trên từng màn hình.

---

# II. MỤC LỤC

**Chương III — {Tên màn hình 1}**

**Chương IV — {Tên màn hình 2}**

**Chương {N-1}. — Lưu ý khi sử dụng**

**Chương {N}. — Nhật ký thay đổi**

---

# III. {TÊN MÀN HÌNH — IN HOA}

## 1. Tổng quan màn hình

Nhấn **"{Tên menu}"** trên menu trái để mở trang **{Tiêu đề trang}**.

Màn hình chia hai phần:

- **Bên trái:** {mô tả panel lọc}
- **Bên phải:** {mô tả bảng / kết quả}

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình 3.1 — Toàn cảnh trang {Tên màn hình}**

## 2. Tìm kiếm danh sách

**Các bước:**

1. (Tuỳ chọn) Chọn **{Trường lọc}** — hoặc để **"-- Tất cả --"**.
2. Nhấn **"Tìm kiếm"**.

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình 3.2 — Panel lọc**

## 3. Ý nghĩa các cột trên bảng

1. **{Tên cột 1}** — {giải thích ngắn}
2. **{Tên cột 2}** — {giải thích ngắn}

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình 3.3 — Bảng danh sách**

---

# {SỐ LA MÃ CUỐI - 1}. LƯU Ý KHI SỬ DỤNG

## 1. {Tình huống thường gặp}

{Hướng dẫn xử lý — checklist nếu phù hợp}

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình {N}.1 — {Mô tả hộp thoại / màn hình}**

---

# {SỐ LA MÃ CUỐI}. NHẬT KÝ THAY ĐỔI

- **YYYY-MM-DD:** Phiên bản khởi tạo hướng dẫn sử dụng.
```

## Mẫu rút gọn — màn hình báo cáo

```markdown
# V. {TÊN BÁO CÁO — IN HOA}

## 1. Tổng quan màn hình

Nhấn **"{Tên menu}"** trên menu trái.

## 2. Bộ lọc tìm kiếm

- **{Điều kiện 1}** — {giải thích}
- **Từ ngày** / **Đến ngày** — khoảng thời gian tra cứu

**Mặc định khi mở trang:** {ghi giá trị mặc định nếu có trong JS}

## 3. Thực hiện tìm kiếm

1. Điều chỉnh các điều kiện lọc.
2. Nhấn **"Tìm kiếm"**.
3. Xem bảng kết quả bên phải.

## 6. Xuất file báo cáo

1. Thiết lập bộ lọc và nhấn **"Tìm kiếm"**.
2. Nhấn **"Xuất Excel"**.

Tên file tải về: {PatternTênFile}_ddMMyyyy (ddMMyyyy là ngày xuất).
```

## Quy ước bắt buộc

| Thành phần | Quy tắc |
|------------|---------|
| Chương | `# [LA MÃ]. [IN HOA]` — I, II, III, … |
| Mục trong chương | `## 1.`, `## 2.` — đánh số lại từ 1 mỗi chương |
| Nút / nhãn UI | **"Tên hiển thị"** |
| Ảnh minh hoạ | `(vùng trống — dán ảnh chụp màn hình tại đây)` + `**Hình {chương}.{thứ tự} — …**` |
| Phân cách chương | Dòng `---` giữa các chương lớn |
| Output path | `Docs/HDSD/HDSD_{TênApp}.md` (mặc định; đổi chỉ khi user yêu cầu) |

## Khi cập nhật HDSD có sẵn trong project

Nếu `Docs/HDSD/HDSD_*.md` đã tồn tại **trong dự án đang làm việc**, đọc file đó để giữ phong cách và lịch sử — nhưng **không** coi đó là mẫu cố định trong bundle `.cursor`.
