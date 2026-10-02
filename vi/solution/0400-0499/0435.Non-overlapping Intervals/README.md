---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals)

[中文文档](/solution/0400-0499/0435.Non-overlapping%20Intervals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các khoảng <code>intervals</code>, trong đó <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>. Hãy trả về <em>số khoảng ít nhất cần xóa để các khoảng còn lại không chồng lấn</em>.</p>

<p><strong>Lưu ý</strong>, hai khoảng chỉ tiếp xúc tại một điểm được xem là <strong>không chồng lấn</strong>. Ví dụ, <code>[1, 2]</code> và <code>[2, 3]</code> không chồng lấn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,2],[2,3],[3,4],[1,3]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có thể xóa [1,3] để các khoảng còn lại không chồng lấn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,2],[1,2],[1,2]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Cần xóa hai khoảng [1,2] để các khoảng còn lại không chồng lấn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,2],[2,3]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không cần xóa khoảng nào vì chúng đã không chồng lấn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= intervals.length &lt;= 10<sup>5</sup></code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>-5 * 10<sup>4</sup> &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 5 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Xóa ít khoảng nhất để phần còn lại rời nhau tương đương với việc giữ lại tập hợp lớn nhất gồm các khoảng không chồng lấn. Duyệt mọi tập con sẽ quá tốn kém.
>
> Sắp xếp theo điểm kết thúc, rồi giữ một khoảng nếu nó bắt đầu tại hoặc sau điểm kết thúc của khoảng được giữ gần nhất. Khoảng kết thúc sớm nhất chừa nhiều chỗ nhất cho các khoảng sau, nên ta sắp xếp theo điểm kết thúc.
>
> Khởi tạo đáp án bằng $n$ rồi giảm đi một cho mỗi khoảng được giữ lại: tổng số khoảng trừ kích thước của tập hợp tương thích lớn nhất.

<!-- thinking:end -->

Đầu tiên, sắp xếp các khoảng theo điểm kết thúc tăng dần. Dùng biến $\textit{pre}$ để lưu điểm kết thúc của khoảng trước đó và biến $\textit{ans}$ để lưu số khoảng cần xóa. Ban đầu, $\textit{ans} = \textit{intervals.length}$. 

Sau đó, duyệt lần lượt các khoảng. Với mỗi khoảng:

- Nếu điểm bắt đầu của khoảng hiện tại lớn hơn hoặc bằng $\textit{pre}$, ta không cần xóa khoảng này. Cập nhật $\textit{pre}$ thành điểm kết thúc của khoảng hiện tại và giảm $\textit{ans}$ đi một;
- Ngược lại, cần xóa khoảng này nên không cập nhật $\textit{pre}$ và $\textit{ans}$. 

Cuối cùng, trả về $\textit{ans}$. 

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số khoảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: x[1])
        ans = len(intervals)
        pre = -inf
        for l, r in intervals:
            if pre <= l:
                ans -= 1
                pre = r
        return ans
```

#### Java

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
        int ans = intervals.length;
        int pre = Integer.MIN_VALUE;
        for (var e : intervals) {
            int l = e[0], r = e[1];
            if (pre <= l) {
                --ans;
                pre = r;
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
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        ranges::sort(intervals, [](const vector<int>& a, const vector<int>& b) {
            return a[1] < b[1];
        });
        int ans = intervals.size();
        int pre = INT_MIN;
        for (const auto& e : intervals) {
            int l = e[0], r = e[1];
            if (pre <= l) {
                --ans;
                pre = r;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func eraseOverlapIntervals(intervals [][]int) int {
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][1] < intervals[j][1]
	})
	ans := len(intervals)
	pre := math.MinInt32
	for _, e := range intervals {
		l, r := e[0], e[1]
		if pre <= l {
			ans--
			pre = r
		}
	}
	return ans
}
```

#### TypeScript

```ts
function eraseOverlapIntervals(intervals: number[][]): number {
    intervals.sort((a, b) => a[1] - b[1]);
    let [ans, pre] = [intervals.length, -Infinity];
    for (const [l, r] of intervals) {
        if (pre <= l) {
            --ans;
            pre = r;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
