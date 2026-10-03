---
comments: true
difficulty: Medium
rating: 1643
source: Biweekly Contest 62 Q3
tags:
    - String
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [2024. Maximize the Confusion of an Exam](https://leetcode.com/problems/maximize-the-confusion-of-an-exam)

[中文文档](/solution/2000-2099/2024.Maximize%20the%20Confusion%20of%20an%20Exam/README.md)

## Mô tả

<!-- description:start -->

<p>Một giáo viên đang ra một bài kiểm tra gồm <code>n</code> câu hỏi đúng/sai, trong đó <code>&#39;T&#39;</code> biểu thị đúng và <code>&#39;F&#39;</code> biểu thị sai. Thầy muốn làm học sinh bối rối bằng cách <strong>tối đa hóa</strong> số câu hỏi <strong>liên tiếp</strong> có cùng <strong>một</strong> đáp án (nhiều đáp án đúng hoặc nhiều đáp án sai liên tiếp).</p>

<p>Bạn được cho một chuỗi <code>answerKey</code>, trong đó <code>answerKey[i]</code> là đáp án ban đầu cho câu hỏi thứ <code>i<sup>th</sup></code>. Ngoài ra, bạn được cho một số nguyên <code>k</code>, là số lần tối đa bạn có thể thực hiện thao tác sau:</p>

<ul>
	<li>Thay đổi đáp án của bất kỳ câu hỏi nào thành <code>&#39;T&#39;</code> hoặc <code>&#39;F&#39;</code> (tức là đặt <code>answerKey[i]</code> thành <code>&#39;T&#39;</code> hoặc <code>&#39;F&#39;</code>).</li>
</ul>

<p>Hãy trả về <em><strong>số lượng tối đa</strong> các ký tự liên tiếp</em> <code>&#39;T&#39;</code> hoặc <code>&#39;F&#39;</code> <em>trong answer key sau khi thực hiện thao tác không quá</em> <code>k</code> <em>lần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> answerKey = &quot;TTFF&quot;, k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể thay cả hai ký tự &#39;F&#39; thành &#39;T&#39; để biến answerKey thành &quot;<u>TTTT</u>&quot;.
Có bốn ký tự &#39;T&#39; liên tiếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> answerKey = &quot;TFFT&quot;, k = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể thay ký tự &#39;T&#39; đầu tiên thành &#39;F&#39; để biến answerKey thành &quot;<u>FFF</u>T&quot;.
Hoặc ta có thể thay ký tự &#39;T&#39; thứ hai thành &#39;F&#39; để biến answerKey thành &quot;T<u>FFF</u>&quot;.
Trong cả hai trường hợp, có ba ký tự &#39;F&#39; liên tiếp.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> answerKey = &quot;TTFTTFTT&quot;, k = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ta có thể thay ký tự &#39;F&#39; đầu tiên để biến answerKey thành &quot;<u>TTTTT</u>FTT&quot;.
Hoặc ta có thể thay ký tự &#39;F&#39; thứ hai để biến answerKey thành &quot;TTF<u>TTTTT</u>&quot;.
Trong cả hai trường hợp, có năm ký tự &#39;T&#39; liên tiếp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == answerKey.length</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>answerKey[i]</code> là <code>&#39;T&#39;</code> hoặc <code>&#39;F&#39;</code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể đổi không quá $k$ đáp án để tối đa hóa một đoạn liên tiếp chỉ gồm một loại ký tự. Vì $n \le 5 \times 10^4$, không thể duyệt qua mọi đoạn. Việc đổi theo hướng biến thành `T` và theo hướng biến thành `F` là độc lập.
>
> Một cửa sổ hợp lệ khi chứa không quá $k$ ký tự đối lập. Mở rộng đầu phải; nếu vượt quá ngân sách, mở rộng đầu trái.
>
> Hai cửa sổ trượt cho ta đáp án; code duy trì một cửa sổ hợp lệ có độ dài $n-l$.

<!-- thinking:end -->

Ta xây dựng một hàm $\textit{f}(c)$, biểu thị độ dài lớn nhất của các ký tự liên tiếp với điều kiện có thể thay thế không quá $k$ ký tự $c$, trong đó $c$ có thể là 'T' hoặc 'F'. Đáp án là $\max(\textit{f}('T'), \textit{f}('F'))$.

Ta duyệt qua chuỗi $\textit{answerKey}$, sử dụng biến $\textit{cnt}$ để ghi nhận số ký tự $c$ trong cửa sổ hiện tại. Khi $\textit{cnt} > k$, ta di chuyển con trỏ trái của cửa sổ sang phải một vị trí. Sau khi kết thúc vòng lặp, độ dài cửa sổ là độ dài lớn nhất của các ký tự liên tiếp.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

Các bài toán tương tự:

- [487. Max Consecutive Ones II](https://github.com/doocs/leetcode/blob/main/solution/0400-0499/0487.Max%20Consecutive%20Ones%20II/README_EN.md)
- [1004. Max Consecutive Ones III](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1004.Max%20Consecutive%20Ones%20III/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxConsecutiveAnswers(self, answerKey: str, k: int) -> int:
        def f(c: str) -> int:
            cnt = l = 0
            for ch in answerKey:
                cnt += ch == c
                if cnt > k:
                    cnt -= answerKey[l] == c
                    l += 1
            return len(answerKey) - l

        return max(f("T"), f("F"))
```

#### Java

```java
class Solution {
    private char[] s;
    private int k;

    public int maxConsecutiveAnswers(String answerKey, int k) {
        s = answerKey.toCharArray();
        this.k = k;
        return Math.max(f('T'), f('F'));
    }

    private int f(char c) {
        int l = 0, cnt = 0;
        for (char ch : s) {
            cnt += ch == c ? 1 : 0;
            if (cnt > k) {
                cnt -= s[l++] == c ? 1 : 0;
            }
        }
        return s.length - l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxConsecutiveAnswers(string answerKey, int k) {
        int n = answerKey.size();
        auto f = [&](char c) {
            int l = 0, cnt = 0;
            for (char& ch : answerKey) {
                cnt += ch == c;
                if (cnt > k) {
                    cnt -= answerKey[l++] == c;
                }
            }
            return n - l;
        };
        return max(f('T'), f('F'));
    }
};
```

#### Go

```go
func maxConsecutiveAnswers(answerKey string, k int) int {
	f := func(c byte) int {
		l, cnt := 0, 0
		for _, ch := range answerKey {
			if byte(ch) == c {
				cnt++
			}
			if cnt > k {
				if answerKey[l] == c {
					cnt--
				}
				l++
			}
		}
		return len(answerKey) - l
	}
	return max(f('T'), f('F'))
}
```

#### TypeScript

```ts
function maxConsecutiveAnswers(answerKey: string, k: number): number {
    const n = answerKey.length;
    const f = (c: string): number => {
        let [l, cnt] = [0, 0];
        for (const ch of answerKey) {
            cnt += ch === c ? 1 : 0;
            if (cnt > k) {
                cnt -= answerKey[l++] === c ? 1 : 0;
            }
        }
        return n - l;
    };
    return Math.max(f('T'), f('F'));
}
```

#### Rust

```rust
impl Solution {
    pub fn max_consecutive_answers(answer_key: String, k: i32) -> i32 {
        let n = answer_key.len();
        let k = k as usize;
        let s: Vec<char> = answer_key.chars().collect();

        let f = |c: char| -> usize {
            let mut l = 0;
            let mut cnt = 0;
            for &ch in &s {
                cnt += if ch == c { 1 } else { 0 };
                if cnt > k {
                    cnt -= if s[l] == c { 1 } else { 0 };
                    l += 1;
                }
            }
            n - l
        };

        std::cmp::max(f('T'), f('F')) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
