# Prompt: Translate one complete page

Mỗi invocation xử lý đúng một bài.

## Agent chỉ cần

1. Đọc `translation/RULE.md`.
2. Đọc toàn bộ `README_EN.md` của bài.
3. Dịch toàn bộ page sang `vi/.../README.md`.
4. Đọc lại target nhanh một lượt rồi kết thúc.

Không đọc thêm `PROJECT_RULES.md`, `GLOSSARY.md`, review prompt, state/report files trừ khi task đặc biệt yêu cầu.

Không chạy prettier hoặc check script mặc định ở worker. Validation chạy theo batch/CI.

Không tạo task phụ như `Translate comments 0004`; một agent chịu trách nhiệm toàn bộ page được giao.

Code comments mặc định giữ nguyên.
