---
comments: true
difficulty: Medium
rating: 1249
source: Weekly Contest 294 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2279. Maximum Bags With Full Capacity of Rocks](https://leetcode.com/problems/maximum-bags-with-full-capacity-of-rocks)

[中文文档](/solution/2200-2299/2279.Maximum%20Bags%20With%20Full%20Capacity%20of%20Rocks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> túi được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho hai mảng số nguyên <code>capacity</code> và <code>rocks</code>, được đánh chỉ số từ <strong>0</strong>. Túi <code>i<sup>th</sup></code> có thể chứa tối đa <code>capacity[i]</code> viên đá và hiện đang chứa <code>rocks[i]</code> viên đá. Bạn cũng được cho một số nguyên <code>additionalRocks</code>, là số viên đá bổ sung mà bạn có thể đặt vào <strong>bất kỳ</strong> túi nào.</p>

<p>Hãy trả về <em><strong>số túi nhiều nhất</strong> có thể đạt đúng sức chứa sau khi đặt thêm đá vào một số túi.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> capacity = [2,3,4,5], rocks = [1,2,4,4], additionalRocks = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Đặt 1 viên đá vào túi 0 và 1 viên đá vào túi 1.
Số viên đá trong mỗi túi lúc này là [2,3,4,4].
Các túi 0, 1 và 2 đã đạt đúng sức chứa.
Có 3 túi đạt đúng sức chứa, nên ta trả về 3.
Có thể chứng minh rằng không thể có hơn 3 túi đạt đúng sức chứa.
Lưu ý rằng có thể có những cách đặt đá khác nhưng vẫn cho đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> capacity = [10,2,2], rocks = [2,2,0], additionalRocks = 100
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Đặt 8 viên đá vào túi 0 và 2 viên đá vào túi 2.
Số viên đá trong mỗi túi lúc này là [10,2,2].
Các túi 0, 1 và 2 đã đạt đúng sức chứa.
Có 3 túi đạt đúng sức chứa, nên ta trả về 3.
Có thể chứng minh rằng không thể có hơn 3 túi đạt đúng sức chứa.
Lưu ý rằng ta không cần sử dụng hết số đá bổ sung.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == capacity.length == rocks.length</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= capacity[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= rocks[i] &lt;= capacity[i]</code></li>
	<li><code>1 &lt;= additionalRocks &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Đá bổ sung nên được dùng để lấp đầy nhiều túi nhất có thể. Các phần thiếu là độc lập và $n \le 5\times 10^4$, vì vậy nên ưu tiên lấp các phần thiếu nhỏ hơn.
>
> Sắp xếp $capacity[i]-rocks[i]$ rồi trừ dần vào $\textit{additionalRocks}$ theo thứ tự tăng dần; dừng lại khi phần thiếu tiếp theo không thể được đáp ứng.

<!-- thinking:end -->

Trước tiên, ta tính phần sức chứa còn thiếu của mỗi túi, sau đó sắp xếp các phần sức chứa còn thiếu. Tiếp theo, ta duyệt qua các phần sức chứa còn thiếu từ nhỏ đến lớn, đặt đá bổ sung vào các túi cho đến khi dùng hết số đá bổ sung hoặc đã lấp đầy các phần còn thiếu của các túi. Cuối cùng, ta trả về số túi đã đạt đúng sức chứa tại thời điểm đó.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là số lượng túi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBags(
        self, capacity: List[int], rocks: List[int], additionalRocks: int
    ) -> int:
        for i, x in enumerate(rocks):
            capacity[i] -= x
        capacity.sort()
        for i, x in enumerate(capacity):
            additionalRocks -= x
            if additionalRocks < 0:
                return i
        return len(capacity)
```

#### Java

```java
class Solution {
    public int maximumBags(int[] capacity, int[] rocks, int additionalRocks) {
        int n = rocks.length;
        for (int i = 0; i < n; ++i) {
            capacity[i] -= rocks[i];
        }
        Arrays.sort(capacity);
        for (int i = 0; i < n; ++i) {
            additionalRocks -= capacity[i];
            if (additionalRocks < 0) {
                return i;
            }
        }
        return n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumBags(vector<int>& capacity, vector<int>& rocks, int additionalRocks) {
        int n = rocks.size();
        for (int i = 0; i < n; ++i) {
            capacity[i] -= rocks[i];
        }
        ranges::sort(capacity);
        for (int i = 0; i < n; ++i) {
            additionalRocks -= capacity[i];
            if (additionalRocks < 0) {
                return i;
            }
        }
        return n;
    }
};
```

#### Go

```go
func maximumBags(capacity []int, rocks []int, additionalRocks int) int {
	for i, x := range rocks {
		capacity[i] -= x
	}
	sort.Ints(capacity)
	for i, x := range capacity {
		additionalRocks -= x
		if additionalRocks < 0 {
			return i
		}
	}
	return len(capacity)
}
```

#### TypeScript

```ts
function maximumBags(capacity: number[], rocks: number[], additionalRocks: number): number {
    const n = rocks.length;
    for (let i = 0; i < n; ++i) {
        capacity[i] -= rocks[i];
    }
    capacity.sort((a, b) => a - b);
    for (let i = 0; i < n; ++i) {
        additionalRocks -= capacity[i];
        if (additionalRocks < 0) {
            return i;
        }
    }
    return n;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_bags(mut capacity: Vec<i32>, rocks: Vec<i32>, mut additional_rocks: i32) -> i32 {
        for i in 0..rocks.len() {
            capacity[i] -= rocks[i];
        }
        capacity.sort();
        for i in 0..capacity.len() {
            additional_rocks -= capacity[i];
            if additional_rocks < 0 {
                return i as i32;
            }
        }
        capacity.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
