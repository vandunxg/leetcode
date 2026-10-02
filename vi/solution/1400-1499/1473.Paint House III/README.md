---
comments: true
difficulty: Hard
rating: 2056
source: Weekly Contest 192 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1473. Paint House III](https://leetcode.com/problems/paint-house-iii)

[中文文档](/solution/1400-1499/1473.Paint%20House%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Có một dãy gồm <code>m</code> ngôi nhà trong một thành phố nhỏ. Mỗi ngôi nhà phải được sơn bằng một trong <code>n</code> màu (được đánh số từ <code>1</code> đến <code>n</code>); một số ngôi nhà đã được sơn vào mùa hè năm ngoái thì không được sơn lại.</p>

<p>Một khu phố là một nhóm liên tiếp lớn nhất gồm các ngôi nhà được sơn cùng một màu.</p>

<ul>
	<li>Ví dụ: <code>houses = [1,2,2,3,3,2,1,1]</code> chứa <code>5</code> khu phố <code>[{1}, {2,2}, {3,3}, {2}, {1,1}]</code>.</li>
</ul>

<p>Cho một mảng <code>houses</code>, một ma trận <code>m x n</code> <code>cost</code> và một số nguyên <code>target</code>, trong đó:</p>

<ul>
	<li><code>houses[i]</code>: là màu của ngôi nhà <code>i</code>, và bằng <code>0</code> nếu ngôi nhà chưa được sơn.</li>
	<li><code>cost[i][j]</code>: là chi phí sơn ngôi nhà <code>i</code> bằng màu <code>j + 1</code>.</li>
</ul>

<p>Trả về <em>chi phí nhỏ nhất để sơn tất cả các ngôi nhà còn lại sao cho có chính xác</em> <code>target</code> <em>khu phố</em>. Nếu không thể, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> houses = [0,0,0,0,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
<strong>Output:</strong> 9
<strong>Giải thích:</strong> Sơn các ngôi nhà như sau [1,2,2,1,1]
Mảng này chứa target = 3 khu phố, [{1}, {2,2}, {1,1}].
Chi phí sơn tất cả các ngôi nhà (1 + 1 + 1 + 1 + 5) = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> houses = [0,2,1,2,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
<strong>Output:</strong> 11
<strong>Giải thích:</strong> Một số ngôi nhà đã được sơn. Sơn các ngôi nhà như sau [2,2,1,2,2]
Mảng này chứa target = 3 khu phố, [{2,2}, {1}, {2,2}].
Chi phí sơn ngôi nhà đầu tiên và cuối cùng (10 + 1) = 11.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> houses = [3,1,2,3], cost = [[1,1,1],[1,1,1],[1,1,1],[1,1,1]], m = 4, n = 3, target = 3
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Các ngôi nhà đã được sơn, tổng cộng có 4 khu phố [{3},{1},{2},{3}], khác với target = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == houses.length == cost.length</code></li>
	<li><code>n == cost[i].length</code></li>
	<li><code>1 &lt;= m &lt;= 100</code></li>
	<li><code>1 &lt;= n &lt;= 20</code></li>
	<li><code>1 &lt;= target &lt;= m</code></li>
	<li><code>0 &lt;= houses[i] &lt;= n</code></li>
	<li><code>1 &lt;= cost[i][j] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Sơn $m$ ngôi nhà thành chính xác $target$ khu phố và chỉ trả chi phí cho những ngôi nhà chưa được sơn. Trạng thái gồm ngôi nhà $i$, màu $j$ và số khu phố $k$.
>
> Ngôi nhà đã được sơn thì không tốn phí và màu của nó là cố định; nếu chưa sơn, ta thử từng màu và cộng thêm $cost$. Cùng màu thì giữ nguyên $k$, màu mới thì tăng $k$. Đáp án là giá trị nhỏ nhất trong $f[m-1][\cdot][target]$, hoặc $-1$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j][k]$ là chi phí nhỏ nhất để sơn các ngôi nhà từ chỉ số $0$ đến $i$, trong đó ngôi nhà cuối cùng được sơn màu $j$ và tạo thành chính xác $k$ khu phố. Đáp án là $f[m-1][j][\textit{target}]$, với $j$ chạy từ $1$ đến $n$. Ban đầu, ta kiểm tra xem ngôi nhà tại chỉ số $0$ đã được sơn hay chưa. Nếu chưa, thì $f[0][j][1] = \textit{cost}[0][j - 1]$, với $j \in [1,..n]$. Nếu đã được sơn, thì $f[0][\textit{houses}[0]][1] = 0$. Tất cả các giá trị khác của $f[i][j][k]$ được khởi tạo bằng $\infty$.

Tiếp theo, ta bắt đầu duyệt từ chỉ số $i=1$. Với mỗi $i$, ta kiểm tra xem ngôi nhà tại chỉ số $i$ đã được sơn hay chưa:

Nếu chưa được sơn, ta có thể sơn ngôi nhà tại chỉ số $i$ bằng màu $j$. Ta duyệt số khu phố $k$, trong đó $k \in [1,..\min(\textit{target}, i + 1)]$, và duyệt màu của ngôi nhà trước đó $j_0$, trong đó $j_0 \in [1,..n]$. Khi đó, ta có phương trình chuyển trạng thái:

$$
f[i][j][k] = \min_{j_0 \in [1,..n]} \{ f[i - 1][j_0][k - (j \neq j_0)] + \textit{cost}[i][j - 1] \}
$$

Nếu đã được sơn, ta có thể sơn ngôi nhà tại chỉ số $i$ bằng màu $j$. Ta duyệt số khu phố $k$, trong đó $k \in [1,..\min(\textit{target}, i + 1)]$, và duyệt màu của ngôi nhà trước đó $j_0$, trong đó $j_0 \in [1,..n]$. Khi đó, ta có phương trình chuyển trạng thái:

$$
f[i][j][k] = \min_{j_0 \in [1,..n]} \{ f[i - 1][j_0][k - (j \neq j_0)] \}
$$

Cuối cùng, ta trả về $f[m - 1][j][\textit{target}]$, với $j \in [1,..n]$. Nếu tất cả các giá trị $f[m - 1][j][\textit{target}]$ đều bằng $\infty$, thì trả về $-1$.

Độ phức tạp thời gian là $O(m \times n^2 \times \textit{target})$, và độ phức tạp không gian là $O(m \times n \times \textit{target})$. Ở đây, $m$, $n$ và $\textit{target}$ lần lượt là số ngôi nhà, số màu và số khu phố.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(
        self, houses: List[int], cost: List[List[int]], m: int, n: int, target: int
    ) -> int:
        f = [[[inf] * (target + 1) for _ in range(n + 1)] for _ in range(m)]
        if houses[0] == 0:
            for j, c in enumerate(cost[0], 1):
                f[0][j][1] = c
        else:
            f[0][houses[0]][1] = 0
        for i in range(1, m):
            if houses[i] == 0:
                for j in range(1, n + 1):
                    for k in range(1, min(target + 1, i + 2)):
                        for j0 in range(1, n + 1):
                            if j == j0:
                                f[i][j][k] = min(
                                    f[i][j][k], f[i - 1][j][k] + cost[i][j - 1]
                                )
                            else:
                                f[i][j][k] = min(
                                    f[i][j][k], f[i - 1][j0][k - 1] + cost[i][j - 1]
                                )
            else:
                j = houses[i]
                for k in range(1, min(target + 1, i + 2)):
                    for j0 in range(1, n + 1):
                        if j == j0:
                            f[i][j][k] = min(f[i][j][k], f[i - 1][j][k])
                        else:
                            f[i][j][k] = min(f[i][j][k], f[i - 1][j0][k - 1])

        ans = min(f[-1][j][target] for j in range(1, n + 1))
        return -1 if ans >= inf else ans
```

#### Java

```java
class Solution {
    public int minCost(int[] houses, int[][] cost, int m, int n, int target) {
        int[][][] f = new int[m][n + 1][target + 1];
        final int inf = 1 << 30;
        for (int[][] g : f) {
            for (int[] e : g) {
                Arrays.fill(e, inf);
            }
        }
        if (houses[0] == 0) {
            for (int j = 1; j <= n; ++j) {
                f[0][j][1] = cost[0][j - 1];
            }
        } else {
            f[0][houses[0]][1] = 0;
        }
        for (int i = 1; i < m; ++i) {
            if (houses[i] == 0) {
                for (int j = 1; j <= n; ++j) {
                    for (int k = 1; k <= Math.min(target, i + 1); ++k) {
                        for (int j0 = 1; j0 <= n; ++j0) {
                            if (j == j0) {
                                f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j][k] + cost[i][j - 1]);
                            } else {
                                f[i][j][k]
                                    = Math.min(f[i][j][k], f[i - 1][j0][k - 1] + cost[i][j - 1]);
                            }
                        }
                    }
                }
            } else {
                int j = houses[i];
                for (int k = 1; k <= Math.min(target, i + 1); ++k) {
                    for (int j0 = 1; j0 <= n; ++j0) {
                        if (j == j0) {
                            f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j][k]);
                        } else {
                            f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j0][k - 1]);
                        }
                    }
                }
            }
        }
        int ans = inf;
        for (int j = 1; j <= n; ++j) {
            ans = Math.min(ans, f[m - 1][j][target]);
        }
        return ans >= inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(vector<int>& houses, vector<vector<int>>& cost, int m, int n, int target) {
        int f[m][n + 1][target + 1];
        memset(f, 0x3f, sizeof(f));
        if (houses[0] == 0) {
            for (int j = 1; j <= n; ++j) {
                f[0][j][1] = cost[0][j - 1];
            }
        } else {
            f[0][houses[0]][1] = 0;
        }
        for (int i = 1; i < m; ++i) {
            if (houses[i] == 0) {
                for (int j = 1; j <= n; ++j) {
                    for (int k = 1; k <= min(target, i + 1); ++k) {
                        for (int j0 = 1; j0 <= n; ++j0) {
                            if (j == j0) {
                                f[i][j][k] = min(f[i][j][k], f[i - 1][j][k] + cost[i][j - 1]);
                            } else {
                                f[i][j][k] = min(f[i][j][k], f[i - 1][j0][k - 1] + cost[i][j - 1]);
                            }
                        }
                    }
                }
            } else {
                int j = houses[i];
                for (int k = 1; k <= min(target, i + 1); ++k) {
                    for (int j0 = 1; j0 <= n; ++j0) {
                        if (j == j0) {
                            f[i][j][k] = min(f[i][j][k], f[i - 1][j][k]);
                        } else {
                            f[i][j][k] = min(f[i][j][k], f[i - 1][j0][k - 1]);
                        }
                    }
                }
            }
        }
        int ans = 0x3f3f3f3f;
        for (int j = 1; j <= n; ++j) {
            ans = min(ans, f[m - 1][j][target]);
        }
        return ans == 0x3f3f3f3f ? -1 : ans;
    }
};
```

#### Go

```go
func minCost(houses []int, cost [][]int, m int, n int, target int) int {
	f := make([][][]int, m)
	const inf = 1 << 30
	for i := range f {
		f[i] = make([][]int, n+1)
		for j := range f[i] {
			f[i][j] = make([]int, target+1)
			for k := range f[i][j] {
				f[i][j][k] = inf
			}
		}
	}
	if houses[0] == 0 {
		for j := 1; j <= n; j++ {
			f[0][j][1] = cost[0][j-1]
		}
	} else {
		f[0][houses[0]][1] = 0
	}
	for i := 1; i < m; i++ {
		if houses[i] == 0 {
			for j := 1; j <= n; j++ {
				for k := 1; k <= target && k <= i+1; k++ {
					for j0 := 1; j0 <= n; j0++ {
						if j == j0 {
							f[i][j][k] = min(f[i][j][k], f[i-1][j][k]+cost[i][j-1])
						} else {
							f[i][j][k] = min(f[i][j][k], f[i-1][j0][k-1]+cost[i][j-1])
						}
					}
				}
			}
		} else {
			j := houses[i]
			for k := 1; k <= target && k <= i+1; k++ {
				for j0 := 1; j0 <= n; j0++ {
					if j == j0 {
						f[i][j][k] = min(f[i][j][k], f[i-1][j][k])
					} else {
						f[i][j][k] = min(f[i][j][k], f[i-1][j0][k-1])
					}
				}
			}
		}
	}
	ans := inf
	for j := 1; j <= n; j++ {
		ans = min(ans, f[m-1][j][target])
	}
	if ans == inf {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minCost(houses: number[], cost: number[][], m: number, n: number, target: number): number {
    const inf = 1 << 30;
    const f: number[][][] = new Array(m)
        .fill(0)
        .map(() => new Array(n + 1).fill(0).map(() => new Array(target + 1).fill(inf)));
    if (houses[0] === 0) {
        for (let j = 1; j <= n; ++j) {
            f[0][j][1] = cost[0][j - 1];
        }
    } else {
        f[0][houses[0]][1] = 0;
    }
    for (let i = 1; i < m; ++i) {
        if (houses[i] === 0) {
            for (let j = 1; j <= n; ++j) {
                for (let k = 1; k <= Math.min(target, i + 1); ++k) {
                    for (let j0 = 1; j0 <= n; ++j0) {
                        if (j0 === j) {
                            f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j][k] + cost[i][j - 1]);
                        } else {
                            f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j0][k - 1] + cost[i][j - 1]);
                        }
                    }
                }
            }
        } else {
            const j = houses[i];
            for (let k = 1; k <= Math.min(target, i + 1); ++k) {
                for (let j0 = 1; j0 <= n; ++j0) {
                    if (j0 === j) {
                        f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j][k]);
                    } else {
                        f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j0][k - 1]);
                    }
                }
            }
        }
    }
    let ans = inf;
    for (let j = 1; j <= n; ++j) {
        ans = Math.min(ans, f[m - 1][j][target]);
    }
    return ans >= inf ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
