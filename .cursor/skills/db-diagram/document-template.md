# Đặc tả database nghiệp vụ {TÊN_MODULE}

> **Template chuẩn** cho workflow `/db_diagram`. File này nằm trong `.cursor/skills/db-diagram/` — copy cấu trúc và thay `{placeholder}` bằng dữ liệu thực từ phân tích nghiệp vụ + source code. Output ghi vào `docs/db_diagrams/db_diagram_{slug}.md` của project đang làm việc.

---

## 1. Mục tiêu tổng quan

Tài liệu này mô tả các bảng SQL và luồng nghiệp vụ cho:

- {Nhóm nghiệp vụ 1 — ví dụ: master / template}
- {Nhóm nghiệp vụ 2 — ví dụ: phiếu / transaction thực tế}
- {Nhóm nghiệp vụ 3 — ví dụ: kết quả nhập / footer / phê duyệt}

Nguyên tắc thiết kế chính:

- {Nguyên tắc 1 — ví dụ: bảng template cấu hình biểu mẫu gốc}
- {Nguyên tắc 2 — ví dụ: snapshot sang bảng phiếu khi tạo bản ghi thật}
- {Nguyên tắc 3 — ví dụ: không phụ thuộc master khi render dữ liệu lịch sử}
- {Nguyên tắc 4 — ví dụ: logic lưu kết quả = dimension A × dimension B}

---

# 1A. Ma trận {ENTITY} ↔ bảng và ý nghĩa từng bảng

> **Mục đích:** Tra cứu nhanh *{entity} nào dùng bảng nào*, *bảng đó làm gì*, và *giai đoạn nào* (template / phiếu thật / master / kết quả nhập).

## 1A.1. Bản đồ toàn module

| Mã / Loại | Tên hiển thị | Màn hình | Giai đoạn | Tài liệu chi tiết |
| --------- | ------------ | -------- | --------- | ----------------- |
| `{TYPE_A}` | {Tên hiển thị A} | `{ViewA}.cshtml` | Template → Phiếu | **Tài liệu này** (mục 1A.2) |
| `{TYPE_B}` | {Tên hiển thị B} | `{ViewB}.cshtml` | Template → Phiếu | [`db_diagram_{child}.md`](db_diagram_{child}.md) |

**Quy ước đọc bảng dưới đây:**

- **Template** = cấu hình gốc (chưa gắn ngày/ca/phiên cụ thể).
- **Phiếu (ticket)** = bản ghi thực tế; snapshot từ template + sinh cột động nếu có.
- **Kết quả** = dữ liệu người dùng nhập trên grid/form.
- **Dùng chung** = mọi loại trong module đều có thể dùng.

## 1A.2. {ENTITY_CHÍNH} — `{TYPE_A}`

{Mô tả ngắn: màn hình template/phiếu, đơn vị nhập kết quả, loại cột động.}

**Màn hình:** template → `{TemplateView}.cshtml` · phiếu → `{TicketView}.cshtml`

**Đơn vị nhập kết quả:** `{dimension_A}` × `{dimension_B}`

**Cột động:** {mô tả — ví dụ: mốc giờ, ngày trong tháng, tên ca}

### Bảng dùng cho {ENTITY_CHÍNH}

| Bảng | Giai đoạn | Dùng? | Ý nghĩa / vai trò |
| ---- | --------- | ----- | ----------------- |
| `TBL_{PREFIX}_FORM` | Template | ✓ | Header biểu mẫu: mã, tên, loại, bộ phận |
| `TBL_{PREFIX}_TICKET` | Phiếu | ✓ | Header phiếu thật: mã phiếu, ngày, ca, snapshot |
| `TBL_{PREFIX}_TICKET_{DETAIL}` | Kết quả | ✓ | Ô nhập = 1 {dimension_A} × 1 {dimension_B} |

### Bảng **không** dùng

| Bảng | Lý do |
| ---- | ----- |
| `TBL_{OTHER}_*` | Chỉ dùng cho `{TYPE_B}` — xem mục 1A.3 |

### Sơ đồ luồng bảng — {ENTITY_CHÍNH}

