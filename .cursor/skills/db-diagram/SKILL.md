---
name: db-diagram
description: Analyzes business requirements, database design, and source conventions; plans and writes diagram specs to docs/db_diagrams/ per document-template.md (including idempotent SQL script at end). Use when the user invokes /db_diagram, asks for business or database design documentation, or needs a db diagram spec for a module.
disable-model-invocation: true
---

# DB Diagram

Workflow phân tích thiết kế và tạo tài liệu diagram nghiệp vụ/database.

## Mục tiêu

Từ `[prompt]` của user → tài liệu Markdown hoàn chỉnh trong `docs/db_diagrams/` của project, theo [document-template.md](document-template.md).

**Phạm vi:** Chỉ tài liệu phân tích/thiết kế. Không implement code, migration, hay sửa DB.

## Load context (bắt buộc)

Trước khi phân tích:

1. Rule: `.cursor/rules/db-diagram.mdc`
2. Project rules (nếu có): `project.mdc`, `sources-base.mdc`, `architecture-patterns.mdc`
3. Template: [document-template.md](document-template.md)
4. Hướng dẫn authoring: [reference.md](reference.md)

## Workflow

```
Task Progress:
- [ ] 1. Hiểu prompt — làm rõ phạm vi nếu cần (1–3 câu)
- [ ] 2. Phân tích nghiệp vụ (subagent db-business-analyst)
- [ ] 3. Khám phá source code (subagent db-source-explorer)
- [ ] 4. Lập kế hoạch tài liệu (subagent db-diagram-planner)
- [ ] 5. Trình kế hoạch — chờ user xác nhận (Plan mode)
- [ ] 6. Viết tài liệu (subagent db-diagram-writer)
- [ ] 7. Tóm tắt kết quả cho user
```

### Bước 1 — Hiểu prompt

- Xác định module, loại phiếu/entity, hoặc nhóm bảng liên quan.
- Nếu thiếu thông tin: hỏi ngắn (phạm vi in/out, module đã có code hay thiết kế mới, tên file mong muốn).

### Bước 2 — Phân tích nghiệp vụ

Delegate **db-business-analyst** (`.cursor/agents/db-business-analyst.md`).

Thu thập: actor, luồng chính, entity, quy tắc snapshot/template, trạng thái, phê duyệt, cột động (nếu có).

### Bước 3 — Khám phá source code

Delegate **db-source-explorer** (`.cursor/agents/db-source-explorer.md`).

Map: Controller, View, Model, client scripts, stored procedure, bảng SQL, API naming, convention response.

### Bước 4 — Lập kế hoạch

Delegate **db-diagram-planner** (`.cursor/agents/db-diagram-planner.md`).

Output kế hoạch: tên file, outline sections, danh sách bảng, ma trận tra cứu, rủi ro/thiếu dữ liệu.

### Bước 5 — Xác nhận

Trình bày kế hoạch bằng tiếng Việt. **Chỉ ghi file sau khi user đồng ý** (hoặc user đã bật Plan mode và approve).

### Bước 6 — Viết tài liệu

Delegate **db-diagram-writer** (`.cursor/agents/db-diagram-writer.md`).

- Path: `docs/db_diagrams/db_diagram_{slug}.md`
- Tuân thủ rule `db-diagram.mdc`, [document-template.md](document-template.md), [reference.md](reference.md)
- Liên kết chéo tới tài liệu con/cha nếu module lớn (pattern §1A.3)
- **Mục cuối:** Script SQL một lần (CREATE + ALTER) — xem template §{N+3}

### Bước 7 — Hoàn tất

Báo cáo: đường dẫn file, sections chính, giả định, việc cần review thêm.

## Ràng buộc

- Phản hồi user: **tiếng Việt**.
- Không đoán schema — ghi rõ `*(cần xác nhận)*` nếu chưa tìm thấy trong code/DB.
- Giữ naming bảng/cột/API đúng như codebase.
- Module lớn: tách tài liệu con + mục chỉ dẫn (pattern §1A.3 trong template).

## Related

- **Command**: `.cursor/commands/db_diagram.md`
- **Rule**: `.cursor/rules/db-diagram.mdc`
- **Template**: [document-template.md](document-template.md)
- **Subagents**: `db-business-analyst`, `db-source-explorer`, `db-diagram-planner`, `db-diagram-writer`
