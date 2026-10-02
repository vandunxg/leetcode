---
comments: true
difficulty: Easy
rating: 1397
source: Weekly Contest 139 Q1
tags:
    - Math
    - String
    - Greatest Common Divisor
    - Euclidean Algorithm
---

<!-- problem:start -->

# [1071. Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings)

[中文文档](/solution/1000-1099/1071.Greatest%20Common%20Divisor%20of%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Với hai chuỗi <code>s</code> và <code>t</code>, ta nói &quot;<code>t</code> là ước của <code>s</code>&quot; khi và chỉ khi <code>s = t + t + t + ... + t + t</code> (tức là <code>t</code> được nối với chính nó một hoặc nhiều lần).</p>

<p>Cho hai chuỗi <code>str1</code> và <code>str2</code>, hãy trả về <em>chuỗi lớn nhất </em><code>x</code><em> sao cho </em><code>x</code><em> là ước của cả </em><code>str1</code><em> và </em><code>str2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">str1 = &quot;ABCABC&quot;, str2 = &quot;ABC&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;ABC&quot;</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">str1 = &quot;ABABAB&quot;, str2 = &quot;ABAB&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;AB&quot;</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">str1 = &quot;LEET&quot;, str2 = &quot;CODE&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">str1 = &quot;AAAAAB&quot;, str2 = &quot;AAA&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span>​​​​​​​</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= str1.length, str2.length &lt;= 1000</code></li>
	<li><code>str1</code> và <code>str2</code> chỉ gồm các chữ cái tiếng Anh viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi ước chung phải ghép lặp vừa khít cả hai chuỗi, nên độ dài của nó phải là ước của độ dài cả hai chuỗi. Với $m,n\le 1000$, chỉ cần thử các tiền tố theo độ dài giảm dần, bắt đầu từ độ dài chuỗi ngắn hơn.
>
> Với mỗi ứng viên $t=\textit{str1}[:i]$, ta nối lặp nó đến khi đạt độ dài cần thiết rồi so sánh.
>
> Ứng viên $t$ đầu tiên ghép lặp vừa khít cả hai chuỗi là chuỗi dài nhất; nếu không có ứng viên nào, đáp án là chuỗi rỗng.

<!-- thinking:end -->

Duyệt các tiền tố ứng viên $t$ theo độ dài giảm dần, bắt đầu từ độ dài chuỗi ngắn hơn, rồi kiểm tra xem nối lặp $t$ có tạo được $\textit{str1}$ và $\textit{str2}$ hay không. Ứng viên $t$ hợp lệ đầu tiên chính là chuỗi gcd dài nhất.

Độ phức tạp thời gian là $O((m + n) \times \min(m, n))$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ là độ dài của hai chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        def check(a, b):
            c = ""
            while len(c) < len(b):
                c += a
            return c == b

        for i in range(min(len(str1), len(str2)), 0, -1):
            t = str1[:i]
            if check(t, str1) and check(t, str2):
                return t
        return ''
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Cách duyệt vẫn phải thử nhiều độ dài. Nếu tồn tại chuỗi ước chung thì $s_1+s_2=s_2+s_1$, và độ dài lớn nhất là $\gcd(|s_1|,|s_2|)$.
>
> Nếu hai cách nối không bằng nhau, ta trả về chuỗi rỗng; nếu bằng nhau, trả về tiền tố của $s_1$ có độ dài bằng gcd đó.

<!-- thinking:end -->

Nếu tồn tại chuỗi gcd thì $s_1+s_2=s_2+s_1$. Khi đó, độ dài của chuỗi gcd dài nhất là $\gcd(|s_1|,|s_2|)$.

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ là độ dài của hai chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        if str1 + str2 != str2 + str1:
            return ''
        n = gcd(len(str1), len(str2))
        return str1[:n]
```

#### Java

```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        if (!(str1 + str2).equals(str2 + str1)) {
            return "";
        }
        int len = gcd(str1.length(), str2.length());
        return str1.substring(0, len);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        if (str1 + str2 != str2 + str1) return "";
        int n = __gcd(str1.size(), str2.size());
        return str1.substr(0, n);
    }
};
```

#### Go

```go
func gcdOfStrings(str1 string, str2 string) string {
	if str1+str2 != str2+str1 {
		return ""
	}
	n := gcd(len(str1), len(str2))
	return str1[:n]
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### Rust

```rust
impl Solution {
    pub fn gcd_of_strings(str1: String, str2: String) -> String {
        if str1.clone() + &str2 != str2.clone() + &str1 {
            return String::from("");
        }
        fn gcd(a: usize, b: usize) -> usize {
            if b == 0 {
                return a;
            }
            gcd(b, a % b)
        }

        let (m, n) = (str1.len().max(str2.len()), str1.len().min(str2.len()));
        str1[..gcd(m, n)].to_string()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
