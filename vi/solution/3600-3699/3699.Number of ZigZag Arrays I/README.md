---
comments: true
difficulty: Hard
rating: 2123
source: Weekly Contest 469 Q3
---

<!-- problem:start -->

# [3699. Number of ZigZag Arrays I](https://leetcode.com/problems/number-of-zigzag-arrays-i)

[中文文档](/solution/3600-3699/3699.Number%20of%20ZigZag%20Arrays%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>n</code>, <code>l</code> và <code>r</code>.</p>

<p>Một mảng <strong>ZigZag</strong> có độ dài <code>n</code> được định nghĩa như sau:</p>

<ul>
	<li>Mỗi phần tử nằm trong khoảng <code>[l, r]</code>.</li>
	<li>Không có <strong>hai</strong> phần tử kề nhau nào bằng nhau.</li>
	<li>Không có <strong>ba</strong> phần tử liên tiếp nào tạo thành một dãy <strong>tăng nghiêm ngặt</strong> hoặc <strong>giảm nghiêm ngặt</strong>.</li>
</ul>

<p>Trả về tổng số mảng <strong>ZigZag</strong> hợp lệ.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Một <strong>dãy</strong> được gọi là <strong>tăng nghiêm ngặt</strong> nếu mỗi phần tử lớn hơn phần tử đứng trước nó (nếu có).</p>

<p>Một <strong>dãy</strong> được gọi là <strong>giảm nghiêm ngặt</strong> nếu mỗi phần tử nhỏ hơn phần tử đứng trước nó (nếu có).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, l = 4, r = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có 2 mảng ZigZag hợp lệ có độ dài <code>n = 3</code> và sử dụng các giá trị trong khoảng <code>[4, 5]</code>:</p>

<ul>
	<li><code>[4, 5, 4]</code></li>
	<li><code>[5, 4, 5]</code>​​​​​​​</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, l = 1, r = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 10 mảng ZigZag hợp lệ có độ dài <code>n = 3</code> và sử dụng các giá trị trong khoảng <code>[1, 3]</code>:</p>

<ul>
	<li><code>[1, 2, 1]</code>, <code>[1, 3, 1]</code>, <code>[1, 3, 2]</code></li>
	<li><code>[2, 1, 2]</code>, <code>[2, 1, 3]</code>, <code>[2, 3, 1]</code>, <code>[2, 3, 2]</code></li>
	<li><code>[3, 1, 2]</code>, <code>[3, 1, 3]</code>, <code>[3, 2, 3]</code></li>
</ul>

<p>Tất cả các mảng đều thỏa mãn các điều kiện ZigZag.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 2000</code></li>
	<li><code>1 &lt;= l &lt; r &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng zigzag luân phiên dấu của các hiệu liên tiếp. Các giá trị nằm trong $[l,r]$ và $n\le 2000$, nên ta tịnh tiến khoảng về $[0,m-1]$ rồi dùng DP.
>
> $\textit{up}[i]$ và $\textit{down}[i]$ lần lượt đếm các mảng kết thúc tại $i$ mà bước cuối tăng hoặc giảm. Tổng của một bước giảm là tất cả $\textit{up}$ lớn hơn; tổng của một bước tăng là tất cả $\textit{down}$ nhỏ hơn.
>
> Tổng tiền tố và tổng hậu tố giúp mỗi trong $n-1$ vòng lặp chạy trong $O(m)$. Với độ dài $1$, khởi tạo cả hai hướng bằng $1$. Lấy tổng modulo $10^9+7$.

<!-- thinking:end -->

Đặt $m = r - l + 1$ và ánh xạ khoảng $[l, r]$ về $[0, m - 1]$.

Gọi $up[i]$ là số mảng có độ dài hiện tại, kết thúc tại $i$ và bước cuối là tăng; $down[i]$ là số mảng có bước cuối là giảm. Với độ dài $1$ chưa có hướng, nên khởi tạo $up[i] = down[i] = 1$.

Chuyển trạng thái:

- Nếu mảng kết thúc tại $i$ bằng một bước giảm, giá trị trước đó phải lớn hơn $i$ và bước trước đó phải là tăng: $down'[i] = \sum_{j > i} up[j]$;
- Nếu bước cuối là tăng: $up'[i] = \sum_{j < i} down[j]$.

Tổng tiền tố và tổng hậu tố giúp mỗi chuyển trạng thái chạy trong $O(m)$. Lặp lại $n - 1$ lần. Đáp án là tổng của tất cả $up[i] + down[i]$.

Độ phức tạp thời gian là $O(n \times m)$, độ phức tạp không gian là $O(m)$, trong đó $n$ là độ dài mảng và $m$ là kích thước khoảng giá trị.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def zigZagArrays(self, n: int, l: int, r: int) -> int:
        mod = 10**9 + 7
        m = r - l + 1
        up = [1] * m
        down = [1] * m
        for _ in range(n - 1):
            pre = [0] * (m + 1)
            suf = [0] * (m + 1)
            for i in range(m):
                pre[i + 1] = (pre[i] + down[i]) % mod
            for i in range(m - 1, -1, -1):
                suf[i] = (suf[i + 1] + up[i]) % mod
            up = pre[:m]
            down = suf[1:]
        return sum(up + down) % mod
```

#### Java

```java
class Solution {
    public int zigZagArrays(int n, int l, int r) {
        final int mod = (int) 1e9 + 7;
        int m = r - l + 1;
        long[] up = new long[m];
        long[] down = new long[m];
        Arrays.fill(up, 1);
        Arrays.fill(down, 1);
        for (int k = 1; k < n; ++k) {
            long[] pre = new long[m + 1];
            long[] suf = new long[m + 1];
            for (int i = 0; i < m; ++i) {
                pre[i + 1] = (pre[i] + down[i]) % mod;
            }
            for (int i = m - 1; i >= 0; --i) {
                suf[i] = (suf[i + 1] + up[i]) % mod;
            }
            for (int i = 0; i < m; ++i) {
                up[i] = pre[i];
                down[i] = suf[i + 1];
            }
        }
        long ans = 0;
        for (int i = 0; i < m; ++i) {
            ans = (ans + up[i] + down[i]) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int zigZagArrays(int n, int l, int r) {
        const int mod = 1e9 + 7;
        int m = r - l + 1;
        vector<long long> up(m, 1), down(m, 1);
        for (int k = 1; k < n; ++k) {
            vector<long long> pre(m + 1), suf(m + 1);
            for (int i = 0; i < m; ++i) {
                pre[i + 1] = (pre[i] + down[i]) % mod;
            }
            for (int i = m - 1; i >= 0; --i) {
                suf[i] = (suf[i + 1] + up[i]) % mod;
            }
            for (int i = 0; i < m; ++i) {
                up[i] = pre[i];
                down[i] = suf[i + 1];
            }
        }
        long long ans = 0;
        for (int i = 0; i < m; ++i) {
            ans = (ans + up[i] + down[i]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func zigZagArrays(n int, l int, r int) int {
	const mod = int64(1e9 + 7)
	m := r - l + 1
	up := make([]int64, m)
	down := make([]int64, m)
	for i := range up {
		up[i], down[i] = 1, 1
	}
	for k := 1; k < n; k++ {
		pre := make([]int64, m+1)
		suf := make([]int64, m+1)
		for i := 0; i < m; i++ {
			pre[i+1] = (pre[i] + down[i]) % mod
		}
		for i := m - 1; i >= 0; i-- {
			suf[i] = (suf[i+1] + up[i]) % mod
		}
		for i := 0; i < m; i++ {
			up[i] = pre[i]
			down[i] = suf[i+1]
		}
	}
	var ans int64
	for i := 0; i < m; i++ {
		ans = (ans + up[i] + down[i]) % mod
	}
	return int(ans)
}
```

#### C

```c
int zigZagArrays(int n, int l, int r) {
    int mod = 1e9 + 7;
    int m = r - l + 1;
    long long up[m], down[m];
    for (int i = 0; i < m; ++i) {
        up[i] = down[i] = 1;
    }
    for (int k = 1; k < n; ++k) {
        long long pre[m + 1], suf[m + 1];
        memset(pre, 0, sizeof(pre));
        memset(suf, 0, sizeof(suf));
        for (int i = 0; i < m; ++i) {
            pre[i + 1] = (pre[i] + down[i]) % mod;
        }
        for (int i = m - 1; i >= 0; --i) {
            suf[i] = (suf[i + 1] + up[i]) % mod;
        }
        for (int i = 0; i < m; ++i) {
            up[i] = pre[i];
            down[i] = suf[i + 1];
        }
    }
    long long ans = 0;
    for (int i = 0; i < m; ++i) {
        ans = (ans + up[i] + down[i]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
