---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii)

[中文文档](/solution/0500-0599/0541.Reverse%20String%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>, hãy đảo ngược <code>k</code> ký tự đầu tiên trong mỗi nhóm <code>2k</code> ký tự, tính từ đầu chuỗi.</p>

<p>Nếu còn ít hơn <code>k</code> ký tự, hãy đảo ngược tất cả ký tự còn lại. Nếu số ký tự còn lại ít hơn <code>2k</code> nhưng lớn hơn hoặc bằng <code>k</code>, hãy đảo ngược <code>k</code> ký tự đầu tiên và giữ nguyên các ký tự còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "abcdefg", k = 2
<strong>Đầu ra:</strong> "bacdfeg"
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "abcd", k = 2
<strong>Đầu ra:</strong> "bacd"
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Trong mỗi nhóm $2k$ ký tự, chỉ đảo ngược $k$ ký tự đầu tiên. Chỉ cần duyệt các nhóm một lượt.
>
> Chuyển chuỗi thành mảng ký tự, rồi với bước nhảy $2k$, đảo ngược đoạn có độ dài $k$. Nếu nhóm cuối ngắn hơn, chỉ đảo ngược phần còn lại. Cuối cùng nối các ký tự lại thành chuỗi.

<!-- thinking:end -->

Ta duyệt chuỗi $\textit{s}$ theo từng nhóm $\textit{2k}$ ký tự, rồi dùng kỹ thuật two pointers để đảo ngược $\textit{k}$ ký tự đầu tiên trong mỗi nhóm.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{s}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        cs = list(s)
        for i in range(0, len(cs), 2 * k):
            cs[i : i + k] = reversed(cs[i : i + k])
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] cs = s.toCharArray();
        int n = cs.length;
        for (int i = 0; i < n; i += k * 2) {
            for (int l = i, r = Math.min(i + k - 1, n - 1); l < r; ++l, --r) {
                char t = cs[l];
                cs[l] = cs[r];
                cs[r] = t;
            }
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        int n = s.size();
        for (int i = 0; i < n; i += 2 * k) {
            reverse(s.begin() + i, s.begin() + min(i + k, n));
        }
        return s;
    }
};
```

#### Go

```go
func reverseStr(s string, k int) string {
	cs := []byte(s)
	n := len(cs)
	for i := 0; i < n; i += 2 * k {
		for l, r := i, min(i+k-1, n-1); l < r; l, r = l+1, r-1 {
			cs[l], cs[r] = cs[r], cs[l]
		}
	}
	return string(cs)
}
```

#### TypeScript

```ts
function reverseStr(s: string, k: number): string {
    const n = s.length;
    const cs = s.split('');
    for (let i = 0; i < n; i += 2 * k) {
        for (let l = i, r = Math.min(i + k - 1, n - 1); l < r; l++, r--) {
            [cs[l], cs[r]] = [cs[r], cs[l]];
        }
    }
    return cs.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
