# Login Monitor — Sơ đồ quy trình nghiệp vụ

Tài liệu mẫu (gold standard) cho workflow `/flowchart`. Mỗi quy trình có **Bắt đầu**, điều kiện, và **Kết thúc**; nhãn hướng người dùng, không dùng tên bảng/API.

---

## 1. Ghi nhận đăng nhập

Khi user đăng nhập thành công, hệ thống ghi nhận phiên đăng nhập.

```mermaid
flowchart LR
    A([Bắt đầu]) --> B[User đăng nhập]
    B --> C{Thành công?}
    C -->|Không| D[Hiển thị lỗi đăng nhập]
    D --> Z([Kết thúc])
    C -->|Có| E[Ghi nhận lịch sử đăng nhập]
    E --> Z
```

---

## 2. Xem lịch sử đăng nhập

Quản trị viên tra cứu lịch sử đăng nhập theo bộ lọc.

```mermaid
flowchart LR
    A([Bắt đầu]) --> B[Truy cập trang quản trị]
    B --> C{Có quyền?}
    C -->|Không| D[Từ chối truy cập]
    D --> Z([Kết thúc])
    C -->|Có| E[Chọn bộ lọc tìm kiếm]
    E --> F[Nhấn Tìm kiếm]
    F --> G[Hiển thị danh sách lịch sử]
    G --> Z
```

---

## 3. Cấu hình gửi mail báo cáo

Quản trị viên lưu cấu hình mail và lịch gửi báo cáo.

```mermaid
flowchart LR
    A([Bắt đầu]) --> B[Mở màn hình cấu hình mail]
    B --> C[Nhập thông tin mail và lịch gửi]
    C --> D{Hợp lệ?}
    D -->|Không| E[Hiển thị lỗi nhập liệu]
    E --> C
    D -->|Có| F[Lưu cấu hình]
    F --> Z([Kết thúc])
```

---

## 4. Scheduler gửi báo cáo định kỳ

Hệ thống tự chạy theo lịch đã cấu hình.

```mermaid
flowchart LR
    A([Bắt đầu]) --> B[Scheduler chạy theo lịch]
    B --> C{Có cấu hình mail?}
    C -->|Không| D[Bỏ qua lần chạy]
    D --> Z([Kết thúc])
    C -->|Có| E[Tạo báo cáo]
    E --> F[Gửi email]
    F --> Z
```

---

## Tham chiếu

- Bundle example: `.cursor/skills/flowchart/SKILL.md`
- Rule: `.cursor/rules/flowchart.mdc`
