---
comments: true
difficulty: Easy
rating: 1244
source: Weekly Contest 376 Q1
tags:
    - Array
    - Hash Table
    - Math
    - Matrix
---

<!-- problem:start -->

# [2965. Find Missing and Repeated Values](https://leetcode.com/problems/find-missing-and-repeated-values)

[中文文档](/solution/2900-2999/2965.Find%20Missing%20and%20Repeated%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên 2D <code><font face="monospace">grid</font></code> có kích thước <code>n * n</code>, được <strong>đánh chỉ số từ 0</strong>, với các giá trị nằm trong khoảng <code>[1, n<sup>2</sup>]</code>. Mỗi số nguyên xuất hiện <strong>chính xác một lần</strong>, ngoại trừ <code>a</code> xuất hiện <strong>hai lần</strong> và <code>b</code> bị <strong>thiếu</strong>. Nhiệm vụ là tìm số lặp lại và số bị thiếu <code>a</code> và <code>b</code>.</p>

<p><em>Trả về <strong>một mảng số nguyên được đánh chỉ số từ 0</strong></em> <code>ans</code><em> có kích thước </em><code>2</code><em>, trong đó </em><code>ans[0]</code><em> bằng </em><code>a</code><em> và </em><code>ans[1]</code><em> bằng </em><code>b</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,3],[2,2]]
<strong>Đầu ra:</strong> [2,4]
<strong>Giải thích:</strong> Số 2 xuất hiện lặp lại và số 4 bị thiếu, nên đáp án là [2,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[9,1,7],[8,9,2],[3,4,6]]
<strong>Đầu ra:</strong> [9,5]
<strong>Giải thích:</strong> Số 9 xuất hiện lặp lại và số 5 bị thiếu, nên đáp án là [9,5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == grid.length == grid[i].length &lt;= 50</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= n * n</code></li>
	<li>Với mọi <code>x</code> sao cho <code>1 &lt;= x &lt;= n * n</code>, có chính xác một giá trị <code>x</code> không bằng bất kỳ phần tử nào trong grid.</li>
	<li>Với mọi <code>x</code> sao cho <code>1 &lt;= x &lt;= n * n</code>, có chính xác một giá trị <code>x</code> bằng đúng hai phần tử trong grid.</li>
	<li>Với mọi <code>x</code> sao cho <code>1 &lt;= x &lt;= n * n</code>, ngoại trừ hai giá trị, tồn tại chính xác một cặp <code>i, j</code> sao cho <code>0 &lt;= i, j &lt;= n - 1</code> và <code>grid[i][j] == x</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ma trận $n \times n$ phải chứa mỗi giá trị từ $1 \ldots n^2$ đúng một lần, nhưng một giá trị xuất hiện lặp lại và một giá trị bị thiếu. Vì $n \le 50$, chỉ cần một mảng đếm có độ dài $n^2+1$ và hai lần duyệt.
>
> Tần suất bằng $2$ là giá trị lặp lại, còn tần suất bằng $0$ là giá trị bị thiếu. Không cần dùng công thức dạng đóng.

<!-- thinking:end -->

Ta tạo một mảng $cnt$ có độ dài $n^2 + 1$ để đếm số lần xuất hiện của mỗi số trong ma trận.

Tiếp theo, ta duyệt $i \in [1, n^2]$. Nếu $cnt[i] = 2$, thì $i$ là số bị lặp lại và ta gán phần tử đầu tiên của đáp án bằng $i$. Nếu $cnt[i] = 0$, thì $i$ là số bị thiếu và ta gán phần tử thứ hai của đáp án bằng $i$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ là độ dài cạnh của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMissingAndRepeatedValues(self, grid: List[List[int]]) -> List[int]:
        n = len(grid)
        cnt = [0] * (n * n + 1)
        for row in grid:
            for v in row:
                cnt[v] += 1
        ans = [0] * 2
        for i in range(1, n * n + 1):
            if cnt[i] == 2:
                ans[0] = i
            if cnt[i] == 0:
                ans[1] = i
        return ans
```

#### Java

```java
class Solution {
    public int[] findMissingAndRepeatedValues(int[][] grid) {
        int n = grid.length;
        int[] cnt = new int[n * n + 1];
        int[] ans = new int[2];
        for (int[] row : grid) {
            for (int x : row) {
                if (++cnt[x] == 2) {
                    ans[0] = x;
                }
            }
        }
        for (int x = 1;; ++x) {
            if (cnt[x] == 0) {
                ans[1] = x;
                return ans;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findMissingAndRepeatedValues(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<int> cnt(n * n + 1);
        vector<int> ans(2);
        for (auto& row : grid) {
            for (int x : row) {
                if (++cnt[x] == 2) {
                    ans[0] = x;
                }
            }
        }
        for (int x = 1;; ++x) {
            if (cnt[x] == 0) {
                ans[1] = x;
                return ans;
            }
        }
    }
};
```

#### Go

```go
func findMissingAndRepeatedValues(grid [][]int) []int {
	n := len(grid)
	ans := make([]int, 2)
	cnt := make([]int, n*n+1)
	for _, row := range grid {
		for _, x := range row {
			cnt[x]++
			if cnt[x] == 2 {
				ans[0] = x
			}
		}
	}
	for x := 1; ; x++ {
		if cnt[x] == 0 {
			ans[1] = x
			return ans
		}
	}
}
```

#### TypeScript

```ts
function findMissingAndRepeatedValues(grid: number[][]): number[] {
    const n = grid.length;
    const cnt: number[] = Array(n * n + 1).fill(0);
    const ans: number[] = Array(2).fill(0);
    for (const row of grid) {
        for (const x of row) {
            if (++cnt[x] === 2) {
                ans[0] = x;
            }
        }
    }
    for (let x = 1; ; ++x) {
        if (cnt[x] === 0) {
            ans[1] = x;
            return ans;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
