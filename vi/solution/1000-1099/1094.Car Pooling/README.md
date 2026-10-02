---
comments: true
difficulty: Medium
rating: 1441
source: Weekly Contest 142 Q2
tags:
    - Array
    - Prefix Sum
    - Sorting
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1094. Car Pooling](https://leetcode.com/problems/car-pooling)

[中文文档](/solution/1000-1099/1094.Car%20Pooling/README.md)

## Mô tả

<!-- description:start -->

<p>Có một chiếc xe với <code>capacity</code> chỗ ngồi còn trống. Xe chỉ di chuyển về phía đông (tức là không thể quay đầu để đi về phía tây).</p>

<p>Cho số nguyên <code>capacity</code> và mảng <code>trips</code>, trong đó <code>trips[i] = [numPassengers<sub>i</sub>, from<sub>i</sub>, to<sub>i</sub>]</code> cho biết chuyến đi thứ <code>i<sup>th</sup></code> có <code>numPassengers<sub>i</sub></code> hành khách, đón tại vị trí <code>from<sub>i</sub></code> và trả tại vị trí <code>to<sub>i</sub></code>. Các vị trí được tính bằng số km về phía đông so với vị trí ban đầu của xe.</p>

<p>Trả về <code>true</code><em> nếu có thể đón và trả tất cả hành khách trong mọi chuyến đi đã cho; nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> trips = [[2,1,5],[3,3,7]], capacity = 4
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> trips = [[2,1,5],[3,3,7]], capacity = 5
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= trips.length &lt;= 1000</code></li>
	<li><code>trips[i].length == 3</code></li>
	<li><code>1 &lt;= numPassengers<sub>i</sub> &lt;= 100</code></li>
	<li><code>0 &lt;= from<sub>i</sub> &lt; to<sub>i</sub> &lt;= 1000</code></li>
	<li><code>1 &lt;= capacity &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chuyến đi làm tăng số hành khách trên đoạn $[from,to)$. Số hành khách trên xe không được vượt quá capacity. Vì vị trí lớn nhất là $1000$, chỉ cần dùng mảng hiệu.
>
> Cộng số hành khách tại điểm đón, trừ số đó tại điểm trả, rồi tính prefix sum và kiểm tra số hành khách ở mỗi vị trí có vượt quá $\textit{capacity}$ hay không.
>
> Mảng được tính đến điểm trả khách muộn nhất; tại các điểm không có hoạt động, số hành khách vẫn giữ nguyên.

<!-- thinking:end -->

Ta có thể dùng mảng hiệu: cộng số hành khách tại điểm bắt đầu của mỗi chuyến đi và trừ đi tại điểm kết thúc. Cuối cùng, chỉ cần kiểm tra prefix sum của mảng hiệu có vượt quá sức chứa tối đa của xe hay không.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(M)$, trong đó $n$ là số chuyến đi và $M$ là điểm kết thúc lớn nhất trong các chuyến. Với bài này, $M \le 1000$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        mx = max(e[2] for e in trips)
        d = [0] * (mx + 1)
        for x, f, t in trips:
            d[f] += x
            d[t] -= x
        return all(s <= capacity for s in accumulate(d))
```

#### Java

```java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int[] d = new int[1001];
        for (var trip : trips) {
            int x = trip[0], f = trip[1], t = trip[2];
            d[f] += x;
            d[t] -= x;
        }
        int s = 0;
        for (int x : d) {
            s += x;
            if (s > capacity) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        int d[1001]{};
        for (auto& trip : trips) {
            int x = trip[0], f = trip[1], t = trip[2];
            d[f] += x;
            d[t] -= x;
        }
        int s = 0;
        for (int x : d) {
            s += x;
            if (s > capacity) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func carPooling(trips [][]int, capacity int) bool {
	d := [1001]int{}
	for _, trip := range trips {
		x, f, t := trip[0], trip[1], trip[2]
		d[f] += x
		d[t] -= x
	}
	s := 0
	for _, x := range d {
		s += x
		if s > capacity {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function carPooling(trips: number[][], capacity: number): boolean {
    const mx = Math.max(...trips.map(([, , t]) => t));
    const d = Array(mx + 1).fill(0);
    for (const [x, f, t] of trips) {
        d[f] += x;
        d[t] -= x;
    }
    let s = 0;
    for (const x of d) {
        s += x;
        if (s > capacity) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn car_pooling(trips: Vec<Vec<i32>>, capacity: i32) -> bool {
        let mx = trips.iter().map(|e| e[2]).max().unwrap_or(0) as usize;
        let mut d = vec![0; mx + 1];
        for trip in &trips {
            let (x, f, t) = (trip[0], trip[1] as usize, trip[2] as usize);
            d[f] += x;
            d[t] -= x;
        }
        d.iter()
            .scan(0, |acc, &x| {
                *acc += x;
                Some(*acc)
            })
            .all(|s| s <= capacity)
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} trips
 * @param {number} capacity
 * @return {boolean}
 */
var carPooling = function (trips, capacity) {
    const mx = Math.max(...trips.map(([, , t]) => t));
    const d = Array(mx + 1).fill(0);
    for (const [x, f, t] of trips) {
        d[f] += x;
        d[t] -= x;
    }
    let s = 0;
    for (const x of d) {
        s += x;
        if (s > capacity) {
            return false;
        }
    }
    return true;
};
```

#### C#

```cs
public class Solution {
    public bool CarPooling(int[][] trips, int capacity) {
        int mx = trips.Max(x => x[2]);
        int[] d = new int[mx + 1];
        foreach (var trip in trips) {
            int x = trip[0], f = trip[1], t = trip[2];
            d[f] += x;
            d[t] -= x;
        }
        int s = 0;
        foreach (var x in d) {
            s += x;
            if (s > capacity) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
