---
comments: true
difficulty: Medium
rating: 1739
source: Biweekly Contest 166 Q3
tags:
    - Hash Table
    - String
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3694. Distinct Points Reachable After Substring Removal](https://leetcode.com/problems/distinct-points-reachable-after-substring-removal)

[中文文档](/solution/3600-3699/3694.Distinct%20Points%20Reachable%20After%20Substring%20Removal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các ký tự <code>&#39;U&#39;</code>, <code>&#39;D&#39;</code>, <code>&#39;L&#39;</code> và <code>&#39;R&#39;</code>, biểu diễn các bước di chuyển trên một lưới Descartes 2D vô hạn.</p>

<ul>
    <li><code>&#39;U&#39;</code>: Di chuyển từ <code>(x, y)</code> đến <code>(x, y + 1)</code>.</li>
    <li><code>&#39;D&#39;</code>: Di chuyển từ <code>(x, y)</code> đến <code>(x, y - 1)</code>.</li>
    <li><code>&#39;L&#39;</code>: Di chuyển từ <code>(x, y)</code> đến <code>(x - 1, y)</code>.</li>
    <li><code>&#39;R&#39;</code>: Di chuyển từ <code>(x, y)</code> đến <code>(x + 1, y)</code>.</li>
</ul>

<p>Bạn cũng được cho một số nguyên dương <code>k</code>.</p>

<p>Bạn <strong>phải</strong> chọn và xóa <strong>chính xác một</strong> chuỗi con liên tiếp có độ dài <code>k</code> khỏi <code>s</code>. Sau đó, bắt đầu từ tọa độ <code>(0, 0)</code> và thực hiện các bước di chuyển còn lại theo thứ tự.</p>

<p>Trả về một số nguyên biểu thị số lượng tọa độ cuối cùng <strong>khác nhau</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;LUL&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi xóa một chuỗi con có độ dài 1, <code>s</code> có thể là <code>&quot;UL&quot;</code>, <code>&quot;LL&quot;</code> hoặc <code>&quot;LU&quot;</code>. Thực hiện các bước di chuyển này, tọa độ cuối cùng lần lượt là <code>(-1, 1)</code>, <code>(-2, 0)</code> và <code>(-1, 1)</code>. Có hai điểm khác nhau là <code>(-1, 1)</code> và <code>(-2, 0)</code>, nên đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;UDLR&quot;, k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi xóa một chuỗi con có độ dài 4, <code>s</code> chỉ có thể trở thành chuỗi rỗng. Tọa độ cuối cùng sẽ là <code>(0, 0)</code>. Chỉ có một điểm khác nhau là <code>(0, 0)</code>, nên đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;UU&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi xóa một chuỗi con có độ dài 1, <code>s</code> trở thành <code>&quot;U&quot;</code>, luôn kết thúc tại <code>(0, 1)</code>, nên chỉ có một tọa độ cuối cùng khác nhau.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các ký tự <code>&#39;U&#39;</code>, <code>&#39;D&#39;</code>, <code>&#39;L&#39;</code> và <code>&#39;R&#39;</code>.</li>
    <li><code>1 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi xóa một đoạn có độ dài $k$, điểm cuối là độ dời toàn bộ trừ đi độ dời của đoạn đó. Mô phỏng lại phần còn lại cho mọi vị trí xóa sẽ có độ phức tạp bậc hai.
>
> Các mảng prefix lưu $(x,y)$ sau mỗi bước. Khi xóa đoạn $[i-k,i)$, ta đến $(f[n]-(f[i]-f[i-k]),\,g[n]-(g[i]-g[i-k]))$.
>
> Thêm các điểm này vào một set; kích thước của set là số điểm cuối khác nhau. Mỗi vị trí xóa chỉ cần $O(1)$.

<!-- thinking:end -->

Ta có thể sử dụng các mảng prefix sum để theo dõi thay đổi vị trí sau mỗi bước di chuyển. Cụ thể, ta dùng hai mảng prefix sum $f$ và $g$ để lưu thay đổi vị trí trên trục $x$ và trục $y$ tương ứng sau mỗi bước di chuyển.

Khởi tạo $f[0] = 0$ và $g[0] = 0$, biểu thị vị trí ban đầu tại $(0, 0)$. Sau đó, ta duyệt chuỗi $s$, với mỗi ký tự:

- Nếu ký tự là 'U', thì $g[i] = g[i-1] + 1$.
- Nếu ký tự là 'D', thì $g[i] = g[i-1] - 1$.
- Nếu ký tự là 'L', thì $f[i] = f[i-1] - 1$.
- Nếu ký tự là 'R', thì $f[i] = f[i-1] + 1$.

Tiếp theo, ta dùng một hash set để lưu các tọa độ cuối cùng khác nhau. Với mỗi vị trí có thể xóa chuỗi con $i$ (từ $k$ đến $n$), ta tính tọa độ cuối cùng $(a, b)$ sau khi xóa chuỗi con, trong đó $a = f[n] - (f[i] - f[i-k])$ và $b = g[n] - (g[i] - g[i-k])$. Thêm tọa độ $(a, b)$ vào hash set.

Cuối cùng, kích thước của hash set chính là số tọa độ cuối cùng khác nhau.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctPoints(self, s: str, k: int) -> int:
        n = len(s)
        f = [0] * (n + 1)
        g = [0] * (n + 1)
        x = y = 0
        for i, c in enumerate(s, 1):
            if c == "U":
                y += 1
            elif c == "D":
                y -= 1
            elif c == "L":
                x -= 1
            else:
                x += 1
            f[i] = x
            g[i] = y
        st = set()
        for i in range(k, n + 1):
            a = f[n] - (f[i] - f[i - k])
            b = g[n] - (g[i] - g[i - k])
            st.add((a, b))
        return len(st)
```

#### Java

```java
class Solution {
    public int distinctPoints(String s, int k) {
        int n = s.length();
        int[] f = new int[n + 1];
        int[] g = new int[n + 1];
        int x = 0, y = 0;
        for (int i = 1; i <= n; ++i) {
            char c = s.charAt(i - 1);
            if (c == 'U') {
                ++y;
            } else if (c == 'D') {
                --y;
            } else if (c == 'L') {
                --x;
            } else {
                ++x;
            }
            f[i] = x;
            g[i] = y;
        }
        Set<Long> st = new HashSet<>();
        for (int i = k; i <= n; ++i) {
            int a = f[n] - (f[i] - f[i - k]);
            int b = g[n] - (g[i] - g[i - k]);
            st.add(1L * a * n + b);
        }
        return st.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distinctPoints(string s, int k) {
        int n = s.size();
        vector<int> f(n + 1), g(n + 1);
        int x = 0, y = 0;
        for (int i = 1; i <= n; ++i) {
            char c = s[i - 1];
            if (c == 'U')
                ++y;
            else if (c == 'D')
                --y;
            else if (c == 'L')
                --x;
            else
                ++x;
            f[i] = x;
            g[i] = y;
        }
        unordered_set<long long> st;
        for (int i = k; i <= n; ++i) {
            int a = f[n] - (f[i] - f[i - k]);
            int b = g[n] - (g[i] - g[i - k]);
            st.insert(1LL * a * n + b);
        }
        return st.size();
    }
};
```

#### Go

```go
func distinctPoints(s string, k int) int {
	n := len(s)
	f := make([]int, n+1)
	g := make([]int, n+1)
	x, y := 0, 0
	for i := 1; i <= n; i++ {
		c := s[i-1]
		if c == 'U' {
			y++
		} else if c == 'D' {
			y--
		} else if c == 'L' {
			x--
		} else {
			x++
		}
		f[i] = x
		g[i] = y
	}
	st := make(map[int64]struct{})
	for i := k; i <= n; i++ {
		a := f[n] - (f[i] - f[i-k])
		b := g[n] - (g[i] - g[i-k])
		key := int64(a)*int64(n) + int64(b)
		st[key] = struct{}{}
	}
	return len(st)
}
```

#### TypeScript

```ts
function distinctPoints(s: string, k: number): number {
    const n = s.length;
    const f = new Array(n + 1).fill(0);
    const g = new Array(n + 1).fill(0);
    let x = 0,
        y = 0;
    for (let i = 1; i <= n; ++i) {
        const c = s[i - 1];
        if (c === 'U') ++y;
        else if (c === 'D') --y;
        else if (c === 'L') --x;
        else ++x;
        f[i] = x;
        g[i] = y;
    }
    const st = new Set<number>();
    for (let i = k; i <= n; ++i) {
        const a = f[n] - (f[i] - f[i - k]);
        const b = g[n] - (g[i] - g[i - k]);
        st.add(a * n + b);
    }
    return st.size;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn distinct_points(s: String, k: i32) -> i32 {
        let n = s.len();
        let mut f = vec![0; n + 1];
        let mut g = vec![0; n + 1];
        let mut x = 0;
        let mut y = 0;
        let bytes = s.as_bytes();
        for i in 1..=n {
            match bytes[i - 1] as char {
                'U' => y += 1,
                'D' => y -= 1,
                'L' => x -= 1,
                _ => x += 1,
            }
            f[i] = x;
            g[i] = y;
        }
        let mut st = HashSet::new();
        let k = k as usize;
        for i in k..=n {
            let a = f[n] - (f[i] - f[i - k]);
            let b = g[n] - (g[i] - g[i - k]);
            st.insert((a as i64) * (n as i64) + (b as i64));
        }
        st.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
