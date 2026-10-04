---
comments: true
difficulty: Medium
rating: 1560
source: Biweekly Contest 166 Q2
---

<!-- problem:start -->

# [3693. Climbing Stairs II](https://leetcode.com/problems/climbing-stairs-ii)

[中文文档](/solution/3600-3699/3693.Climbing%20Stairs%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang leo một cầu thang có <code>n + 1</code> bậc, được đánh số từ 0 đến <code>n</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <strong>đánh chỉ số từ 1</strong> <code>costs</code> có độ dài <code>n</code>, trong đó <code>costs[i]</code> là chi phí của bậc <code>i</code>.</p>

<p>Từ bậc <code>i</code>, bạn <strong>chỉ</strong> có thể nhảy đến bậc <code>i + 1</code>, <code>i + 2</code>, hoặc <code>i + 3</code>. Chi phí nhảy từ bậc <code>i</code> đến bậc <code>j</code> được định nghĩa là: <code>costs[j] + (j - i)<sup>2</sup></code></p>

<p>Bạn bắt đầu từ bậc 0 với <code>cost = 0</code>.</p>

<p>Trả về tổng chi phí <strong>nhỏ nhất</strong> để đến bậc <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, costs = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một lộ trình tối ưu là <code>0 &rarr; 1 &rarr; 2 &rarr; 4</code></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Bước nhảy</th>
			<th style="border: 1px solid black;">Tính chi phí</th>
			<th style="border: 1px solid black;">Chi phí</th>
		</tr>
	</tbody>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0 &rarr; 1</td>
			<td style="border: 1px solid black;"><code>costs[1] + (1 - 0)<sup>2</sup> = 1 + 1</code></td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1 &rarr; 2</td>
			<td style="border: 1px solid black;"><code>costs[2] + (2 - 1)<sup>2</sup> = 2 + 1</code></td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2 &rarr; 4</td>
			<td style="border: 1px solid black;"><code>costs[4] + (4 - 2)<sup>2</sup> = 4 + 4</code></td>
			<td style="border: 1px solid black;">8</td>
		</tr>
	</tbody>
</table>

<p>Do đó, tổng chi phí nhỏ nhất là <code>2 + 3 + 8 = 13</code></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, costs = [5,1,6,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một lộ trình tối ưu là <code>0 &rarr; 2 &rarr; 4</code></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Bước nhảy</th>
			<th style="border: 1px solid black;">Tính chi phí</th>
			<th style="border: 1px solid black;">Chi phí</th>
		</tr>
	</tbody>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0 &rarr; 2</td>
			<td style="border: 1px solid black;"><code>costs[2] + (2 - 0)<sup>2</sup> = 1 + 4</code></td>
			<td style="border: 1px solid black;">5</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2 &rarr; 4</td>
			<td style="border: 1px solid black;"><code>costs[4] + (4 - 2)<sup>2</sup> = 2 + 4</code></td>
			<td style="border: 1px solid black;">6</td>
		</tr>
	</tbody>
</table>

<p>Do đó, tổng chi phí nhỏ nhất là <code>5 + 6 = 11</code></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, costs = [9,8,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lộ trình tối ưu là <code>0 &rarr; 3</code> với tổng chi phí = <code>costs[3] + (3 - 0)<sup>2</sup> = 3 + 9 = 12</code></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == costs.length &lt;= 10<sup>5</sup>​​​​​​​</code></li>
	<li><code>1 &lt;= costs[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Từ $0$ đến $n$, mỗi bước có thể đi qua $1$, $2$, hoặc $3$ bậc, với chi phí $\textit{costs}[i-1]$ cộng với bình phương khoảng cách. Tính chất cấu trúc con tối ưu dẫn đến một DP tuyến tính.
>
> $f[i]$ là chi phí nhỏ nhất để đến bậc $i$. Chuyển trạng thái từ $i-3,i-2,i-1$ với chi phí $x+(i-j)^2$.
>
> $f[0]=0$ và đáp án là $f[n]$. Mỗi bậc có số lượng bậc trước cố định.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là tổng chi phí nhỏ nhất cần thiết để đến bậc thứ $i$, ban đầu $f[0] = 0$, còn tất cả $f[i] = +\infty$.

Với mỗi bậc $i$, ta có thể nhảy từ bậc thứ $(i-1)$, $(i-2)$ hoặc $(i-3)$, nên phương trình chuyển trạng thái như sau:

$$
f[i] = \min_{j=i-3}^{i-1} (f[j] + \textit{costs}[i - 1] + (i - j)^2)
$$

Trong đó, $\textit{costs}[i]$ là chi phí của bậc thứ $i$, còn $(i - j)^2$ là chi phí nhảy từ bậc thứ $j$ đến bậc thứ $i$. Lưu ý rằng ta cần đảm bảo $j$ không nhỏ hơn $0$.

Đáp án cuối cùng là $f[n]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số bậc.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def climbStairs(self, n: int, costs: List[int]) -> int:
        n = len(costs)
        f = [inf] * (n + 1)
        f[0] = 0
        for i, x in enumerate(costs, 1):
            for j in range(i - 3, i):
                if j >= 0:
                    f[i] = min(f[i], f[j] + x + (i - j) ** 2)
        return f[n]
```

#### Java

```java
class Solution {
    public int climbStairs(int n, int[] costs) {
        int[] f = new int[n + 1];
        final int inf = Integer.MAX_VALUE / 2;
        Arrays.fill(f, inf);
        f[0] = 0;
        for (int i = 1; i <= n; ++i) {
            int x = costs[i - 1];
            for (int j = Math.max(0, i - 3); j < i; ++j) {
                f[i] = Math.min(f[i], f[j] + x + (i - j) * (i - j));
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int climbStairs(int n, vector<int>& costs) {
        vector<int> f(n + 1, INT_MAX / 2);
        f[0] = 0;
        for (int i = 1; i <= n; ++i) {
            int x = costs[i - 1];
            for (int j = max(0, i - 3); j < i; ++j) {
                f[i] = min(f[i], f[j] + x + (i - j) * (i - j));
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func climbStairs(n int, costs []int) int {
	const inf = int(1e9)
	f := make([]int, n+1)
	for i := range f {
		f[i] = inf
	}
	f[0] = 0
	for i := 1; i <= n; i++ {
		x := costs[i-1]
		for j := max(0, i-3); j < i; j++ {
			f[i] = min(f[i], f[j]+x+(i-j)*(i-j))
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function climbStairs(n: number, costs: number[]): number {
    const inf = Number.MAX_SAFE_INTEGER / 2;
    const f = Array(n + 1).fill(inf);
    f[0] = 0;

    for (let i = 1; i <= n; ++i) {
        const x = costs[i - 1];
        for (let j = Math.max(0, i - 3); j < i; ++j) {
            f[i] = Math.min(f[i], f[j] + x + (i - j) * (i - j));
        }
    }
    return f[n];
}
```

#### Rust

```rust
impl Solution {
    pub fn climb_stairs(n: i32, costs: Vec<i32>) -> i32 {
        let n = n as usize;
        let inf = i32::MAX / 2;
        let mut f = vec![inf; n + 1];
        f[0] = 0;
        for i in 1..=n {
            let x = costs[i - 1];
            for j in (i.saturating_sub(3))..i {
                f[i] = f[i].min(f[j] + x + ((i - j) * (i - j)) as i32);
            }
        }
        f[n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
