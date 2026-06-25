# DB Diagram

## Description

Phân tích nghiệp vụ, thiết kế database và cấu trúc source code — sau đó lên kế hoạch và tạo tài liệu diagram vào `docs/db_diagrams/`, theo template chuẩn [`.cursor/skills/db-diagram/document-template.md`](../skills/db-diagram/document-template.md) (portable, không phụ thuộc thư mục `docs` của project).

**Chỉ tạo tài liệu phân tích/thiết kế** — không implement code, không sửa database, không build.

## How to use

```
/db_diagram [prompt]
```

- `[prompt]`: Mô tả nghiệp vụ/module cần phân tích thiết kế (ví dụ: "Module kho nguyên liệu", "Phiếu xuất kho bán thành phẩm").
- **Khuyến nghị:** Bật **Plan mode** của Cursor trước khi gõ lệnh để agent lập kế hoạch và xác nhận trước khi ghi file.

## Steps

1. **Load context**: Đọc và áp dụng:
   - Skill: `.cursor/skills/db-diagram/SKILL.md`
   - Rule: `.cursor/rules/db-diagram.mdc`
   - Project rules: `project.mdc`, `sources-base.mdc`, `architecture-patterns.mdc`
   - Template: `.cursor/skills/db-diagram/document-template.md`
   - Hướng dẫn: `.cursor/skills/db-diagram/reference.md`
2. **Phân tích nghiệp vụ**: Dùng subagent **db-business-analyst** (`.cursor/agents/db-business-analyst.md`) — thu thập entity, luồng, quy tắc nghiệp vụ từ prompt và codebase.
3. **Khám phá source code**: Dùng subagent **db-source-explorer** (`.cursor/agents/db-source-explorer.md`) — map Controller/View/Model, API `fn*`, bảng SQL, convention dự án.
4. **Lập kế hoạch tài liệu**: Dùng subagent **db-diagram-planner** (`.cursor/agents/db-diagram-planner.md`) — xác định tên file, outline sections, bảng nào cần mô tả, ma trận tra cứu.
5. **Xác nhận với user** (Plan mode): Trình bày kế hoạch ngắn gọn; chỉ ghi file sau khi user đồng ý hoặc chỉnh sửa phạm vi.
6. **Viết tài liệu**: Dùng subagent **db-diagram-writer** (`.cursor/agents/db-diagram-writer.md`) — tạo/cập nhật file trong `docs/db_diagrams/`, **gồm mục Script SQL cuối** (một script idempotent).
7. **Hoàn tất**: Tóm tắt file đã tạo, đường dẫn, và các điểm cần review thêm.

## When to use

- Cần đặc tả database + nghiệp vụ cho module mới hoặc module chưa có tài liệu.
- Cần ma trận tra cứu bảng ↔ màn hình ↔ luồng dữ liệu trước khi implement.
- Cần chuẩn hóa tài liệu thiết kế theo template `.cursor/skills/db-diagram/document-template.md`.

## Examples

**Example 1 — Module mới**  
User (Plan mode): `/db_diagram Module quản lý kế hoạch cắt vải`  
Agent: Phân tích nghiệp vụ + source → kế hoạch outline → xác nhận → `docs/db_diagrams/db_diagram_cutting_plan.md`.

**Example 2 — Mở rộng module có sẵn**  
User: `/db_diagram Bổ sung tài liệu cho phiếu COMPLIANCE_CONFIRMATION`  
Agent: Đọc code QCPatrol + bảng hiện có → tạo `docs/db_diagrams/db_diagram_COMPLIANCE_CONFIRMATION.md` (hoặc cập nhật nếu đã tồn tại).

## Important notes

- **Ngôn ngữ phản hồi:** Tiếng Việt (kế hoạch, câu hỏi, tóm tắt).
- **Output path:** Ghi vào `docs/db_diagrams/` của project đang làm việc.
- **Naming file:** `db_diagram_{module_slug}.md` (snake_case, UPPER nếu là FORM_TYPE).
- Nếu prompt mơ hồ, hỏi 1–3 câu ngắn trước khi lập kế hoạch.
- Không dừng giữa chừng: hoàn thành phân tích + tài liệu (sau khi user xác nhận kế hoạch).

## Related

- **Skill**: `.cursor/skills/db-diagram/SKILL.md`
- **Rule**: `.cursor/rules/db-diagram.mdc`
- **Template**: `.cursor/skills/db-diagram/document-template.md`
- **Hướng dẫn**: `.cursor/skills/db-diagram/reference.md`
- **Subagents**: `db-business-analyst`, `db-source-explorer`, `db-diagram-planner`, `db-diagram-writer`
