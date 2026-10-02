---
comments: true
difficulty: Easy
rating: 1280
source: Biweekly Contest 34 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [1572. Matrix Diagonal Sum](https://leetcode.com/problems/matrix-diagonal-sum)

[中文文档](/solution/1500-1599/1572.Matrix%20Diagonal%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận vuông <code>mat</code>, hãy trả về tổng các đường chéo của ma trận.</p>

<p>Chỉ tính tổng tất cả phần tử trên đường chéo chính và các phần tử trên đường chéo phụ không thuộc đường chéo chính.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1572.Matrix%20Diagonal%20Sum/images/sample_1911.png" style="width: 336px; height: 174px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[<strong>1</strong>,2,<strong>3</strong>],
&nbsp;             [4,<strong>5</strong>,6],
&nbsp;             [<strong>7</strong>,8,<strong>9</strong>]]
<strong>Đầu ra:</strong> 25
<strong>Giải thích: </strong>Tổng các đường chéo: 1 + 5 + 9 + 3 + 7 = 25
Lưu ý rằng phần tử mat[1][1] = 5 chỉ được tính một lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[<strong>1</strong>,1,1,<strong>1</strong>],
&nbsp;             [1,<strong>1</strong>,<strong>1</strong>,1],
&nbsp;             [1,<strong>1</strong>,<strong>1</strong>,1],
&nbsp;             [<strong>1</strong>,1,1,<strong>1</strong>]]
<strong>Đầu ra:</strong> 8
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[<strong>5</strong>]]
<strong>Đầu ra:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == mat.length == mat[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt từng hàng

<!-- thinking:start -->

> **Tư duy**
>
> Cần tính tổng hai đường chéo của ma trận vuông; phần tử trung tâm khi bậc ma trận lẻ sẽ bị tính hai lần. $n$ nhỏ, nên lấy hai phần tử đường chéo ở mỗi hàng và bỏ qua phần tử trung tâm trùng nhau.
>
> Hàng $i$ đóng góp $row[i]$ và $row[n-i-1]$. Khi hai chỉ số trùng nhau, chỉ cộng một lần; nếu không thì cộng cả hai. Chỉ cần duyệt qua các hàng một lần là tính được tổng.

<!-- thinking:end -->

Ta có thể duyệt từng hàng $\textit{row}[i]$ của ma trận. Với mỗi hàng, ta lấy các phần tử trên hai đường chéo là $\textit{row}[i][i]$ và $\textit{row}[i][n - i - 1]$, trong đó $n$ là số hàng của ma trận. Nếu $i = n - i - 1$, hai đường chéo chỉ có một phần tử ở hàng hiện tại; nếu không thì có hai phần tử. Ta cộng các phần tử này vào đáp án.

Sau khi duyệt hết các hàng, ta thu được đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số hàng của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def diagonalSum(self, mat: List[List[int]]) -> int:
        ans = 0
        n = len(mat)
        for i, row in enumerate(mat):
            j = n - i - 1
            ans += row[i] + (0 if j == i else row[j])
        return ans
```

#### Java

```java
class Solution {
    public int diagonalSum(int[][] mat) {
        int ans = 0;
        int n = mat.length;
        for (int i = 0; i < n; ++i) {
            int j = n - i - 1;
            ans += mat[i][i] + (i == j ? 0 : mat[i][j]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int diagonalSum(vector<vector<int>>& mat) {
        int ans = 0;
        int n = mat.size();
        for (int i = 0; i < n; ++i) {
            int j = n - i - 1;
            ans += mat[i][i] + (i == j ? 0 : mat[i][j]);
        }
        return ans;
    }
};
```

#### Go

```go
func diagonalSum(mat [][]int) (ans int) {
	n := len(mat)
	for i, row := range mat {
		ans += row[i]
		if j := n - i - 1; j != i {
			ans += row[j]
		}
	}
	return
}
```

#### TypeScript

```ts
function diagonalSum(mat: number[][]): number {
    let ans = 0;
    const n = mat.length;
    for (let i = 0; i < n; ++i) {
        const j = n - i - 1;
        ans += mat[i][i] + (i === j ? 0 : mat[i][j]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn diagonal_sum(mat: Vec<Vec<i32>>) -> i32 {
        let n = mat.len();
        let mut ans = 0;

        for i in 0..n {
            ans += mat[i][i];
            let j = n - i - 1;
            if j != i {
                ans += mat[i][j];
            }
        }

        ans
    }
}
```

#### C

```c
int diagonalSum(int** mat, int matSize, int* matColSize) {
    int ans = 0;
    for (int i = 0; i < matSize; ++i) {
        ans += mat[i][i];
        int j = matSize - i - 1;
        if (j != i) {
            ans += mat[i][j];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
