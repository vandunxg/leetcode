---
comments: true
difficulty: Medium
rating: 1648
source: Weekly Contest 469 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3698. Split Array With Minimum Difference](https://leetcode.com/problems/split-array-with-minimum-difference)

[中文文档](/solution/3600-3699/3698.Split%20Array%20With%20Minimum%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Chia mảng thành <strong>chính xác</strong> hai <span data-keyword="subarray-nonempty">mảng con</span> là <code>left</code> và <code>right</code>, sao cho <code>left</code> <strong><span data-keyword="strictly-increasing-array">tăng dần nghiêm ngặt</span> </strong> và <code>right</code> <strong><span data-keyword="strictly-decreasing-array">giảm dần nghiêm ngặt</span></strong>.</p>

<p>Trả về <strong>độ chênh lệch tuyệt đối nhỏ nhất có thể</strong> giữa tổng của <code>left</code> và <code>right</code>. Nếu không tồn tại cách chia hợp lệ, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>left</code></th>
			<th style="border: 1px solid black;"><code>right</code></th>
			<th style="border: 1px solid black;">Tính hợp lệ</th>
			<th style="border: 1px solid black;"><code>left</code> tổng</th>
			<th style="border: 1px solid black;"><code>right</code> tổng</th>
			<th style="border: 1px solid black;">Độ chênh lệch tuyệt đối</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[1]</td>
			<td style="border: 1px solid black;">[3, 2]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;"><code>|1 - 5| = 4</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[1, 3]</td>
			<td style="border: 1px solid black;">[2]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>|4 - 2| = 2</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, độ chênh lệch tuyệt đối nhỏ nhất là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>left</code></th>
			<th style="border: 1px solid black;"><code>right</code></th>
			<th style="border: 1px solid black;">Tính hợp lệ</th>
			<th style="border: 1px solid black;"><code>left</code> tổng</th>
			<th style="border: 1px solid black;"><code>right</code> tổng</th>
			<th style="border: 1px solid black;">Độ chênh lệch tuyệt đối</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[1]</td>
			<td style="border: 1px solid black;">[2, 4, 3]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">9</td>
			<td style="border: 1px solid black;">-</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[1, 2]</td>
			<td style="border: 1px solid black;">[4, 3]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;"><code>|3 - 7| = 4</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[1, 2, 4]</td>
			<td style="border: 1px solid black;">[3]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;"><code>|7 - 3| = 4</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, độ chênh lệch tuyệt đối nhỏ nhất là 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại cách chia hợp lệ, nên đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hai mảng ghi nhận tính đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Một điểm cắt sau $i$ cần nửa bên trái tăng dần nghiêm ngặt và nửa bên phải giảm dần nghiêm ngặt, đồng thời tối thiểu hóa độ chênh lệch tuyệt đối giữa hai tổng. Với $n\le 10^5$, không thể kiểm tra lại tính đơn điệu tại mỗi điểm cắt.
>
> Tổng tiền tố cho ta hai tổng. $f[i]$ cho biết $[0,i]$ có tăng dần nghiêm ngặt hay không, còn $g[i]$ cho biết $[i,n-1]$ có giảm dần nghiêm ngặt hay không; mỗi mảng được xây dựng trong một lần duyệt.
>
> Một điểm cắt hợp lệ khi và chỉ khi $f[i]$ và $g[i+1]$ cùng đúng; cập nhật $|s[i]-(s[n-1]-s[i])|$. Nếu không có điểm cắt nào, trả về $-1$.

<!-- thinking:end -->

Ta dùng một mảng tổng tiền tố $s$ để ghi nhận tổng tiền tố của mảng, trong đó $s[i]$ biểu diễn tổng của mảng $[0,..i]$. Sau đó, ta dùng hai mảng boolean $f$ và $g$ để ghi nhận tính đơn điệu của các tiền tố và hậu tố tương ứng, trong đó $f[i]$ cho biết mảng $[0,..i]$ có tăng dần nghiêm ngặt hay không, còn $g[i]$ cho biết mảng $[i,..n-1]$ có giảm dần nghiêm ngặt hay không.

Cuối cùng, ta duyệt các vị trí $i$ của mảng với $0 \leq i < n-1$. Nếu cả $f[i]$ và $g[i+1]$ đều đúng, ta có thể tính tổng của $left$ và $right$, lần lượt là $s[i]$ và $s[n-1]-s[i]$, rồi cập nhật đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitArray(self, nums: List[int]) -> int:
        s = list(accumulate(nums))
        n = len(nums)
        f = [True] * n
        for i in range(1, n):
            f[i] = f[i - 1]
            if nums[i] <= nums[i - 1]:
                f[i] = False
        g = [True] * n
        for i in range(n - 2, -1, -1):
            g[i] = g[i + 1]
            if nums[i] <= nums[i + 1]:
                g[i] = False
        ans = inf
        for i in range(n - 1):
            if f[i] and g[i + 1]:
                s1 = s[i]
                s2 = s[n - 1] - s[i]
                ans = min(ans, abs(s1 - s2))
        return ans if ans < inf else -1
```

#### Java

```java
class Solution {
    public long splitArray(int[] nums) {
        int n = nums.length;
        long[] s = new long[n];
        s[0] = nums[0];
        boolean[] f = new boolean[n];
        Arrays.fill(f, true);
        boolean[] g = new boolean[n];
        Arrays.fill(g, true);
        for (int i = 1; i < n; ++i) {
            s[i] = s[i - 1] + nums[i];
            f[i] = f[i - 1];
            if (nums[i] <= nums[i - 1]) {
                f[i] = false;
            }
        }
        for (int i = n - 2; i >= 0; --i) {
            g[i] = g[i + 1];
            if (nums[i] <= nums[i + 1]) {
                g[i] = false;
            }
        }
        final long inf = Long.MAX_VALUE;
        long ans = inf;
        for (int i = 0; i < n - 1; ++i) {
            if (f[i] && g[i + 1]) {
                long s1 = s[i];
                long s2 = s[n - 1] - s[i];
                ans = Math.min(ans, Math.abs(s1 - s2));
            }
        }
        return ans < inf ? ans : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long splitArray(vector<int>& nums) {
        int n = nums.size();
        vector<long long> s(n);
        s[0] = nums[0];
        vector<bool> f(n, true), g(n, true);

        for (int i = 1; i < n; ++i) {
            s[i] = s[i - 1] + nums[i];
            f[i] = f[i - 1];
            if (nums[i] <= nums[i - 1]) {
                f[i] = false;
            }
        }
        for (int i = n - 2; i >= 0; --i) {
            g[i] = g[i + 1];
            if (nums[i] <= nums[i + 1]) {
                g[i] = false;
            }
        }

        const long long inf = LLONG_MAX;
        long long ans = inf;
        for (int i = 0; i < n - 1; ++i) {
            if (f[i] && g[i + 1]) {
                long long s1 = s[i];
                long long s2 = s[n - 1] - s[i];
                ans = min(ans, llabs(s1 - s2));
            }
        }
        return ans < inf ? ans : -1;
    }
};
```

#### Go

```go
func splitArray(nums []int) int64 {
	n := len(nums)
	s := make([]int64, n)
	f := make([]bool, n)
	g := make([]bool, n)
	for i := range f {
		f[i] = true
		g[i] = true
	}

	s[0] = int64(nums[0])
	for i := 1; i < n; i++ {
		s[i] = s[i-1] + int64(nums[i])
		f[i] = f[i-1]
		if nums[i] <= nums[i-1] {
			f[i] = false
		}
	}
	for i := n - 2; i >= 0; i-- {
		g[i] = g[i+1]
		if nums[i] <= nums[i+1] {
			g[i] = false
		}
	}

	const inf = int64(^uint64(0) >> 1)
	ans := inf
	for i := 0; i < n-1; i++ {
		if f[i] && g[i+1] {
			s1 := s[i]
			s2 := s[n-1] - s[i]
			ans = min(ans, abs(s1-s2))
		}
	}
	if ans < inf {
		return ans
	}
	return -1
}

func abs(x int64) int64 {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function splitArray(nums: number[]): number {
    const n = nums.length;
    const s: number[] = Array(n);
    const f: boolean[] = Array(n).fill(true);
    const g: boolean[] = Array(n).fill(true);

    s[0] = nums[0];
    for (let i = 1; i < n; ++i) {
        s[i] = s[i - 1] + nums[i];
        f[i] = f[i - 1];
        if (nums[i] <= nums[i - 1]) {
            f[i] = false;
        }
    }

    for (let i = n - 2; i >= 0; --i) {
        g[i] = g[i + 1];
        if (nums[i] <= nums[i + 1]) {
            g[i] = false;
        }
    }

    const INF = Number.MAX_SAFE_INTEGER;
    let ans = INF;

    for (let i = 0; i < n - 1; ++i) {
        if (f[i] && g[i + 1]) {
            const s1 = s[i];
            const s2 = s[n - 1] - s[i];
            ans = Math.min(ans, Math.abs(s1 - s2));
        }
    }

    return ans < INF ? ans : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