```text
[Giai đoạn TEMPLATE — {TemplateView}]
TBL_{PREFIX}_FORM
 └── TBL_{PREFIX}_{CHILD}
      └── TBL_{PREFIX}_{GRANDCHILD}

[Giai đoạn PHIẾU — {TicketView}]
TBL_{PREFIX}_TICKET
 ├── TBL_{PREFIX}_TICKET_{CHILD}              ← snapshot
 ├── TBL_{PREFIX}_TICKET_{SLOT}               ← cột động
 │    └── TBL_{PREFIX}_TICKET_{RESULT}        ← nhập ô
 └── TBL_{PREFIX}_TICKET_{FOOTER_OR_APPROVAL}
```

## 1A.3. {ENTITY_PHỤ} — chỉ dẫn sang tài liệu con

| Mã / Loại | Khác {ENTITY_CHÍNH} ở điểm nào | Tài liệu |
| --------- | ------------------------------ | -------- |
| `{TYPE_B}` | {điểm khác biệt schema/UI} | [`db_diagram_{child}.md`](db_diagram_{child}.md) |

## 1A.4. Bảng dùng chung

| Bảng | Ý nghĩa |
| ---- | ------- |
| `TBL_{SHARED_MASTER}` | Danh mục dùng chung (bộ phận, ca, trạng thái, …) |
| `TBL_{APPROVAL}_HEADER` / `_DETAIL` | Master cây phê duyệt |
| `TBL_{PREFIX}_TICKET_APPROVAL_*` | Snapshot phê duyệt trên phiếu |

---

# 2. Nhóm bảng {TÊN_NHÓM_1}

## 2.1. `TBL_{TABLE_NAME}`

{Bảng master / template / phiếu — mô tả một dòng.}

| Cột chính | Mô tả |
| --------- | ----- |
| `{TABLE}_ID` | Id bản ghi (PK) |
| `{FK_COLUMN}` | FK tới `TBL_{PARENT}` |
| `{BUSINESS_COLUMN}` | {Mô tả nghiệp vụ} |
| `STATUS_ID` | Trạng thái (`ACTIVE`, `PENDING`, …) |
| `CREATE_USER`, `CREATE_DATE`, `LAST_UPDATE_USER`, `LAST_UPDATE_DATE` | Audit |

Cách dùng:

- {Bullet 1 — vai trò trong luồng}
- {Bullet 2 — quan hệ parent/child}
- {Bullet 3 — ràng buộc unique nếu có}

---

## 2.2. `TBL_{TABLE_NAME_2}`

| Cột chính | Mô tả |
| --------- | ----- |
| … | … |

Cách dùng:

- …

---

# 3. Nhóm bảng {TÊN_NHÓM_2}

{Lặp cấu trúc §2 cho từng bảng trong nhóm.}

---

# {N}. Luồng dữ liệu chính

## {N}.1. {Tên luồng — ví dụ: Tạo template}

1. Tạo `TBL_{PREFIX}_FORM`.
2. Tạo các bản ghi con ở `TBL_{PREFIX}_{CHILD}`.
3. {Bước tiếp theo — đánh số rõ thứ tự insert}

---

## {N}.2. {Tên luồng — ví dụ: Tạo phiếu thật}

1. Insert `TBL_{PREFIX}_TICKET`.
2. Snapshot {child tables} từ template.
3. Sinh cột động vào `TBL_{PREFIX}_TICKET_{SLOT}`.
4. Snapshot phê duyệt nếu có.

---

## {N}.3. {Tên luồng — ví dụ: Nhập kết quả}

1. UI render hàng/cột từ snapshot + slot.
2. Người dùng nhập vào `TBL_{PREFIX}_TICKET_{RESULT}`.
3. Ràng buộc: mỗi `{KEY_A} + {KEY_B}` chỉ có một kết quả.
4. {Bước ký tên / footer nếu có}

---

## {N}.4. {Tên luồng — ví dụ: Phê duyệt}

1. Lấy danh sách bước từ bảng approval detail.
2. Duyệt tuyến tính theo `APPROVAL_LEVEL`.
3. Cấp sau chỉ duyệt khi cấp trước `APPROVED`.
4. {Quy tắc auto-approve hoặc reject nếu có}

---

# {N+1}. Tổng hợp danh sách bảng SQL và chức năng

| Bảng SQL | Chức năng |
| -------- | --------- |
| `TBL_{TABLE_1}` | {Chức năng ngắn} |
| `TBL_{TABLE_2}` | {Chức năng ngắn} |

---

# {N+2}. Sơ đồ quan hệ rút gọn

