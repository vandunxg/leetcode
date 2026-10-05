---
comments: true
difficulty: Medium
rating: 1414
source: Biweekly Contest 168 Q1
tags:
    - Two Pointers
    - Binary Search
    - Enumeration
---

<!-- problem:start -->

# [3722. Lexicographically Smallest String After Reverse](https://leetcode.com/problems/lexicographically-smallest-string-after-reverse)

[中文文档](/solution/3700-3799/3722.Lexicographically%20Smallest%20String%20After%20Reverse/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> có độ dài <code>n</code>, chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn phải thực hiện <strong>đúng</strong> một thao tác bằng cách chọn một số nguyên <code>k</code> sao cho <code>1 &lt;= k &lt;= n</code> và thực hiện một trong hai thao tác sau:</p>

<ul>
	<li>đảo ngược <strong>đầu tiên</strong> <code>k</code> ký tự của <code>s</code>, hoặc</li>
	<li>đảo ngược <strong>cuối cùng</strong> <code>k</code> ký tự của <code>s</code>.</li>
</ul>

<p>Trả về chuỗi <strong><span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span></strong> có thể thu được sau khi thực hiện <strong>đúng</strong> một thao tác như trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;dcab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;acdb&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>k = 3</code>, đảo ngược 3 ký tự đầu tiên.</li>
	<li>Đảo ngược <code>&quot;dca&quot;</code> thành <code>&quot;acd&quot;</code>, thu được chuỗi <code>s = &quot;acdb&quot;</code>, đây là chuỗi nhỏ nhất theo thứ tự từ điển có thể đạt được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aabb&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>k = 3</code>, đảo ngược 3 ký tự cuối cùng.</li>
	<li>Đảo ngược <code>&quot;bba&quot;</code> thành <code>&quot;abb&quot;</code>, nên chuỗi thu được là <code>&quot;aabb&quot;</code>, đây là chuỗi nhỏ nhất theo thứ tự từ điển có thể đạt được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zxy&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;xzy&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>k = 2</code>, đảo ngược 2 ký tự đầu tiên.</li>
	<li>Đảo ngược <code>&quot;zx&quot;</code> thành <code>&quot;xz&quot;</code>, nên chuỗi thu được là <code>&quot;xzy&quot;</code>, đây là chuỗi nhỏ nhất theo thứ tự từ điển có thể đạt được.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải đảo ngược đúng một tiền tố hoặc hậu tố, nên chỉ có $n$ lựa chọn cho $k$. Chỉ cần duyệt qua từng $k$, tạo cả hai ứng viên rồi chọn chuỗi nhỏ nhất theo thứ tự từ điển.

<!-- thinking:end -->

Ta có thể liệt kê tất cả các giá trị có thể có của $k$ ($1 \leq k \leq n$). Với mỗi $k$, ta tính chuỗi thu được khi đảo ngược $k$ ký tự đầu tiên và chuỗi thu được khi đảo ngược $k$ ký tự cuối cùng, sau đó chọn chuỗi nhỏ nhất theo thứ tự từ điển trong số chúng làm đáp án cuối cùng.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lexSmallest(self, s: str) -> str:
        ans = s
        for k in range(1, len(s) + 1):
            t1 = s[:k][::-1] + s[k:]
            t2 = s[:-k] + s[-k:][::-1]
            ans = min(ans, t1, t2)
        return ans
```

#### Java

```java
class Solution {
    public String lexSmallest(String s) {
        String ans = s;
        int n = s.length();
        for (int k = 1; k <= n; ++k) {
            String t1 = new StringBuilder(s.substring(0, k)).reverse().toString() + s.substring(k);
            String t2 = s.substring(0, n - k)
                + new StringBuilder(s.substring(n - k)).reverse().toString();
            if (t1.compareTo(ans) < 0) {
                ans = t1;
            }
            if (t2.compareTo(ans) < 0) {
                ans = t2;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string lexSmallest(string s) {
        string ans = s;
        int n = s.size();
        for (int k = 1; k <= n; ++k) {
            string t1 = s.substr(0, k);
            reverse(t1.begin(), t1.end());
            t1 += s.substr(k);

            string t2 = s.substr(0, n - k);
            string suffix = s.substr(n - k);
            reverse(suffix.begin(), suffix.end());
            t2 += suffix;

            ans = min({ans, t1, t2});
        }
        return ans;
    }
};
```

#### Go

```go
func lexSmallest(s string) string {
	ans := s
	n := len(s)
	for k := 1; k <= n; k++ {
		t1r := []rune(s[:k])
		slices.Reverse(t1r)
		t1 := string(t1r) + s[k:]

		t2r := []rune(s[n-k:])
		slices.Reverse(t2r)
		t2 := s[:n-k] + string(t2r)

		ans = min(ans, t1, t2)
	}
	return ans
}
```

#### TypeScript

```ts
function lexSmallest(s: string): string {
    let ans = s;
    const n = s.length;
    for (let k = 1; k <= n; ++k) {
        const t1 = reverse(s.slice(0, k)) + s.slice(k);
        const t2 = s.slice(0, n - k) + reverse(s.slice(n - k));
        ans = [ans, t1, t2].sort()[0];
    }
    return ans;
}

function reverse(s: string): string {
    return s.split('').reverse().join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
