---
comments: true
difficulty: Medium
rating: 1734
source: Weekly Contest 167 Q3
tags:
    - Array
    - Binary Search
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [1292. Maximum Side Length of a Square with Sum Less than or Equal to Threshold](https://leetcode.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold)

[中文文档](/solution/1200-1299/1292.Maximum%20Side%20Length%20of%20a%20Square%20with%20Sum%20Less%20than%20or%20Equal%20to%20Threshold/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> <code>mat</code> và số nguyên <code>threshold</code>. Hãy trả về <em>độ dài cạnh lớn nhất của hình vuông có tổng các phần tử không vượt quá </em><code>threshold</code><em>; nếu không có hình vuông nào như vậy thì trả về </em><code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1292.Maximum%20Side%20Length%20of%20a%20Square%20with%20Sum%20Less%20than%20or%20Equal%20to%20Threshold/images/e1.png" style="width: 335px; height: 186px;" />
<pre>
<strong>Input:</strong> mat = [[1,1,3,2,4,3,2],[1,1,3,2,4,3,2],[1,1,3,2,4,3,2]], threshold = 4
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Như hình minh họa, độ dài cạnh lớn nhất của hình vuông có tổng không vượt quá 4 là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> mat = [[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2]], threshold = 1
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>0 &lt;= mat[i][j] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= threshold &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix sum 2D + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Tổng của hình vuông không giảm khi cạnh tăng (các phần tử đều không âm), nên tính khả thi có tính đơn điệu. Vì $m,n \le 300$, duyệt các độ dài cạnh và các vị trí góc trên trái, đồng thời tính tổng mỗi hình vuông trong $O(k^2)$ sẽ quá chậm. Prefix sum 2D giúp tính tổng mỗi hình vuông trong $O(1)$, sau đó ta tìm kiếm nhị phân độ dài cạnh.
>
> Hàm kiểm tra duyệt các vị trí góc trên trái và so sánh prefix sum với threshold. Tìm kiếm nhị phân trả về độ dài cạnh lớn nhất thỏa điều kiện.

<!-- thinking:end -->

Ta tính trước mảng prefix sum 2D $s$, trong đó $s[i + 1][j + 1]$ là tổng các phần tử của ma trận $mat$ trong hình chữ nhật từ $(0, 0)$ đến $(i, j)$. Nhờ đó, ta có thể tính tổng phần tử trong bất kỳ vùng hình vuông nào trong $O(1)$.

Tiếp theo, ta dùng tìm kiếm nhị phân để tìm độ dài cạnh lớn nhất. Với mỗi cạnh $k$ cần kiểm tra, ta duyệt mọi vị trí $(i, j)$ có thể là góc trên trái của hình vuông và tính tổng phần tử $v$ trong đó. Nếu $v \leq threshold$, tồn tại hình vuông cạnh $k$ có tổng không vượt quá threshold; nếu không, không có hình vuông nào như vậy với cạnh $k$ hiện tại.

Độ phức tạp thời gian là $O(m \times n \times \log \min(m, n))$ và độ phức tạp không gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSideLength(self, mat: List[List[int]], threshold: int) -> int:
        def check(k: int) -> bool:
            for i in range(m - k + 1):
                for j in range(n - k + 1):
                    v = s[i + k][j + k] - s[i][j + k] - s[i + k][j] + s[i][j]
                    if v <= threshold:
                        return True
            return False

        m, n = len(mat), len(mat[0])
        s = [[0] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(mat, 1):
            for j, x in enumerate(row, 1):
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x
        l, r = 0, min(m, n)
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int threshold;
    private int[][] s;

    public int maxSideLength(int[][] mat, int threshold) {
        m = mat.length;
        n = mat[0].length;
        this.threshold = threshold;
        s = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + mat[i - 1][j - 1];
            }
        }
        int l = 0, r = Math.min(m, n);
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int k) {
        for (int i = 0; i < m - k + 1; ++i) {
            for (int j = 0; j < n - k + 1; ++j) {
                if (s[i + k][j + k] - s[i][j + k] - s[i + k][j] + s[i][j] <= threshold) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSideLength(vector<vector<int>>& mat, int threshold) {
        int m = mat.size(), n = mat[0].size();
        int s[m + 1][n + 1];
        memset(s, 0, sizeof(s));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + mat[i - 1][j - 1];
            }
        }
        auto check = [&](int k) {
            for (int i = 0; i < m - k + 1; ++i) {
                for (int j = 0; j < n - k + 1; ++j) {
                    if (s[i + k][j + k] - s[i][j + k] - s[i + k][j] + s[i][j] <= threshold) {
                        return true;
                    }
                }
            }
            return false;
        };
        int l = 0, r = min(m, n);
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maxSideLength(mat [][]int, threshold int) int {
	m, n := len(mat), len(mat[0])
	s := make([][]int, m+1)
	for i := range s {
		s[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + mat[i-1][j-1]
		}
	}
	check := func(k int) bool {
		for i := 0; i < m-k+1; i++ {
			for j := 0; j < n-k+1; j++ {
				if s[i+k][j+k]-s[i][j+k]-s[i+k][j]+s[i][j] <= threshold {
					return true
				}
			}
		}
		return false
	}
	l, r := 0, min(m, n)
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function maxSideLength(mat: number[][], threshold: number): number {
    const m = mat.length;
    const n = mat[0].length;
    const s: number[][] = Array(m + 1)
        .fill(0)
        .map(() => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + mat[i - 1][j - 1];
        }
    }
    const check = (k: number): boolean => {
        for (let i = 0; i < m - k + 1; ++i) {
            for (let j = 0; j < n - k + 1; ++j) {
                if (s[i + k][j + k] - s[i + k][j] - s[i][j + k] + s[i][j] <= threshold) {
                    return true;
                }
            }
        }
        return false;
    };

    let l = 0;
    let r = Math.min(m, n);
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_side_length(mat: Vec<Vec<i32>>, threshold: i32) -> i32 {
        let m = mat.len();
        let n = mat[0].len();
        let mut s = vec![vec![0; n + 1]; m + 1];
        for i in 1..=m {
            for j in 1..=n {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + mat[i - 1][j - 1];
            }
        }
        let check = |k: usize| -> bool {
            for i in 0..=m - k {
                for j in 0..=n - k {
                    if s[i + k][j + k] - s[i][j + k] - s[i + k][j] + s[i][j] <= threshold {
                        return true;
                    }
                }
            }
            false
        };
        let mut l = 0usize;
        let mut r = m.min(n);
        while l < r {
            let mid = (l + r + 1) >> 1;
            if check(mid) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        l as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
