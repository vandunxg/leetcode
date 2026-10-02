---
comments: true
difficulty: Medium
rating: 1919
source: Weekly Contest 146 Q3
tags:
    - Stack
    - Greedy
    - Array
    - Dynamic Programming
    - Cartesian Tree
    - Monotonic Stack
---

<!-- problem:start -->

# [1130. Minimum Cost Tree From Leaf Values](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values)

[中文文档](/solution/1100-1199/1130.Minimum%20Cost%20Tree%20From%20Leaf%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <code>arr</code>, xét tất cả cây nhị phân thỏa mãn:</p>

<ul>
	<li>Mỗi node có <code>0</code> hoặc <code>2</code> node con;</li>
	<li>Các giá trị trong <code>arr</code> lần lượt tương ứng với giá trị của từng <strong>lá</strong> khi duyệt inorder cây.</li>
	<li>Giá trị của mỗi node không phải lá bằng tích giữa giá trị lá lớn nhất trong cây con trái và giá trị lá lớn nhất trong cây con phải.</li>
</ul>

<p>Trong tất cả cây nhị phân có thể tạo, hãy trả về <em>tổng nhỏ nhất có thể của giá trị các node không phải lá</em>. Đảm bảo tổng này vừa với số nguyên <strong>32-bit</strong>.</p>

<p>Node là <strong>lá</strong> khi và chỉ khi nó không có node con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1130.Minimum%20Cost%20Tree%20From%20Leaf%20Values/images/tree1.jpg" style="width: 500px; height: 169px;" />
<pre>
<strong>Input:</strong> arr = [6,2,4]
<strong>Output:</strong> 32
<strong>Giải thích:</strong> Có hai cây có thể tạo như hình.
Cây thứ nhất có tổng giá trị các node không phải lá là 36, còn cây thứ hai có tổng là 32.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1130.Minimum%20Cost%20Tree%20From%20Leaf%20Values/images/tree2.jpg" style="width: 224px; height: 145px;" />
<pre>
<strong>Input:</strong> arr = [4,11]
<strong>Output:</strong> 44
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 40</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 15</code></li>
	<li>Đảm bảo đáp án vừa với số nguyên có dấu <strong>32-bit</strong> (tức là nhỏ hơn 2<sup>31</sup>).</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự các lá khi duyệt inorder đã cố định; cấu trúc cây được quyết định bởi vị trí chia. Nếu thử mọi vị trí chia bằng đệ quy mà không ghi nhớ, ta sẽ tính lại cùng một đoạn nhiều lần.
>
> $dfs(i,j)$ trả về chi phí nhỏ nhất của các node không phải lá và giá trị lá lớn nhất trong đoạn $[i,j]$. Thử từng vị trí chia $k$ và cộng tích của hai giá trị lá lớn nhất. Có $O(n^2)$ đoạn và mỗi đoạn xét $O(n)$ vị trí chia, nên khi dùng ghi nhớ, độ phức tạp là $O(n^3)$.

<!-- thinking:end -->

Theo đề bài, các giá trị trong mảng $arr$ tương ứng lần lượt với giá trị các node lá theo thứ tự duyệt inorder của cây. Ta có thể chia mảng thành hai mảng con không rỗng, tương ứng với cây con trái và cây con phải, rồi đệ quy tìm tổng giá trị nhỏ nhất của các node không phải lá trong từng cây con.

Ta định nghĩa hàm $dfs(i, j)$ là tổng nhỏ nhất có thể của giá trị các node không phải lá trong đoạn chỉ số $[i, j]$ của mảng $arr$. Đáp án là $dfs(0, n - 1)$, với $n$ là độ dài mảng $arr$.

Hàm $dfs(i, j)$ được tính như sau:

- Nếu $i = j$, đoạn $arr[i..j]$ chỉ có một phần tử và không có node nào không phải lá, nên $dfs(i, j) = 0$.
- Nếu không, duyệt $k \in [i, j - 1]$ để chia mảng $arr$ thành hai mảng con $arr[i..k]$ và $arr[k + 1..j]$. Với mỗi $k$, tính đệ quy $dfs(i, k)$ và $dfs(k + 1, j)$. Trong đó, $dfs(i, k)$ là tổng nhỏ nhất của các node không phải lá trong đoạn chỉ số $[i, k]$ của mảng $arr$, còn $dfs(k + 1, j)$ là tổng nhỏ nhất tương ứng trong đoạn $[k + 1, j]$. Khi đó, $dfs(i, j) = \min_{i \leq k < j} \{dfs(i, k) + dfs(k + 1, j) + \max_{i \leq t \leq k} \{arr[t]\} \max_{k < t \leq j} \{arr[t]\}\}$.

Tóm lại, ta có:

$$
dfs(i, j) = \begin{cases}
0, & \textit{if } i = j \\
\min_{i \leq k < j} \{dfs(i, k) + dfs(k + 1, j) + \max_{i \leq t \leq k} \{arr[t]\} \max_{k < t \leq j} \{arr[t]\}\}, & \textit{if } i < j
\end{cases}
$$

Trong quá trình đệ quy trên, có thể dùng ghi nhớ để tránh tính lại. Ngoài ra, dùng mảng $g$ lưu giá trị lớn nhất của các node lá trong đoạn chỉ số $[i, j]$ của mảng $arr$. Nhờ đó, ta tối ưu được cách tính $dfs(i, j)$:

$$
dfs(i, j) = \begin{cases}
0, & \textit{if } i = j \\
\min_{i \leq k < j} \{dfs(i, k) + dfs(k + 1, j) + g[i][k] \cdot g[k + 1][j]\}, & \textit{if } i < j
\end{cases}
$$

Cuối cùng, trả về $dfs(0, n - 1)$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, với $n$ là độ dài mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mctFromLeafValues(self, arr: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> Tuple:
            if i == j:
                return 0, arr[i]
            s, mx = inf, -1
            for k in range(i, j):
                s1, mx1 = dfs(i, k)
                s2, mx2 = dfs(k + 1, j)
                t = s1 + s2 + mx1 * mx2
                if s > t:
                    s = t
                    mx = max(mx1, mx2)
            return s, mx

        return dfs(0, len(arr) - 1)[0]
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[][] g;

    public int mctFromLeafValues(int[] arr) {
        int n = arr.length;
        f = new Integer[n][n];
        g = new int[n][n];
        for (int i = n - 1; i >= 0; --i) {
            g[i][i] = arr[i];
            for (int j = i + 1; j < n; ++j) {
                g[i][j] = Math.max(g[i][j - 1], arr[j]);
            }
        }
        return dfs(0, n - 1);
    }

    private int dfs(int i, int j) {
        if (i == j) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = 1 << 30;
        for (int k = i; k < j; k++) {
            ans = Math.min(ans, dfs(i, k) + dfs(k + 1, j) + g[i][k] * g[k + 1][j]);
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mctFromLeafValues(vector<int>& arr) {
        int n = arr.size();
        int f[n][n];
        int g[n][n];
        memset(f, 0, sizeof(f));
        for (int i = n - 1; ~i; --i) {
            g[i][i] = arr[i];
            for (int j = i + 1; j < n; ++j) {
                g[i][j] = max(g[i][j - 1], arr[j]);
            }
        }
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i == j) {
                return 0;
            }
            if (f[i][j] > 0) {
                return f[i][j];
            }
            int ans = 1 << 30;
            for (int k = i; k < j; ++k) {
                ans = min(ans, dfs(i, k) + dfs(k + 1, j) + g[i][k] * g[k + 1][j]);
            }
            return f[i][j] = ans;
        };
        return dfs(0, n - 1);
    }
};
```

#### Go

```go
func mctFromLeafValues(arr []int) int {
	n := len(arr)
	f := make([][]int, n)
	g := make([][]int, n)
	for i := range g {
		f[i] = make([]int, n)
		g[i] = make([]int, n)
		g[i][i] = arr[i]
		for j := i + 1; j < n; j++ {
			g[i][j] = max(g[i][j-1], arr[j])
		}
	}
	var dfs func(int, int) int
	dfs = func(i, j int) int {
		if i == j {
			return 0
		}
		if f[i][j] > 0 {
			return f[i][j]
		}
		f[i][j] = 1 << 30
		for k := i; k < j; k++ {
			f[i][j] = min(f[i][j], dfs(i, k)+dfs(k+1, j)+g[i][k]*g[k+1][j])
		}
		return f[i][j]
	}
	return dfs(0, n-1)
}
```

#### TypeScript

```ts
function mctFromLeafValues(arr: number[]): number {
    const n = arr.length;
    const f: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(0));
    const g: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(0));
    for (let i = n - 1; i >= 0; --i) {
        g[i][i] = arr[i];
        for (let j = i + 1; j < n; ++j) {
            g[i][j] = Math.max(g[i][j - 1], arr[j]);
        }
    }
    const dfs = (i: number, j: number): number => {
        if (i === j) {
            return 0;
        }
        if (f[i][j] > 0) {
            return f[i][j];
        }
        let ans = 1 << 30;
        for (let k = i; k < j; ++k) {
            ans = Math.min(ans, dfs(i, k) + dfs(k + 1, j) + g[i][k] * g[k + 1][j]);
        }
        return (f[i][j] = ans);
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
> Đệ quy và cache ở cách 1 được chuyển thành quy hoạch động trên đoạn. Tính trước $g[i][j]$ là giá trị lá lớn nhất, rồi điền $f[i][j]$ theo độ dài đoạn tăng dần. Công thức chuyển trạng thái giống cách tìm kiếm có ghi nhớ, nhưng không cần call stack.

<!-- thinking:end -->

Có thể chuyển cách tìm kiếm có ghi nhớ ở lời giải 1 thành quy hoạch động.

Định nghĩa $f[i][j]$ là tổng nhỏ nhất có thể của giá trị các node không phải lá trong đoạn chỉ số $[i, j]$ của mảng $arr$, và $g[i][j]$ là giá trị lớn nhất của các node lá trong đoạn đó. Khi đó, công thức chuyển trạng thái là:

$$
f[i][j] = \begin{cases}
0, & \textit{if } i = j \\
\min_{i \leq k < j} \{f[i][k] + f[k + 1][j] + g[i][k] \cdot g[k + 1][j]\}, & \textit{if } i < j
\end{cases}
$$

Cuối cùng, trả về $f[0][n - 1]$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, với $n$ là độ dài mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mctFromLeafValues(self, arr: List[int]) -> int:
        n = len(arr)
        f = [[0] * n for _ in range(n)]
        g = [[0] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            g[i][i] = arr[i]
            for j in range(i + 1, n):
                g[i][j] = max(g[i][j - 1], arr[j])
                f[i][j] = min(
                    f[i][k] + f[k + 1][j] + g[i][k] * g[k + 1][j] for k in range(i, j)
                )
        return f[0][n - 1]
```

#### Java

```java
class Solution {
    public int mctFromLeafValues(int[] arr) {
        int n = arr.length;
        int[][] f = new int[n][n];
        int[][] g = new int[n][n];
        for (int i = n - 1; i >= 0; --i) {
            g[i][i] = arr[i];
            for (int j = i + 1; j < n; ++j) {
                g[i][j] = Math.max(g[i][j - 1], arr[j]);
                f[i][j] = 1 << 30;
                for (int k = i; k < j; ++k) {
                    f[i][j] = Math.min(f[i][j], f[i][k] + f[k + 1][j] + g[i][k] * g[k + 1][j]);
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
    int mctFromLeafValues(vector<int>& arr) {
        int n = arr.size();
        int f[n][n];
        int g[n][n];
        memset(f, 0, sizeof(f));
        for (int i = n - 1; ~i; --i) {
            g[i][i] = arr[i];
            for (int j = i + 1; j < n; ++j) {
                g[i][j] = max(g[i][j - 1], arr[j]);
                f[i][j] = 1 << 30;
                for (int k = i; k < j; ++k) {
                    f[i][j] = min(f[i][j], f[i][k] + f[k + 1][j] + g[i][k] * g[k + 1][j]);
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func mctFromLeafValues(arr []int) int {
	n := len(arr)
	f := make([][]int, n)
	g := make([][]int, n)
	for i := range g {
		f[i] = make([]int, n)
		g[i] = make([]int, n)
	}
	for i := n - 1; i >= 0; i-- {
		g[i][i] = arr[i]
		for j := i + 1; j < n; j++ {
			g[i][j] = max(g[i][j-1], arr[j])
			f[i][j] = 1 << 30
			for k := i; k < j; k++ {
				f[i][j] = min(f[i][j], f[i][k]+f[k+1][j]+g[i][k]*g[k+1][j])
			}
		}
	}
	return f[0][n-1]
}
```

#### TypeScript

```ts
function mctFromLeafValues(arr: number[]): number {
    const n = arr.length;
    const f: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(0));
    const g: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(0));
    for (let i = n - 1; i >= 0; --i) {
        g[i][i] = arr[i];
        for (let j = i + 1; j < n; ++j) {
            g[i][j] = Math.max(g[i][j - 1], arr[j]);
            f[i][j] = 1 << 30;
            for (let k = i; k < j; ++k) {
                f[i][j] = Math.min(f[i][j], f[i][k] + f[k + 1][j] + g[i][k] * g[k + 1][j]);
            }
        }
    }
    return f[0][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
