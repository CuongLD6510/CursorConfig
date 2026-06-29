# HDSD — Mẫu cấu trúc (portable)

File này nằm trong bundle `.cursor`, dùng làm mẫu khi viết HDSD cho **bất kỳ dự án nào**. Không tham chiếu file HDSD cụ thể của một project.

## Nguyên tắc nội dung

- Mỗi chương màn hình: **tổng quan** + **chức năng/thao tác đặc biệt** — ngắn gọn.
- **Không** viết mục riêng cho tìm kiếm, lọc, lọc nhanh cột nếu theo pattern chuẩn của hệ thống.
- **Không** có chương **NHẬT KÝ THAY ĐỔI** ở cuối tài liệu.

## Khung toàn tài liệu

```markdown
# I. GIỚI THIỆU

Website **{Tên hệ thống}** gồm {N} màn hình chức năng chính. Mở từng màn hình bằng cách nhấn tên mục tương ứng trên **menu bên trái**:

1. **{Tên menu 1}** — {mô tả một câu}
2. **{Tên menu 2}** — {mô tả một câu}

Tài liệu dưới đây hướng dẫn thao tác trên từng màn hình — tập trung vào tổng quan và các chức năng đặc biệt của từng màn hình.

---

# II. MỤC LỤC

**Chương III — {Tên màn hình 1}**

**Chương IV — {Tên màn hình 2}**

**Chương {N}. — Lưu ý khi sử dụng**

---

# III. {TÊN MÀN HÌNH — IN HOA}

## 1. Tổng quan màn hình

Nhấn **"{Tên menu}"** trên menu trái để mở trang **{Tiêu đề trang}**.

Màn hình chia hai phần:

- **Bên trái:** {mô tả panel lọc — chỉ nhắc điều kiện đặc biệt nếu có, ví dụ giá trị mặc định, bắt buộc chọn trước khi tải dữ liệu}
- **Bên phải:** {mô tả bảng / kết quả}

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình 3.1 — Toàn cảnh trang {Tên màn hình}**

## 2. Ý nghĩa các cột trên bảng

Chỉ liệt kê cột cần giải thích nghiệp vụ:

1. **{Tên cột 1}** — {giải thích ngắn}
2. **{Tên cột 2}** — {giải thích ngắn}

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình 3.2 — Bảng danh sách**

## 3. Thêm mới / Chỉnh sửa

{Chỉ mô tả trường bắt buộc, phụ thuộc combobox, validation hoặc quy tắc khác biệt so với màn hình danh mục chuẩn}

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình 3.3 — Form thêm mới**

---

# {SỐ LA MÃ CUỐI}. LƯU Ý KHI SỬ DỤNG

## 1. {Tình huống thường gặp}

{Hướng dẫn xử lý — checklist nếu phù hợp}

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình {N}.1 — {Mô tả hộp thoại / màn hình}**
```

## Mẫu rút gọn — màn hình báo cáo

```markdown
# V. {TÊN BÁO CÁO — IN HOA}

## 1. Tổng quan màn hình

Nhấn **"{Tên menu}"** trên menu trái.

**Mặc định khi mở trang:** {ghi giá trị mặc định hoặc điều kiện đặc biệt nếu có trong JS — không liệt kê từng ô lọc thông thường}

(vùng trống — dán ảnh chụp màn hình tại đây)

**Hình 5.1 — Toàn cảnh báo cáo**

## 2. Ý nghĩa bảng kết quả

1. **{Tên cột}** — {giải thích}
2. **TỔNG CỘNG** — {nếu có dòng tổng}

## 3. Xuất file báo cáo

1. Thiết lập điều kiện (nếu cần điều kiện đặc biệt — ghi rõ).
2. Nhấn **"Xuất Excel"**.

Tên file tải về: {PatternTênFile}_ddMMyyyy (ddMMyyyy là ngày xuất).
```

## Quy ước bắt buộc

| Thành phần | Quy tắc |
|------------|---------|
| Chương | `# [LA MÃ]. [IN HOA]` — I, II, III, … |
| Mục trong chương | `## 1.`, `## 2.` — đánh số lại từ 1 mỗi chương; chỉ thêm mục khi cần |
| Nút / nhãn UI | **"Tên hiển thị"** |
| Ảnh minh hoạ | `(vùng trống — dán ảnh chụp màn hình tại đây)` + `**Hình {chương}.{thứ tự} — …**` |
| Phân cách chương | Dòng `---` giữa các chương lớn |
| Output path | `Docs/HDSD/HDSD_{TênApp}.md` (mặc định; đổi chỉ khi user yêu cầu) |
| Tìm kiếm / lọc | Không mục riêng trừ khi đặc biệt — gộp ngắn trong tổng quan |
| Nhật ký thay đổi | **Không** có ở cuối tài liệu |

## Khi cập nhật HDSD có sẵn trong project

Nếu `Docs/HDSD/HDSD_*.md` đã tồn tại **trong dự án đang làm việc**, đọc file đó để giữ phong cách — nhưng **không** coi đó là mẫu cố định trong bundle `.cursor`. Khi cập nhật: rút gọn hoặc xóa mục tìm kiếm/lọc cơ bản và xóa chương **NHẬT KÝ THAY ĐỔI** nếu còn.