```text
TBL_{PARENT_TEMPLATE}
 └── TBL_{CHILD_TEMPLATE}
      └── TBL_{GRANDCHILD_TEMPLATE}


TBL_{PARENT_TICKET}
 ├── TBL_{CHILD_TICKET}
 │    └── TBL_{RESULT}
 ├── TBL_{SLOT}
 │    └── TBL_{RESULT}
 └── TBL_{FOOTER}
```

---

# {N+3}. Script SQL (chạy một lần)

> **Mục đích:** Một script SQL Server duy nhất — tạo bảng mới và cập nhật bảng cũ theo đúng schema đã mô tả ở các mục trên. Chạy **một lần** trên DB đích; idempotent (chạy lại an toàn).

**Quy tắc viết script:**

- Gộp **CREATE bảng mới** + **ALTER bảng đã tồn tại** + **seed tùy chọn** trong **một** khối ` ```sql ` duy nhất.
- Bảng mới: `IF OBJECT_ID('dbo.TBL_{NAME}', 'U') IS NULL` → `CREATE TABLE`.
- Bảng cũ thiếu cột: `IF COL_LENGTH('dbo.TBL_{NAME}', '{COLUMN}') IS NULL` → `ALTER TABLE ... ADD`.
- Thứ tự: bảng cha trước con; `GO` giữa các batch.
- PK `INT IDENTITY`, audit (`CREATE_USER`, `CREATE_DATE`, `LAST_UPDATE_USER`, `LAST_UPDATE_DATE`), `STATUS_ID` default `ACTIVE` nếu project dùng pattern này.
- Seed chỉ khi cần template mặc định: bọc `IF NOT EXISTS (...)`.

```sql
/* ============================================================
   {TÊN_MODULE} — tạo bảng mới + cập nhật bảng cũ
   Chạy script này một lần trên SQL Server.
   Idempotent: chạy lại không tạo trùng bảng/cột.
   ============================================================ */

-- {N+3}.1 Bảng mới — {mô tả ngắn, ví dụ: Template header}
IF OBJECT_ID('dbo.TBL_{PREFIX}_FORM', 'U') IS NULL
BEGIN
    CREATE TABLE dbo.TBL_{PREFIX}_FORM (
        {PREFIX}_FORM_ID     INT IDENTITY(1,1) NOT NULL,
        FORM_CODE            NVARCHAR(50)  NOT NULL,
        FORM_NAME            NVARCHAR(500) NOT NULL,
        STATUS_ID            NVARCHAR(50)  NOT NULL CONSTRAINT DF_{PREFIX}_FORM_STATUS DEFAULT ('ACTIVE'),
        CREATE_USER          NVARCHAR(50)  NOT NULL,
        CREATE_DATE          DATETIME      NOT NULL,
        LAST_UPDATE_USER     NVARCHAR(50)  NULL,
        LAST_UPDATE_DATE     DATETIME      NOT NULL,
        CONSTRAINT PK_{PREFIX}_FORM PRIMARY KEY ({PREFIX}_FORM_ID),
        CONSTRAINT UQ_{PREFIX}_FORM_CODE UNIQUE (FORM_CODE)
    );
END
GO

-- {N+3}.2 Bảng mới — {mô tả, ví dụ: Template child + FK}
IF OBJECT_ID('dbo.TBL_{PREFIX}_FORM_{CHILD}', 'U') IS NULL
BEGIN
    CREATE TABLE dbo.TBL_{PREFIX}_FORM_{CHILD} (
        {PREFIX}_FORM_{CHILD}_ID INT IDENTITY(1,1) NOT NULL,
        {PREFIX}_FORM_ID         INT           NOT NULL,
        {BUSINESS_COLUMN}        NVARCHAR(500) NOT NULL,
        STATUS_ID                NVARCHAR(50)  NOT NULL CONSTRAINT DF_{PREFIX}_FORM_{CHILD}_STATUS DEFAULT ('ACTIVE'),
        CREATE_USER              NVARCHAR(50)  NOT NULL,
        CREATE_DATE              DATETIME      NOT NULL,
        LAST_UPDATE_USER         NVARCHAR(50)  NULL,
        LAST_UPDATE_DATE         DATETIME      NOT NULL,
        CONSTRAINT PK_{PREFIX}_FORM_{CHILD} PRIMARY KEY ({PREFIX}_FORM_{CHILD}_ID),
        CONSTRAINT FK_{PREFIX}_FORM_{CHILD}_FORM FOREIGN KEY ({PREFIX}_FORM_ID)
            REFERENCES dbo.TBL_{PREFIX}_FORM ({PREFIX}_FORM_ID)
    );
