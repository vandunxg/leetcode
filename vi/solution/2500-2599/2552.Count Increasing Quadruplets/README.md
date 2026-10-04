---
comments: true
difficulty: Hard
rating: 2432
source: Weekly Contest 330 Q4
tags:
    - Binary Indexed Tree
    - Array
    - Dynamic Programming
    - Enumeration
    - Prefix Sum
---

<!-- problem:start -->

# [2552. Count Increasing Quadruplets](https://leetcode.com/problems/count-increasing-quadruplets)

[中文文档](/solution/2500-2599/2552.Count%20Increasing%20Quadruplets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> có kích thước <code>n</code>, chứa tất cả các số từ <code>1</code> đến <code>n</code>, hãy trả về <em>số lượng bộ bốn tăng dần</em>.</p>

<p>Một bộ bốn <code>(i, j, k, l)</code> là bộ bốn tăng dần nếu:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; k &lt; l &lt; n</code>, và</li>
	<li><code>nums[i] &lt; nums[k] &lt; nums[j] &lt; nums[l]</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,3,2,4,5]
<strong>Output:</strong> 2
<strong>Giải thích:</strong>
- Với i = 0, j = 1, k = 2 và l = 3, nums[i] &lt; nums[k] &lt; nums[j] &lt; nums[l].
- Với i = 0, j = 1, k = 2 và l = 4, nums[i] &lt; nums[k] &lt; nums[j] &lt; nums[l].
Không còn bộ bốn nào khác, nên ta trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,4]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Chỉ tồn tại một bộ bốn với i = 0, j = 1, k = 2, l = 3, nhưng vì nums[j] &lt; nums[k], ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>4 &lt;= nums.length &lt;= 4000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums.length</code></li>
	<li>Tất cả các số nguyên trong <code>nums</code> là <strong>duy nhất</strong>. <code>nums</code> là một hoán vị.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Tiền xử lý

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ bốn $i<j<k<l$ thỏa mãn $nums[i]<nums[k]<nums[j]<nums[l]$. Vòng lặp bốn tầng không phù hợp với $n\le 4000$, còn việc đếm trực tiếp cho từng cặp $(j,k)$ vẫn có thể có độ phức tạp bậc ba.
>
> Cố định $j$ và quét $k$ về bên phải, đồng thời giảm dần số lượng giá trị phía sau $>nums[j]$ để lưu vào $f[j][k]$. Tương tự, quét $j$ về bên trái của mỗi $k$ để tính $g[j][k]$. Nhân hai giá trị này khi $nums[j]>nums[k]$. Toàn bộ quá trình có độ phức tạp $O(n^2)$.

<!-- thinking:end -->

Ta có thể liệt kê $j$ và $k$ trong bộ bốn, khi đó bài toán được chuyển thành việc, với $j$ và $k$ hiện tại:

- Đếm số lượng $l$ thỏa mãn $l > k$ và $nums[l] > nums[j]$;
- Đếm số lượng $i$ thỏa mãn $i < j$ và $nums[i] < nums[k]$.

Ta có thể sử dụng hai mảng hai chiều $f$ và $g$ để lưu hai thông tin này. Trong đó, $f[j][k]$ biểu diễn số lượng $l$ thỏa mãn $l > k$ và $nums[l] > nums[j]$, còn $g[j][k]$ biểu diễn số lượng $i$ thỏa mãn $i < j$ và $nums[i] < nums[k]$.

Do đó, đáp án là tổng của tất cả $f[j][k] \times g[j][k]$.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countQuadruplets(self, nums: List[int]) -> int:
        n = len(nums)
        f = [[0] * n for _ in range(n)]
        g = [[0] * n for _ in range(n)]
        for j in range(1, n - 2):
            cnt = sum(nums[l] > nums[j] for l in range(j + 1, n))
            for k in range(j + 1, n - 1):
                if nums[j] > nums[k]:
                    f[j][k] = cnt
                else:
                    cnt -= 1
        for k in range(2, n - 1):
            cnt = sum(nums[i] < nums[k] for i in range(k))
            for j in range(k - 1, 0, -1):
                if nums[j] > nums[k]:
                    g[j][k] = cnt
                else:
                    cnt -= 1
        return sum(
            f[j][k] * g[j][k] for j in range(1, n - 2) for k in range(j + 1, n - 1)
        )
```

#### Java

```java
class Solution {
    public long countQuadruplets(int[] nums) {
        int n = nums.length;
        int[][] f = new int[n][n];
        int[][] g = new int[n][n];
        for (int j = 1; j < n - 2; ++j) {
            int cnt = 0;
            for (int l = j + 1; l < n; ++l) {
                if (nums[l] > nums[j]) {
                    ++cnt;
                }
            }
            for (int k = j + 1; k < n - 1; ++k) {
                if (nums[j] > nums[k]) {
                    f[j][k] = cnt;
                } else {
                    --cnt;
                }
            }
        }
        long ans = 0;
        for (int k = 2; k < n - 1; ++k) {
            int cnt = 0;
            for (int i = 0; i < k; ++i) {
                if (nums[i] < nums[k]) {
                    ++cnt;
                }
            }
            for (int j = k - 1; j > 0; --j) {
                if (nums[j] > nums[k]) {
                    g[j][k] = cnt;
                    ans += (long) f[j][k] * g[j][k];
                } else {
                    --cnt;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
const int N = 4001;
int f[N][N];
int g[N][N];

class Solution {
public:
    long long countQuadruplets(vector<int>& nums) {
        int n = nums.size();
        memset(f, 0, sizeof f);
        memset(g, 0, sizeof g);
        for (int j = 1; j < n - 2; ++j) {
            int cnt = 0;
            for (int l = j + 1; l < n; ++l) {
                if (nums[l] > nums[j]) {
                    ++cnt;
                }
            }
            for (int k = j + 1; k < n - 1; ++k) {
                if (nums[j] > nums[k]) {
                    f[j][k] = cnt;
                } else {
                    --cnt;
                }
            }
        }
        long long ans = 0;
        for (int k = 2; k < n - 1; ++k) {
            int cnt = 0;
            for (int i = 0; i < k; ++i) {
                if (nums[i] < nums[k]) {
                    ++cnt;
                }
            }
            for (int j = k - 1; j > 0; --j) {
                if (nums[j] > nums[k]) {
                    g[j][k] = cnt;
                    ans += 1ll * f[j][k] * g[j][k];
                } else {
                    --cnt;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countQuadruplets(nums []int) int64 {
	n := len(nums)
	f := make([][]int, n)
	g := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		g[i] = make([]int, n)
	}
	for j := 1; j < n-2; j++ {
		cnt := 0
		for l := j + 1; l < n; l++ {
			if nums[l] > nums[j] {
				cnt++
			}
		}
		for k := j + 1; k < n-1; k++ {
			if nums[j] > nums[k] {
				f[j][k] = cnt
			} else {
				cnt--
			}
		}
	}
	ans := 0
	for k := 2; k < n-1; k++ {
		cnt := 0
		for i := 0; i < k; i++ {
			if nums[i] < nums[k] {
				cnt++
			}
		}
		for j := k - 1; j > 0; j-- {
			if nums[j] > nums[k] {
				g[j][k] = cnt
				ans += f[j][k] * g[j][k]
			} else {
				cnt--
			}
		}
	}
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
