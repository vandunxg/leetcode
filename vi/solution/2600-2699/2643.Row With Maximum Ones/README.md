---
comments: true
difficulty: Easy
rating: 1174
source: Weekly Contest 341 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [2643. Row With Maximum Ones](https://leetcode.com/problems/row-with-maximum-ones)

[中文文档](/solution/2600-2699/2643.Row%20With%20Maximum%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận nhị phân <code>m x n</code> <code>mat</code>, hãy tìm vị trí <strong>0-indexed</strong> của hàng chứa <strong>nhiều</strong> số <strong>1</strong> nhất, và số lượng số 1 trong hàng đó.</p>

<p>Nếu có nhiều hàng có cùng số lượng số 1 lớn nhất, hãy chọn hàng có <strong>số thứ tự hàng nhỏ nhất</strong>.</p>

<p>Trả về<em> một mảng chứa chỉ số của hàng và số lượng số 1 trong hàng đó.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[0,1],[1,0]]
<strong>Đầu ra:</strong> [0,1]
<strong>Giải thích:</strong> Cả hai hàng có cùng số lượng 1&#39;s. Vì vậy, ta trả về chỉ số của hàng nhỏ hơn là 0 và số lượng số 1 lớn nhất (1<code>)</code>. Do đó, đáp án là [0,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[0,0,0],[0,1,1]]
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong> Hàng có chỉ số 1 có số lượng số 1 lớn nhất <code>(2)</code>. Vì vậy, ta trả về chỉ số của hàng đó là <code>1</code> và số lượng số 1. Do đó, đáp án là [1,2].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[0,0],[1,1],[0,0]]
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong> Hàng có chỉ số 1 có số lượng số 1 lớn nhất (2). Do đó, đáp án là [1,2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code>&nbsp;</li>
	<li><code>n == mat[i].length</code>&nbsp;</li>
	<li><code>1 &lt;= m, n &lt;= 100</code>&nbsp;</li>
	<li><code>mat[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ma trận chỉ chứa các giá trị $0/1$; ta cần tìm hàng có nhiều số 1 nhất, và nếu hòa thì chọn chỉ số nhỏ hơn. Vì có nhiều nhất $100$ hàng và cột, chỉ cần tính tổng từng hàng và lưu lại cặp kết quả tốt nhất.

<!-- thinking:end -->

Ta khởi tạo một mảng $\textit{ans} = [0, 0]$ để lưu chỉ số của hàng có nhiều số $1$ nhất và số lượng số $1$.

Sau đó, ta duyệt qua từng hàng của ma trận:

- Tính số lượng $1$s trong hàng hiện tại, ký hiệu là $\textit{cnt}$ (vì ma trận chỉ chứa các giá trị $0$s và $1$s, ta có thể tính trực tiếp tổng của hàng).
- Nếu $\textit{ans}[1] < \textit{cnt}$, cập nhật $\textit{ans} = [i, \textit{cnt}]$.

Sau khi duyệt xong, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rowAndMaximumOnes(self, mat: List[List[int]]) -> List[int]:
        ans = [0, 0]
        for i, row in enumerate(mat):
            cnt = sum(row)
            if ans[1] < cnt:
                ans = [i, cnt]
        return ans
```

#### Java

```java
class Solution {
    public int[] rowAndMaximumOnes(int[][] mat) {
        int[] ans = new int[2];
        for (int i = 0; i < mat.length; ++i) {
            int cnt = 0;
            for (int x : mat[i]) {
                cnt += x;
            }
            if (ans[1] < cnt) {
                ans[0] = i;
                ans[1] = cnt;
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
    vector<int> rowAndMaximumOnes(vector<vector<int>>& mat) {
        vector<int> ans(2);
        for (int i = 0; i < mat.size(); ++i) {
            int cnt = accumulate(mat[i].begin(), mat[i].end(), 0);
            if (ans[1] < cnt) {
                ans = {i, cnt};
            }
        }
        return ans;
    }
};
```

#### Go

```go
func rowAndMaximumOnes(mat [][]int) []int {
	ans := []int{0, 0}
	for i, row := range mat {
		cnt := 0
		for _, x := range row {
			cnt += x
		}
		if ans[1] < cnt {
			ans = []int{i, cnt}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function rowAndMaximumOnes(mat: number[][]): number[] {
    const ans: number[] = [0, 0];
    for (let i = 0; i < mat.length; i++) {
        const cnt = mat[i].reduce((sum, num) => sum + num, 0);
        if (ans[1] < cnt) {
            ans[0] = i;
            ans[1] = cnt;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn row_and_maximum_ones(mat: Vec<Vec<i32>>) -> Vec<i32> {
        let mut ans = vec![0, 0];
        for (i, row) in mat.iter().enumerate() {
            let cnt = row.iter().sum();
            if ans[1] < cnt {
                ans = vec![i as i32, cnt];
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int[] RowAndMaximumOnes(int[][] mat) {
        int[] ans = new int[2];
        for (int i = 0; i < mat.Length; i++) {
            int cnt = mat[i].Sum();
            if (ans[1] < cnt) {
                ans = new int[] { i, cnt };
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
