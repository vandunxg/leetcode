---
comments: true
difficulty: Hard
rating: 2122
source: Weekly Contest 512 Q4
tags:
    - Graph
    - Array
    - Matrix
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [4003. Minimum Cost Path with Alternating Directions III](https://leetcode.com/problems/minimum-cost-path-with-alternating-directions-iii)

[中文文档](/solution/4000-4099/4003.Minimum%20Cost%20Path%20with%20Alternating%20Directions%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>m</code> và <code>n</code> biểu diễn số hàng và số cột của một lưới. Mục tiêu của bạn là đi tới ô <code>(m - 1, n - 1)</code>. Bạn cũng được cho một mảng số nguyên 2D <code>penalty</code>.</p>

<p>Chi phí đi vào ô <code>(i, j)</code> là <code>(i + 1) * (j + 1)</code>.</p>

<p>Bạn bắt đầu tại ô <code>(0, 0)</code> và trả chi phí đi vào ô này ngay từ đầu. Các hành động sau khi đi vào <code>(0, 0)</code> được đánh số bắt đầu từ 1.</p>

<p>Ở mỗi hành động, bạn có thể đi tới ô <strong>kề</strong> hoặc chờ tại ô hiện tại. Một bước đi tuân theo quy tắc chẵn lẻ nếu:</p>

<ul>
	<li>Ở hành động có số <strong>lẻ</strong>, bạn đi <strong>sang phải</strong> hoặc <strong>xuống dưới</strong>.</li>
	<li>Ở hành động có số <strong>chẵn</strong>, bạn đi <strong>sang trái</strong> hoặc <strong>lên trên</strong>.</li>
</ul>

<p>Chi phí của một hành động được xác định như sau:</p>

<ul>
	<li>Nếu đi theo quy tắc chẵn lẻ, chỉ trả chi phí đi vào ô đích.</li>
	<li>Nếu đi theo hướng <strong>vi phạm</strong> quy tắc chẵn lẻ, trả chi phí đi vào ô đích cộng với <code>penalty[i][j]</code>, trong đó <code>(i, j)</code> là ô xuất phát.</li>
	<li>Nếu <strong>chờ</strong> tại ô <code>(i, j)</code>, trả <code>penalty[i][j]</code>.</li>
</ul>

<p>Sau mỗi lần đi hoặc chờ, số thứ tự hành động tăng thêm 1. Vì vậy, tính chẵn lẻ cần tuân theo sẽ đổi sau mỗi hành động, bất kể có trả penalty hay không.</p>

<p>Hãy trả về tổng chi phí <strong>nhỏ nhất</strong> cần có để đi tới <code>(m - 1, n - 1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 2, penalty = [[5,3],[1,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<ul>
	<li>Bắt đầu tại ô <code>(0, 0)</code> với chi phí đi vào là <code>(0 + 1) * (0 + 1) = 1</code>.</li>
	<li><strong>Bước 1</strong>: Đi xuống ô <code>(1, 0)</code> với chi phí đi vào là <code>(1 + 1) * (0 + 1) = 2</code>.</li>
	<li><strong>Bước 2</strong>: Đi sang phải tới ô <code>(1, 1)</code> với chi phí đi vào là <code>(1 + 1) * (1 + 1) = 4</code> và trả thêm <code>penalty[1][0] = 1</code> vì vi phạm quy tắc chẵn lẻ cho bước chẵn.</li>
</ul>

<p>Do đó, tổng chi phí là <code>1 + 2 + 4 + 1 = 8</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 2, penalty = [[0,7],[3,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<ul>
	<li>Bắt đầu tại ô <code>(0, 0)</code> với chi phí đi vào là <code>(0 + 1) * (0 + 1) = 1</code>.</li>
	<li><strong>Bước 1</strong>: Chờ tại ô <code>(0, 0)</code> với chi phí thêm <code>penalty[0][0] = 0</code> để chuyển sang tính chẵn.</li>
	<li><strong>Bước 2</strong>: Đi sang phải tới ô <code>(0, 1)</code> với chi phí đi vào là <code>(0 + 1) * (1 + 1) = 2</code> và trả thêm <code>penalty[0][0] = 0</code> vì vi phạm quy tắc chẵn lẻ cho bước chẵn.</li>
	<li><strong>Bước 3</strong>: Đi xuống ô <code>(1, 1)</code> với chi phí đi vào là <code>(1 + 1) * (1 + 1) = 4</code>.</li>
</ul>

<p>Do đó, tổng chi phí là <code>1 + 0 + 2 + 0 + 4 = 7</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 3, penalty = [[8,0,9],[7,4,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<ul>
	<li>Bắt đầu tại ô <code>(0, 0)</code> với chi phí đi vào là <code>(0 + 1) * (0 + 1) = 1</code>.</li>
	<li><strong>Bước 1</strong>: Đi sang phải tới ô <code>(0, 1)</code> với chi phí đi vào là <code>(0 + 1) * (1 + 1) = 2</code>.</li>
	<li><strong>Bước 2</strong>: Đi sang phải tới ô <code>(0, 2)</code> với chi phí đi vào là <code>(0 + 1) * (2 + 1) = 3</code> và trả thêm <code>penalty[0][1] = 0</code> vì vi phạm quy tắc chẵn lẻ cho bước chẵn.</li>
	<li><strong>Bước 3</strong>: Đi xuống ô <code>(1, 2)</code> với chi phí đi vào là <code>(1 + 1) * (2 + 1) = 6</code>.</li>
</ul>

<p>Do đó, tổng chi phí là <code>1 + 2 + 3 + 0 + 6 = 12</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>penalty.length == m</code></li>
	<li><code>penalty[i].length == n</code></li>
	<li><code>0 &lt;= penalty[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dijkstra

<!-- thinking:start -->

> **Tư duy**
>
> Tại mỗi ô, ta có thể đi hoặc chờ. Tính chẵn lẻ của bước tiếp theo bị đảo, còn chi phí đi phụ thuộc vào việc hướng đi có khớp với tính chẵn lẻ đó hay không. Với $mn\le 10^5$, đệ quy chỉ theo ô không thể mô tả gọn cả thao tác chờ lẫn penalty do hướng đi.
>
> Ghép vị trí với tính chẵn lẻ của bước tiếp theo sẽ biến thao tác chờ và bốn hướng đi thành các cạnh không âm: chờ trả $\textit{penalty}$ rồi đảo tính chẵn lẻ; đi cộng chi phí ô đích và cộng penalty tương tự nếu hướng đi không khớp với tính chẵn lẻ hiện tại.
>
> Đường đi ngắn nhất trên đồ thị trạng thái này chính là lần đầu ta tới đích, và Dijkstra sẽ tính được nó.

<!-- thinking:end -->

Chi phí đi vào ô $(i, j)$ là $(i+1)(j+1)$. Các hành động được đánh số từ $1$: ở hành động lẻ, ta nên đi sang phải hoặc xuống dưới; ở hành động chẵn, đi sang trái hoặc lên trên; ngoài ra có thể chờ tại chỗ. Đi ngược quy tắc chẵn lẻ phải trả thêm $\textit{penalty}$ của ô hiện tại, và chờ cũng tốn $\textit{penalty}$. Sau mỗi hành động, tính chẵn lẻ cần tuân theo bị đảo.

Dùng trạng thái $(i, j, k)$ biểu diễn chi phí nhỏ nhất khi đang ở $(i, j)$ và hành động tiếp theo có tính chẵn lẻ $k$ ($k = 1$ là hành động lẻ, $k = 0$ là hành động chẵn). Trạng thái đầu là $(0, 0, 1)$ với chi phí $1$.

Từ trạng thái hiện tại, ta có thể:

- **Chờ**: cộng $\textit{penalty}[i][j]$ và đảo tính chẵn lẻ;
- **Di chuyển**: xét bốn hướng, cộng chi phí đi vào ô đích; nếu hướng đi không khớp với tính chẵn lẻ hiện tại thì cộng thêm $\textit{penalty}[i][j]$, sau đó đảo tính chẵn lẻ tại ô mới.

Chạy Dijkstra trên đồ thị trạng thái; lần đầu trạng thái $(m-1, n-1)$ được lấy ra chính là đáp án.

Độ phức tạp thời gian là $O(mn \log (mn))$ và độ phức tạp không gian là $O(mn)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, m: int, n: int, penalty: List[List[int]]) -> int:
        dist = [[[inf] * 2 for _ in range(n)] for _ in range(m)]
        dist[0][0][1] = 1
        pq = [(1, 0, 0, 1)]
        dirs = ((-1, 0), (0, 1), (0, -1), (1, 0))
        while pq:
            d, i, j, k = heappop(pq)
            if i == m - 1 and j == n - 1:
                return d
            if d > dist[i][j][k]:
                continue

            p = penalty[i][j]
            nd = d + p
            if nd < dist[i][j][k ^ 1]:
                dist[i][j][k ^ 1] = nd
                heappush(pq, (nd, i, j, k ^ 1))

            for idx, (dx, dy) in enumerate(dirs):
                x, y = i + dx, j + dy
                if 0 <= x < m and 0 <= y < n:
                    nd = d + (x + 1) * (y + 1) + (idx & 1 ^ k) * p
                    if nd < dist[x][y][k ^ 1]:
                        dist[x][y][k ^ 1] = nd
                        heappush(pq, (nd, x, y, k ^ 1))
```

#### Java

```java
class Solution {
    public long minCost(int m, int n, int[][] penalty) {
        long[][][] dist = new long[m][n][2];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                Arrays.fill(dist[i][j], Long.MAX_VALUE);
            }
        }
        dist[0][0][1] = 1;

        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> Long.compare(a[0], b[0]));
        pq.offer(new long[] {1, 0, 0, 1});

        int[][] dirs = {{-1, 0}, {0, 1}, {0, -1}, {1, 0}};

        while (!pq.isEmpty()) {
            long[] cur = pq.poll();
            long d = cur[0];
            int i = (int) cur[1];
            int j = (int) cur[2];
            int k = (int) cur[3];

            if (i == m - 1 && j == n - 1) {
                return d;
            }
            if (d > dist[i][j][k]) {
                continue;
            }

            int p = penalty[i][j];

            long nd = d + p;
            if (nd < dist[i][j][k ^ 1]) {
                dist[i][j][k ^ 1] = nd;
                pq.offer(new long[] {nd, i, j, k ^ 1});
            }

            for (int idx = 0; idx < 4; idx++) {
                int x = i + dirs[idx][0];
                int y = j + dirs[idx][1];
                if (0 <= x && x < m && 0 <= y && y < n) {
                    nd = d + (long) (x + 1) * (y + 1) + ((idx & 1) ^ k) * (long) p;
                    if (nd < dist[x][y][k ^ 1]) {
                        dist[x][y][k ^ 1] = nd;
                        pq.offer(new long[] {nd, x, y, k ^ 1});
                    }
                }
            }
        }

        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCost(int m, int n, vector<vector<int>>& penalty) {
        vector<vector<array<long long, 2>>> dist(
            m, vector<array<long long, 2>>(n, {LLONG_MAX, LLONG_MAX}));
        dist[0][0][1] = 1;

        priority_queue<
            array<long long, 4>,
            vector<array<long long, 4>>,
            greater<>>
            pq;
        pq.push({1, 0, 0, 1});

        int dirs[4][2] = {{-1, 0}, {0, 1}, {0, -1}, {1, 0}};

        while (!pq.empty()) {
            auto [d, i, j, k] = pq.top();
            pq.pop();

            if (i == m - 1 && j == n - 1) {
                return d;
            }
            if (d > dist[i][j][k]) {
                continue;
            }

            int p = penalty[i][j];

            long long nd = d + p;
            if (nd < dist[i][j][k ^ 1]) {
                dist[i][j][k ^ 1] = nd;
                pq.push({nd, i, j, k ^ 1});
            }

            for (int idx = 0; idx < 4; idx++) {
                int x = i + dirs[idx][0];
                int y = j + dirs[idx][1];
                if (0 <= x && x < m && 0 <= y && y < n) {
                    nd = d + 1LL * (x + 1) * (y + 1) + (((idx & 1) ^ k) ? p : 0);
                    if (nd < dist[x][y][k ^ 1]) {
                        dist[x][y][k ^ 1] = nd;
                        pq.push({nd, (long long) x, (long long) y, (long long) (k ^ 1)});
                    }
                }
            }
        }

        return -1;
    }
};
```

#### Go

```go
const inf int64 = 1 << 60

type tuple struct {
	d    int64
	i, j int
	k    int
}

type hp []tuple

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].d < h[j].d }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *hp) Push(x any) {
	*h = append(*h, x.(tuple))
}

func (h *hp) Pop() any {
	a := *h
	v := a[len(a)-1]
	*h = a[:len(a)-1]
	return v
}

func minCost(m int, n int, penalty [][]int) int64 {
	dist := make([][][]int64, m)
	for i := range dist {
		dist[i] = make([][]int64, n)
		for j := range dist[i] {
			dist[i][j] = []int64{inf, inf}
		}
	}
	dist[0][0][1] = 1

	pq := hp{{1, 0, 0, 1}}
	heap.Init(&pq)

	dirs := [][2]int{{-1, 0}, {0, 1}, {0, -1}, {1, 0}}

	for pq.Len() > 0 {
		cur := heap.Pop(&pq).(tuple)
		d, i, j, k := cur.d, cur.i, cur.j, cur.k

		if i == m-1 && j == n-1 {
			return d
		}
		if d > dist[i][j][k] {
			continue
		}

		p := penalty[i][j]

		nd := d + int64(p)
		if nd < dist[i][j][k^1] {
			dist[i][j][k^1] = nd
			heap.Push(&pq, tuple{nd, i, j, k ^ 1})
		}

		for idx, dir := range dirs {
			x, y := i+dir[0], j+dir[1]
			if 0 <= x && x < m && 0 <= y && y < n {
				nd = d + int64((x+1)*(y+1)+((idx&1)^k)*p)
				if nd < dist[x][y][k^1] {
					dist[x][y][k^1] = nd
					heap.Push(&pq, tuple{nd, x, y, k ^ 1})
				}
			}
		}
	}

	return -1
}
```

#### TypeScript

```ts
function minCost(m: number, n: number, penalty: number[][]): number {
    const dist = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => [Infinity, Infinity]),
    );
    dist[0][0][1] = 1;

    const pq = new MinPriorityQueue<number[]>(x => x[0]);
    pq.enqueue([1, 0, 0, 1]);

    const dirs = [
        [-1, 0],
        [0, 1],
        [0, -1],
        [1, 0],
    ];

    while (!pq.isEmpty()) {
        const [d, i, j, k] = pq.dequeue();

        if (i === m - 1 && j === n - 1) {
            return d;
        }
        if (d > dist[i][j][k]) {
            continue;
        }

        const p = penalty[i][j];

        let nd = d + p;
        if (nd < dist[i][j][k ^ 1]) {
            dist[i][j][k ^ 1] = nd;
            pq.enqueue([nd, i, j, k ^ 1]);
        }

        for (let idx = 0; idx < 4; idx++) {
            const [dx, dy] = dirs[idx];
            const x = i + dx;
            const y = j + dy;
            if (0 <= x && x < m && 0 <= y && y < n) {
                nd = d + (x + 1) * (y + 1) + ((idx & 1) ^ k) * p;
                if (nd < dist[x][y][k ^ 1]) {
                    dist[x][y][k ^ 1] = nd;
                    pq.enqueue([nd, x, y, k ^ 1]);
                }
            }
        }
    }

    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
