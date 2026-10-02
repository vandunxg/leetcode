---
comments: true
difficulty: Hard
tags:
    - Array
    - Sorting
    - Sweep Line
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [759. Employee Free Time 🔒](https://leetcode.com/problems/employee-free-time)

[中文文档](/solution/0700-0799/0759.Employee%20Free%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách <code>schedule</code> gồm lịch làm việc của các nhân viên.</p>

<p>Mỗi nhân viên có một danh sách <code>Intervals</code> không giao nhau, được sắp xếp theo thứ tự.</p>

<p>Trả về danh sách các khoảng hữu hạn biểu diễn <b>thời gian rảnh chung có độ dài dương</b> của <i>tất cả</i> nhân viên, cũng theo thứ tự tăng dần.</p>

<p>(Dù biểu diễn <code>Intervals</code> dưới dạng <code>[x, y]</code>, các phần tử bên trong là đối tượng <code>Intervals</code>, không phải list hay array. Ví dụ, <code>schedule[0][0].start = 1</code>, <code>schedule[0][0].end = 2</code>, còn <code>schedule[0][0][0]</code> không được định nghĩa).&nbsp; Ngoài ra, không đưa các khoảng như [5, 5] vào kết quả vì chúng có độ dài bằng 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]
<strong>Đầu ra:</strong> [[3,4]]
<strong>Giải thích:</strong> Có tổng cộng ba nhân viên; các khoảng thời gian rảnh chung là [-inf, 1], [3, 4], [10, inf].
Ta loại bỏ các khoảng chứa inf vì chúng không hữu hạn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> schedule = [[[1,3],[6,7]],[[2,4]],[[2,5],[9,12]]]
<strong>Đầu ra:</strong> [[5,6],[7,9]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= schedule.length , schedule[i].length &lt;= 50</code></li>
	<li><code>0 &lt;= schedule[i].start &lt; schedule[i].end &lt;= 10^8</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gộp khoảng

<!-- thinking:start -->

> **Tư duy**
>
> Tìm các khoảng trống khi mọi nhân viên đều rảnh. Các khoảng của từng người đã được sắp xếp, nhưng danh sách gộp chung thì chưa.
>
> Các khoảng có người đang làm việc tạo thành hợp của toàn bộ các khoảng; những khoảng trống trong hợp đó chính là thời gian rảnh chung.
>
> Sắp xếp theo thời điểm bắt đầu, gộp các khoảng giao nhau, rồi lấy $[a.end, b.start)$ giữa hai khoảng đã gộp liên tiếp.

<!-- thinking:end -->

Ta gộp khoảng thời gian làm việc của tất cả nhân viên vào một danh sách, sau đó sắp xếp và gộp các khoảng giao nhau. Cuối cùng, duyệt danh sách đã gộp để tìm các khoảng thời gian rảnh nằm giữa hai khoảng liền kề.

Độ phức tạp thời gian là $O(mn \log(mn))$ và độ phức tạp không gian là $O(mn)$, trong đó $m$ là số nhân viên và $n$ là số khoảng làm việc của mỗi nhân viên.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for an Interval.
class Interval:
    def __init__(self, start: int = None, end: int = None):
        self.start = start
        self.end = end
"""


class Solution:
    def employeeFreeTime(self, schedule: "[[Interval]]") -> "[Interval]":
        intervals = []
        for e in schedule:
            intervals.extend(e)
        intervals.sort(key=lambda x: (x.start, x.end))
        merged = [intervals[0]]
        for x in intervals[1:]:
            if merged[-1].end < x.start:
                merged.append(x)
            else:
                merged[-1].end = max(merged[-1].end, x.end)
        ans = []
        for a, b in pairwise(merged):
            ans.append(Interval(a.end, b.start))
        return ans
```

#### Java

```java
/*
// Definition for an Interval.
class Interval {
    public int start;
    public int end;

    public Interval() {}

    public Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
};
*/

class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<Interval> intervals = new ArrayList<>();
        for (List<Interval> e : schedule) {
            intervals.addAll(e);
        }

        intervals.sort((a, b) -> a.start == b.start ? a.end - b.end : a.start - b.start);

        List<Interval> merged = new ArrayList<>();
        merged.add(intervals.get(0));
        for (int i = 1; i < intervals.size(); ++i) {
            Interval last = merged.get(merged.size() - 1);
            Interval cur = intervals.get(i);
            if (last.end < cur.start) {
                merged.add(cur);
            } else {
                last.end = Math.max(last.end, cur.end);
            }
        }

        List<Interval> ans = new ArrayList<>();
        for (int i = 1; i < merged.size(); ++i) {
            Interval a = merged.get(i - 1);
            Interval b = merged.get(i);
            ans.add(new Interval(a.end, b.start));
        }

        return ans;
    }
}
```

#### C++

```cpp
/*
// Definition for an Interval.
class Interval {
public:
    int start;
    int end;

    Interval() {}

    Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
};
*/

class Solution {
public:
    vector<Interval> employeeFreeTime(vector<vector<Interval>> schedule) {
        vector<Interval> intervals;
        for (auto& e : schedule) {
            intervals.insert(intervals.end(), e.begin(), e.end());
        }

        sort(intervals.begin(), intervals.end(), [](const Interval& a, const Interval& b) {
            if (a.start == b.start) return a.end < b.end;
            return a.start < b.start;
        });

        vector<Interval> merged;
        merged.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); ++i) {
            auto& last = merged.back();
            auto& cur = intervals[i];
            if (last.end < cur.start) {
                merged.push_back(cur);
            } else {
                last.end = max(last.end, cur.end);
            }
        }

        vector<Interval> ans;
        for (int i = 1; i < merged.size(); ++i) {
            auto& a = merged[i - 1];
            auto& b = merged[i];
            ans.emplace_back(a.end, b.start);
        }

        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for an Interval.
 * type Interval struct {
 *     Start int
 *     End   int
 * }
 */

func employeeFreeTime(schedule [][]*Interval) []*Interval {
	var intervals []*Interval
	for _, e := range schedule {
		intervals = append(intervals, e...)
	}

	sort.Slice(intervals, func(i, j int) bool {
		if intervals[i].Start == intervals[j].Start {
			return intervals[i].End < intervals[j].End
		}
		return intervals[i].Start < intervals[j].Start
	})

	merged := []*Interval{intervals[0]}
	for _, cur := range intervals[1:] {
		last := merged[len(merged)-1]
		if last.End < cur.Start {
			merged = append(merged, cur)
		} else if cur.End > last.End {
			last.End = cur.End
		}
	}

	var ans []*Interval
	for i := 1; i < len(merged); i++ {
		a, b := merged[i-1], merged[i]
		ans = append(ans, &Interval{Start: a.End, End: b.Start})
	}

	return ans
}
```

#### TypeScript

```ts
/**
 * // Definition for an Interval.
 * class Interval {
 *    start: number;
 *    end: number;
 *    constructor(start: number, end: number) {
 *        this.start = start;
 *        this.end = end;
 *    }
 * }
 */
function employeeFreeTime(schedule: Interval[][]): Interval[] {
    const intervals: Interval[] = [];
    for (const e of schedule) {
        intervals.push(...e);
    }

    intervals.sort((a, b) => (a.start === b.start ? a.end - b.end : a.start - b.start));

    const merged: Interval[] = [intervals[0]];
    for (let i = 1; i < intervals.length; ++i) {
        const last = merged[merged.length - 1];
        const cur = intervals[i];
        if (last.end < cur.start) {
            merged.push(cur);
        } else {
            last.end = Math.max(last.end, cur.end);
        }
    }

    const ans: Interval[] = [];
    for (let i = 1; i < merged.length; ++i) {
        const a = merged[i - 1];
        const b = merged[i];
        ans.push(new Interval(a.end, b.start));
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
