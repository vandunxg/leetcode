---
comments: true
difficulty: Easy
rating: 1234
source: Weekly Contest 153 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1184. Distance Between Bus Stops](https://leetcode.com/problems/distance-between-bus-stops)

[中文文档](/solution/1100-1199/1184.Distance%20Between%20Bus%20Stops/README.md)

## Mô tả

<!-- description:start -->

<p>Một tuyến xe buýt có <code>n</code> trạm được đánh số từ <code>0</code> đến <code>n - 1</code>, sắp xếp thành một vòng tròn. Ta biết khoảng cách giữa mọi cặp trạm liền kề; <code>distance[i]</code> là khoảng cách giữa trạm số&nbsp;<code>i</code> và trạm <code>(i + 1) % n</code>.</p>

<p>Xe buýt có thể chạy theo cả hai hướng&nbsp;là chiều kim đồng hồ và ngược chiều kim đồng hồ.</p>

<p>Trả về khoảng cách ngắn nhất giữa hai trạm&nbsp;<code>start</code>&nbsp;và <code>destination</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1184.Distance%20Between%20Bus%20Stops/images/untitled-diagram-1.jpg" style="width: 388px; height: 240px;" /></p>

<pre>
<strong>Đầu vào:</strong> distance = [1,2,3,4], start = 0, destination = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Khoảng cách giữa trạm 0 và trạm 1 là 1 hoặc 9; giá trị nhỏ nhất là 1.</pre>

<p>&nbsp;</p>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1184.Distance%20Between%20Bus%20Stops/images/untitled-diagram-1-1.jpg" style="width: 388px; height: 240px;" /></p>

<pre>
<strong>Đầu vào:</strong> distance = [1,2,3,4], start = 0, destination = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Khoảng cách giữa trạm 0 và trạm 2 là 3 hoặc 7; giá trị nhỏ nhất là 3.
</pre>

<p>&nbsp;</p>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1184.Distance%20Between%20Bus%20Stops/images/untitled-diagram-1-2.jpg" style="width: 388px; height: 240px;" /></p>

<pre>
<strong>Đầu vào:</strong> distance = [1,2,3,4], start = 0, destination = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Khoảng cách giữa trạm 0 và trạm 3 là 6 hoặc 4; giá trị nhỏ nhất là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n&nbsp;&lt;= 10^4</code></li>
	<li><code>distance.length == n</code></li>
	<li><code>0 &lt;= start, destination &lt; n</code></li>
	<li><code>0 &lt;= distance[i] &lt;= 10^4</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các trạm tạo thành một vòng tròn nên chỉ cần xét hai cung theo chiều kim đồng hồ và ngược chiều kim đồng hồ. Tính tổng độ dài vòng tròn là $s$, đi theo một hướng từ $start$ đến $destination$ để được $t$, rồi lấy $\min(t,s-t)$. Không cần dùng graph.

<!-- thinking:end -->

Trước tiên, tính tổng khoảng cách $s$ mà xe buýt đi qua, sau đó mô phỏng hành trình. Bắt đầu từ trạm xuất phát, mỗi lần di chuyển sang trạm kế tiếp cho đến khi đến đích, đồng thời ghi nhận quãng đường đã đi là $t$. Cuối cùng, trả về giá trị nhỏ hơn giữa $t$ và $s - t$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{distance}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distanceBetweenBusStops(
        self, distance: List[int], start: int, destination: int
    ) -> int:
        s = sum(distance)
        t, n = 0, len(distance)
        while start != destination:
            t += distance[start]
            start = (start + 1) % n
        return min(t, s - t)
```

#### Java

```java
class Solution {
    public int distanceBetweenBusStops(int[] distance, int start, int destination) {
        int s = Arrays.stream(distance).sum();
        int n = distance.length, t = 0;
        while (start != destination) {
            t += distance[start];
            start = (start + 1) % n;
        }
        return Math.min(t, s - t);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distanceBetweenBusStops(vector<int>& distance, int start, int destination) {
        int s = accumulate(distance.begin(), distance.end(), 0);
        int t = 0, n = distance.size();
        while (start != destination) {
            t += distance[start];
            start = (start + 1) % n;
        }
        return min(t, s - t);
    }
};
```

#### Go

```go
func distanceBetweenBusStops(distance []int, start int, destination int) int {
	s, t := 0, 0
	for _, x := range distance {
		s += x
	}
	for start != destination {
		t += distance[start]
		start = (start + 1) % len(distance)
	}
	return min(t, s-t)
}
```

#### TypeScript

```ts
function distanceBetweenBusStops(distance: number[], start: number, destination: number): number {
    const s = distance.reduce((a, b) => a + b, 0);
    const n = distance.length;
    let t = 0;
    while (start !== destination) {
        t += distance[start];
        start = (start + 1) % n;
    }
    return Math.min(t, s - t);
}
```

#### Rust

```rust
impl Solution {
    pub fn distance_between_bus_stops(distance: Vec<i32>, start: i32, destination: i32) -> i32 {
        let s: i32 = distance.iter().sum();
        let mut t = 0;
        let n = distance.len();
        let mut start = start as usize;
        let destination = destination as usize;

        while start != destination {
            t += distance[start];
            start = (start + 1) % n;
        }

        t.min(s - t)
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} distance
 * @param {number} start
 * @param {number} destination
 * @return {number}
 */
var distanceBetweenBusStops = function (distance, start, destination) {
    const s = distance.reduce((a, b) => a + b, 0);
    const n = distance.length;
    let t = 0;
    while (start !== destination) {
        t += distance[start];
        start = (start + 1) % n;
    }
    return Math.min(t, s - t);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
