---
comments: true
difficulty: Medium
rating: 1798
source: Weekly Contest 432 Q2
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3418. Maximum Amount of Money Robot Can Earn](https://leetcode.com/problems/maximum-amount-of-money-robot-can-earn)

[中文文档](/solution/3400-3499/3418.Maximum%20Amount%20of%20Money%20Robot%20Can%20Earn/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một lưới <code>m x n</code>. Một robot bắt đầu ở góc trên bên trái của lưới <code>(0, 0)</code> và muốn đi đến góc dưới bên phải <code>(m - 1, n - 1)</code>. Robot có thể di chuyển sang phải hoặc xuống dưới tại bất kỳ thời điểm nào.</p>

<p>Mỗi ô trong lưới chứa một giá trị <code>coins[i][j]</code>:</p>

<ul>
	<li>Nếu <code>coins[i][j] &gt;= 0</code>, robot nhận được số coin tương ứng.</li>
	<li>Nếu <code>coins[i][j] &lt; 0</code>, robot gặp một tên cướp và tên cướp lấy đi số coin bằng <strong>giá trị tuyệt đối</strong> của <code>coins[i][j]</code>.</li>
</ul>

<p>Robot có một khả năng đặc biệt để <strong>vô hiệu hóa tên cướp</strong> ở nhiều nhất <strong>2 ô</strong> trên đường đi, ngăn chúng lấy coin ở những ô đó.</p>

<p><strong>Lưu ý:</strong> Tổng số coin của robot có thể là số âm.</p>

<p>Trả về lợi nhuận <strong>lớn nhất</strong> mà robot có thể nhận được trên đường đi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coins = [[0,1,-1],[1,-2,3],[2,-3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu để nhận được nhiều coin nhất là:</p>

<ol>
	<li>Bắt đầu tại <code>(0, 0)</code> với <code>0</code> coin (tổng số coin = <code>0</code>).</li>
	<li>Di chuyển đến <code>(0, 1)</code>, nhận được <code>1</code> coin (tổng số coin = <code>0 + 1 = 1</code>).</li>
	<li>Di chuyển đến <code>(1, 1)</code>, nơi có một tên cướp lấy đi <code>2</code> coin. Robot sử dụng một lượt vô hiệu hóa tại đây để tránh bị cướp (tổng số coin = <code>1</code>).</li>
	<li>Di chuyển đến <code>(1, 2)</code>, nhận được <code>3</code> coin (tổng số coin = <code>1 + 3 = 4</code>).</li>
	<li>Di chuyển đến <code>(2, 2)</code>, nhận được <code>4</code> coin (tổng số coin = <code>4 + 4 = 8</code>).</li>
</ol>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coins = [[10,10,10],[10,10,10]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">40</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu để nhận được nhiều coin nhất là:</p>

<ol>
	<li>Bắt đầu tại <code>(0, 0)</code> với <code>10</code> coin (tổng số coin = <code>10</code>).</li>
	<li>Di chuyển đến <code>(0, 1)</code>, nhận được <code>10</code> coin (tổng số coin = <code>10 + 10 = 20</code>).</li>
	<li>Di chuyển đến <code>(0, 2)</code>, nhận thêm <code>10</code> coin (tổng số coin = <code>20 + 10 = 30</code>).</li>
	<li>Di chuyển đến <code>(1, 2)</code>, nhận <code>10</code> coin cuối cùng (tổng số coin = <code>30 + 10 = 40</code>).</li>
</ol>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == coins.length</code></li>
	<li><code>n == coins[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>-1000 &lt;= coins[i][j] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Robot đi từ góc trên bên trái đến góc dưới bên phải, chỉ di chuyển sang phải hoặc xuống dưới, và có thể vô hiệu hóa nhiều nhất hai lần bị cướp (coi một ô âm như $0$). Lưới $500\times 500$ nhân với ba số lượt còn lại đủ nhỏ để dùng memoization.
>
> Nếu bỏ qua số lượt vô hiệu hóa còn lại trong path DP, ta sẽ trộn lẫn những nghiệm tối ưu không thể so sánh với nhau.
>
> Trạng thái $(i,j,k)$ là lợi nhuận tốt nhất khi bắt đầu từ ô đó với $k$ lượt còn lại. Ta cộng giá trị của ô rồi di chuyển xuống dưới hoặc sang phải; nếu ô có giá trị âm và $k>0$, ta có thể bỏ qua giá trị đó và sử dụng một lượt. Tại ô đích, ta đưa giá trị âm về $0$ chỉ khi vẫn còn lượt vô hiệu hóa.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i, j, k)$, biểu diễn số coin lớn nhất mà robot có thể thu thập khi bắt đầu từ $(i, j)$ với $k$ cơ hội vô hiệu hóa còn lại. Robot chỉ có thể di chuyển sang phải hoặc xuống dưới, nên giá trị của $\textit{dfs}(i, j, k)$ chỉ phụ thuộc vào $\textit{dfs}(i + 1, j, k)$ và $\textit{dfs}(i, j + 1, k)$.

- Nếu $i \geq m$ hoặc $j \geq n$, nghĩa là robot đã đi ra ngoài lưới, ta trả về một giá trị rất nhỏ.
- Nếu $i = m - 1$ và $j = n - 1$, nghĩa là robot đã đến góc dưới bên phải của lưới. Nếu $k > 0$, robot có thể chọn vô hiệu hóa tên cướp tại vị trí hiện tại, nên ta trả về $\max(0, \textit{coins}[i][j])$. Nếu $k = 0$, robot không thể vô hiệu hóa tên cướp tại vị trí hiện tại, nên ta trả về $\textit{coins}[i][j]$.
- Nếu $\textit{coins}[i][j] < 0$, nghĩa là có một tên cướp tại vị trí hiện tại. Nếu $k > 0$, robot có thể chọn vô hiệu hóa tên cướp tại vị trí hiện tại, nên ta trả về $\textit{coins}[i][j] + \max(\textit{dfs}(i + 1, j, k), \textit{dfs}(i, j + 1, k))$. Nếu $k = 0$, robot không thể vô hiệu hóa tên cướp tại vị trí hiện tại, nên ta trả về $\textit{coins}[i][j] + \max(\textit{dfs}(i + 1, j, k), \textit{dfs}(i, j + 1, k))$.

Dựa trên phân tích trên, ta có thể viết code cho tìm kiếm có memoization.

Độ phức tạp thời gian là $O(m \times n \times k)$, và độ phức tạp không gian là $O(m \times n \times k)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của mảng hai chiều $\textit{coins}$, còn $k$ là số cơ hội vô hiệu hóa, bằng $3$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumAmount(self, coins: List[List[int]]) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if i >= m or j >= n:
                return -inf
            if i == m - 1 and j == n - 1:
                return max(coins[i][j], 0) if k else coins[i][j]
            ans = coins[i][j] + max(dfs(i + 1, j, k), dfs(i, j + 1, k))
            if coins[i][j] < 0 and k:
                ans = max(ans, dfs(i + 1, j, k - 1), dfs(i, j + 1, k - 1))
            return ans

        m, n = len(coins), len(coins[0])
        ans = dfs(0, 0, 2)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private Integer[][][] f;
    private int[][] coins;
    private int m;
    private int n;

    public int maximumAmount(int[][] coins) {
        m = coins.length;
        n = coins[0].length;
        this.coins = coins;
        f = new Integer[m][n][3];
        return dfs(0, 0, 2);
    }

    private int dfs(int i, int j, int k) {
        if (i >= m || j >= n) {
            return Integer.MIN_VALUE / 2;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        if (i == m - 1 && j == n - 1) {
            return k > 0 ? Math.max(0, coins[i][j]) : coins[i][j];
        }
        int ans = coins[i][j] + Math.max(dfs(i + 1, j, k), dfs(i, j + 1, k));
        if (coins[i][j] < 0 && k > 0) {
            ans = Math.max(ans, Math.max(dfs(i + 1, j, k - 1), dfs(i, j + 1, k - 1)));
        }
        return f[i][j][k] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumAmount(vector<vector<int>>& coins) {
        int m = coins.size(), n = coins[0].size();
        vector<vector<vector<int>>> f(m, vector<vector<int>>(n, vector<int>(3, -1)));
        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> int {
            if (i >= m || j >= n) {
                return INT_MIN / 2;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }
            if (i == m - 1 && j == n - 1) {
                return k > 0 ? max(0, coins[i][j]) : coins[i][j];
            }
            int ans = coins[i][j] + max(dfs(i + 1, j, k), dfs(i, j + 1, k));
            if (coins[i][j] < 0 && k > 0) {
                ans = max({ans, dfs(i + 1, j, k - 1), dfs(i, j + 1, k - 1)});
            }
            return f[i][j][k] = ans;
        };
        return dfs(0, 0, 2);
    }
};
```

#### Go

```go
func maximumAmount(coins [][]int) int {
	m, n := len(coins), len(coins[0])
	f := make([][][]int, m)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, 3)
			for k := range f[i][j] {
				f[i][j][k] = math.MinInt32
			}
		}
	}
	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if i >= m || j >= n {
			return math.MinInt32 / 2
		}
		if f[i][j][k] != math.MinInt32 {
			return f[i][j][k]
		}
		if i == m-1 && j == n-1 {
			if k > 0 {
				return max(0, coins[i][j])
			}
			return coins[i][j]
		}
		ans := coins[i][j] + max(dfs(i+1, j, k), dfs(i, j+1, k))
		if coins[i][j] < 0 && k > 0 {
			ans = max(ans, max(dfs(i+1, j, k-1), dfs(i, j+1, k-1)))
		}
		f[i][j][k] = ans
		return ans
	}
	return dfs(0, 0, 2)
}
```

#### TypeScript

```ts
function maximumAmount(coins: number[][]): number {
    const [m, n] = [coins.length, coins[0].length];
    const f = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array(3).fill(-Infinity)),
    );
    const dfs = (i: number, j: number, k: number): number => {
        if (i >= m || j >= n) {
            return -Infinity;
        }
        if (f[i][j][k] !== -Infinity) {
            return f[i][j][k];
        }
        if (i === m - 1 && j === n - 1) {
            return k > 0 ? Math.max(0, coins[i][j]) : coins[i][j];
        }
        let ans = coins[i][j] + Math.max(dfs(i + 1, j, k), dfs(i, j + 1, k));
        if (coins[i][j] < 0 && k > 0) {
            ans = Math.max(ans, dfs(i + 1, j, k - 1), dfs(i, j + 1, k - 1));
        }
        return (f[i][j][k] = ans);
    };
    return dfs(0, 0, 2);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
