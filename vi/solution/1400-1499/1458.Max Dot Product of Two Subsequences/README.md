---
comments: true
difficulty: Hard
rating: 1823
source: Weekly Contest 190 Q4
tags:
    - Array
    - Dynamic Programming
    - Longest Common Subsequence
---

<!-- problem:start -->

# [1458. Max Dot Product of Two Subsequences](https://leetcode.com/problems/max-dot-product-of-two-subsequences)

[中文文档](/solution/1400-1499/1458.Max%20Dot%20Product%20of%20Two%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng <code>nums1</code>&nbsp;và <code><font face="monospace">nums2</font></code><font face="monospace">.</font></p>

<p>Trả về tích vô hướng lớn nhất&nbsp;giữa&nbsp;các dãy con <strong>không rỗng</strong> của nums1 và nums2 có cùng độ dài.</p>

<p>Dãy con của một mảng là một mảng mới được tạo từ mảng ban đầu bằng cách xóa một số phần tử (có thể không xóa phần tử nào) mà không làm thay đổi thứ tự tương đối của các phần tử còn lại. (Ví dụ,&nbsp;<code>[2,3,5]</code>&nbsp;là một dãy con của&nbsp;<code>[1,2,3,4,5]</code>&nbsp;trong khi <code>[1,5,3]</code>&nbsp;thì không).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,1,-2,5], nums2 = [3,0,-6]
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Chọn dãy con [2,-2] từ nums1 và dãy con [3,-6] từ nums2.
Tích vô hướng của chúng là (2*3 + (-2)*(-6)) = 18.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [3,-2], nums2 = [2,-6,7]
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong> Chọn dãy con [3] từ nums1 và dãy con [7] từ nums2.
Tích vô hướng của chúng là (3*7) = 21.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [-1,-1], nums2 = [1,1]
<strong>Đầu ra:</strong> -1
<strong>Giải thích: </strong>Chọn dãy con [-1] từ nums1 và dãy con [1] từ nums2.
Tích vô hướng của chúng là -1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums1.length, nums2.length &lt;= 500</code></li>
    <li><code>-1000 &lt;= nums1[i], nums2[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con phải không rỗng và các giá trị có thể âm. $m,n\le 500$. Nếu chỉ lấy các tích dương như LCS, ta sẽ bỏ sót trường hợp chỉ có một cặp phần tử âm.
>
> $f[i][j]$ là tích vô hướng tốt nhất của hai prefix: bỏ một đầu mút, hoặc ghép chúng rồi tùy chọn bỏ qua prefix âm qua $\max(0,f[i-1][j-1])+x\cdot y$. $-\infty$ đảm bảo phải có ít nhất một cặp phần tử.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ biểu diễn tích vô hướng lớn nhất của hai dãy con được tạo từ $i$ phần tử đầu tiên của $\textit{nums1}$ và $j$ phần tử đầu tiên của $\textit{nums2}$. Ban đầu, $f[i][j] = -\infty$.

Với $f[i][j]$, ta có các trường hợp sau:

1. Không chọn $\textit{nums1}[i-1]$ hoặc không chọn $\textit{nums2}[j-1]$, tức là $f[i][j] = \max(f[i-1][j], f[i][j-1])$;
2. Chọn $\textit{nums1}[i-1]$ và $\textit{nums2}[j-1]$, tức là $f[i][j] = \max(f[i][j], \max(0, f[i-1][j-1]) + \textit{nums1}[i-1] \times \textit{nums2}[j-1])$.

Đáp án cuối cùng là $f[m][n]$.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là độ dài của các mảng $\textit{nums1}$ và $\textit{nums2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDotProduct(self, nums1: List[int], nums2: List[int]) -> int:
        m, n = len(nums1), len(nums2)
        f = [[-inf] * (n + 1) for _ in range(m + 1)]
        for i, x in enumerate(nums1, 1):
            for j, y in enumerate(nums2, 1):
                v = x * y
                f[i][j] = max(f[i - 1][j], f[i][j - 1], max(0, f[i - 1][j - 1]) + v)
        return f[m][n]
```

#### Java

```java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int[][] f = new int[m + 1][n + 1];
        for (var g : f) {
            Arrays.fill(g, Integer.MIN_VALUE);
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int v = nums1[i - 1] * nums2[j - 1];
                f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
                f[i][j] = Math.max(f[i][j], Math.max(f[i - 1][j - 1], 0) + v);
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
    int maxDotProduct(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        int f[m + 1][n + 1];
        memset(f, 0xc0, sizeof f);
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int v = nums1[i - 1] * nums2[j - 1];
                f[i][j] = max(f[i - 1][j], f[i][j - 1]);
                f[i][j] = max(f[i][j], max(0, f[i - 1][j - 1]) + v);
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func maxDotProduct(nums1 []int, nums2 []int) int {
	m, n := len(nums1), len(nums2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
		for j := range f[i] {
			f[i][j] = math.MinInt32
		}
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			v := nums1[i-1] * nums2[j-1]
			f[i][j] = max(f[i-1][j], f[i][j-1])
			f[i][j] = max(f[i][j], max(0, f[i-1][j-1])+v)
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function maxDotProduct(nums1: number[], nums2: number[]): number {
    const m = nums1.length;
    const n = nums2.length;
    const f = Array.from({ length: m + 1 }, () => Array.from({ length: n + 1 }, () => -Infinity));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            const v = nums1[i - 1] * nums2[j - 1];
            f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
            f[i][j] = Math.max(f[i][j], Math.max(0, f[i - 1][j - 1]) + v);
        }
    }
    return f[m][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn max_dot_product(nums1: Vec<i32>, nums2: Vec<i32>) -> i32 {
        let m = nums1.len();
        let n = nums2.len();
        let mut f = vec![vec![i32::MIN; n + 1]; m + 1];

        for i in 1..=m {
            for j in 1..=n {
                let v = nums1[i - 1] * nums2[j - 1];
                f[i][j] = f[i][j].max(f[i - 1][j]).max(f[i][j - 1]);
                f[i][j] = f[i][j].max(f[i - 1][j - 1].max(0) + v);
            }
        }

        f[m][n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
