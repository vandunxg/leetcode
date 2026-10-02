---
comments: true
difficulty: Medium
rating: 1805
source: Weekly Contest 134 Q3
tags:
    - Array
    - Dynamic Programming
    - Longest Common Subsequence
---

<!-- problem:start -->

# [1035. Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines)

[中文文档](/solution/1000-1099/1035.Uncrossed%20Lines/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>. Ta viết các số trong <code>nums1</code> và <code>nums2</code> theo đúng thứ tự đã cho trên hai đường ngang riêng biệt.</p>

<p>Ta có thể vẽ các đường nối: mỗi đường thẳng nối hai số <code>nums1[i]</code> và <code>nums2[j]</code> cần thỏa mãn:</p>

<ul>
	<li><code>nums1[i] == nums2[j]</code>; và</li>
	<li>đường nối được vẽ không giao với bất kỳ đường nối (không nằm ngang) nào khác.</li>
</ul>

<p>Lưu ý rằng các đường nối không được giao nhau kể cả tại hai đầu mút (tức mỗi số chỉ có thể thuộc về một đường nối).</p>

<p>Trả về <em>số đường nối không giao nhau lớn nhất có thể vẽ theo cách này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1035.Uncrossed%20Lines/images/142.png" style="width: 400px; height: 286px;" />
<pre>
<strong>Đầu vào:</strong> nums1 = [1,4,2], nums2 = [1,2,4]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể vẽ 2 đường nối không giao nhau như trong hình.
Không thể vẽ 3 đường nối không giao nhau, vì đường từ nums1[1] = 4 đến nums2[2] = 4 sẽ giao với đường từ nums1[2]=2 đến nums2[1]=2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,5,1,2,5], nums2 = [10,5,2,1,5,2]
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,3,7,1,7,5], nums2 = [1,9,2,5,1]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 500</code></li>
	<li><code>1 &lt;= nums1[i], nums2[j] &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp giá trị bằng nhau được nối mà không giao nhau tạo thành một dãy con chung, nên bài toán tương đương LCS. Với $m,n\le 500$, có thể dùng bảng $O(mn)$.
>
> $f[i][j]$ là số đường nối lớn nhất giữa hai prefix: nếu phần tử cuối bằng nhau thì lấy $f[i-1][j-1]+1$; nếu không, chọn kết quả tốt hơn khi bỏ một phía.
>
> Đáp án là $f[m][n]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số đường nối lớn nhất giữa $i$ phần tử đầu tiên của $\textit{nums1}$ và $j$ phần tử đầu tiên của $\textit{nums2}$. Ban đầu, $f[i][j] = 0$, và đáp án là $f[m][n]$.

Khi $\textit{nums1}[i-1] = \textit{nums2}[j-1]$, ta có thể thêm một đường nối dựa trên $i-1$ phần tử đầu của $\textit{nums1}$ và $j-1$ phần tử đầu của $\textit{nums2}$. Khi đó, $f[i][j] = f[i-1][j-1] + 1$.

Khi $\textit{nums1}[i-1] \neq \textit{nums2}[j-1]$, ta chọn lời giải tốt hơn giữa hai trường hợp: xét $i-1$ phần tử đầu của $\textit{nums1}$ cùng $j$ phần tử đầu của $\textit{nums2}$, hoặc xét $i$ phần tử đầu của $\textit{nums1}$ cùng $j-1$ phần tử đầu của $\textit{nums2}$. Khi đó, $f[i][j] = \max(f[i-1][j], f[i][j-1])$.

Cuối cùng, trả về $f[m][n]$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của $\textit{nums1}$ và $\textit{nums2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        m, n = len(nums1), len(nums2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i, x in enumerate(nums1, 1):
            for j, y in enumerate(nums2, 1):
                if x == y:
                    f[i][j] = f[i - 1][j - 1] + 1
                else:
                    f[i][j] = max(f[i - 1][j], f[i][j - 1])
        return f[m][n]
```

#### Java

```java
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                } else {
                    f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
                }
            }
        }
        return f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        int f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                } else {
                    f[i][j] = max(f[i - 1][j], f[i][j - 1]);
                }
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func maxUncrossedLines(nums1 []int, nums2 []int) int {
	m, n := len(nums1), len(nums2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if nums1[i-1] == nums2[j-1] {
				f[i][j] = f[i-1][j-1] + 1
			} else {
				f[i][j] = max(f[i-1][j], f[i][j-1])
			}
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function maxUncrossedLines(nums1: number[], nums2: number[]): number {
    const m = nums1.length;
    const n = nums2.length;
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (nums1[i - 1] === nums2[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
            } else {
                f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
            }
        }
    }
    return f[m][n];
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var maxUncrossedLines = function (nums1, nums2) {
    const m = nums1.length;
    const n = nums2.length;
    const f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (nums1[i - 1] === nums2[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
            } else {
                f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
            }
        }
    }
    return f[m][n];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
