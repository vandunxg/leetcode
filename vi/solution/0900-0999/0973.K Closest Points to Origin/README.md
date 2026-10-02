---
comments: true
difficulty: Medium
tags:
    - Geometry
    - Array
    - Math
    - Divide and Conquer
    - Quickselect
    - Sorting
    - Heap (Priority Queue)
    - K-D Tree
---

<!-- problem:start -->

# [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin)

[中文文档](/solution/0900-0999/0973.K%20Closest%20Points%20to%20Origin/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu thị một điểm trên mặt phẳng <strong>X-Y</strong>, và số nguyên <code>k</code>. Hãy trả về <code>k</code> điểm gần gốc tọa độ <code>(0, 0)</code> nhất.</p>

<p>Khoảng cách giữa hai điểm trên mặt phẳng <strong>X-Y</strong> là khoảng cách Euclid (tức là <code>&radic;(x<sub>1</sub> - x<sub>2</sub>)<sup>2</sup> + (y<sub>1</sub> - y<sub>2</sub>)<sup>2</sup></code>).</p>

<p>Bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>. Đảm bảo rằng đáp án là <strong>duy nhất</strong> (không tính thứ tự các điểm).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0973.K%20Closest%20Points%20to%20Origin/images/closestplane1.jpg" style="width: 400px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,3],[-2,2]], k = 1
<strong>Đầu ra:</strong> [[-2,2]]
<strong>Giải thích:</strong>
Khoảng cách giữa (1, 3) và gốc tọa độ là sqrt(10).
Khoảng cách giữa (-2, 2) và gốc tọa độ là sqrt(8).
Vì sqrt(8) &lt; sqrt(10), điểm (-2, 2) gần gốc tọa độ hơn.
Ta chỉ cần điểm gần gốc tọa độ nhất với k = 1, nên đáp án là [[-2,2]].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[3,3],[5,-1],[-2,4]], k = 2
<strong>Đầu ra:</strong> [[3,3],[-2,4]]
<strong>Giải thích:</strong> Đáp án [[-2,4],[3,3]] cũng được chấp nhận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= points.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Tìm $k$ điểm gần gốc tọa độ nhất. Sắp xếp theo khoảng cách Euclid rồi lấy $k$ điểm đầu tiên có độ phức tạp $O(n\log n)$, phù hợp với $n\le 10^4$.

<!-- thinking:end -->

Ta sắp xếp tất cả điểm theo khoảng cách đến gốc tọa độ tăng dần, sau đó lấy $k$ điểm đầu tiên.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $\textit{points}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        points.sort(key=lambda p: hypot(p[0], p[1]))
        return points[:k]
