---
comments: true
difficulty: Medium
rating: 2130
source: Weekly Contest 135 Q3
tags:
    - Array
    - Dynamic Programming
    - Polygon
    - Triangulation
---

<!-- problem:start -->

# [1039. Minimum Score Triangulation of Polygon](https://leetcode.com/problems/minimum-score-triangulation-of-polygon)

[中文文档](/solution/1000-1099/1039.Minimum%20Score%20Triangulation%20of%20Polygon/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đa giác lồi có <code>n</code> cạnh, mỗi đỉnh mang một giá trị nguyên. Mảng số nguyên <code>values</code> được cho sao cho <code>values[i]</code> là giá trị của đỉnh thứ <code>i</code> theo <strong>chiều kim đồng hồ</strong>.</p>

<p><strong>Tam giác hóa</strong> đa giác là quá trình chia đa giác thành các tam giác sao cho đỉnh của mỗi tam giác cũng là đỉnh của đa giác ban đầu. Không được dùng hình dạng nào khác ngoài tam giác. Quá trình này tạo ra <code>n - 2</code> tam giác.</p>

<p>Hãy <strong>tam giác hóa</strong> đa giác. <em>Trọng số</em> của mỗi tam giác là tích giá trị tại các đỉnh của nó. Tổng điểm của phép chia là tổng <em>trọng số</em> của cả <code>n - 2</code> tam giác.</p>

<p>Trả về <em>điểm nhỏ nhất có thể đạt được</em> khi <strong>tam giác hóa</strong> đa giác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1039.Minimum%20Score%20Triangulation%20of%20Polygon/images/ex0-2.png" style="width: 200px; height: 200px;" /></strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">values = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong> Đa giác đã là một tam giác, nên điểm của tam giác duy nhất là 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1039.Minimum%20Score%20Triangulation%20of%20Polygon/images/ex1-2.png" style="width: 432px; height: 200px;" /></p>

<p><strong>Đầu vào:</strong> <span class="example-io">values = [3,7,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">144</span></p>

<p><strong>Giải thích:</strong> Có hai cách chia, với điểm tương ứng là: 3*7*5 + 4*5*7 = 245 hoặc 3*4*5 + 3*4*7 = 144.<br />
Điểm nhỏ nhất là 144.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1039.Minimum%20Score%20Triangulation%20of%20Polygon/images/ex2.png" style="width: 200px; height: 200px;" />​​​​​​​</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">values = [1,3,1,4,1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong> Phép chia có điểm nhỏ nhất là 1*1*3 + 1*1*4 + 1*1*5 + 1*1*1 = 13.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == values.length</code></li>
	<li><code>3 &lt;= n &lt;= 50</code></li>
	<li><code>1 &lt;= values[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Số cách tam giác hóa là số Catalan; vì $n\le 50$, không thể liệt kê toàn bộ các cây. Mỗi cách tam giác hóa đoạn $(i,j)$ đều có một tam giác với đỉnh thứ ba là $k$ và hai đa giác con nhỏ hơn ở hai bên.
>
> Trạng thái là cặp $(i,j)$. Hàm memoization $\textit{dfs}(i,j)$ thử từng $k\in(i,j)$, rồi cộng $values[i]\cdot values[k]\cdot values[j]$ với kết quả của hai bài toán con. Hai đỉnh kề nhau trả về $0$.
>
> Đáp án là $\textit{dfs}(0,n-1)$.

<!-- thinking:end -->

Ta định nghĩa hàm $\text{dfs}(i, j)$ là điểm nhỏ nhất khi chia phần đa giác từ đỉnh $i$ đến đỉnh $j$. Đáp án là $\text{dfs}(0, n - 1)$.

Cách tính $\text{dfs}(i, j)$ như sau:

- Nếu $i + 1 = j$, phần đa giác chỉ có hai đỉnh và không thể chia thành tam giác, nên trả về $0$;
- Nếu không, duyệt các đỉnh $k$ nằm giữa $i$ và $j$, tức $i \lt k \lt j$. Chia phần đa giác từ đỉnh $i$ đến $j$ thành hai bài toán con: chia từ $i$ đến $k$ và từ $k$ đến $j$. Điểm nhỏ nhất của hai bài toán con lần lượt là $\text{dfs}(i, k)$ và $\text{dfs}(k, j)$. Điểm của tam giác tạo bởi các đỉnh $i$, $j$ và $k$ là $\text{values}[i] \times \text{values}[k] \times \text{values}[j]$. Do đó, điểm của cách chia này là $\text{dfs}(i, k) + \text{dfs}(k, j) + \text{values}[i] \times \text{values}[k] \times \text{values}[j]$. Lấy giá trị nhỏ nhất trong tất cả khả năng để có $\text{dfs}(i, j)$. 

Để tránh tính toán lặp lại, dùng memoization, tức lưu các giá trị hàm đã tính bằng hash table hoặc mảng.

Cuối cùng, trả về $\text{dfs}(0, n - 1)$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số đỉnh của đa giác.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minScoreTriangulation(self, values: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i + 1 == j:
                return 0
            return min(
                dfs(i, k) + dfs(k, j) + values[i] * values[k] * values[j]
                for k in range(i + 1, j)
            )

        return dfs(0, len(values) - 1)
```

#### Java

```java
class Solution {
    private int n;
    private int[] values;
    private Integer[][] f;

    public int minScoreTriangulation(int[] values) {
        n = values.length;
        this.values = values;
        f = new Integer[n][n];
        return dfs(0, n - 1);
    }

    private int dfs(int i, int j) {
        if (i + 1 == j) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = 1 << 30;
        for (int k = i + 1; k < j; ++k) {
            ans = Math.min(ans, dfs(i, k) + dfs(k, j) + values[i] * values[k] * values[j]);
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minScoreTriangulation(vector<int>& values) {
        int n = values.size();
        vector<vector<int>> f(n, vector<int>(n));
        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i + 1 == j) {
                return 0;
            }
            if (f[i][j]) {
                return f[i][j];
            }
            int ans = 1 << 30;
            for (int k = i + 1; k < j; ++k) {
                ans = min(ans, dfs(i, k) + dfs(k, j) + values[i] * values[k] * values[j]);
            }
            return f[i][j] = ans;
        };
        return dfs(0, n - 1);
    }
};
```

#### Go

```go
func minScoreTriangulation(values []int) int {
	n := len(values)
	f := [50][50]int{}
	var dfs func(int, int) int
	dfs = func(i, j int) int {
		if i+1 == j {
			return 0
		}
		if f[i][j] != 0 {
			return f[i][j]
		}
		f[i][j] = 1 << 30
		for k := i + 1; k < j; k++ {
			f[i][j] = min(f[i][j], dfs(i, k)+dfs(k, j)+values[i]*values[k]*values[j])
		}
		return f[i][j]
	}
	return dfs(0, n-1)
}
```

#### TypeScript

```ts
function minScoreTriangulation(values: number[]): number {
    const n = values.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => 0));
    const dfs = (i: number, j: number): number => {
        if (i + 1 === j) {
            return 0;
        }
        if (f[i][j] > 0) {
            return f[i][j];
        }
        let ans = 1 << 30;
        for (let k = i + 1; k < j; ++k) {
            ans = Math.min(ans, dfs(i, k) + dfs(k, j) + values[i] * values[k] * values[j]);
        }
        f[i][j] = ans;
        return ans;
    };
    return dfs(0, n - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Memoization vẫn dùng đệ quy và các trạng thái phụ thuộc vào những đoạn con. Có thể điền bảng từ dưới lên với cùng công thức chuyển, tính các đoạn ngắn trước rồi đến đoạn dài hơn.
>
> Duyệt $i$ giảm dần và $j$ tăng dần để tính $f[i][j]$, vẫn xét các giá trị $k$ như trước. Đáp án là $f[0][n-1]$.

<!-- thinking:end -->

Có thể chuyển cách memoization ở Lời giải 1 thành quy hoạch động.

Định nghĩa $f[i][j]$ là điểm nhỏ nhất khi chia phần đa giác từ đỉnh $i$ đến đỉnh $j$. Ban đầu, $f[i][j] = 0$, và đáp án là $f[0][n-1]$.

Với $f[i][j]$ (khi $i + 1 \lt j$), trước tiên khởi tạo $f[i][j]$ bằng $\infty$.

Duyệt các đỉnh $k$ nằm giữa $i$ và $j$, tức $i \lt k \lt j$. Chia phần đa giác từ đỉnh $i$ đến $j$ thành hai bài toán con: chia từ $i$ đến $k$ và từ $k$ đến $j$. Điểm nhỏ nhất của hai bài toán con lần lượt là $f[i][k]$ và $f[k][j]$. Điểm của tam giác tạo bởi các đỉnh $i$, $j$ và $k$ là $\text{values}[i] \times \text{values}[k] \times \text{values}[j]$. Vì vậy, điểm của cách chia này là $f[i][k] + f[k][j] + \text{values}[i] \times \text{values}[k] \times \text{values}[j]$. Lấy giá trị nhỏ nhất trong tất cả khả năng để cập nhật $f[i][j]$. 

Tóm lại, ta có công thức chuyển trạng thái:

$$
f[i][j]=
\begin{cases}
0, & i+1=j \\
\infty, & i+1<j \\
\min_{i<k<j} \{f[i][k]+f[k][j]+\text{values}[i] \times \text{values}[k] \times \text{values}[j]\}, & i+1<j
\end{cases}
$$

Khi duyệt $i$ và $j$, có hai cách sắp xếp thứ tự:

1. Duyệt $i$ từ lớn đến nhỏ và $j$ từ nhỏ đến lớn. Nhờ vậy, khi tính trạng thái $f[i][j]$, các trạng thái $f[i][k]$ và $f[k][j]$ đã được tính.
2. Duyệt độ dài đoạn $l$ từ nhỏ đến lớn, với $3 \leq l \leq n$. Sau đó duyệt đầu trái $i$ của đoạn và tính đầu phải theo công thức $j = i + l - 1$. Cách này cũng đảm bảo khi tính đoạn lớn $f[i][j]$, các đoạn nhỏ hơn $f[i][k]$ và $f[k][j]$ đã được tính.

Cuối cùng, trả về $f[0][n-1]$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số đỉnh của đa giác.

Bài toán liên quan:

- [1312. Minimum Insertion Steps to Make a String Palindrome](https://github.com/doocs/leetcode/blob/main/solution/1300-1399/1312.Minimum%20Insertion%20Steps%20to%20Make%20a%20String%20Palindrome/README.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minScoreTriangulation(self, values: List[int]) -> int:
        n = len(values)
        f = [[0] * n for _ in range(n)]
        for i in range(n - 3, -1, -1):
            for j in range(i + 2, n):
                f[i][j] = min(
                    f[i][k] + f[k][j] + values[i] * values[k] * values[j]
                    for k in range(i + 1, j)
                )
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int minScoreTriangulation(int[] values) {
        int n = values.length;
        int[][] f = new int[n][n];
        for (int i = n - 3; i >= 0; --i) {
            for (int j = i + 2; j < n; ++j) {
                f[i][j] = 1 << 30;
                for (int k = i + 1; k < j; ++k) {
                    f[i][j]
                        = Math.min(f[i][j], f[i][k] + f[k][j] + values[i] * values[k] * values[j]);
                }
            }
        }
        return f[0][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minScoreTriangulation(vector<int>& values) {
        int n = values.size();
        vector<vector<int>> f(n, vector<int>(n));
        for (int i = n - 3; i >= 0; --i) {
            for (int j = i + 2; j < n; ++j) {
                f[i][j] = 1 << 30;
                for (int k = i + 1; k < j; ++k) {
                    f[i][j] = min(f[i][j], f[i][k] + f[k][j] + values[i] * values[k] * values[j]);
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func minScoreTriangulation(values []int) int {
	n := len(values)
	f := [50][50]int{}
	for i := n - 3; i >= 0; i-- {
		for j := i + 2; j < n; j++ {
			f[i][j] = 1 << 30
			for k := i + 1; k < j; k++ {
				f[i][j] = min(f[i][j], f[i][k]+f[k][j]+values[i]*values[k]*values[j])
			}
		}
	}
	return f[0][n-1]
}
```

#### TypeScript

```ts
function minScoreTriangulation(values: number[]): number {
    const n = values.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => 0));
    for (let i = n - 3; i >= 0; --i) {
        for (let j = i + 2; j < n; ++j) {
            f[i][j] = 1 << 30;
            for (let k = i + 1; k < j; ++k) {
                f[i][j] = Math.min(f[i][j], f[i][k] + f[k][j] + values[i] * values[k] * values[j]);
            }
        }
    }
    return f[0][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Quy hoạch động (Cách cài đặt khác)

<!-- thinking:start -->

> **Tư duy**
>
> Khi duyệt hai đầu mút lồng nhau, cần đảm bảo các đoạn con đã được tính. Duyệt độ dài $l$ từ $3$ đến $n$, rồi duyệt $i$ và đặt $j=i+l-1$, sẽ đảm bảo hai đoạn con ngắn hơn đều đã có kết quả.
>
> Công thức truy hồi không đổi; chỉ thay thứ tự vòng lặp thành “độ dài rồi vị trí”.

<!-- thinking:end -->

Ở Lời giải 2, ta đã nêu hai cách duyệt. Ở đây, dùng cách thứ hai: duyệt độ dài đoạn $l$ từ nhỏ đến lớn, với $3 \leq l \leq n$. Sau đó duyệt đầu trái $i$ của đoạn và tính đầu phải theo công thức $j = i + l - 1$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số đỉnh của đa giác.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minScoreTriangulation(self, values: List[int]) -> int:
        n = len(values)
        f = [[0] * n for _ in range(n)]
        for l in range(3, n + 1):
            for i in range(n - l + 1):
                j = i + l - 1
                f[i][j] = min(
                    f[i][k] + f[k][j] + values[i] * values[k] * values[j]
                    for k in range(i + 1, j)
                )
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int minScoreTriangulation(int[] values) {
        int n = values.length;
        int[][] f = new int[n][n];
        for (int l = 3; l <= n; ++l) {
            for (int i = 0; i + l - 1 < n; ++i) {
                int j = i + l - 1;
                f[i][j] = 1 << 30;
                for (int k = i + 1; k < j; ++k) {
                    f[i][j]
                        = Math.min(f[i][j], f[i][k] + f[k][j] + values[i] * values[k] * values[j]);
                }
            }
        }
        return f[0][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minScoreTriangulation(vector<int>& values) {
        int n = values.size();
        vector<vector<int>> f(n, vector<int>(n));
        for (int l = 3; l <= n; ++l) {
            for (int i = 0; i + l - 1 < n; ++i) {
                int j = i + l - 1;
                f[i][j] = 1 << 30;
                for (int k = i + 1; k < j; ++k) {
                    f[i][j] = min(f[i][j], f[i][k] + f[k][j] + values[i] * values[k] * values[j]);
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func minScoreTriangulation(values []int) int {
	n := len(values)
	f := [50][50]int{}
	for l := 3; l <= n; l++ {
		for i := 0; i+l-1 < n; i++ {
			j := i + l - 1
			f[i][j] = 1 << 30
			for k := i + 1; k < j; k++ {
				f[i][j] = min(f[i][j], f[i][k]+f[k][j]+values[i]*values[k]*values[j])
			}
		}
	}
	return f[0][n-1]
}
```

#### TypeScript

```ts
function minScoreTriangulation(values: number[]): number {
    const n = values.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => 0));
    for (let l = 3; l <= n; ++l) {
        for (let i = 0; i + l - 1 < n; ++i) {
            const j = i + l - 1;
            f[i][j] = 1 << 30;
            for (let k = i + 1; k < j; ++k) {
                f[i][j] = Math.min(f[i][j], f[i][k] + f[k][j] + values[i] * values[k] * values[j]);
            }
        }
    }
    return f[0][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
