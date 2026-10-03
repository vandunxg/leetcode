---
comments: true
difficulty: Medium
rating: 1671
source: Weekly Contest 225 Q3
tags:
    - Bit Manipulation
    - Array
    - Divide and Conquer
    - Matrix
    - Prefix Sum
    - Quickselect
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1738. Find Kth Largest XOR Coordinate Value](https://leetcode.com/problems/find-kth-largest-xor-coordinate-value)

[中文文档](/solution/1700-1799/1738.Find%20Kth%20Largest%20XOR%20Coordinate%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <code>matrix</code> hai chiều kích thước <code>m x n</code>, gồm các số nguyên không âm, cùng một số nguyên <code>k</code>.</p>

<p><strong>Giá trị</strong> của tọa độ <code>(a, b)</code> trong ma trận là XOR của mọi <code>matrix[i][j]</code> với <code>0 &lt;= i &lt;= a &lt; m</code> và <code>0 &lt;= j &lt;= b &lt; n</code> <strong>(đánh chỉ số từ 0)</strong>.</p>

<p>Hãy tìm giá trị lớn thứ <code>k<sup>th</sup></code> <strong>(đánh chỉ số từ 1)</strong> trong tất cả các tọa độ của <code>matrix</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> matrix = [[5,2],[1,6]], k = 1
<strong>Output:</strong> 7
<strong>Giải thích:</strong> Giá trị của tọa độ (0,1) là 5 XOR 2 = 7, đây là giá trị lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> matrix = [[5,2],[1,6]], k = 2
<strong>Output:</strong> 5
<strong>Giải thích:</strong> Giá trị của tọa độ (0,0) là 5 = 5, đây là giá trị lớn thứ hai.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> matrix = [[5,2],[1,6]], k = 3
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Giá trị của tọa độ (1,0) là 5 XOR 1 = 4, đây là giá trị lớn thứ ba.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 1000</code></li>
	<li><code>0 &lt;= matrix[i][j] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= m * n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: XOR tiền tố hai chiều + Sắp xếp hoặc Quickselect

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị tại $(i,j)$ là XOR của hình chữ nhật tiền tố. Tính lại từng hình chữ nhật có độ phức tạp $O(m^2n^2)$ nên không phù hợp khi $m,n\le 1000$.
>
> XOR tiền tố hai chiều thỏa mãn $s[i][j]=s[i-1][j]\oplus s[i][j-1]\oplus s[i-1][j-1]\oplus matrix[i-1][j-1]$, nên có thể liệt kê mọi giá trị trong $O(mn)$.
>
> Chọn phần tử lớn thứ $k$ trong danh sách bằng cách sắp xếp hoặc dùng heap.

<!-- thinking:end -->

Ta định nghĩa mảng XOR tiền tố hai chiều $s$, trong đó $s[i][j]$ biểu diễn kết quả XOR của các phần tử trong $i$ hàng đầu tiên và $j$ cột đầu tiên của ma trận, tức là

$$
s[i][j] = \bigoplus_{0 \leq x \leq i, 0 \leq y \leq j} matrix[x][y]
$$

Và có thể tính $s[i][j]$ từ ba phần tử $s[i - 1][j]$, $s[i][j - 1]$ và $s[i - 1][j - 1]$, tức là

$$
s[i][j] = s[i - 1][j] \oplus s[i][j - 1] \oplus s[i - 1][j - 1] \oplus matrix[i - 1][j - 1]
$$

Ta duyệt ma trận, tính tất cả $s[i][j]$, sau đó sắp xếp chúng và trả về phần tử lớn thứ $k$. Nếu không muốn sắp xếp, có thể dùng thuật toán quickselect để tối ưu độ phức tạp thời gian.

Độ phức tạp thời gian là $O(m \times n \times \log (m \times n))$ hoặc $O(m \times n)$, còn độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthLargestValue(self, matrix: List[List[int]], k: int) -> int:
        m, n = len(matrix), len(matrix[0])
        s = [[0] * (n + 1) for _ in range(m + 1)]
        ans = []
        for i in range(m):
            for j in range(n):
                s[i + 1][j + 1] = s[i + 1][j] ^ s[i][j + 1] ^ s[i][j] ^ matrix[i][j]
                ans.append(s[i + 1][j + 1])
        return nlargest(k, ans)[-1]
```

#### Java

```java
class Solution {
    public int kthLargestValue(int[][] matrix, int k) {
        int m = matrix.length, n = matrix[0].length;
        int[][] s = new int[m + 1][n + 1];
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                s[i + 1][j + 1] = s[i + 1][j] ^ s[i][j + 1] ^ s[i][j] ^ matrix[i][j];
                ans.add(s[i + 1][j + 1]);
            }
        }
        Collections.sort(ans);
        return ans.get(ans.size() - k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kthLargestValue(vector<vector<int>>& matrix, int k) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> s(m + 1, vector<int>(n + 1));
        vector<int> ans;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                s[i + 1][j + 1] = s[i + 1][j] ^ s[i][j + 1] ^ s[i][j] ^ matrix[i][j];
                ans.push_back(s[i + 1][j + 1]);
            }
        }
        sort(ans.begin(), ans.end());
        return ans[ans.size() - k];
    }
};
```

#### Go

```go
func kthLargestValue(matrix [][]int, k int) int {
	m, n := len(matrix), len(matrix[0])
	s := make([][]int, m+1)
	for i := range s {
		s[i] = make([]int, n+1)
	}
	var ans []int
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			s[i+1][j+1] = s[i+1][j] ^ s[i][j+1] ^ s[i][j] ^ matrix[i][j]
			ans = append(ans, s[i+1][j+1])
		}
	}
	sort.Ints(ans)
	return ans[len(ans)-k]
}
```

#### TypeScript

```ts
function kthLargestValue(matrix: number[][], k: number): number {
    const m: number = matrix.length;
    const n: number = matrix[0].length;
    const s = Array.from({ length: m + 1 }, () => Array.from({ length: n + 1 }, () => 0));
    const ans: number[] = [];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            s[i + 1][j + 1] = s[i + 1][j] ^ s[i][j + 1] ^ s[i][j] ^ matrix[i][j];
            ans.push(s[i + 1][j + 1]);
        }
    }
    ans.sort((a, b) => b - a);
    return ans[k - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
