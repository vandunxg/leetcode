---
comments: true
difficulty: Hard
rating: 1876
source: Weekly Contest 181 Q4
tags:
    - String
    - String Matching
    - Hash Function
    - Rolling Hash
    - KMP
    - Extended KMP
---

<!-- problem:start -->

# [1392. Longest Happy Prefix](https://leetcode.com/problems/longest-happy-prefix)

[中文文档](/solution/1300-1399/1392.Longest%20Happy%20Prefix/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi được gọi là <strong>happy prefix</strong> nếu nó là một prefix <strong>không rỗng</strong> đồng thời cũng là suffix của chuỗi đó (không tính chính chuỗi đó).</p>

<p>Cho chuỗi <code>s</code>, hãy trả về <em><strong>happy prefix dài nhất</strong> của</em> <code>s</code>. Nếu không có prefix nào như vậy, trả về chuỗi rỗng <code>&quot;&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;level&quot;
<strong>Đầu ra:</strong> &quot;l&quot;
<strong>Giải thích:</strong> s có 4 prefix không bao gồm chính nó (&quot;l&quot;, &quot;le&quot;, &quot;lev&quot;, &quot;leve&quot;) và các suffix (&quot;l&quot;, &quot;el&quot;, &quot;vel&quot;, &quot;evel&quot;). Prefix dài nhất đồng thời là suffix là &quot;l&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ababab&quot;
<strong>Đầu ra:</strong> &quot;abab&quot;
<strong>Giải thích:</strong> &quot;abab&quot; là prefix dài nhất đồng thời cũng là suffix. Hai đoạn này có thể chồng lấn trong chuỗi ban đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: String Hashing

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm prefix thực sự dài nhất đồng thời là suffix. So sánh lần lượt mọi độ dài sẽ có độ phức tạp bậc hai khi $n \le 10^5$. Bắt đầu từ prefix thực sự dài nhất, nếu $s[:-i]==s[i:]$ thì ta tìm được đáp án; nếu không có độ dài nào thỏa mãn, kết quả là chuỗi rỗng.

<!-- thinking:end -->

**String Hashing** là phương pháp ánh xạ một chuỗi bất kỳ độ dài nào thành số nguyên không âm, với xác suất collision gần như bằng không. Phương pháp này tính hash value của chuỗi để nhanh chóng xác định hai chuỗi có bằng nhau hay không.

Ta chọn giá trị BASE cố định và xem chuỗi như một số trong hệ cơ số BASE, gán một giá trị lớn hơn 0 cho mỗi ký tự. Thông thường, các giá trị được gán nhỏ hơn BASE nhiều. Ví dụ, với chuỗi chỉ gồm chữ cái viết thường, ta có thể gán a=1, b=2, ..., z=26. Ta chọn giá trị MOD cố định và lấy phần dư khi chia số biểu diễn trong hệ cơ số BASE cho MOD làm hash value của chuỗi.

Thông thường, ta chọn BASE=131 hoặc BASE=13331 để xác suất collision của hash value cực thấp. Nếu hai chuỗi có cùng hash value, ta xem chúng là bằng nhau. MOD thường được chọn là $2^{64}$. Trong C++, có thể dùng trực tiếp kiểu unsigned long long để lưu hash value này. Khi tính toán, ta không xử lý tràn số; khi xảy ra tràn, phép tính tự động tương đương với lấy modulo $2^{64}$, tránh các phép modulo kém hiệu quả.

Trừ khi dữ liệu được tạo ra cực kỳ đặc biệt, thuật toán hash trên khó có khả năng xảy ra collision. Thuật toán này thường được dùng trong các lời giải tiêu chuẩn cho bài toán. Ta cũng có thể chọn các giá trị BASE và MOD phù hợp (chẳng hạn các số nguyên tố lớn) rồi tính nhiều nhóm hash. Chỉ khi tất cả kết quả đều giống nhau, ta mới xem các chuỗi ban đầu là bằng nhau; cách này khiến việc tạo dữ liệu gây lỗi hash càng khó hơn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPrefix(self, s: str) -> str:
        for i in range(1, len(s)):
            if s[:-i] == s[i:]:
                return s[i:]
        return ''
```

#### Java

```java
class Solution {
    private long[] p;
    private long[] h;

    public String longestPrefix(String s) {
        int base = 131;
        int n = s.length();
        p = new long[n + 10];
        h = new long[n + 10];
        p[0] = 1;
        for (int i = 0; i < n; ++i) {
            p[i + 1] = p[i] * base;
            h[i + 1] = h[i] * base + s.charAt(i);
        }
        for (int l = n - 1; l > 0; --l) {
            if (get(1, l) == get(n - l + 1, n)) {
                return s.substring(0, l);
            }
        }
        return "";
    }

    private long get(int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }
}
```

#### C++

```cpp
typedef unsigned long long ULL;

class Solution {
public:
    string longestPrefix(string s) {
        int base = 131;
        int n = s.size();
        ULL p[n + 10];
        ULL h[n + 10];
        p[0] = 1;
        h[0] = 0;
        for (int i = 0; i < n; ++i) {
            p[i + 1] = p[i] * base;
            h[i + 1] = h[i] * base + s[i];
        }
        for (int l = n - 1; l > 0; --l) {
            ULL prefix = h[l];
            ULL suffix = h[n] - h[n - l] * p[l];
            if (prefix == suffix) return s.substr(0, l);
        }
        return "";
    }
};
```

#### Go

```go
func longestPrefix(s string) string {
	base := 131
	n := len(s)
	p := make([]int, n+10)
	h := make([]int, n+10)
	p[0] = 1
	for i, c := range s {
		p[i+1] = p[i] * base
		h[i+1] = h[i]*base + int(c)
	}
	for l := n - 1; l > 0; l-- {
		prefix, suffix := h[l], h[n]-h[n-l]*p[l]
		if prefix == suffix {
			return s[:l]
		}
	}
	return ""
}
```

#### TypeScript

```ts
function longestPrefix(s: string): string {
    const n = s.length;
    for (let i = n - 1; i >= 0; i--) {
        if (s.slice(0, i) === s.slice(n - i, n)) {
            return s.slice(0, i);
        }
    }
    return '';
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_prefix(s: String) -> String {
        let n = s.len();
        for i in (0..n).rev() {
            if s[0..i] == s[n - i..n] {
                return s[0..i].to_string();
            }
        }
        String::new()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thuật toán KMP

<!-- thinking:start -->

> **Tư duy**
>
> So sánh toàn bộ các lát cắt vẫn phải quét $\Theta(n)$ ký tự cho mỗi độ dài. Bảng $\textit{next}$ của KMP chính là độ dài prefix thực sự dài nhất bằng suffix. Thêm một sentinel vào cuối chuỗi rồi lấy $\textit{next}[-1]$ sẽ cho độ dài đó trong thời gian tuyến tính.

<!-- thinking:end -->

Theo đề bài, ta cần tìm happy prefix dài nhất của một chuỗi, tức prefix dài nhất đồng thời cũng là suffix. Ta có thể dùng thuật toán KMP để giải bài toán này.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPrefix(self, s: str) -> str:
        s += "#"
        n = len(s)
        next = [0] * n
        next[0] = -1
        i, j = 2, 0
        while i < n:
            if s[i - 1] == s[j]:
                j += 1
                next[i] = j
                i += 1
            elif j:
                j = next[j]
            else:
                next[i] = 0
                i += 1
        return s[: next[-1]]
```

#### Java

```java
class Solution {
    public String longestPrefix(String s) {
        s += "#";
        int n = s.length();
        int[] next = new int[n];
        next[0] = -1;
        for (int i = 2, j = 0; i < n;) {
            if (s.charAt(i - 1) == s.charAt(j)) {
                next[i++] = ++j;
            } else if (j > 0) {
                j = next[j];
            } else {
                next[i++] = 0;
            }
        }
        return s.substring(0, next[n - 1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string longestPrefix(string s) {
        s.push_back('#');
        int n = s.size();
        int next[n];
        next[0] = -1;
        next[1] = 0;
        for (int i = 2, j = 0; i < n;) {
            if (s[i - 1] == s[j]) {
                next[i++] = ++j;
            } else if (j > 0) {
                j = next[j];
            } else {
                next[i++] = 0;
            }
        }
        return s.substr(0, next[n - 1]);
    }
};
```

#### Go

```go
func longestPrefix(s string) string {
	s += "#"
	n := len(s)
	next := make([]int, n)
	next[0], next[1] = -1, 0
	for i, j := 2, 0; i < n; {
		if s[i-1] == s[j] {
			j++
			next[i] = j
			i++
		} else if j > 0 {
			j = next[j]
		} else {
			next[i] = 0
			i++
		}
	}
	return s[:next[n-1]]
}
```

#### TypeScript

```ts
function longestPrefix(s: string): string {
    s += '#';
    const n = s.length;
    const next: number[] = Array(n).fill(0);
    next[0] = -1;
    for (let i = 2, j = 0; i < n;) {
        if (s[i - 1] === s[j]) {
            next[i++] = ++j;
        } else if (j > 0) {
            j = next[j];
        } else {
            next[i++] = 0;
        }
    }
    return s.slice(0, next[n - 1]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
