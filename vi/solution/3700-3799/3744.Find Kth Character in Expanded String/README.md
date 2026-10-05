---
comments: true
difficulty: Medium
tags:
    - String
---

<!-- problem:start -->

# [3744. Find Kth Character in Expanded String 🔒](https://leetcode.com/problems/find-kth-character-in-expanded-string)

[Tài liệu tiếng Trung](/solution/3700-3799/3744.Find%20Kth%20Character%20in%20Expanded%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> gồm một hoặc nhiều từ, các từ được phân cách bằng một dấu cách. Mỗi từ trong <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Ta tạo chuỗi <strong>mở rộng</strong> <code>t</code> từ <code>s</code> như sau:</p>

<ul>
	<li>Với mỗi <strong>từ</strong> trong <code>s</code>, lặp lại ký tự đầu tiên một lần, ký tự thứ hai hai lần, và tiếp tục như vậy.</li>
</ul>

<p>Ví dụ, nếu <code>s = &quot;hello world&quot;</code>, thì <code>t = &quot;heelllllllooooo woorrrllllddddd&quot;</code>.</p>

<p>Đồng thời, cho một số nguyên <code>k</code>, đại diện cho một chỉ số <strong>hợp lệ</strong> của chuỗi <code>t</code>.</p>

<p>Hãy trả về ký tự thứ <code>k<sup>th</sup></code> của chuỗi <code>t</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;hello world&quot;, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;h&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>t = &quot;heelllllllooooo woorrrllllddddd&quot;</code>. Do đó, đáp án là <code>t[0] = &quot;h&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;hello world&quot;, k = 15</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot; &quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>t = &quot;heelllllllooooo woorrrllllddddd&quot;</code>. Do đó, đáp án là <code>t[15] = &quot; &quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường và dấu cách <code>&#39; &#39;</code>.</li>
	<li><code>s</code> <strong>không chứa</strong> dấu cách ở đầu hoặc cuối.</li>
	<li>Tất cả các từ trong <code>s</code> được phân cách bằng <strong>một dấu cách</strong>.</li>
	<li><code>0 &lt;= k &lt; t.length</code>. Nghĩa là <code>k</code> là một chỉ số <code>t</code> <strong>hợp lệ</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi mở rộng có thể dài hơn $k$ rất nhiều, nên ta không được tạo nó. Chữ cái thứ $i$ của một từ được lặp lại $i+1$ lần, và độ dài mở rộng của một từ là một số tam giác. Ta lần lượt trừ độ dài của từng từ (cùng với dấu cách ngay sau nó) khỏi $k$ cho đến khi phần còn lại rơi vào một từ, sau đó duyệt qua các đoạn lặp của từ đó.

<!-- thinking:end -->

Trước hết, ta tách chuỗi $\textit{s}$ thành nhiều từ bằng dấu cách. Với mỗi từ $\textit{w}$, ta có thể tính độ dài mà nó chiếm trong chuỗi mở rộng $\textit{t}$ là $m=\frac{(1+|\textit{w}|)\cdot |\textit{w}|}{2}$.

Nếu $k = m$, điều đó có nghĩa là ký tự thứ $k$ là một dấu cách, và ta có thể trả về trực tiếp một dấu cách.

Nếu $k > m$, điều đó có nghĩa là ký tự thứ $k$ không nằm trong phần mở rộng của từ hiện tại. Ta trừ độ dài mở rộng $m$ của từ hiện tại và độ dài dấu cách $1$ khỏi $k$, rồi tiếp tục xử lý từ tiếp theo.

Ngược lại, ký tự thứ $k$ nằm trong phần mở rộng của từ hiện tại. Ta có thể tìm ký tự thứ $k$ bằng cách mô phỏng quá trình mở rộng:

- Khởi tạo biến $\textit{cur} = 0$ để biểu diễn số ký tự đã được mở rộng.
- Duyệt qua từng ký tự $\textit{w}[i]$ của từ $\textit{w}$:
    - Tăng $\textit{cur}$ thêm $i + 1$.
    - Nếu $k < \textit{cur}$, điều đó có nghĩa là ký tự thứ $k$ là $\textit{w}[i]$, và ta trả về ký tự này.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthCharacter(self, s: str, k: int) -> str:
        for w in s.split():
            m = (1 + len(w)) * len(w) // 2
            if k == m:
                return " "
            if k > m:
                k -= m + 1
            else:
                cur = 0
                for i in range(len(w)):
                    cur += i + 1
                    if k < cur:
                        return w[i]
```

#### Java

```java
class Solution {
    public char kthCharacter(String s, long k) {
        for (String w : s.split(" ")) {
            long m = (1L + w.length()) * w.length() / 2;
            if (k == m) {
                return ' ';
            }
            if (k > m) {
                k -= m + 1;
            } else {
                long cur = 0;
                for (int i = 0;; ++i) {
                    cur += i + 1;
                    if (k < cur) {
                        return w.charAt(i);
                    }
                }
            }
        }
        return ' ';
    }
}
```

#### C++

```cpp
class Solution {
public:
    char kthCharacter(string s, long long k) {
        stringstream ss(s);
        string w;
        while (ss >> w) {
            long long m = (1 + (long long) w.size()) * (long long) w.size() / 2;
            if (k == m) {
                return ' ';
            }
            if (k > m) {
                k -= m + 1;
            } else {
                long long cur = 0;
                for (int i = 0;; ++i) {
                    cur += i + 1;
                    if (k < cur) {
                        return w[i];
                    }
                }
            }
        }
        return ' ';
    }
};
```

#### Go

```go
func kthCharacter(s string, k int64) byte {
	for _, w := range strings.Split(s, " ") {
		m := (1 + int64(len(w))) * int64(len(w)) / 2
		if k == m {
			return ' '
		}
		if k > m {
			k -= m + 1
		} else {
			var cur int64
			for i := 0; ; i++ {
				cur += int64(i + 1)
				if k < cur {
					return w[i]
				}
			}
		}
	}
	return ' '
}
```

#### TypeScript

```ts
function kthCharacter(s: string, k: number): string {
    for (const w of s.split(' ')) {
        const m = ((1 + w.length) * w.length) / 2;
        if (k === m) {
            return ' ';
        }
        if (k > m) {
            k -= m + 1;
        } else {
            let cur = 0;
            for (let i = 0; ; ++i) {
                cur += i + 1;
                if (k < cur) {
                    return w[i];
                }
            }
        }
    }
    return ' ';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
