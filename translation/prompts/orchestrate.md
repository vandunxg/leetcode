# Prompt: Orchestrate page-level translation

Mục tiêu: dịch nhiều bài song song với **1 agent = 1 page** và giảm tối đa token/toolcall.

## Cách chạy

1. Chọn N bài chưa dịch.
2. Giao mỗi bài cho đúng một agent.
3. Mỗi agent chỉ đọc:
   - `translation/RULE.md`
   - `README_EN.md` của bài được giao
4. Agent dịch toàn bộ page sang `vi/.../README.md`.
5. Agent đọc lại nhanh target một lượt rồi kết thúc.

Nếu dùng 20 agents thì mặc định là 20 bài khác nhau chạy song song.

## Không làm ở worker

Worker không chạy mặc định:

- prettier;
- `check_vi.py`;
- `check_thinking.py`;
- report/hash/state;
- coverage map;
- reviewer riêng;
- glossary proposal;
- progress metadata.

Worker cũng không chia bài thành task comment/heading/description/solution riêng.

## Validation

Orchestrator hoặc CI chạy validation theo batch sau khi một nhóm page hoàn tất.

Chỉ rerun/fix những page fail validation; không bắt tất cả worker lặp lại check giống nhau.

## Ownership

Một target file chỉ có một writer tại một thời điểm.

Code và code comments mặc định giữ nguyên theo `translation/RULE.md`.
