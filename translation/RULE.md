# LeetCode Vietnamese Translation Rule

Mục tiêu: **dịch nhanh, đúng meaning, đọc tự nhiên như tài liệu do developer Việt viết**.

## 1. Một agent = một bài

Mỗi agent nhận đúng một `README_EN.md` và chịu trách nhiệm toàn bộ target `vi/.../README.md` tương ứng.

Không chia một bài thành task riêng cho comment, heading, description, solution hay example.

## 2. Cách dịch

- Dịch meaning, không dịch word-by-word.
- Không thêm ý, không tóm tắt, không tự sửa algorithm/complexity.
- Câu tiếng Việt ngắn gọn, tự nhiên.
- Giữ English technical/domain term nếu developer dùng tự nhiên hơn: `stack`, `queue`, `heap`, `hash map`, `node`, `request`, `cache`, `async`, `upstream`, `downstream`, v.v.
- Không cố Việt hóa term chỉ để câu “thuần Việt”.
- Với từ phổ thông có bản Việt tự nhiên thì dùng tiếng Việt: `array → mảng`, `string → chuỗi`, `index → chỉ số` khi nói vị trí trong mảng.

Style tham chiếu:

> System chủ yếu phục vụ nhân viên procurement và finance, phụ trách tra cứu sản phẩm, tạo order, approval, đồng bộ trạng thái thanh toán và tra cứu fulfillment. Backend được tách thành các module order, inventory và approval; khi tạo order, trước tiên sẽ validate request và price, sau đó tạo order và reserve inventory. Sau khi thành công, system gửi message để downstream hoàn thành các task async như thông báo approval.

## 3. Phần giữ nguyên

Không sửa:

- front matter;
- H1 tên bài chính thức;
- code và code comments;
- identifier / inline code;
- LaTeX/math;
- URL;
- HTML comment marker;
- input/output literal;
- language tab như `Python3`, `Java`, `Go`.

Giữ nguyên thứ tự section.

Nhãn chuẩn:

- `Description → Mô tả`
- `Solutions → Lời giải`
- `Solution N → Lời giải N`
- `Thinking → Tư duy`
- `Example → Ví dụ`
- `Input → Đầu vào`
- `Output → Đầu ra`
- `Explanation → Giải thích`
- `Constraints → Ràng buộc`
- `Follow-up → Câu hỏi mở rộng`

## 4. Workflow

Mỗi agent chỉ làm 3 việc:

1. Đọc toàn bộ `README_EN.md`.
2. Dịch toàn bộ page sang target `vi/.../README.md`.
3. Đọc lại target một lượt để bắt lỗi rõ ràng rồi kết thúc.

Không chạy prettier/check script cho từng worker trừ khi task cụ thể yêu cầu hoặc có lỗi nghi ngờ.

Validation tự động chạy theo batch/CI, không phải trách nhiệm mặc định của từng agent.

## 5. Không làm

Không tạo:

- report;
- hash;
- state;
- coverage map;
- reviewer riêng;
- audit;
- progress metadata;
- glossary proposal;
- subtask chỉ để dịch comment.

Nếu dùng 20 agents thì mặc định là **20 bài khác nhau chạy song song**.
