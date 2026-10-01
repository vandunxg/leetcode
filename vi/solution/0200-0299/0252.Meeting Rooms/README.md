---
comments: true
difficulty: Easy
tags:
    - Array
    - Sorting
    - Quick Sort
---

<!-- problem:start -->

# [252. Meeting Rooms 🔒](https://leetcode.com/problems/meeting-rooms)

[中文文档](/solution/0200-0299/0252.Meeting%20Rooms/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các khoảng thời gian họp&nbsp;<code>intervals</code>, trong đó <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>.</p>

<p>Một người có thể tham dự tất cả cuộc họp nếu không có hai khoảng thời gian họp nào bị chồng lấn. Cuộc họp kết thúc tại thời điểm <code>t</code> và cuộc họp bắt đầu tại thời điểm <code>t</code> <strong>không</strong> bị chồng lấn.</p>

<p>​​​​​​​Trả về <code>true</code> nếu một người có thể tham dự tất cả cuộc họp. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> intervals = [[0,30],[5,10],[15,20]]
<strong>Đầu ra:</strong> false
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> intervals = [[7,10],[2,4]]
<strong>Đầu ra:</strong> true
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= intervals.length &lt;= 10<sup>4</sup></code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;&nbsp;end<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Một người không thể tham dự hai cuộc họp bị chồng lấn. Sau khi sắp xếp theo thời gian bắt đầu, chỉ cần bảo đảm mỗi cuộc họp kết thúc không muộn hơn thời điểm bắt đầu của cuộc họp tiếp theo.

<!-- thinking:end -->

Ta sắp xếp các cuộc họp theo thời gian bắt đầu, rồi duyệt lần lượt danh sách đã sắp xếp. Nếu thời gian bắt đầu của cuộc họp hiện tại nhỏ hơn thời gian kết thúc của cuộc họp trước đó, nghĩa là hai cuộc họp bị chồng lấn và ta trả về `false`. Nếu không, ta tiếp tục duyệt.

Nếu duyệt hết mà không phát hiện khoảng thời gian nào bị chồng lấn, ta trả về `true`.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số cuộc họp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
        intervals.sort()
        return all(a[1] <= b[0] for a, b in pairwise(intervals))
```

#### Java

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        for (int i = 1; i < intervals.length; ++i) {
            if (intervals[i - 1][1] > intervals[i][0]) {
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
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        ranges::sort(intervals, [](const auto& a, const auto& b) {
            return a[0] < b[0];
        });
        for (int i = 1; i < intervals.size(); ++i) {
            if (intervals[i - 1][1] > intervals[i][0]) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canAttendMeetings(intervals [][]int) bool {
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})
	for i := 1; i < len(intervals); i++ {
		if intervals[i][0] < intervals[i-1][1] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function canAttendMeetings(intervals: number[][]): boolean {
    intervals.sort((a, b) => a[0] - b[0]);
    for (let i = 1; i < intervals.length; ++i) {
        if (intervals[i][0] < intervals[i - 1][1]) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_attend_meetings(mut intervals: Vec<Vec<i32>>) -> bool {
        intervals.sort_by(|a, b| a[0].cmp(&b[0]));
        for i in 1..intervals.len() {
            if intervals[i - 1][1] > intervals[i][0] {
                return false;
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
