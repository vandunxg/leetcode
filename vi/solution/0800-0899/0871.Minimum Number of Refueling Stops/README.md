---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Dynamic Programming
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [871. Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops)

[中文文档](/solution/0800-0899/0871.Minimum%20Number%20of%20Refueling%20Stops/README.md)

## Mô tả

<!-- description:start -->

<p>Một chiếc xe đi từ điểm xuất phát đến đích, nằm cách điểm xuất phát <code>target</code> dặm về phía đông.</p>

<p>Dọc đường có các trạm xăng. Mảng <code>stations</code> biểu diễn các trạm này, trong đó <code>stations[i] = [position<sub>i</sub>, fuel<sub>i</sub>]</code> cho biết trạm xăng thứ <code>i</code> cách điểm xuất phát <code>position<sub>i</sub></code> dặm về phía đông và có <code>fuel<sub>i</sub></code> lít xăng.</p>

<p>Xe có bình xăng dung tích vô hạn, ban đầu chứa <code>startFuel</code> lít xăng. Xe tiêu thụ một lít xăng cho mỗi dặm di chuyển. Khi đến trạm xăng, xe có thể dừng lại để đổ xăng, chuyển toàn bộ xăng ở trạm vào xe.</p>

<p>Trả về <em>số lần dừng đổ xăng ít nhất để xe đến được đích</em>. Nếu không thể đến đích, trả về <code>-1</code>.</p>

<p>Lưu ý: nếu xe đến trạm xăng khi còn <code>0</code> nhiên liệu, xe vẫn có thể đổ xăng tại đó. Nếu xe đến đích khi còn <code>0</code> nhiên liệu thì vẫn được tính là đã đến nơi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> target = 1, startFuel = 1, stations = []
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Ta có thể đến đích mà không cần đổ xăng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> target = 100, startFuel = 1, stations = [[10,100]]
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Ta không thể đến đích (thậm chí không đến được trạm xăng đầu tiên).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> target = 100, startFuel = 10, stations = [[10,60],[20,30],[30,30],[60,40]]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Ban đầu xe có 10 lít xăng.
Xe đi đến vị trí 10, tiêu thụ 10 lít xăng. Sau đó, xe đổ thêm xăng, từ 0 lít lên 60 lít.
Tiếp theo, xe đi từ vị trí 10 đến vị trí 60 (tiêu thụ 50 lít xăng),
rồi đổ thêm xăng, từ 10 lít lên 50 lít. Sau đó xe tiếp tục đi và đến đích.
Xe đã dừng đổ xăng 2 lần trên đường, nên ta trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target, startFuel &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= stations.length &lt;= 500</code></li>
	<li><code>1 &lt;= position<sub>i</sub> &lt; position<sub>i+1</sub> &lt; target</code></li>
	<li><code>1 &lt;= fuel<sub>i</sub> &lt; 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue (Max-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Chọn các lần đổ xăng sao cho đến đích với số lần dừng ít nhất. Đích có thể ở rất xa, nhưng số trạm tối đa chỉ là $500$. Nếu bình xăng không đủ để đến trạm tiếp theo, ta nên lấy xăng từ trạm đã đi qua có nhiều xăng nhất.
>
> Duyệt các trạm theo vị trí và đưa lượng xăng của những trạm đã đi qua vào max-heap. Khi nhiên liệu còn âm, lấy lượng xăng lớn nhất khỏi heap. Nếu heap rỗng thì không thể đến đích. Xem đích như một trạm cuối cùng.

<!-- thinking:end -->

Ta có thể dùng priority queue (max-heap) $\textit{pq}$ để lưu lượng xăng ở tất cả trạm đã đi qua. Mỗi khi không đủ xăng, ta greedy lấy lượng xăng lớn nhất — phần tử đầu của $\textit{pq}$ — và tăng số lần đổ xăng $\textit{ans}$. Nếu $\textit{pq}$ rỗng mà vẫn thiếu xăng, nghĩa là không thể đến đích, nên trả về $-1$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số trạm xăng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minRefuelStops(
        self, target: int, startFuel: int, stations: List[List[int]]
    ) -> int:
        pq = []
        ans = pre = 0
        stations.append([target, 0])
        for pos, fuel in stations:
            dist = pos - pre
            startFuel -= dist
            while startFuel < 0 and pq:
                startFuel -= heappop(pq)
                ans += 1
            if startFuel < 0:
                return -1
            heappush(pq, -fuel)
            pre = pos
        return ans
```

#### Java

```java
class Solution {
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        int n = stations.length;
        int ans = 0, pre = 0;
        for (int i = 0; i <= n; ++i) {
            int pos = i < n ? stations[i][0] : target;
            int dist = pos - pre;
            startFuel -= dist;
            while (startFuel < 0 && !pq.isEmpty()) {
                startFuel += pq.poll();
                ++ans;
            }
            if (startFuel < 0) {
                return -1;
            }
            if (i < n) {
                pq.offer(stations[i][1]);
                pre = stations[i][0];
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
    int minRefuelStops(int target, int startFuel, vector<vector<int>>& stations) {
        priority_queue<int> pq;
        stations.push_back({target, 0});
        int ans = 0, pre = 0;
        for (const auto& station : stations) {
            int pos = station[0], fuel = station[1];
            int dist = pos - pre;
            startFuel -= dist;
            while (startFuel < 0 && !pq.empty()) {
                startFuel += pq.top();
                pq.pop();
                ++ans;
            }
            if (startFuel < 0) {
                return -1;
            }
            pq.push(fuel);
            pre = pos;
        }
        return ans;
    }
};
```

#### Go

```go
func minRefuelStops(target int, startFuel int, stations [][]int) int {
	pq := &hp{}
	ans, pre := 0, 0
	stations = append(stations, []int{target, 0})
	for _, station := range stations {
		pos, fuel := station[0], station[1]
		dist := pos - pre
		startFuel -= dist
		for startFuel < 0 && pq.Len() > 0 {
			startFuel += heap.Pop(pq).(int)
			ans++
		}
		if startFuel < 0 {
			return -1
		}
		heap.Push(pq, fuel)
		pre = pos
	}
	return ans
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

#### TypeScript

```ts
function minRefuelStops(target: number, startFuel: number, stations: number[][]): number {
    const pq = new MaxPriorityQueue<number>();
    let [ans, pre] = [0, 0];
    stations.push([target, 0]);
    for (const [pos, fuel] of stations) {
        const dist = pos - pre;
        startFuel -= dist;
        while (startFuel < 0 && !pq.isEmpty()) {
            startFuel += pq.dequeue();
            ans++;
        }
        if (startFuel < 0) {
            return -1;
        }
        pq.enqueue(fuel);
        pre = pos;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::BinaryHeap;

impl Solution {
    pub fn min_refuel_stops(target: i32, mut start_fuel: i32, mut stations: Vec<Vec<i32>>) -> i32 {
        let mut pq = BinaryHeap::new();
        let mut ans = 0;
        let mut pre = 0;

        stations.push(vec![target, 0]);

        for station in stations {
            let pos = station[0];
            let fuel = station[1];
            let dist = pos - pre;
            start_fuel -= dist;

            while start_fuel < 0 && !pq.is_empty() {
                start_fuel += pq.pop().unwrap();
                ans += 1;
            }

            if start_fuel < 0 {
                return -1;
            }

            pq.push(fuel);
            pre = pos;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
