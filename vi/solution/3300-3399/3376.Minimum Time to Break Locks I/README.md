---
comments: true
difficulty: Medium
rating: 1793
source: Biweekly Contest 145 Q2
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [3376. Minimum Time to Break Locks I](https://leetcode.com/problems/minimum-time-to-break-locks-i)

[中文文档](/solution/3300-3399/3376.Minimum%20Time%20to%20Break%20Locks%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bob bị mắc kẹt trong ngục tối và phải phá <code>n</code> khóa, mỗi khóa cần một lượng <strong>năng lượng</strong> nhất định để phá. Năng lượng cần thiết cho mỗi khóa được lưu trong một mảng có tên <code>strength</code>, trong đó <code>strength[i]</code> biểu thị năng lượng cần để phá khóa thứ <code>i<sup>th</sup></code>.</p>

<p>Để phá một khóa, Bob sử dụng một thanh kiếm có các đặc điểm sau:</p>

<ul>
	<li>Năng lượng ban đầu của thanh kiếm là 0.</li>
	<li>Hệ số ban đầu <code><font face="monospace">x</font></code> làm năng lượng của thanh kiếm tăng là 1.</li>
	<li>Mỗi phút, năng lượng của thanh kiếm tăng thêm một lượng bằng hệ số hiện tại <code>x</code>.</li>
	<li>Để phá khóa thứ <code>i<sup>th</sup></code>, năng lượng của thanh kiếm phải đạt <strong>ít nhất</strong> <code>strength[i]</code>.</li>
	<li>Sau khi phá một khóa, năng lượng của thanh kiếm được đặt lại về 0, còn hệ số <code>x</code> tăng thêm một giá trị cho trước <code>k</code>.</li>
</ul>

<p>Nhiệm vụ của bạn là xác định thời gian <strong>nhỏ nhất</strong> tính theo phút cần để Bob phá tất cả <code>n</code> khóa và thoát khỏi ngục tối.</p>

<p>Trả về thời gian <strong>nhỏ nhất</strong> cần để phá tất cả <code>n</code> khóa.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">strength = [3,4,1], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Thời gian</th>
			<th style="border: 1px solid black;">Năng lượng</th>
			<th style="border: 1px solid black;">x</th>
			<th style="border: 1px solid black;">Hành động</th>
			<th style="border: 1px solid black;">x sau cập nhật</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không làm gì</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Phá khóa thứ 3<sup>rd</sup></td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Không làm gì</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Phá khóa thứ 2<sup>nd</sup></td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">Phá khóa thứ 1<sup>st</sup></td>
			<td style="border: 1px solid black;">3</td>
		</tr>
	</tbody>
</table>

<p>Không thể phá tất cả các khóa trong thời gian dưới 4 phút; do đó, đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">strength = [2,5,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Thời gian</th>
			<th style="border: 1px solid black;">Năng lượng</th>
			<th style="border: 1px solid black;">x</th>
			<th style="border: 1px solid black;">Hành động</th>
			<th style="border: 1px solid black;">x sau cập nhật</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không làm gì</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không làm gì</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Phá khóa thứ 1<sup>st</sup></td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">Không làm gì</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">Phá khóa thứ 2<sup>n</sup><sup>d</sup></td>
			<td style="border: 1px solid black;">5</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">Phá khóa thứ 3<sup>r</sup><sup>d</sup></td>
			<td style="border: 1px solid black;">7</td>
		</tr>
	</tbody>
</table>

<p>Không thể phá tất cả các khóa trong thời gian dưới 5 phút; do đó, đáp án là 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == strength.length</code></li>
	<li><code>1 &lt;= n &lt;= 8</code></li>
	<li><code>1 &lt;= k &lt;= 10</code></li>
	<li><code>1 &lt;= strength[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự phá các khóa làm thay đổi năng lượng: $x$ bắt đầu từ $1$ và tăng thêm $K$ sau mỗi lần phá. Vì $n \le 8$, ta có thể dùng DP trên các tập con.
>
> $\textit{dfs}(i)$ là thời gian còn lại sau khi đã phá tập khóa $i$. Năng lượng là $1+|i|\cdot K$; một khóa chưa phá $s$ cần $\lceil s/x \rceil$ phút.
>
> Với tập đầy đủ, kết quả là $0$. Có $2^n$ trạng thái và $n$ chuyển trạng thái, phù hợp với giới hạn đề bài.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinimumTime(self, strength: List[int], K: int) -> int:
        @cache
        def dfs(i: int) -> int:
            if i == (1 << len(strength)) - 1:
                return 0
            cnt = i.bit_count()
            x = 1 + cnt * K
            ans = inf
            for j, s in enumerate(strength):
                if i >> j & 1 ^ 1:
                    ans = min(ans, dfs(i | 1 << j) + (s + x - 1) // x)
            return ans

        return dfs(0)
```

#### Java

```java
class Solution {
    private List<Integer> strength;
    private Integer[] f;
    private int k;
    private int n;

    public int findMinimumTime(List<Integer> strength, int K) {
        n = strength.size();
        f = new Integer[1 << n];
        k = K;
        this.strength = strength;
        return dfs(0);
    }

    private int dfs(int i) {
        if (i == (1 << n) - 1) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int cnt = Integer.bitCount(i);
        int x = 1 + cnt * k;
        f[i] = 1 << 30;
        for (int j = 0; j < n; ++j) {
            if ((i >> j & 1) == 0) {
                f[i] = Math.min(f[i], dfs(i | 1 << j) + (strength.get(j) + x - 1) / x);
            }
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMinimumTime(vector<int>& strength, int K) {
        int n = strength.size();
        int f[1 << n];
        memset(f, -1, sizeof(f));
        int k = K;
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (i == (1 << n) - 1) {
                return 0;
            }
            if (f[i] != -1) {
                return f[i];
            }
            int cnt = __builtin_popcount(i);
            int x = 1 + k * cnt;
            f[i] = INT_MAX;
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1 ^ 1) {
                    f[i] = min(f[i], dfs(i | 1 << j) + (strength[j] + x - 1) / x);
                }
            }
            return f[i];
        };
        return dfs(0);
    }
};
```

#### Go

```go
func findMinimumTime(strength []int, K int) int {
	n := len(strength)
	f := make([]int, 1<<n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if i == 1<<n-1 {
			return 0
		}
		if f[i] != -1 {
			return f[i]
		}
		x := 1 + K*bits.OnesCount(uint(i))
		f[i] = 1 << 30
		for j, s := range strength {
			if i>>j&1 == 0 {
				f[i] = min(f[i], dfs(i|1<<j)+(s+x-1)/x)
			}
		}
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function findMinimumTime(strength: number[], K: number): number {
    const n = strength.length;
    const f: number[] = Array(1 << n).fill(-1);
    const dfs = (i: number): number => {
        if (i === (1 << n) - 1) {
            return 0;
        }
        if (f[i] !== -1) {
            return f[i];
        }
        f[i] = Infinity;
        const x = 1 + K * bitCount(i);
        for (let j = 0; j < n; ++j) {
            if (((i >> j) & 1) == 0) {
                f[i] = Math.min(f[i], dfs(i | (1 << j)) + Math.ceil(strength[j] / x));
            }
        }
        return f[i];
    };
    return dfs(0);
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
