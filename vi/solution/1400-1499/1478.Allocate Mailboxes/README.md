---
comments: true
difficulty: Hard
rating: 2190
source: Biweekly Contest 28 Q4
tags:
    - Array
    - Math
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1478. Allocate Mailboxes](https://leetcode.com/problems/allocate-mailboxes)

[中文文档](/solution/1400-1499/1478.Allocate%20Mailboxes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>houses</code>, trong đó <code>houses[i]</code> là vị trí của ngôi nhà thứ <code>i</code> trên một con phố, cùng một số nguyên <code>k</code>, hãy phân bổ <code>k</code> hộp thư trên con phố.</p>

<p>Trả về <em><strong>tổng khoảng cách nhỏ nhất</strong> giữa mỗi ngôi nhà và hộp thư gần nhất của nó</em>.</p>

<p>Các bộ test được tạo sao cho đáp án nằm trong phạm vi của số nguyên 32-bit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1478.Allocate%20Mailboxes/images/sample_11_1816.png" style="width: 454px; height: 154px;" />
<pre>
<strong>Đầu vào:</strong> houses = [1,4,8,10,20], k = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Phân bổ các hộp thư tại các vị trí 3, 9 và 20.
Tổng khoảng cách nhỏ nhất từ mỗi ngôi nhà đến hộp thư gần nhất là |3-1| + |4-3| + |9-8| + |10-9| + |20-20| = 5
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1478.Allocate%20Mailboxes/images/sample_2_1816.png" style="width: 433px; height: 154px;" />
<pre>
<strong>Đầu vào:</strong> houses = [2,3,5,12,18], k = 2
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Phân bổ các hộp thư tại các vị trí 3 và 14.
Tổng khoảng cách nhỏ nhất từ mỗi ngôi nhà đến hộp thư gần nhất là |2-3| + |3-3| + |5-3| + |12-14| + |18-14| = 9.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= houses.length &lt;= 100</code></li>
	<li><code>1 &lt;= houses[i] &lt;= 10<sup>4</sup></code></li>
	<li>Tất cả các số nguyên trong <code>houses</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Có $n\le 100$ ngôi nhà và $k$ hộp thư. Một hộp thư trên một đoạn liên tiếp sẽ nằm tại trung vị; sau khi sắp xếp, chi phí thỏa mãn $g[i][j]=g[i+1][j-1]+houses[j]-houses[i]$.
>
> $f[i][j]$ là chi phí nhỏ nhất cho $i+1$ ngôi nhà đầu tiên với $j$ hộp thư; ta duyệt vị trí cắt trước đó $p$ và cộng thêm $g[p+1][i]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là tổng khoảng cách nhỏ nhất giữa các ngôi nhà và hộp thư gần nhất của chúng khi đặt $j$ hộp thư cho $i+1$ ngôi nhà đầu tiên. Ban đầu, $f[i][j] = \infty$, và đáp án cuối cùng là $f[n-1][k]$.

Ta có thể duyệt qua ngôi nhà cuối cùng $p$ do hộp thư thứ $j-1$ phụ trách, tức là $0 \leq p \leq i-1$. Hộp thư thứ $j$ sẽ phụ trách các ngôi nhà trong đoạn $[p+1, \dots, i]$. Gọi $g[i][j]$ là tổng khoảng cách nhỏ nhất khi đặt một hộp thư cho các ngôi nhà trong đoạn $[i, \dots, j]$. Công thức chuyển trạng thái là:

$$
f[i][j] = \min_{0 \leq p \leq i-1} \{f[p][j-1] + g[p+1][i]\}
$$

trong đó $g[i][j]$ được tính như sau:

$$
g[i][j] = g[i + 1][j - 1] + \textit{houses}[j] - \textit{houses}[i]
$$

Độ phức tạp thời gian là $O(n^2 \times k)$, và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số lượng ngôi nhà.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDistance(self, houses: List[int], k: int) -> int:
        houses.sort()
        n = len(houses)
        g = [[0] * n for _ in range(n)]
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                g[i][j] = g[i + 1][j - 1] + houses[j] - houses[i]
        f = [[inf] * (k + 1) for _ in range(n)]
        for i in range(n):
            f[i][1] = g[0][i]
            for j in range(2, min(k + 1, i + 2)):
                for p in range(i):
                    f[i][j] = min(f[i][j], f[p][j - 1] + g[p + 1][i])
        return f[-1][k]
```

#### Java

```java
class Solution {
    public int minDistance(int[] houses, int k) {
        Arrays.sort(houses);
        int n = houses.length;
        int[][] g = new int[n][n];
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                g[i][j] = g[i + 1][j - 1] + houses[j] - houses[i];
            }
        }
        int[][] f = new int[n][k + 1];
        final int inf = 1 << 30;
        for (int[] e : f) {
            Arrays.fill(e, inf);
        }
        for (int i = 0; i < n; ++i) {
            f[i][1] = g[0][i];
            for (int j = 2; j <= k && j <= i + 1; ++j) {
                for (int p = 0; p < i; ++p) {
                    f[i][j] = Math.min(f[i][j], f[p][j - 1] + g[p + 1][i]);
                }
            }
        }
        return f[n - 1][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDistance(vector<int>& houses, int k) {
        int n = houses.size();
        sort(houses.begin(), houses.end());
        int g[n][n];
        memset(g, 0, sizeof(g));
        for (int i = n - 2; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                g[i][j] = g[i + 1][j - 1] + houses[j] - houses[i];
            }
        }
        int f[n][k + 1];
        memset(f, 0x3f, sizeof(f));
        for (int i = 0; i < n; ++i) {
            f[i][1] = g[0][i];
            for (int j = 1; j <= k && j <= i + 1; ++j) {
                for (int p = 0; p < i; ++p) {
                    f[i][j] = min(f[i][j], f[p][j - 1] + g[p + 1][i]);
                }
            }
        }
        return f[n - 1][k];
    }
};
```

#### Go

```go
func minDistance(houses []int, k int) int {
	sort.Ints(houses)
	n := len(houses)
	g := make([][]int, n)
	f := make([][]int, n)
	const inf = 1 << 30
	for i := range g {
		g[i] = make([]int, n)
		f[i] = make([]int, k+1)
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			g[i][j] = g[i+1][j-1] + houses[j] - houses[i]
		}
	}
	for i := 0; i < n; i++ {
		f[i][1] = g[0][i]
		for j := 2; j <= k && j <= i+1; j++ {
			for p := 0; p < i; p++ {
				f[i][j] = min(f[i][j], f[p][j-1]+g[p+1][i])
			}
		}
	}
	return f[n-1][k]
}
```

#### TypeScript

```ts
function minDistance(houses: number[], k: number): number {
    houses.sort((a, b) => a - b);
    const n = houses.length;
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(0));

    for (let i = n - 2; i >= 0; i--) {
        for (let j = i + 1; j < n; j++) {
            g[i][j] = g[i + 1][j - 1] + houses[j] - houses[i];
        }
    }

    const inf = Number.POSITIVE_INFINITY;
    const f: number[][] = Array.from({ length: n }, () => Array(k + 1).fill(inf));

    for (let i = 0; i < n; i++) {
        f[i][1] = g[0][i];
    }

    for (let j = 2; j <= k; j++) {
        for (let i = j - 1; i < n; i++) {
            for (let p = i - 1; p >= 0; p--) {
                f[i][j] = Math.min(f[i][j], f[p][j - 1] + g[p + 1][i]);
            }
        }
    }

    return f[n - 1][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