```

#### Java

```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        Arrays.sort(
            points, (p1, p2) -> Math.hypot(p1[0], p1[1]) - Math.hypot(p2[0], p2[1]) > 0 ? 1 : -1);
        return Arrays.copyOfRange(points, 0, k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        sort(points.begin(), points.end(), [](const vector<int>& p1, const vector<int>& p2) {
            return hypot(p1[0], p1[1]) < hypot(p2[0], p2[1]);
        });
        return vector<vector<int>>(points.begin(), points.begin() + k);
    }
};
```

#### Go

```go
func kClosest(points [][]int, k int) [][]int {
	sort.Slice(points, func(i, j int) bool {
		return math.Hypot(float64(points[i][0]), float64(points[i][1])) < math.Hypot(float64(points[j][0]), float64(points[j][1]))
	})
	return points[:k]
}
```

#### TypeScript

```ts
function kClosest(points: number[][], k: number): number[][] {
    points.sort((a, b) => Math.hypot(a[0], a[1]) - Math.hypot(b[0], b[1]));
    return points.slice(0, k);
}
```

#### Rust

```rust
impl Solution {
    pub fn k_closest(mut points: Vec<Vec<i32>>, k: i32) -> Vec<Vec<i32>> {
        points.sort_by(|a, b| {
            let dist_a = f64::hypot(a[0] as f64, a[1] as f64);
            let dist_b = f64::hypot(b[0] as f64, b[1] as f64);
            dist_a.partial_cmp(&dist_b).unwrap()
        });
        points.into_iter().take(k as usize).collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp toàn bộ cũng phải sắp thứ tự cho $n-k$ điểm xa nhất. Max-heap kích thước $k$ chỉ giữ lại $k$ điểm gần nhất hiện tại, với độ phức tạp $O(n\log k)$.

<!-- thinking:end -->

Ta có thể dùng priority queue (max heap) để duy trì $k$ điểm gần gốc tọa độ nhất.

Độ phức tạp thời gian là $O(n \times \log k)$ và độ phức tạp không gian là $O(k)$. Trong đó, $n$ là độ dài của mảng $\textit{points}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        max_q = []
        for i, (x, y) in enumerate(points):
            dist = math.hypot(x, y)
            heappush(max_q, (-dist, i))
            if len(max_q) > k:
                heappop(max_q)
        return [points[i] for _, i in max_q]
```

#### Java

```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        PriorityQueue<int[]> maxQ = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        for (int i = 0; i < points.length; ++i) {
            int x = points[i][0], y = points[i][1];
            maxQ.offer(new int[] {x * x + y * y, i});
            if (maxQ.size() > k) {
                maxQ.poll();
            }
        }
        int[][] ans = new int[k][2];
        for (int i = 0; i < k; ++i) {
            ans[i] = points[maxQ.poll()[1]];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        priority_queue<pair<double, int>> pq;
        for (int i = 0, n = points.size(); i < n; ++i) {
            double dist = hypot(points[i][0], points[i][1]);
            pq.push({dist, i});
            if (pq.size() > k) {
                pq.pop();
            }
        }
        vector<vector<int>> ans;
        while (!pq.empty()) {
            ans.push_back(points[pq.top().second]);
            pq.pop();
        }
        return ans;
    }
};
```

#### Go

```go
func kClosest(points [][]int, k int) [][]int {
	maxQ := hp{}
	for i, p := range points {
		dist := math.Hypot(float64(p[0]), float64(p[1]))
		heap.Push(&maxQ, pair{dist, i})
		if len(maxQ) > k {
			heap.Pop(&maxQ)
		}
	}
	ans := make([][]int, k)
	for i, p := range maxQ {
		ans[i] = points[p.i]
	}
	return ans
}

type pair struct {
	dist float64
	i    int
}

type hp []pair

func (h hp) Len() int { return len(h) }
func (h hp) Less(i, j int) bool {
	a, b := h[i], h[j]
	return a.dist > b.dist
}
func (h hp) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)   { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function kClosest(points: number[][], k: number): number[][] {
    const maxQ = new MaxPriorityQueue<{ point: number[]; dist: number }>(entry => entry.dist);
    for (const [x, y] of points) {
        const dist = x * x + y * y;
        maxQ.enqueue({ point: [x, y], dist });
        if (maxQ.size() > k) {
            maxQ.dequeue();
        }
    }
    return maxQ.toArray().map(entry => entry.point);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Heap vẫn tốn thêm hệ số logarithm. Số điểm nằm trong một khoảng cách tăng đơn điệu theo ngưỡng khoảng cách, vì vậy ta tìm kiếm nhị phân ngưỡng sao cho có ít nhất $k$ điểm nằm trong ngưỡng đó, rồi lấy mọi điểm có khoảng cách không vượt quá giá trị tới hạn này.

<!-- thinking:end -->

Ta nhận thấy khoảng cách càng tăng thì số điểm được tính vào càng nhiều. Tồn tại một giá trị tới hạn sao cho số điểm trước giá trị này không quá $k$, còn số điểm sau giá trị này lớn hơn $k$.

Do đó, ta có thể dùng tìm kiếm nhị phân trên khoảng cách. Ở mỗi lượt, ta đếm số điểm có khoảng cách nhỏ hơn hoặc bằng ngưỡng hiện tại. Nếu số điểm đó lớn hơn hoặc bằng $k$, giá trị tới hạn nằm ở phía trái, nên ta đặt biên phải bằng ngưỡng hiện tại; ngược lại, giá trị tới hạn nằm ở phía phải, nên ta đặt biên trái bằng ngưỡng hiện tại cộng một.

Sau khi tìm kiếm nhị phân kết thúc, ta chỉ cần trả về các điểm có khoảng cách nhỏ hơn hoặc bằng biên trái.

Độ phức tạp thời gian là $O(n \times \log M)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài mảng $\textit{points}$, còn $M$ là giá trị khoảng cách lớn nhất.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        dist = [x * x + y * y for x, y in points]
        l, r = 0, max(dist)
        while l < r:
            mid = (l + r) >> 1
            cnt = sum(d <= mid for d in dist)
            if cnt >= k:
                r = mid
            else:
                l = mid + 1
        return [points[i] for i, d in enumerate(dist) if d <= l]
```

#### Java

```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        int n = points.length;
        int[] dist = new int[n];
        int r = 0;
        for (int i = 0; i < n; ++i) {
            int x = points[i][0], y = points[i][1];
            dist[i] = x * x + y * y;
            r = Math.max(r, dist[i]);
        }
        int l = 0;
        while (l < r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int d : dist) {
                if (d <= mid) {
                    ++cnt;
                }
            }
            if (cnt >= k) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        int[][] ans = new int[k][0];
        for (int i = 0, j = 0; i < n; ++i) {
            if (dist[i] <= l) {
                ans[j++] = points[i];
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
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        int n = points.size();
        int dist[n];
        int r = 0;
        for (int i = 0; i < n; ++i) {
            int x = points[i][0], y = points[i][1];
            dist[i] = x * x + y * y;
            r = max(r, dist[i]);
        }
        int l = 0;
        while (l < r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int d : dist) {
                cnt += d <= mid;
            }
            if (cnt >= k) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        vector<vector<int>> ans;
        for (int i = 0; i < n; ++i) {
            if (dist[i] <= l) {
                ans.emplace_back(points[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func kClosest(points [][]int, k int) (ans [][]int) {
	n := len(points)
	dist := make([]int, n)
	l, r := 0, 0
	for i, p := range points {
		dist[i] = p[0]*p[0] + p[1]*p[1]
		r = max(r, dist[i])
	}
	for l < r {
		mid := (l + r) >> 1
		cnt := 0
		for _, d := range dist {
			if d <= mid {
				cnt++
			}
		}
		if cnt >= k {
			r = mid
		} else {
			l = mid + 1
		}
	}
	for i, p := range points {
		if dist[i] <= l {
			ans = append(ans, p)
		}
	}
	return
}
```

#### TypeScript

```ts
function kClosest(points: number[][], k: number): number[][] {
    const dist = points.map(([x, y]) => x * x + y * y);
    let [l, r] = [0, Math.max(...dist)];
    while (l < r) {
        const mid = (l + r) >> 1;
        let cnt = 0;
        for (const d of dist) {
            if (d <= mid) {
                ++cnt;
            }
        }
        if (cnt >= k) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return points.filter((_, i) => dist[i] <= l);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
