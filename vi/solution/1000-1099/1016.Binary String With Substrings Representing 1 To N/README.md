---
comments: true
difficulty: Medium
rating: 1779
source: Weekly Contest 129 Q4
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1016. Binary String With Substrings Representing 1 To N](https://leetcode.com/problems/binary-string-with-substrings-representing-1-to-n)

[中文文档](/solution/1000-1099/1016.Binary%20String%20With%20Substrings%20Representing%201%20To%20N/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi nhị phân <code>s</code> và số nguyên dương <code>n</code>, trả về <code>true</code><em> nếu biểu diễn nhị phân của mọi số nguyên trong đoạn </em><code>[1, n]</code><em> đều là </em><strong>chuỗi con</strong><em> của </em><code>s</code><em>; ngược lại, trả về </em><code>false</code><em>.</em></p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp bên trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "0110", n = 3
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "0110", n = 4
<strong>Đầu ra:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhận xét then chốt

<!-- thinking:start -->

> **Tư duy**
>
> $n$ có thể lên đến $10^9$, nên không thể kiểm tra lần lượt biểu diễn nhị phân của mọi số trong $[1,n]$. Độ dài $s$ tối đa là $1000$, do đó chuỗi không thể chứa biểu diễn của hơn 1000 giá trị khác nhau; vì vậy nếu $n>1000$ thì đáp án chắc chắn là false.
>
> Nếu biểu diễn nhị phân của $x$ xuất hiện trong $s$, thì biểu diễn của $\lfloor x/2\rfloor$ (bỏ bit cuối) cũng xuất hiện. Vì vậy, chỉ cần kiểm tra nửa trên $[\lfloor n/2\rfloor+1,n]$.
>
> Khi $n\le 1000$, ta kiểm tra các số này bằng cách tìm chuỗi con thông thường.

<!-- thinking:end -->

Ta nhận thấy độ dài chuỗi $s$ không vượt quá $1000$, nên $s$ có thể biểu diễn tối đa $1000$ số nhị phân. Vì vậy, nếu $n \gt 1000$, chắc chắn $s$ không thể chứa biểu diễn nhị phân của mọi số nguyên trong đoạn $[1,.. n]$.

Ngoài ra, với số nguyên $x$, nếu biểu diễn nhị phân của $x$ là chuỗi con của $s$, thì biểu diễn nhị phân của $\lfloor x / 2 \rfloor$ cũng là chuỗi con của $s$. Do đó, ta chỉ cần kiểm tra biểu diễn nhị phân của các số nguyên trong đoạn $[\lfloor n / 2 \rfloor + 1,.. n]$ có phải là chuỗi con của $s$ hay không.

Độ phức tạp thời gian là $O(m^2 \times \log m)$ và độ phức tạp không gian là $O(\log n)$, trong đó $m$ là độ dài chuỗi $s$, còn $n$ là số nguyên dương được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def queryString(self, s: str, n: int) -> bool:
        if n > 1000:
            return False
        return all(bin(i)[2:] in s for i in range(n, n // 2, -1))
```

#### Java

```java
class Solution {
    public boolean queryString(String s, int n) {
        if (n > 1000) {
            return false;
        }
        for (int i = n; i > n / 2; i--) {
            if (!s.contains(Integer.toBinaryString(i))) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool queryString(string s, int n) {
        if (n > 1000) {
            return false;
        }
        for (int i = n; i > n / 2; --i) {
            string b = bitset<32>(i).to_string();
            b = b.substr(b.find_first_not_of('0'));
            if (s.find(b) == string::npos) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func queryString(s string, n int) bool {
	if n > 1000 {
		return false
	}
	for i := n; i > n/2; i-- {
		if !strings.Contains(s, strconv.FormatInt(int64(i), 2)) {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function queryString(s: string, n: number): boolean {
    if (n > 1000) {
        return false;
    }
    for (let i = n; i > n / 2; --i) {
        if (s.indexOf(i.toString(2)) === -1) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn query_string(s: String, n: i32) -> bool {
        if n > 1000 {
            return false;
        }
        for i in (n / 2 + 1..=n).rev() {
            if !s.contains(&format!("{:b}", i)) {
                return false;
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
