---
comments: true
difficulty: Medium
rating: 1292
source: Biweekly Contest 138 Q2
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [3271. Hash Divided String](https://leetcode.com/problems/hash-divided-string)

[中文文档](/solution/3200-3299/3271.Hash%20Divided%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> có độ dài <code>n</code> và một số nguyên <code>k</code>, trong đó <code>n</code> là <strong>bội số</strong> của <code>k</code>. Nhiệm vụ của bạn là băm chuỗi <code>s</code> thành một chuỗi mới có tên là <code>result</code>, với độ dài bằng <code>n / k</code>.</p>

<p>Đầu tiên, chia <code>s</code> thành <code>n / k</code> <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong>, mỗi chuỗi có độ dài <code>k</code>. Sau đó, khởi tạo <code>result</code> là một chuỗi <strong>rỗng</strong>.</p>

<p>Với mỗi <strong>chuỗi con</strong> theo thứ tự từ đầu chuỗi:</p>

<ul>
	<li><strong>Giá trị băm</strong> của một ký tự là chỉ số của ký tự đó trong <strong>bảng chữ cái tiếng Anh</strong><!-- notionvc: 4b67483a-fa95-40b6-870d-2eacd9bc18d8 --> (ví dụ: <code>&#39;a&#39; &rarr;<!-- notionvc: d3f8e4c2-23cd-41ad-a14b-101dfe4c5aba --> 0</code>, <code>&#39;b&#39; &rarr;<!-- notionvc: d3f8e4c2-23cd-41ad-a14b-101dfe4c5aba --> 1</code>, ..., <code>&#39;z&#39; &rarr;<!-- notionvc: d3f8e4c2-23cd-41ad-a14b-101dfe4c5aba --> 25</code>).</li>
	<li>Tính <em>tổng</em> của tất cả <strong>giá trị băm</strong> của các ký tự trong chuỗi con.</li>
	<li>Tìm phần dư của tổng này khi chia cho 26, gọi là <code>hashedChar</code>.</li>
	<li>Xác định ký tự trong bảng chữ cái tiếng Anh viết thường tương ứng với <code>hashedChar</code>.</li>
	<li>Thêm ký tự đó vào cuối <code>result</code>.</li>
</ul>

<p>Trả về <code>result</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcd&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;bf&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con thứ nhất: <code>&quot;ab&quot;</code>, <code>0 + 1 = 1</code>, <code>1 % 26 = 1</code>, <code>result[0] = &#39;b&#39;</code>.</p>

<p>Chuỗi con thứ hai: <code>&quot;cd&quot;</code>, <code>2 + 3 = 5</code>, <code>5 % 26 = 5</code>, <code>result[1] = &#39;f&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;mxz&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;i&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con duy nhất: <code>&quot;mxz&quot;</code>, <code>12 + 23 + 25 = 60</code>, <code>60 % 26 = 8</code>, <code>result[0] = &#39;i&#39;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 100</code></li>
	<li><code>k &lt;= s.length &lt;= 1000</code></li>
	<li><code>s.length</code> chia hết cho <code>k</code>.</li>
	<li><code>s</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi khối gồm $k$ ký tự trở thành một chữ cái: $\sum (s_j-\texttt{a}) \bmod 26$. Vì $n\le 1000$ và $k$ chia hết $n$, chỉ cần duyệt trực tiếp từng khối.
>
> Tăng chỉ số theo bước $k$, tính tổng mã trong khối, lấy modulo $26$, rồi chuyển ngược thành ký tự. Chỉ cần một lượt duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể mô phỏng quá trình theo các bước được mô tả trong đề bài.

Duyệt chuỗi $s$, mỗi lần lấy $k$ ký tự và tính tổng các giá trị băm của chúng, gọi là $t$. Sau đó, lấy $t$ modulo $26$ để tìm ký tự tương ứng và thêm ký tự đó vào cuối chuỗi kết quả.

Cuối cùng, trả về chuỗi kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Không tính phần bộ nhớ dùng cho chuỗi kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stringHash(self, s: str, k: int) -> str:
        ans = []
        for i in range(0, len(s), k):
            t = 0
            for j in range(i, i + k):
                t += ord(s[j]) - ord("a")
            hashedChar = t % 26
            ans.append(chr(ord("a") + hashedChar))
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String stringHash(String s, int k) {
        StringBuilder ans = new StringBuilder();
        int n = s.length();
        for (int i = 0; i < n; i += k) {
            int t = 0;
            for (int j = i; j < i + k; ++j) {
                t += s.charAt(j) - 'a';
            }
            int hashedChar = t % 26;
            ans.append((char) ('a' + hashedChar));
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string stringHash(string s, int k) {
        string ans;
        int n = s.length();
        for (int i = 0; i < n; i += k) {
            int t = 0;
            for (int j = i; j < i + k; ++j) {
                t += s[j] - 'a';
            }
            int hashedChar = t % 26;
            ans += ('a' + hashedChar);
        }
        return ans;
    }
};
```

#### Go

```go
func stringHash(s string, k int) string {
	n := len(s)
	ans := make([]byte, 0, n/k)

	for i := 0; i < n; i += k {
		t := 0
		for j := i; j < i+k; j++ {
			t += int(s[j] - 'a')
		}
		hashedChar := t % 26
		ans = append(ans, 'a'+byte(hashedChar))
	}

	return string(ans)
}
```

#### TypeScript

```ts
function stringHash(s: string, k: number): string {
    const ans: string[] = [];
    const n: number = s.length;

    for (let i = 0; i < n; i += k) {
        let t: number = 0;
        for (let j = i; j < i + k; j++) {
            t += s.charCodeAt(j) - 97;
        }
        const hashedChar: number = t % 26;
        ans.push(String.fromCharCode(97 + hashedChar));
    }

    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
