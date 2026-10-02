---
comments: true
difficulty: Medium
rating: 1375
source: Biweekly Contest 15 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [1288. Remove Covered Intervals](https://leetcode.com/problems/remove-covered-intervals)

[中文文档](/solution/1200-1299/1288.Remove%20Covered%20Intervals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>intervals</code>, trong đó <code>intervals[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> biểu diễn khoảng <code>[l<sub>i</sub>, r<sub>i</sub>)</code>. Hãy xóa mọi khoảng được một khoảng khác trong danh sách bao phủ.</p>

<p>Khoảng <code>[a, b)</code> được khoảng <code>[c, d)</code> bao phủ khi và chỉ khi <code>c &lt;= a</code> và <code>b &lt;= d</code>.</p>

<p>Trả về <em>số khoảng còn lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> intervals = [[1,4],[3,6],[2,8]]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Khoảng [3,6] được [2,8] bao phủ nên bị xóa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> intervals = [[1,4],[2,3]]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= intervals.length &lt;= 1000</code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt; r<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả các khoảng được cho đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta loại các khoảng bị khoảng khác bao phủ. Sắp xếp theo đầu trái tăng dần; nếu bằng nhau thì sắp xếp đầu phải giảm dần, nhờ đó một khoảng đứng trước không thể bị khoảng hẹp hơn đứng sau bao phủ. Ta lưu đầu phải lớn nhất đã gặp: nếu đầu phải hiện tại lớn hơn hẳn thì khoảng đó không bị bao phủ. Cách sắp xếp biến bài toán bao phủ hai chiều thành phép kiểm tra đầu phải một chiều.

<!-- thinking:end -->

Ta sắp xếp các khoảng theo đầu trái tăng dần; nếu đầu trái bằng nhau thì sắp xếp theo đầu phải giảm dần.

Sau khi sắp xếp, ta duyệt các khoảng. Nếu đầu phải của khoảng hiện tại lớn hơn đầu phải trước đó, nghĩa là khoảng hiện tại không bị bao phủ, nên ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số khoảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeCoveredIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: (x[0], -x[1]))
        ans = 0
        pre = -inf
        for _, cur in intervals:
            if cur > pre:
                ans += 1
                pre = cur
        return ans
```

#### Java

```java
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int ans = 0, pre = Integer.MIN_VALUE;
        for (var e : intervals) {
            int cur = e[1];
            if (cur > pre) {
                ++ans;
                pre = cur;
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
    int removeCoveredIntervals(vector<vector<int>>& intervals) {
        ranges::sort(intervals, [](const vector<int>& a, const vector<int>& b) {
            return a[0] == b[0] ? a[1] > b[1] : a[0] < b[0];
        });
        int ans = 0, pre = INT_MIN;
        for (const auto& e : intervals) {
            int cur = e[1];
            if (cur > pre) {
                ++ans;
                pre = cur;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeCoveredIntervals(intervals [][]int) (ans int) {
	sort.Slice(intervals, func(i, j int) bool {
		if intervals[i][0] == intervals[j][0] {
			return intervals[i][1] > intervals[j][1]
		}
		return intervals[i][0] < intervals[j][0]
	})
	pre := math.MinInt32
	for _, e := range intervals {
		cur := e[1]
		if cur > pre {
			ans++
			pre = cur
		}
	}
	return
}
```

#### TypeScript

```ts
function removeCoveredIntervals(intervals: number[][]): number {
    intervals.sort((a, b) => (a[0] === b[0] ? b[1] - a[1] : a[0] - b[0]));
    let ans = 0;
    let pre = -Infinity;
    for (const [_, cur] of intervals) {
        if (cur > pre) {
            ++ans;
            pre = cur;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} intervals
 * @return {number}
 */
var removeCoveredIntervals = function (intervals) {
    intervals.sort((a, b) => (a[0] === b[0] ? b[1] - a[1] : a[0] - b[0]));
    let ans = 0;
    let pre = -Infinity;
    for (const [_, cur] of intervals) {
        if (cur > pre) {
            ++ans;
            pre = cur;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
