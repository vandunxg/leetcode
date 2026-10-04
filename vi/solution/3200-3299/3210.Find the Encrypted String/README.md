---
comments: true
difficulty: Easy
rating: 1179
source: Weekly Contest 405 Q1
tags:
    - String
---

<!-- problem:start -->

# [3210. Find the Encrypted String](https://leetcode.com/problems/find-the-encrypted-string)

[中文文档](/solution/3200-3299/3210.Find%20the%20Encrypted%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> và một số nguyên <code>k</code>. Mã hóa chuỗi bằng thuật toán sau:</p>

<ul>
	<li>Với mỗi ký tự <code>c</code> trong <code>s</code>, thay <code>c</code> bằng ký tự thứ <code>k<sup>th</sup></code> sau <code>c</code> trong chuỗi (theo cách tuần hoàn).</li>
</ul>

<p>Trả về <em>chuỗi đã mã hóa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;dart&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;tdar&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, ký tự thứ 3<sup>rd</sup> sau <code>&#39;d&#39;</code> là <code>&#39;t&#39;</code>.</li>
	<li>Với <code>i = 1</code>, ký tự thứ 3<sup>rd</sup> sau <code>&#39;a&#39;</code> là <code>&#39;d&#39;</code>.</li>
	<li>Với <code>i = 2</code>, ký tự thứ 3<sup>rd</sup> sau <code>&#39;r&#39;</code> là <code>&#39;a&#39;</code>.</li>
	<li>Với <code>i = 3</code>, ký tự thứ 3<sup>rd</sup> sau <code>&#39;t&#39;</code> là <code>&#39;r&#39;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaa&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aaa&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì tất cả các ký tự đều giống nhau, chuỗi đã mã hóa cũng sẽ giống như vậy.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 100$, ta có thể thay mỗi ký tự bằng ký tự cách đó $k$ bước. $k$ có thể lên tới $10^4$, nên việc tiến từng $k$ bước sẽ khiến chuỗi quay vòng nhiều lần, trong khi phép dịch vòng chỉ phụ thuộc vào $k\bmod n$.
>
> Với mỗi $i$, ghi $s[(i+k)\bmod n]$ vào một chuỗi mới. Chỉ cần một lượt duyệt tuyến tính để tạo đáp án mà không phải xoay $k$ vòng.

<!-- thinking:end -->

Ta có thể sử dụng phương pháp mô phỏng. Với ký tự thứ $i^{th}$ của chuỗi, ta thay nó bằng ký tự ở vị trí $(i + k) \bmod n$ của chuỗi.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getEncryptedString(self, s: str, k: int) -> str:
        cs = list(s)
        n = len(s)
        for i in range(n):
            cs[i] = s[(i + k) % n]
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String getEncryptedString(String s, int k) {
        char[] cs = s.toCharArray();
        int n = cs.length;
        for (int i = 0; i < n; ++i) {
            cs[i] = s.charAt((i + k) % n);
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string getEncryptedString(string s, int k) {
        int n = s.length();
        string cs(n, ' ');
        for (int i = 0; i < n; ++i) {
            cs[i] = s[(i + k) % n];
        }
        return cs;
    }
};
```

#### Go

```go
func getEncryptedString(s string, k int) string {
	cs := []byte(s)
	for i := range s {
		cs[i] = s[(i+k)%len(s)]
	}
	return string(cs)
}
```

#### TypeScript

```ts
function getEncryptedString(s: string, k: number): string {
    const cs: string[] = [];
    const n = s.length;
    for (let i = 0; i < n; ++i) {
        cs[i] = s[(i + k) % n];
    }
    return cs.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
