---
comments: true
difficulty: Easy
rating: 1264
source: Weekly Contest 275 Q1
tags:
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [2133. Check if Every Row and Column Contains All Numbers](https://leetcode.com/problems/check-if-every-row-and-column-contains-all-numbers)

[中文文档](/solution/2100-2199/2133.Check%20if%20Every%20Row%20and%20Column%20Contains%20All%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Một ma trận <code>n x n</code> được coi là <strong>hợp lệ</strong> nếu mọi hàng và mọi cột đều chứa <strong>tất cả</strong> các số nguyên từ <code>1</code> đến <code>n</code> (<strong>bao gồm cả hai đầu mút</strong>).</p>

<p>Cho một ma trận số nguyên <code>n x n</code> là <code>matrix</code>, hãy trả về <code>true</code> <em>nếu ma trận <strong>hợp lệ</strong>.</em> Ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2133.Check%20if%20Every%20Row%20and%20Column%20Contains%20All%20Numbers/images/example1drawio.png" style="width: 250px; height: 251px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,2,3],[3,1,2],[2,3,1]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Trong trường hợp này, n = 3, mọi hàng và cột đều chứa các số 1, 2 và 3.
Do đó, ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2133.Check%20if%20Every%20Row%20and%20Column%20Contains%20All%20Numbers/images/example2drawio.png" style="width: 250px; height: 251px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,1,1],[1,2,3],[1,2,3]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Trong trường hợp này, n = 3, nhưng hàng đầu tiên và cột đầu tiên không chứa số 2 hoặc 3.
Do đó, ta trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == matrix.length == matrix[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= matrix[i][j] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng và cột phải chứa các số $1\ldots n$ đúng một lần. Với $n\le 100$, chỉ cần kiểm tra tập hợp của mỗi dòng có kích thước $n$ là đủ.
>
> Các hàng chính là ma trận; các cột có thể lấy từ ma trận chuyển vị. Một tập hợp có kích thước $n$ nghĩa là không có phần tử trùng lặp, do đó với miền giá trị đã cho, đó là một hoán vị của $1\ldots n$.
>
> Kiểm tra mọi dãy trong $\texttt{chain}(\textit{matrix},\texttt{zip}(*\textit{matrix}))$.

<!-- thinking:end -->

Duyệt qua từng hàng và cột của ma trận, dùng một hash table để ghi nhận xem mỗi số đã xuất hiện hay chưa. Nếu có số xuất hiện nhiều hơn một lần trong một hàng hoặc cột, trả về `false`; ngược lại, trả về `true`.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là kích thước của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkValid(self, matrix: List[List[int]]) -> bool:
        n = len(matrix)
        return all(len(set(row)) == n for row in chain(matrix, zip(*matrix)))
```

#### Java

```java
class Solution {
    public boolean checkValid(int[][] matrix) {
        int n = matrix.length;
        boolean[] vis = new boolean[n + 1];
        for (var row : matrix) {
            Arrays.fill(vis, false);
            for (int x : row) {
                if (vis[x]) {
                    return false;
                }
                vis[x] = true;
            }
        }
        for (int j = 0; j < n; ++j) {
            Arrays.fill(vis, false);
            for (int i = 0; i < n; ++i) {
                if (vis[matrix[i][j]]) {
                    return false;
                }
                vis[matrix[i][j]] = true;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkValid(vector<vector<int>>& matrix) {
        int n = matrix.size();
        bool vis[n + 1];
        for (const auto& row : matrix) {
            memset(vis, false, sizeof(vis));
            for (int x : row) {
                if (vis[x]) {
                    return false;
                }
                vis[x] = true;
            }
        }
        for (int j = 0; j < n; ++j) {
            memset(vis, false, sizeof(vis));
            for (int i = 0; i < n; ++i) {
                if (vis[matrix[i][j]]) {
                    return false;
                }
                vis[matrix[i][j]] = true;
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkValid(matrix [][]int) bool {
	n := len(matrix)
	for _, row := range matrix {
		vis := make([]bool, n+1)
		for _, x := range row {
			if vis[x] {
				return false
			}
			vis[x] = true
		}
	}
	for j := 0; j < n; j++ {
		vis := make([]bool, n+1)
		for i := 0; i < n; i++ {
			if vis[matrix[i][j]] {
				return false
			}
			vis[matrix[i][j]] = true
		}
	}
	return true
}
```

#### TypeScript

```ts
function checkValid(matrix: number[][]): boolean {
    const n = matrix.length;
    const vis: boolean[] = Array(n + 1).fill(false);
    for (const row of matrix) {
        vis.fill(false);
        for (const x of row) {
            if (vis[x]) {
                return false;
            }
            vis[x] = true;
        }
    }
    for (let j = 0; j < n; ++j) {
        vis.fill(false);
        for (let i = 0; i < n; ++i) {
            if (vis[matrix[i][j]]) {
                return false;
            }
            vis[matrix[i][j]] = true;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
