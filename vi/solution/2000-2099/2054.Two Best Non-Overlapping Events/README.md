---
comments: true
difficulty: Medium
rating: 1883
source: Biweekly Contest 64 Q2
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2054. Two Best Non-Overlapping Events](https://leetcode.com/problems/two-best-non-overlapping-events)

[中文文档](/solution/2000-2099/2054.Two%20Best%20Non-Overlapping%20Events/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>events</code> <strong>được đánh chỉ số từ 0</strong>, trong đó <code>events[i] = [startTime<sub>i</sub>, endTime<sub>i</sub>, value<sub>i</sub>]</code>. Sự kiện thứ <code>i<sup>th</sup></code> bắt đầu tại <code>startTime<sub>i</sub></code><sub> </sub> và kết thúc tại <code>endTime<sub>i</sub></code>; nếu tham dự sự kiện này, bạn sẽ nhận được giá trị <code>value<sub>i</sub></code>. Bạn có thể chọn tham dự <strong>tối đa</strong> <strong>hai</strong> sự kiện <strong>không chồng lấp</strong> sao cho tổng giá trị của chúng là <strong>lớn nhất</strong>.</p>

<p>Trả về <em>tổng <strong>lớn nhất</strong> này.</em></p>

<p>Lưu ý rằng thời gian bắt đầu và thời gian kết thúc là <strong>bao gồm cả hai đầu mút</strong>: nghĩa là bạn không thể tham dự hai sự kiện mà một sự kiện bắt đầu và sự kiện kia kết thúc tại cùng một thời điểm. Cụ thể hơn, nếu bạn tham dự một sự kiện có thời gian kết thúc là <code>t</code>, sự kiện tiếp theo phải bắt đầu từ <code>t + 1</code> trở đi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2054.Two%20Best%20Non-Overlapping%20Events/images/untitled-diagramdrawio.png" style="width: 400px; height: 86px;" />
<pre>
<strong>Đầu vào:</strong> events = [[1,3,2],[4,5,2],[2,4,3]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Chọn hai sự kiện màu xanh, 0 và 1, có tổng bằng 2 + 2 = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="Example 1 Diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2054.Two%20Best%20Non-Overlapping%20Events/images/2054b.png" style="width: 400px; height: 86px;" />
<pre>
<strong>Đầu vào:</strong> events = [[1,3,2],[4,5,2],[1,5,5]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích: </strong>Chọn sự kiện 2, có giá trị bằng 5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2054.Two%20Best%20Non-Overlapping%20Events/images/2054c.png" style="width: 400px; height: 74px;" />
<pre>
<strong>Đầu vào:</strong> events = [[1,5,3],[1,5,1],[6,6,5]]
<strong>Đầu ra:</strong> 8
<strong>Giải thích: </strong>Chọn các sự kiện 0 và 2, có tổng bằng 3 + 5 = 8.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= events.length &lt;= 10<sup>5</sup></code></li>
	<li><code>events[i].length == 3</code></li>
	<li><code>1 &lt;= startTime<sub>i</sub> &lt;= endTime<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= value<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Cần chọn tối đa hai sự kiện không chồng lấp có tổng giá trị lớn nhất. Duyệt từng cặp có độ phức tạp bậc hai, không phù hợp với $n \le 10^5$. Sau khi sắp xếp theo thời gian bắt đầu, giá trị lớn nhất của một sự kiện trong mỗi hậu tố tạo thành một mảng cố định.
>
> $f[i]$ là giá trị lớn nhất trong hậu tố đó. Với mỗi sự kiện được chọn làm sự kiện đầu tiên, ta tìm kiếm nhị phân sự kiện đầu tiên có thời gian bắt đầu lớn hơn thời gian kết thúc của nó rồi cộng thêm $f[idx]$ (hoặc chỉ chọn riêng sự kiện đó).
>
> Sắp xếp kết hợp tìm kiếm nhị phân cho độ phức tạp $O(n \log n)$.

<!-- thinking:end -->

Ta có thể sắp xếp các sự kiện theo thời gian bắt đầu, sau đó tiền xử lý giá trị lớn nhất bắt đầu từ mỗi sự kiện, tức là $f[i]$ biểu diễn giá trị lớn nhất khi chọn một sự kiện từ sự kiện thứ $i$ đến sự kiện cuối cùng.

Sau đó, ta duyệt qua từng sự kiện. Với mỗi sự kiện, ta dùng tìm kiếm nhị phân để tìm sự kiện đầu tiên có thời gian bắt đầu lớn hơn thời gian kết thúc của sự kiện hiện tại, gọi là $\textit{idx}$. Giá trị lớn nhất khi bắt đầu từ sự kiện hiện tại là $f[\textit{idx}]$ cộng với giá trị của sự kiện hiện tại, đây là giá trị lớn nhất có thể đạt được khi chọn sự kiện hiện tại làm sự kiện đầu tiên. Ta lấy giá trị lớn nhất trong tất cả các giá trị này.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng sự kiện.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTwoEvents(self, events: List[List[int]]) -> int:
        events.sort()
        n = len(events)
        f = [events[-1][2]] * n
        for i in range(n - 2, -1, -1):
            f[i] = max(f[i + 1], events[i][2])
        ans = 0
        for _, e, v in events:
            idx = bisect_right(events, e, key=lambda x: x[0])
            if idx < n:
                v += f[idx]
            ans = max(ans, v)
        return ans
```

#### Java

```java
class Solution {
    public int maxTwoEvents(int[][] events) {
        Arrays.sort(events, (a, b) -> a[0] - b[0]);
        int n = events.length;
        int[] f = new int[n + 1];
        for (int i = n - 1; i >= 0; --i) {
            f[i] = Math.max(f[i + 1], events[i][2]);
        }
        int ans = 0;
        for (int[] e : events) {
            int v = e[2];
            int left = 0, right = n;
            while (left < right) {
                int mid = (left + right) >> 1;
                if (events[mid][0] > e[1]) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            if (left < n) {
                v += f[left];
            }
            ans = Math.max(ans, v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTwoEvents(vector<vector<int>>& events) {
        ranges::sort(events);
        int n = events.size();
        vector<int> f(n + 1);
        for (int i = n - 1; ~i; --i) {
            f[i] = max(f[i + 1], events[i][2]);
        }
        int ans = 0;
        for (const auto& e : events) {
            int v = e[2];
            int left = 0, right = n;
            while (left < right) {
                int mid = (left + right) >> 1;
                if (events[mid][0] > e[1]) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            if (left < n) {
                v += f[left];
            }
            ans = max(ans, v);
        }
        return ans;
    }
};
```

#### Go

```go
func maxTwoEvents(events [][]int) int {
	sort.Slice(events, func(i, j int) bool {
		return events[i][0] < events[j][0]
	})
	n := len(events)
	f := make([]int, n+1)
	for i := n - 1; i >= 0; i-- {
		f[i] = max(f[i+1], events[i][2])
	}
	ans := 0
	for _, e := range events {
		v := e[2]
		left, right := 0, n
		for left < right {
			mid := (left + right) >> 1
			if events[mid][0] > e[1] {
				right = mid
			} else {
				left = mid + 1
			}
		}
		if left < n {
			v += f[left]
		}
		ans = max(ans, v)
	}
	return ans
}
```

#### TypeScript

```ts
function maxTwoEvents(events: number[][]): number {
    events.sort((a, b) => a[0] - b[0]);
    const n = events.length;
    const f: number[] = Array(n + 1).fill(0);
    for (let i = n - 1; ~i; --i) {
        f[i] = Math.max(f[i + 1], events[i][2]);
    }
    let ans = 0;
    for (const [_, end, v] of events) {
        let [left, right] = [0, n];
        while (left < right) {
            const mid = (left + right) >> 1;
            if (events[mid][0] > end) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        const t = left < n ? f[left] : 0;
        ans = Math.max(ans, t + v);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_two_events(mut events: Vec<Vec<i32>>) -> i32 {
        events.sort_by(|a, b| a[0].cmp(&b[0]));

        let n: usize = events.len();
        let mut f: Vec<i32> = vec![0; n + 1];

        for i in (0..n).rev() {
            f[i] = f[i + 1].max(events[i][2]);
        }

        let mut ans: i32 = 0;

        for e in &events {
            let mut v: i32 = e[2];

            let mut left: usize = 0;
            let mut right: usize = n;
            while left < right {
                let mid = (left + right) >> 1;
                if events[mid][0] > e[1] {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }

            if left < n {
                v += f[left];
            }

            ans = ans.max(v);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
