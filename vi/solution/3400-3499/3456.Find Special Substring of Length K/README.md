---
comments: true
difficulty: Easy
rating: 1244
source: Weekly Contest 437 Q1
tags:
    - String
---

<!-- problem:start -->

# [3456. Find Special Substring of Length K](https://leetcode.com/problems/find-special-substring-of-length-k)

[中文文档](/solution/3400-3499/3456.Find%20Special%20Substring%20of%20Length%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Hãy xác định xem trong <code>s</code> có tồn tại một <span data-keyword="substring-nonempty">chuỗi con</span> có độ dài <strong>chính xác</strong> bằng <code>k</code> và thỏa mãn các điều kiện sau hay không:</p>

<ol>
	<li>Chuỗi con chỉ gồm <strong>một ký tự khác nhau duy nhất</strong> (chẳng hạn <code>&quot;aaa&quot;</code> hoặc <code>&quot;bbb&quot;</code>).</li>
	<li>Nếu có một ký tự <strong>ngay trước</strong> chuỗi con, ký tự đó phải khác với ký tự trong chuỗi con.</li>
	<li>Nếu có một ký tự <strong>ngay sau</strong> chuỗi con, ký tự đó cũng phải khác với ký tự trong chuỗi con.</li>
</ol>

<p>Trả về <code>true</code> nếu tồn tại chuỗi con như vậy. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaabaaa&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con <code>s[4..6] == &quot;aaa&quot;</code> thỏa mãn các điều kiện.</p>

<ul>
	<li>Chuỗi con có độ dài bằng 3.</li>
	<li>Tất cả các ký tự đều giống nhau.</li>
	<li>Ký tự trước <code>&quot;aaa&quot;</code> là <code>&#39;b&#39;</code>, khác với <code>&#39;a&#39;</code>.</li>
	<li>Không có ký tự nào sau <code>&quot;aaa&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có chuỗi con nào có độ dài 2 chỉ gồm một ký tự và thỏa mãn các điều kiện.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi con đặc biệt là một đoạn gồm cùng một chữ cái, có độ dài chính xác bằng $k$, chứ không phải một phần của đoạn dài hơn. Vì $|s|\le 100$, chỉ cần quét theo các đoạn liên tiếp là đủ.
>
> Một cửa sổ có độ dài $k$ có thể chấp nhận một phần của đoạn các ký tự giống nhau dài hơn khi cả hai đầu đều khớp.
>
> Hai con trỏ giúp cô lập từng đoạn ký tự giống nhau và chỉ trả về kết quả đúng khi độ dài đoạn bằng $k$.

<!-- thinking:end -->

Về bản chất, bài toán yêu cầu chúng ta tìm từng đoạn gồm các ký tự giống nhau liên tiếp, sau đó xác định xem có chuỗi con nào có độ dài $k$ hay không. Nếu có, trả về $\textit{true}$; ngược lại, trả về $\textit{false}$.

Ta có thể dùng hai con trỏ $l$ và $r$ để duyệt chuỗi $s$. Khi $s[l] = s[r]$, ta di chuyển $r$ sang phải cho đến khi $s[r] \neq s[l]$. Khi đó, kiểm tra xem $r - l$ có bằng $k$ hay không. Nếu có, trả về $\textit{true}$; nếu không, di chuyển $l$ đến $r$ và tiếp tục duyệt.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasSpecialSubstring(self, s: str, k: int) -> bool:
        l, n = 0, len(s)
        while l < n:
            r = l
            while r < n and s[r] == s[l]:
                r += 1
            if r - l == k:
                return True
            l = r
        return False
```

#### Java

```java
class Solution {
    public boolean hasSpecialSubstring(String s, int k) {
        int n = s.length();
        for (int l = 0, cnt = 0; l < n;) {
            int r = l + 1;
            while (r < n && s.charAt(r) == s.charAt(l)) {
                ++r;
            }
            if (r - l == k) {
                return true;
            }
            l = r;
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasSpecialSubstring(string s, int k) {
        int n = s.length();
        for (int l = 0, cnt = 0; l < n;) {
            int r = l + 1;
            while (r < n && s[r] == s[l]) {
                ++r;
            }
            if (r - l == k) {
                return true;
            }
            l = r;
        }
        return false;
    }
};
```

#### Go

```go
func hasSpecialSubstring(s string, k int) bool {
	n := len(s)
	for l := 0; l < n; {
		r := l + 1
		for r < n && s[r] == s[l] {
			r++
		}
		if r-l == k {
			return true
		}
		l = r
	}
	return false
}
```

#### TypeScript

```ts
function hasSpecialSubstring(s: string, k: number): boolean {
    const n = s.length;
    for (let l = 0; l < n;) {
        let r = l + 1;
        while (r < n && s[r] === s[l]) {
            r++;
        }
        if (r - l === k) {
            return true;
        }
        l = r;
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
