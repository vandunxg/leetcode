---
comments: true
difficulty: Medium
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [467. Unique Substrings in Wraparound String](https://leetcode.com/problems/unique-substrings-in-wraparound-string)

[中文文档](/solution/0400-0499/0467.Unique%20Substrings%20in%20Wraparound%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Quy ước chuỗi <code>base</code> là chuỗi tuần hoàn vô hạn tạo từ <code>&quot;abcdefghijklmnopqrstuvwxyz&quot;</code>, nên <code>base</code> có dạng như sau:</p>

<ul>
	<li><code>&quot;...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd....&quot;</code>.</li>
</ul>

<p>Cho chuỗi <code>s</code>. Hãy trả về <em>số lượng <strong>chuỗi con không rỗng khác nhau</strong> của </em><code>s</code><em> xuất hiện trong </em><code>base</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có chuỗi con &quot;a&quot; của s xuất hiện trong base.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cac&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có hai chuỗi con (&quot;a&quot;, &quot;c&quot;) của s xuất hiện trong base.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;zab&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có sáu chuỗi con (&quot;z&quot;, &quot;a&quot;, &quot;b&quot;, &quot;za&quot;, &quot;ab&quot; và &quot;zab&quot;) của s xuất hiện trong base.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chuỗi con liên tiếp khác nhau xuất hiện trong chuỗi tuần hoàn vô hạn. Liệt kê và hash mọi chuỗi con sẽ tốn thời gian bậc hai. Với các đoạn kết thúc bằng cùng một chữ cái, đoạn ngắn hơn luôn nằm trong đoạn dài hơn.
>
> Gọi $f[c]$ là độ dài đoạn hợp lệ dài nhất kết thúc bằng $c$; đáp án là tổng các giá trị trong $f$. Khi duyệt chuỗi, tăng $k$ nếu hai chữ cái liên tiếp chênh nhau $1$ theo modulo $26$; nếu không thì đặt lại $k$.
>
> Chỉ giữ giá trị $k$ lớn nhất cho mỗi chữ cái kết thúc giúp loại bỏ các chuỗi con trùng lặp.

<!-- thinking:end -->

Định nghĩa mảng $f$ có độ dài $26$, trong đó $f[i]$ là độ dài chuỗi con liên tiếp dài nhất kết thúc bằng ký tự thứ $i$. Đáp án là tổng các phần tử trong $f$.

Dùng biến $k$ để biểu thị độ dài chuỗi con liên tiếp dài nhất kết thúc tại ký tự hiện tại. Duyệt chuỗi $s$. Với mỗi ký tự $c$, nếu hiệu giữa $c$ và ký tự trước đó $s[i - 1]$ là $1$ theo modulo $26$, tăng $k$ thêm $1$; nếu không, đặt lại $k$ thành $1$. Sau đó cập nhật $f[c]$ bằng giá trị lớn hơn giữa $f[c]$ và $k$.

Cuối cùng, trả về tổng các phần tử trong $f$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là bảng ký tự; ở đây, đó là tập hợp các chữ cái viết thường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSubstringInWraproundString(self, s: str) -> int:
        f = defaultdict(int)
        k = 0
        for i, c in enumerate(s):
            if i and (ord(c) - ord(s[i - 1])) % 26 == 1:
                k += 1
            else:
                k = 1
            f[c] = max(f[c], k)
        return sum(f.values())
```

#### Java

```java
class Solution {
    public int findSubstringInWraproundString(String s) {
        int[] f = new int[26];
        int n = s.length();
        for (int i = 0, k = 0; i < n; ++i) {
            if (i > 0 && (s.charAt(i) - s.charAt(i - 1) + 26) % 26 == 1) {
                ++k;
            } else {
                k = 1;
            }
            f[s.charAt(i) - 'a'] = Math.max(f[s.charAt(i) - 'a'], k);
        }
        return Arrays.stream(f).sum();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findSubstringInWraproundString(string s) {
        int f[26]{};
        int n = s.length();
        for (int i = 0, k = 0; i < n; ++i) {
            if (i && (s[i] - s[i - 1] + 26) % 26 == 1) {
                ++k;
            } else {
                k = 1;
            }
            f[s[i] - 'a'] = max(f[s[i] - 'a'], k);
        }
        return accumulate(begin(f), end(f), 0);
    }
};
```

#### Go

```go
func findSubstringInWraproundString(s string) (ans int) {
	f := [26]int{}
	k := 0
	for i := range s {
		if i > 0 && (s[i]-s[i-1]+26)%26 == 1 {
			k++
		} else {
			k = 1
		}
		f[s[i]-'a'] = max(f[s[i]-'a'], k)
	}
	for _, x := range f {
		ans += x
	}
	return
}
```

#### TypeScript

```ts
function findSubstringInWraproundString(s: string): number {
    const idx = (c: string): number => c.charCodeAt(0) - 97;
    const f: number[] = Array(26).fill(0);
    const n = s.length;
    for (let i = 0, k = 0; i < n; ++i) {
        const j = idx(s[i]);
        if (i && (j - idx(s[i - 1]) + 26) % 26 === 1) {
            ++k;
        } else {
            k = 1;
        }
        f[j] = Math.max(f[j], k);
    }
    return f.reduce((acc, cur) => acc + cur, 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn find_substring_in_wrapround_string(s: String) -> i32 {
        let idx = |c: u8| -> usize { (c - b'a') as usize };
        let mut f = vec![0; 26];
        let n = s.len();
        let s = s.as_bytes();
        let mut k = 0;
        for i in 0..n {
            let j = idx(s[i]);
            if i > 0 && ((j as i32) - (idx(s[i - 1]) as i32) + 26) % 26 == 1 {
                k += 1;
            } else {
                k = 1;
            }
            f[j] = f[j].max(k);
        }

        f.iter().sum()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
