---
comments: true
difficulty: Medium
rating: 1989
source: Weekly Contest 489 Q3
tags:
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3844. Longest Almost-Palindromic Substring](https://leetcode.com/problems/longest-almost-palindromic-substring)

[中文文档](/solution/3800-3899/3844.Longest%20Almost-Palindromic%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Một <span data-keyword="substring-nonempty">chuỗi con</span> là <strong>almost-palindromic</strong> nếu sau khi xóa <strong>đúng</strong> một ký tự, nó trở thành một <span data-keyword="palindrome-string">palindrome</span>.</p>

<p>Trả về một số nguyên biểu thị độ dài của <strong>dài nhất</strong> <strong>almost-palindromic</strong> chuỗi con trong <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abca&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn chuỗi con <code>&quot;<u><strong>abca</strong></u>&quot;</code>.</p>

<ul>
	<li>Xóa <code>&quot;ab<u><strong>c</strong></u>a&quot;</code>.</li>
	<li>Chuỗi trở thành <code>&quot;aba&quot;</code>, đây là một palindrome.</li>
	<li>Do đó, <code>&quot;abca&quot;</code> là almost-palindromic.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn chuỗi con <code>&quot;<u><strong>abba</strong></u>&quot;</code>.</p>

<ul>
	<li>Xóa <code>&quot;a<u><strong>b</strong></u>ba&quot;</code>.</li>
	<li>Chuỗi trở thành <code>&quot;aba&quot;</code>, đây là một palindrome.</li>
	<li>Do đó, <code>&quot;abba&quot;</code> là almost-palindromic.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zzabba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn chuỗi con <code>&quot;z<u><strong>zabba</strong></u>&quot;</code>.</p>

<ul>
	<li>Xóa <code>&quot;<u><strong>z</strong></u>abba&quot;</code>.</li>
	<li>Chuỗi trở thành <code>&quot;abba&quot;</code>, đây là một palindrome.</li>
	<li>Do đó, <code>&quot;zabba&quot;</code> là almost-palindromic.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 2500</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê vị trí tâm của palindrome

<!-- thinking:start -->

> **Tư duy**
>
> Một almost-palindrome trở thành palindrome sau khi xóa đúng một ký tự. Với $n \le 2500$, việc kiểm tra tất cả chuỗi con theo thời gian bậc ba là quá chậm.
>
> Palindrome thông thường mở rộng từ một tâm; với almost-palindrome, ta bỏ qua chỉ số bên trái hoặc bên phải tại lần không khớp đầu tiên rồi tiếp tục mở rộng.
>
> Liệt kê các tâm $(i,i)$ và $(i,i+1)$, mở rộng đến lần không khớp đầu tiên, sau đó bỏ qua bên trái hoặc bên phải thêm một lần, rồi lấy đoạn bao phủ dài nhất (không quá $n$).
>
> Mỗi tâm mất $O(n)$, nên tổng thời gian là $O(n^2)$.

<!-- thinking:end -->

Ta ký hiệu độ dài của chuỗi $s$ là $n$.

Ta định nghĩa một hàm $f(l, r)$, biểu thị việc tính độ dài của chuỗi con almost-palindromic dài nhất có thể thu được khi bắt đầu từ $l$ và $r$, mở rộng về cả hai phía của chuỗi và xóa một ký tự.

Trong hàm $f(l, r)$, trước tiên ta mở rộng về cả hai phía cho đến khi các điều kiện $l \geq 0$, $r \lt n$ và $s[l] = s[r]$ không còn đồng thời đúng. Tại thời điểm này, ta có thể bỏ qua $l$ hoặc $r$. Nếu bỏ qua $l$, ta tiếp tục mở rộng từ $(l - 1, r)$ về cả hai phía; nếu bỏ qua $r$, ta tiếp tục mở rộng từ $(l, r + 1)$ về cả hai phía. Ta tính độ dài chuỗi con almost-palindromic dài nhất trong cả hai trường hợp và lấy giá trị lớn hơn. Lưu ý rằng độ dài chuỗi con almost-palindromic dài nhất không thể vượt quá $n$.

Cuối cùng, ta liệt kê vị trí tâm $i$ của palindrome, tính độ dài chuỗi con almost-palindromic dài nhất thu được khi bắt đầu từ $(i, i)$ và $(i, i + 1)$, mở rộng về cả hai phía và xóa một ký tự, rồi lấy giá trị lớn nhất trong các trường hợp đó.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def almostPalindromic(self, s: str) -> int:
        def f(l: int, r: int) -> int:
            while l >= 0 and r < n and s[l] == s[r]:
                l -= 1
                r += 1
            l1, r1 = l - 1, r
            l2, r2 = l, r + 1
            while l1 >= 0 and r1 < n and s[l1] == s[r1]:
                l1 -= 1
                r1 += 1
            while l2 >= 0 and r2 < n and s[l2] == s[r2]:
                l2 -= 1
                r2 += 1
            return min(n, max(r1 - l1 - 1, r2 - l2 - 1))

        n = len(s)
        ans = 0
        for i in range(n):
            a = f(i, i)
            b = f(i, i + 1)
            ans = max(ans, a, b)
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private char[] s;

    public int almostPalindromic(String s) {
        n = s.length();
        this.s = s.toCharArray();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            ans = Math.max(ans, f(i, i));
            ans = Math.max(ans, f(i, i + 1));
        }
        return ans;
    }

    private int f(int l, int r) {
        while (l >= 0 && r < n && s[l] == s[r]) {
            l--;
            r++;
        }

        int l1 = l - 1, r1 = r;
        int l2 = l, r2 = r + 1;

        while (l1 >= 0 && r1 < n && s[l1] == s[r1]) {
            l1--;
            r1++;
        }
        while (l2 >= 0 && r2 < n && s[l2] == s[r2]) {
            l2--;
            r2++;
        }

        return Math.min(n, Math.max(r1 - l1 - 1, r2 - l2 - 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int almostPalindromic(string s) {
        int n = s.size();

        auto f = [&](int l, int r) {
            while (l >= 0 && r < n && s[l] == s[r]) {
                --l;
                ++r;
            }

            int l1 = l - 1, r1 = r;
            int l2 = l, r2 = r + 1;

            while (l1 >= 0 && r1 < n && s[l1] == s[r1]) {
                --l1;
                ++r1;
            }
            while (l2 >= 0 && r2 < n && s[l2] == s[r2]) {
                --l2;
                ++r2;
            }

            return min(n, max(r1 - l1 - 1, r2 - l2 - 1));
        };

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, f(i, i));
            ans = max(ans, f(i, i + 1));
        }

        return ans;
    }
};
```

#### Go

```go
func almostPalindromic(s string) int {
	n := len(s)

	f := func(l, r int) int {
		for l >= 0 && r < n && s[l] == s[r] {
			l--
			r++
		}

		l1, r1 := l-1, r
		l2, r2 := l, r+1

		for l1 >= 0 && r1 < n && s[l1] == s[r1] {
			l1--
			r1++
		}
		for l2 >= 0 && r2 < n && s[l2] == s[r2] {
			l2--
			r2++
		}
		return min(n, max(r1-l1-1, r2-l2-1))
	}

	ans := 0
	for i := 0; i < n; i++ {
		ans = max(ans, f(i, i), f(i, i+1))
	}
	return ans
}
```

#### TypeScript

```ts
function almostPalindromic(s: string): number {
    const n = s.length;

    const f = (l: number, r: number): number => {
        while (l >= 0 && r < n && s[l] === s[r]) {
            l--;
            r++;
        }

        let l1 = l - 1,
            r1 = r;
        let l2 = l,
            r2 = r + 1;

        while (l1 >= 0 && r1 < n && s[l1] === s[r1]) {
            l1--;
            r1++;
        }
        while (l2 >= 0 && r2 < n && s[l2] === s[r2]) {
            l2--;
            r2++;
        }

        return Math.min(n, Math.max(r1 - l1 - 1, r2 - l2 - 1));
    };

    let ans = 0;
    for (let i = 0; i < n; i++) {
        ans = Math.max(ans, f(i, i));
        ans = Math.max(ans, f(i, i + 1));
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
