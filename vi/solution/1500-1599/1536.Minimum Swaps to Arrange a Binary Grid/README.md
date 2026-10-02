---
comments: true
difficulty: Medium
rating: 1880
source: Weekly Contest 200 Q3
tags:
    - Greedy
    - Array
    - Matrix
---

<!-- problem:start -->

# [1536. Minimum Swaps to Arrange a Binary Grid](https://leetcode.com/problems/minimum-swaps-to-arrange-a-binary-grid)

[中文文档](/solution/1500-1599/1536.Minimum%20Swaps%20to%20Arrange%20a%20Binary%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>grid</code> nhị phân kích thước <code>n x n</code>, trong một bước bạn có thể chọn hai <strong>hàng kề nhau</strong> và đổi chỗ chúng.</p>

<p>Một grid được gọi là <strong>hợp lệ</strong> nếu mọi ô phía trên đường chéo chính đều là <strong>số 0</strong>.</p>

<p>Trả về <em>số bước nhỏ nhất</em> cần thiết để grid hợp lệ, hoặc <strong>-1</strong> nếu không thể.</p>

<p>Đường chéo chính của grid là đường chéo bắt đầu tại ô <code>(1, 1)</code> và kết thúc tại ô <code>(n, n)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1536.Minimum%20Swaps%20to%20Arrange%20a%20Binary%20Grid/images/fw.jpg" style="width: 750px; height: 141px;" />
<pre>
<strong>Input:</strong> grid = [[0,0,1],[1,1,0],[1,0,0]]
<strong>Output:</strong> 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1536.Minimum%20Swaps%20to%20Arrange%20a%20Binary%20Grid/images/e2.jpg" style="width: 270px; height: 270px;" />
<pre>
<strong>Input:</strong> grid = [[0,1,1,0],[0,1,1,0],[0,1,1,0],[0,1,1,0]]
<strong>Output:</strong> -1
<strong>Explanation:</strong> All rows are similar, swaps have no effect on the grid.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1536.Minimum%20Swaps%20to%20Arrange%20a%20Binary%20Grid/images/e3.jpg" style="width: 200px; height: 200px;" />
<pre>
<strong>Input:</strong> grid = [[1,0,0],[1,1,0],[1,1,1]]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code> <code>== grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>grid[i][j]</code> is either <code>0</code> or <code>1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Các phép đổi hàng kề nhau phải làm cho mọi hàng $i$ chỉ có số 0 ở bên phải đường chéo. Chỉ vị trí số $1$ ngoài cùng bên phải của mỗi hàng là quan trọng: hàng đó hợp lệ ở vị trí $i$ khi chỉ số này không vượt quá $i$. Với $n\le 200$, ta có thể tham lam theo từng hàng.
>
> Với mỗi $i$, trong các hàng chưa dùng, chọn hàng đầu tiên có số $1$ ngoài cùng bên phải không vượt quá $i$, rồi đưa nó lên vị trí $i$ với chi phí $k-i$. Nếu không có hàng nào, bài toán vô nghiệm. Việc thỏa mãn các hàng trên trước không cản trở các hàng sau, vốn yêu cầu ít số 0 ở cuối hơn.

<!-- thinking:end -->

Ta xử lý từng hàng. Với hàng thứ $i$, vị trí của số 1 cuối cùng phải nhỏ hơn hoặc bằng $i$. Ta tìm hàng đầu tiên thỏa điều kiện trong $[i, n)$, gọi là $k$. Sau đó, bắt đầu từ hàng thứ $k$, đổi chỗ các hàng kề nhau lên trên cho đến hàng thứ $i$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài cạnh của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwaps(self, grid: List[List[int]]) -> int:
        n = len(grid)
        pos = [-1] * n
        for i in range(n):
            for j in range(n - 1, -1, -1):
                if grid[i][j] == 1:
                    pos[i] = j
                    break
        ans = 0
        for i in range(n):
            k = -1
            for j in range(i, n):
                if pos[j] <= i:
                    ans += j - i
                    k = j
                    break
            if k == -1:
                return -1
            while k > i:
                pos[k], pos[k - 1] = pos[k - 1], pos[k]
                k -= 1
        return ans
```

#### Java

```java
class Solution {
    public int minSwaps(int[][] grid) {
        int n = grid.length;
        int[] pos = new int[n];
        Arrays.fill(pos, -1);
        for (int i = 0; i < n; ++i) {
            for (int j = n - 1; j >= 0; --j) {
                if (grid[i][j] == 1) {
                    pos[i] = j;
                    break;
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int k = -1;
            for (int j = i; j < n; ++j) {
                if (pos[j] <= i) {
                    ans += j - i;
                    k = j;
                    break;
                }
            }
            if (k == -1) {
                return -1;
            }
            for (; k > i; --k) {
                int t = pos[k];
                pos[k] = pos[k - 1];
                pos[k - 1] = t;
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
    int minSwaps(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<int> pos(n, -1);
        for (int i = 0; i < n; ++i) {
            for (int j = n - 1; j >= 0; --j) {
                if (grid[i][j] == 1) {
                    pos[i] = j;
                    break;
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int k = -1;
            for (int j = i; j < n; ++j) {
                if (pos[j] <= i) {
                    ans += j - i;
                    k = j;
                    break;
                }
            }
            if (k == -1) {
                return -1;
            }
            for (; k > i; --k) {
                swap(pos[k], pos[k - 1]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minSwaps(grid [][]int) (ans int) {
	n := len(grid)
	pos := make([]int, n)
	for i := range pos {
		pos[i] = -1
	}
	for i := 0; i < n; i++ {
		for j := n - 1; j >= 0; j-- {
			if grid[i][j] == 1 {
				pos[i] = j
				break
			}
		}
	}
	for i := 0; i < n; i++ {
		k := -1
		for j := i; j < n; j++ {
			if pos[j] <= i {
				ans += j - i
				k = j
				break
			}
		}
		if k == -1 {
			return -1
		}
		for ; k > i; k-- {
			pos[k], pos[k-1] = pos[k-1], pos[k]
		}
	}
	return
}
```

#### TypeScript

```ts
function minSwaps(grid: number[][]): number {
    const n = grid.length;
    const pos: number[] = Array(n).fill(-1);
    for (let i = 0; i < n; ++i) {
        for (let j = n - 1; ~j; --j) {
            if (grid[i][j] === 1) {
                pos[i] = j;
                break;
            }
        }
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        let k = -1;
        for (let j = i; j < n; ++j) {
            if (pos[j] <= i) {
                ans += j - i;
                k = j;
                break;
            }
        }
        if (k === -1) {
            return -1;
        }
        for (; k > i; --k) {
            [pos[k], pos[k - 1]] = [pos[k - 1], pos[k]];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
