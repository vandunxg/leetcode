---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Sliding Window
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray)

[中文文档](/solution/0700-0799/0718.Maximum%20Length%20of%20Repeated%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, hãy trả về <em>độ dài lớn nhất của mảng con xuất hiện trong <strong>cả hai</strong> mảng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Mảng con chung dài nhất là [3,2,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Mảng con chung dài nhất là [0,0,0,0,0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tìm mảng con liên tiếp chung dài nhất. Độ dài mảng có thể đến $1000$, nên so khớp từ mọi cặp vị trí bắt đầu quá chậm; LCS DP cũng không đảm bảo tính liên tiếp.
>
> Vì mảng con phải liên tiếp, suffix chỉ được kéo dài khi cặp phần tử hiện tại bằng nhau: độ dài bằng suffix của hai prefix cộng một; nếu không thì bằng không.
>
> Gọi $f[i][j]$ là độ dài suffix chung kết thúc tại $nums1[i-1]$ và $nums2[j-1]$. Lấy giá trị lớn nhất trên toàn bộ bảng. Độ phức tạp thời gian là $O(mn)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        m, n = len(nums1), len(nums2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        ans = 0
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if nums1[i - 1] == nums2[j - 1]:
                    f[i][j] = f[i - 1][j - 1] + 1
                    ans = max(ans, f[i][j])
        return ans
```

#### Java

```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[][] f = new int[m + 1][n + 1];
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                    ans = Math.max(ans, f[i][j]);
                }
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
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                    ans = max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLength(nums1 []int, nums2 []int) (ans int) {
	m, n := len(nums1), len(nums2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if nums1[i-1] == nums2[j-1] {
				f[i][j] = f[i-1][j-1] + 1
				if ans < f[i][j] {
					ans = f[i][j]
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findLength(nums1: number[], nums2: number[]): number {
    const m = nums1.length;
    const n = nums2.length;
    const f = Array.from({ length: m + 1 }, _ => new Array(n + 1).fill(0));
    let ans = 0;
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (nums1[i - 1] == nums2[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
                ans = Math.max(ans, f[i][j]);
            }
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var findLength = function (nums1, nums2) {
    const m = nums1.length;
    const n = nums2.length;
    const f = Array.from({ length: m + 1 }, _ => new Array(n + 1).fill(0));
    let ans = 0;
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (nums1[i - 1] == nums2[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
                ans = Math.max(ans, f[i][j]);
            }
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