END
GO

-- {N+3}.3 Bảng mới — {mô tả, ví dụ: Phiếu / ticket}
IF OBJECT_ID('dbo.TBL_{PREFIX}_TICKET', 'U') IS NULL
BEGIN
    CREATE TABLE dbo.TBL_{PREFIX}_TICKET (
        {PREFIX}_TICKET_ID   INT IDENTITY(1,1) NOT NULL,
        TICKET_CODE          NVARCHAR(50)  NOT NULL,
        {PREFIX}_FORM_ID     INT           NOT NULL,
        FORM_CODE            NVARCHAR(50)  NOT NULL,
        STATUS_ID            NVARCHAR(50)  NOT NULL,
        CREATE_USER          NVARCHAR(50)  NOT NULL,
        CREATE_DATE          DATETIME      NOT NULL,
        LAST_UPDATE_USER     NVARCHAR(50)  NULL,
        LAST_UPDATE_DATE     DATETIME      NOT NULL,
        CONSTRAINT PK_{PREFIX}_TICKET PRIMARY KEY ({PREFIX}_TICKET_ID),
        CONSTRAINT UQ_{PREFIX}_TICKET_CODE UNIQUE (TICKET_CODE),
        CONSTRAINT FK_{PREFIX}_TICKET_FORM FOREIGN KEY ({PREFIX}_FORM_ID)
            REFERENCES dbo.TBL_{PREFIX}_FORM ({PREFIX}_FORM_ID)
    );
END
GO

-- {N+3}.4 Cập nhật bảng cũ — thêm cột mới (nếu bảng đã tồn tại từ trước)
IF OBJECT_ID('dbo.TBL_{EXISTING_TABLE}', 'U') IS NOT NULL
BEGIN
    IF COL_LENGTH('dbo.TBL_{EXISTING_TABLE}', '{NEW_COLUMN}') IS NULL
        ALTER TABLE dbo.TBL_{EXISTING_TABLE} ADD {NEW_COLUMN} NVARCHAR(50) NULL;

    -- Backfill giá trị mặc định trước khi NOT NULL (nếu cần)
    -- UPDATE dbo.TBL_{EXISTING_TABLE} SET {NEW_COLUMN} = N'{default}' WHERE {NEW_COLUMN} IS NULL;

    -- IF COL_LENGTH(...) IS NOT NULL AND exists nullable:
    --     ALTER TABLE dbo.TBL_{EXISTING_TABLE} ALTER COLUMN {NEW_COLUMN} NVARCHAR(50) NOT NULL;
END
GO

-- {N+3}.5 Index / ràng buộc bổ sung (tùy chọn, idempotent)
IF NOT EXISTS (
    SELECT 1 FROM sys.indexes
    WHERE name = 'IX_{PREFIX}_TICKET_FORM_ID' AND object_id = OBJECT_ID('dbo.TBL_{PREFIX}_TICKET')
)
    CREATE INDEX IX_{PREFIX}_TICKET_FORM_ID ON dbo.TBL_{PREFIX}_TICKET ({PREFIX}_FORM_ID);
GO

-- {N+3}.6 Seed dữ liệu mặc định (tùy chọn — chỉ khi chưa có)
IF NOT EXISTS (SELECT 1 FROM dbo.TBL_{PREFIX}_FORM WHERE STATUS_ID = 'ACTIVE')
BEGIN
    DECLARE @FormId INT;
    DECLARE @Now DATETIME = GETDATE();
    DECLARE @User NVARCHAR(50) = 'SYSTEM';

    INSERT INTO dbo.TBL_{PREFIX}_FORM (
        FORM_CODE, FORM_NAME, STATUS_ID,
        CREATE_USER, CREATE_DATE, LAST_UPDATE_USER, LAST_UPDATE_DATE
    ) VALUES (
        N'{DEFAULT_CODE}', N'{Tên template mặc định}', 'ACTIVE',
        @User, @Now, @User, @Now
    );
    SET @FormId = SCOPE_IDENTITY();

    -- INSERT child rows linked to @FormId ...
END
GO
```

**Đồng bộ với schema:** Mọi bảng/cột trong script phải khớp mục §2+ và §{N+1}. Nếu thiếu cột so với diagram → bổ sung trước khi hoàn tất tài liệu.
