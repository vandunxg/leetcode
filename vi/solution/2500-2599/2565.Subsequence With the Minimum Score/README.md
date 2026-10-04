---
comments: true
difficulty: Hard
rating: 2432
source: Weekly Contest 332 Q4
tags:
    - Two Pointers
    - String
    - Binary Search
---

<!-- problem:start -->

# [2565. Subsequence With the Minimum Score](https://leetcode.com/problems/subsequence-with-the-minimum-score)

[中文文档](/solution/2500-2599/2565.Subsequence%20With%20the%20Minimum%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>s</code> và <code>t</code>.</p>

<p>Bạn có thể xóa một số ký tự bất kỳ khỏi chuỗi <code>t</code>.</p>

<p>Điểm số của chuỗi là <code>0</code> nếu không xóa ký tự nào khỏi chuỗi <code>t</code>; ngược lại:</p>

<ul>
	<li>Gọi <code>left</code> là chỉ số nhỏ nhất trong số các ký tự bị xóa.</li>
	<li>Gọi <code>right</code> là chỉ số lớn nhất trong số các ký tự bị xóa.</li>
</ul>

<p>Khi đó, điểm số của chuỗi là <code>right - left + 1</code>.</p>

<p>Trả về <em>điểm số nhỏ nhất có thể để biến </em><code>t</code><em> thành một subsequence của </em><code>s</code><em>.</em></p>

<p>Một <strong>subsequence</strong> của một chuỗi là một chuỗi mới được tạo thành từ chuỗi ban đầu bằng cách xóa một số ký tự (có thể không xóa ký tự nào) nhưng không làm thay đổi thứ tự tương đối của các ký tự còn lại. (Ví dụ, <code>&quot;ace&quot;</code> là một subsequence của <code>&quot;<u>a</u>b<u>c</u>d<u>e</u>&quot;</code>, còn <code>&quot;aec&quot;</code> thì không).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abacaba&quot;, t = &quot;bzaa&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Trong ví dụ này, ta xóa ký tự &quot;z&quot; tại chỉ số 1 (đánh chỉ số từ 0).
Chuỗi t trở thành &quot;baa&quot;, là một subsequence của chuỗi &quot;abacaba&quot;, và điểm số là 1 - 1 + 1 = 1.
Có thể chứng minh rằng 1 là điểm số nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cde&quot;, t = &quot;xyz&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong ví dụ này, ta xóa các ký tự &quot;x&quot;, &quot;y&quot; và &quot;z&quot; tại các chỉ số 0, 1 và 2 (đánh chỉ số từ 0).
Chuỗi t trở thành &quot;&quot;, là một subsequence của chuỗi &quot;cde&quot;, và điểm số là 2 - 0 + 1 = 3.
Có thể chứng minh rằng 3 là điểm số nhỏ nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý tiền tố và hậu tố + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Xóa một đoạn của $t$ (có thể là đoạn rỗng hoặc toàn bộ chuỗi) để phần tiền tố và hậu tố còn lại là hai subsequence không giao nhau của $s$, đồng thời tối thiểu hóa độ dài đoạn bị xóa. Nếu thử mọi đoạn thì độ phức tạp lớn hơn bậc hai.
>
> Đoạn bị xóa càng dài thì điều kiện càng dễ thỏa mãn, vì vậy ta tìm kiếm nhị phân theo độ dài $x$. $f[j]$ là chỉ số nhỏ nhất trong $s$ được dùng khi ghép $t[0..j]$; $g[j]$ là chỉ số lớn nhất được dùng cho $t[j..]$. Sau khi xóa $[k,k+x)$, hai phần tương thích với nhau khi và chỉ khi $f[k-1]<g[k+x]$.

<!-- thinking:end -->

Theo đề bài, ta biết rằng đoạn các chỉ số cần xóa là `[left, right]`. Cách tối ưu là xóa tất cả các ký tự trong đoạn `[left, right]`. Nói cách khác, ta cần xóa một chuỗi con khỏi chuỗi $t$, sao cho phần tiền tố còn lại của chuỗi $t$ có thể khớp với phần tiền tố của chuỗi $s$, phần hậu tố còn lại của chuỗi $t$ có thể khớp với phần hậu tố của chuỗi $s$, đồng thời phần tiền tố và hậu tố của chuỗi $s$ không giao nhau. Lưu ý rằng việc khớp ở đây là khớp subsequence.

Do đó, ta có thể tiền xử lý để thu được các mảng $f$ và $g$, trong đó $f[i]$ biểu diễn số ký tự nhỏ nhất trong tiền tố $t[0,..i]$ của chuỗi $t$ khớp với các ký tự đầu tiên $[0,..f[i]]$ của chuỗi $s$; tương tự, $g[i]$ biểu diễn số ký tự lớn nhất trong hậu tố $t[i,..n-1]$ của chuỗi $t$ khớp với các ký tự cuối cùng $[g[i],..n-1]$ của chuỗi $s$.

Độ dài các ký tự bị xóa có tính đơn điệu. Nếu điều kiện được thỏa mãn sau khi xóa một chuỗi có độ dài $x$, thì chắc chắn điều kiện cũng được thỏa mãn sau khi xóa một chuỗi có độ dài $x+1$. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm độ dài nhỏ nhất thỏa mãn điều kiện.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $t$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumScore(self, s: str, t: str) -> int:
        def check(x):
            for k in range(n):
                i, j = k - 1, k + x
                l = f[i] if i >= 0 else -1
                r = g[j] if j < n else m + 1
                if l < r:
                    return True
            return False

        m, n = len(s), len(t)
        f = [inf] * n
        g = [-1] * n
        i, j = 0, 0
        while i < m and j < n:
            if s[i] == t[j]:
                f[j] = i
                j += 1
            i += 1
        i, j = m - 1, n - 1
        while i >= 0 and j >= 0:
            if s[i] == t[j]:
                g[j] = i
                j -= 1
            i -= 1

        return bisect_left(range(n + 1), True, key=check)
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[] f;
    private int[] g;

    public int minimumScore(String s, String t) {
        m = s.length();
        n = t.length();
        f = new int[n];
        g = new int[n];
        for (int i = 0; i < n; ++i) {
            f[i] = 1 << 30;
            g[i] = -1;
        }
        for (int i = 0, j = 0; i < m && j < n; ++i) {
            if (s.charAt(i) == t.charAt(j)) {
                f[j] = i;
                ++j;
            }
        }
        for (int i = m - 1, j = n - 1; i >= 0 && j >= 0; --i) {
            if (s.charAt(i) == t.charAt(j)) {
                g[j] = i;
                --j;
            }
        }
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private boolean check(int len) {
        for (int k = 0; k < n; ++k) {
            int i = k - 1, j = k + len;
            int l = i >= 0 ? f[i] : -1;
            int r = j < n ? g[j] : m + 1;
            if (l < r) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumScore(string s, string t) {
        int m = s.size(), n = t.size();
        vector<int> f(n, 1e6);
        vector<int> g(n, -1);
        for (int i = 0, j = 0; i < m && j < n; ++i) {
            if (s[i] == t[j]) {
                f[j] = i;
                ++j;
            }
        }
        for (int i = m - 1, j = n - 1; i >= 0 && j >= 0; --i) {
            if (s[i] == t[j]) {
                g[j] = i;
                --j;
            }
        }

        auto check = [&](int len) {
            for (int k = 0; k < n; ++k) {
                int i = k - 1, j = k + len;
                int l = i >= 0 ? f[i] : -1;
                int r = j < n ? g[j] : m + 1;
                if (l < r) {
                    return true;
                }
            }
            return false;
        };

        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func minimumScore(s string, t string) int {
	m, n := len(s), len(t)
	f := make([]int, n)
	g := make([]int, n)
	for i := range f {
		f[i] = 1 << 30
		g[i] = -1
	}
	for i, j := 0, 0; i < m && j < n; i++ {
		if s[i] == t[j] {
			f[j] = i
			j++
		}
	}
	for i, j := m-1, n-1; i >= 0 && j >= 0; i-- {
		if s[i] == t[j] {
			g[j] = i
			j--
		}
	}
	return sort.Search(n+1, func(x int) bool {
		for k := 0; k < n; k++ {
			i, j := k-1, k+x
			l, r := -1, m+1
			if i >= 0 {
				l = f[i]
			}
			if j < n {
				r = g[j]
			}
			if l < r {
				return true
			}
		}
		return false
	})
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
