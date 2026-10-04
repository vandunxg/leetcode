---
comments: true
difficulty: Hard
rating: 2692
source: Weekly Contest 413 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3277. Maximum XOR Score Subarray Queries](https://leetcode.com/problems/maximum-xor-score-subarray-queries)

[中文文档](/solution/3200-3299/3277.Maximum%20XOR%20Score%20Subarray%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> gồm <code>n</code> phần tử và một mảng số nguyên 2D <code>queries</code> kích thước <code>q</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.</p>

<p>Với mỗi truy vấn, hãy tìm <strong>điểm XOR lớn nhất</strong> của mọi <span data-keyword="subarray">mảng con</span> của <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.</p>

<p><strong>Điểm XOR</strong> của một mảng <code>a</code> được tính bằng cách liên tục thực hiện các thao tác sau trên <code>a</code> cho đến khi chỉ còn lại một phần tử, đó chính là <strong>điểm số</strong>:</p>

<ul>
	<li>Đồng thời thay <code>a[i]</code> bằng <code>a[i] XOR a[i + 1]</code> với mọi chỉ số <code>i</code> trừ chỉ số cuối cùng.</li>
	<li>Xóa phần tử cuối cùng của <code>a</code>.</li>
</ul>

<p>Trả về một mảng <code>answer</code> có kích thước <code>q</code>, trong đó <code>answer[i]</code> là đáp án của truy vấn <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,8,4,32,16,1], queries = [[0,2],[1,4],[0,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[12,60,60]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong truy vấn đầu tiên, <code>nums[0..2]</code> có 6 mảng con là <code>[2]</code>, <code>[8]</code>, <code>[4]</code>, <code>[2, 8]</code>, <code>[8, 4]</code> và <code>[2, 8, 4]</code>, với điểm XOR tương ứng là 2, 8, 4, 10, 12 và 6. Đáp án của truy vấn là 12, giá trị lớn nhất trong tất cả các điểm XOR.</p>

<p>Trong truy vấn thứ hai, mảng con của <code>nums[1..4]</code> có điểm XOR lớn nhất là <code>nums[1..4]</code>, với điểm số bằng 60.</p>

<p>Trong truy vấn thứ ba, mảng con của <code>nums[0..5]</code> có điểm XOR lớn nhất là <code>nums[1..4]</code>, với điểm số bằng 60.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,7,3,2,8,5,1], queries = [[0,3],[1,5],[2,4],[2,6],[5,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[7,14,11,14,5]</span></p>

<p><strong>Giải thích:</strong></p>

<table height="70" width="472">
	<thead>
		<tr>
			<th>Chỉ số</th>
			<th>nums[l<sub>i</sub>..r<sub>i</sub>]</th>
			<th>Mảng con có điểm XOR lớn nhất</th>
			<th>Điểm XOR lớn nhất của mảng con</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>0</td>
			<td>[0, 7, 3, 2]</td>
			<td>[7]</td>
			<td>7</td>
		</tr>
		<tr>
			<td>1</td>
			<td>[7, 3, 2, 8, 5]</td>
			<td>[7, 3, 2, 8]</td>
			<td>14</td>
		</tr>
		<tr>
			<td>2</td>
			<td>[3, 2, 8]</td>
			<td>[3, 2, 8]</td>
			<td>11</td>
		</tr>
		<tr>
			<td>3</td>
			<td>[3, 2, 8, 5, 1]</td>
			<td>[2, 8, 5, 1]</td>
			<td>14</td>
		</tr>
		<tr>
			<td>4</td>
			<td>[5, 1]</td>
			<td>[5]</td>
			<td>5</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 2000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>1 &lt;= q == queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2 </code></li>
	<li><code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Điểm XOR của một mảng con được định nghĩa đệ quy; $q\le 10^5$ và $n\le 2000$ khiến việc tính lại từ đầu cho mỗi truy vấn là không thể. Điểm số là quy hoạch động trên đoạn $f[i][j]=f[i][j-1]\oplus f[i+1][j]$, còn mỗi truy vấn yêu cầu giá trị lớn nhất trong các đoạn con của $[l,r]$.
>
> Ta cũng duy trì $g[i][j]=\max(f[i][j],g[i][j-1],g[i+1][j])$. Điền bảng theo thứ tự $i$ giảm dần và $j$ tăng dần, sau đó mỗi truy vấn có thể trả lời bằng $g[l][r]$ trong $O(1)$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là giá trị XOR của đoạn $\textit{nums}[i..j]$. Theo mô tả bài toán, ta suy ra công thức chuyển trạng thái:

$$
f[i][j] = f[i][j-1] \oplus f[i+1][j]
$$

trong đó $\oplus$ biểu thị phép toán XOR.

Ta tiếp tục định nghĩa $g[i][j]$ là giá trị lớn nhất của $f[i][j]$. Công thức chuyển trạng thái là:

$$
g[i][j] = \max(f[i][j], g[i][j-1], g[i+1][j])
$$

Cuối cùng, ta duyệt qua mảng truy vấn. Với mỗi truy vấn $[l, r]$, ta thêm $g[l][r]$ vào mảng đáp án.

Độ phức tạp thời gian là $O(n^2 + m)$, và độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ và $m$ lần lượt là độ dài của các mảng $\textit{nums}$ và $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSubarrayXor(
        self, nums: List[int], queries: List[List[int]]
    ) -> List[int]:
        n = len(nums)
        f = [[0] * n for _ in range(n)]
        g = [[0] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            f[i][i] = g[i][i] = nums[i]
            for j in range(i + 1, n):
                f[i][j] = f[i][j - 1] ^ f[i + 1][j]
                g[i][j] = max(f[i][j], g[i][j - 1], g[i + 1][j])
        return [g[l][r] for l, r in queries]
```

#### Java

```java
class Solution {
    public int[] maximumSubarrayXor(int[] nums, int[][] queries) {
        int n = nums.length;
        int[][] f = new int[n][n];
        int[][] g = new int[n][n];
        for (int i = n - 1; i >= 0; --i) {
            f[i][i] = nums[i];
            g[i][i] = nums[i];
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = f[i][j - 1] ^ f[i + 1][j];
                g[i][j] = Math.max(f[i][j], Math.max(g[i][j - 1], g[i + 1][j]));
            }
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int l = queries[i][0], r = queries[i][1];
            ans[i] = g[l][r];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maximumSubarrayXor(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        vector<vector<int>> f(n, vector<int>(n));
        vector<vector<int>> g(n, vector<int>(n));
        for (int i = n - 1; i >= 0; --i) {
            f[i][i] = nums[i];
            g[i][i] = nums[i];
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = f[i][j - 1] ^ f[i + 1][j];
                g[i][j] = max({f[i][j], g[i][j - 1], g[i + 1][j]});
            }
        }
        vector<int> ans;
        for (const auto& q : queries) {
            int l = q[0], r = q[1];
            ans.push_back(g[l][r]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSubarrayXor(nums []int, queries [][]int) (ans []int) {
	n := len(nums)
	f := make([][]int, n)
	g := make([][]int, n)
	for i := 0; i < n; i++ {
		f[i] = make([]int, n)
		g[i] = make([]int, n)
	}
	for i := n - 1; i >= 0; i-- {
		f[i][i] = nums[i]
		g[i][i] = nums[i]
		for j := i + 1; j < n; j++ {
			f[i][j] = f[i][j-1] ^ f[i+1][j]
			g[i][j] = max(f[i][j], max(g[i][j-1], g[i+1][j]))
		}
	}
	for _, q := range queries {
		l, r := q[0], q[1]
		ans = append(ans, g[l][r])
	}
	return
}
```

#### TypeScript

```ts
function maximumSubarrayXor(nums: number[], queries: number[][]): number[] {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = n - 1; i >= 0; i--) {
        f[i][i] = nums[i];
        g[i][i] = nums[i];
        for (let j = i + 1; j < n; j++) {
            f[i][j] = f[i][j - 1] ^ f[i + 1][j];
            g[i][j] = Math.max(f[i][j], Math.max(g[i][j - 1], g[i + 1][j]));
        }
    }
    return queries.map(([l, r]) => g[l][r]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
