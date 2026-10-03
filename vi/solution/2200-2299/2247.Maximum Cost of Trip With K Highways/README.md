---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Graph
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [2247. Maximum Cost of Trip With K Highways 🔒](https://leetcode.com/problems/maximum-cost-of-trip-with-k-highways)

[中文文档](/solution/2200-2299/2247.Maximum%20Cost%20of%20Trip%20With%20K%20Highways/README.md)

## Mô tả

<!-- description:start -->

<p>Một loạt đường cao tốc kết nối <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n - 1</code>. Cho một mảng số nguyên 2 chiều <code>highways</code>, trong đó <code>highways[i] = [city1<sub>i</sub>, city2<sub>i</sub>, toll<sub>i</sub>]</code> cho biết có một đường cao tốc nối <code>city1<sub>i</sub></code> và <code>city2<sub>i</sub></code>, cho phép ô tô đi từ <code>city1<sub>i</sub></code> đến <code>city2<sub>i</sub></code> và <strong>ngược lại</strong> với chi phí là <code>toll<sub>i</sub></code>.</p>

<p>Cho thêm một số nguyên <code>k</code>. Bạn thực hiện một chuyến đi đi qua <strong>chính xác</strong> <code>k</code> đường cao tốc. Bạn có thể bắt đầu ở bất kỳ thành phố nào, nhưng mỗi thành phố chỉ được ghé thăm <strong>nhiều nhất</strong> một lần trong chuyến đi.</p>

<p>Trả về <em>chi phí <strong>lớn nhất</strong> của chuyến đi. Nếu không có chuyến đi nào thỏa mãn yêu cầu, trả về </em><code>-1</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2247.Maximum%20Cost%20of%20Trip%20With%20K%20Highways/images/image-20220418173304-1.png" style="height: 200px; width: 327px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, highways = [[0,1,4],[2,1,3],[1,4,11],[3,2,3],[3,4,2]], k = 3
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong>
Một chuyến đi có thể thực hiện là đi từ 0 -&gt; 1 -&gt; 4 -&gt; 3. Chi phí của chuyến đi này là 4 + 11 + 2 = 17.
Một chuyến đi khác có thể thực hiện là đi từ 4 -&gt; 1 -&gt; 2 -&gt; 3. Chi phí là 11 + 3 + 3 = 17.
Có thể chứng minh rằng 17 là chi phí lớn nhất có thể có của mọi chuyến đi hợp lệ.

Lưu ý rằng chuyến đi 4 -&gt; 1 -&gt; 0 -&gt; 1 không hợp lệ vì thành phố 1 được ghé thăm hai lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2247.Maximum%20Cost%20of%20Trip%20With%20K%20Highways/images/image-20220418173342-2.png" style="height: 200px; width: 217px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, highways = [[0,1,3],[2,3,2]], k = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có chuyến đi hợp lệ nào có độ dài 2, vì vậy trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 15</code></li>
	<li><code>1 &lt;= highways.length &lt;= 50</code></li>
	<li><code>highways[i].length == 3</code></li>
	<li><code>0 &lt;= city1<sub>i</sub>, city2<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>city1<sub>i</sub> != city2<sub>i</sub></code></li>
	<li><code>0 &lt;= toll<sub>i</sub> &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 50</code></li>
	<li>Không có đường cao tốc trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm một đường đi đơn gồm chính xác $k$ cạnh và có tổng chi phí lớn nhất. Nếu $k \ge n$ thì không có đủ thành phố phân biệt. Vì $n \le 15$, ta có thể dùng quy hoạch động trên tập con.
>
> Gọi $f[S][j]$ là chi phí lớn nhất khi đi qua các thành phố trong $S$ và kết thúc tại $j$, với các trạng thái chỉ có một thành phố được khởi tạo bằng $0$. Chuyển trạng thái từ một hàng xóm $h$ bằng $f[S\setminus\{j\}][h]+\textit{cost}(h,j)$. Khi $|S|=k+1$, ta cập nhật đáp án.

<!-- thinking:end -->

Ta nhận thấy bài toán yêu cầu đi qua chính xác $k$ con đường và mỗi thành phố chỉ được ghé thăm một lần. Số thành phố là $n$, nên ta chỉ có thể đi qua nhiều nhất $n - 1$ con đường. Do đó, nếu $k \ge n$, ta không thể thỏa mãn yêu cầu của bài toán và có thể trực tiếp trả về $-1$.

Ngoài ra, ta cũng nhận thấy số thành phố $n$ không vượt quá $15$, điều này gợi ý rằng ta có thể dùng quy hoạch động nén trạng thái để giải bài toán. Ta dùng một số nhị phân có độ dài $n$ để biểu diễn các thành phố đã đi qua, trong đó bit thứ $i$ bằng $1$ cho biết đã đi qua thành phố thứ $i$, còn bằng $0$ cho biết thành phố thứ $i$ chưa được đi qua.

Ta dùng $f[i][j]$ để biểu diễn chi phí chuyến đi lớn nhất khi các thành phố đã đi qua là $i$ và thành phố cuối cùng đã đi qua là $j$. Ban đầu, $f[2^i][i]=0$, còn các trạng thái khác có giá trị $f[i][j]=-\infty$.

Hãy xét cách chuyển trạng thái của $f[i][j]$. Với $f[i]$, ta duyệt qua mọi thành phố $j$. Nếu bit thứ $j$ của $i$ bằng $1$, ta có thể đi đến thành phố $j$ từ một thành phố khác $h$ thông qua con đường nối hai thành phố; khi đó, giá trị của $f[i][j]$ là giá trị lớn nhất của $f[i][h]+cost(h, j)$, trong đó $cost(h, j)$ biểu diễn chi phí đi từ thành phố $h$ đến thành phố $j$. Vì vậy, ta có công thức chuyển trạng thái:

$$
f[i][j]=\max_{h \in \textit{city}}\{f[i \backslash j][h]+cost(h, j)\}
$$

trong đó $i \backslash j$ biểu diễn việc đổi bit thứ $j$ của $i$ thành $0$.

Sau khi tính $f[i][j]$, ta kiểm tra xem số thành phố đã đi qua có bằng $k+1$ hay không, tức là số lượng số $1$ trong biểu diễn nhị phân của $i$ có bằng $k+1$ hay không. Nếu có, ta cập nhật đáp án theo $ans = \max(ans, f[i][j])$.

Độ phức tạp thời gian là $O(2^n \times n^2)$, còn độ phức tạp không gian là $O(2^n \times n)$, trong đó $n$ là số thành phố.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumCost(self, n: int, highways: List[List[int]], k: int) -> int:
        if k >= n:
            return -1
        g = defaultdict(list)
        for a, b, cost in highways:
            g[a].append((b, cost))
            g[b].append((a, cost))
        f = [[-inf] * n for _ in range(1 << n)]
        for i in range(n):
            f[1 << i][i] = 0
        ans = -1
        for i in range(1 << n):
            for j in range(n):
                if i >> j & 1:
                    for h, cost in g[j]:
                        if i >> h & 1:
                            f[i][j] = max(f[i][j], f[i ^ (1 << j)][h] + cost)
                if i.bit_count() == k + 1:
                    ans = max(ans, f[i][j])
        return ans
```

#### Java

```java
class Solution {
    public int maximumCost(int n, int[][] highways, int k) {
        if (k >= n) {
            return -1;
        }
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, h -> new ArrayList<>());
        for (int[] h : highways) {
            int a = h[0], b = h[1], cost = h[2];
            g[a].add(new int[] {b, cost});
            g[b].add(new int[] {a, cost});
        }
        int[][] f = new int[1 << n][n];
        for (int[] e : f) {
            Arrays.fill(e, -(1 << 30));
        }
        for (int i = 0; i < n; ++i) {
            f[1 << i][i] = 0;
        }
        int ans = -1;
        for (int i = 0; i < 1 << n; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    for (var e : g[j]) {
                        int h = e[0], cost = e[1];
                        if ((i >> h & 1) == 1) {
                            f[i][j] = Math.max(f[i][j], f[i ^ (1 << j)][h] + cost);
                        }
                    }
                }
                if (Integer.bitCount(i) == k + 1) {
                    ans = Math.max(ans, f[i][j]);
                }
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
    int maximumCost(int n, vector<vector<int>>& highways, int k) {
        if (k >= n) {
            return -1;
        }
        vector<pair<int, int>> g[n];
        for (auto& h : highways) {
            int a = h[0], b = h[1], cost = h[2];
            g[a].emplace_back(b, cost);
            g[b].emplace_back(a, cost);
        }
        int f[1 << n][n];
        memset(f, -0x3f, sizeof(f));
        for (int i = 0; i < n; ++i) {
            f[1 << i][i] = 0;
        }
        int ans = -1;
        for (int i = 0; i < 1 << n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    for (auto& [h, cost] : g[j]) {
                        if (i >> h & 1) {
                            f[i][j] = max(f[i][j], f[i ^ (1 << j)][h] + cost);
                        }
                    }
                }
                if (__builtin_popcount(i) == k + 1) {
                    ans = max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumCost(n int, highways [][]int, k int) int {
	if k >= n {
		return -1
	}
	g := make([][][2]int, n)
	for _, h := range highways {
		a, b, cost := h[0], h[1], h[2]
		g[a] = append(g[a], [2]int{b, cost})
		g[b] = append(g[b], [2]int{a, cost})
	}
	f := make([][]int, 1<<n)
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -(1 << 30)
		}
	}
	for i := 0; i < n; i++ {
		f[1<<i][i] = 0
	}
	ans := -1
	for i := 0; i < 1<<n; i++ {
		for j := 0; j < n; j++ {
			if i>>j&1 == 1 {
				for _, e := range g[j] {
					h, cost := e[0], e[1]
					if i>>h&1 == 1 {
						f[i][j] = max(f[i][j], f[i^(1<<j)][h]+cost)
					}
				}
			}
			if bits.OnesCount(uint(i)) == k+1 {
				ans = max(ans, f[i][j])
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maximumCost(n: number, highways: number[][], k: number): number {
    if (k >= n) {
        return -1;
    }
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (const [a, b, cost] of highways) {
        g[a].push([b, cost]);
        g[b].push([a, cost]);
    }
    const f: number[][] = Array(1 << n)
        .fill(0)
        .map(() => Array(n).fill(-(1 << 30)));
    for (let i = 0; i < n; ++i) {
        f[1 << i][i] = 0;
    }
    let ans = -1;
    for (let i = 0; i < 1 << n; ++i) {
        for (let j = 0; j < n; ++j) {
            if ((i >> j) & 1) {
                for (const [h, cost] of g[j]) {
                    if ((i >> h) & 1) {
                        f[i][j] = Math.max(f[i][j], f[i ^ (1 << j)][h] + cost);
                    }
                }
            }
            if (bitCount(i) === k + 1) {
                ans = Math.max(ans, f[i][j]);
            }
        }
    }
    return ans;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
