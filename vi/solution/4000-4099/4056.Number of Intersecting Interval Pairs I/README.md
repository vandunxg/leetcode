---
comments: true
difficulty: Easy
rating: 1161
source: Weekly Contest 520 Q1
---

<!-- problem:start -->

# [4056. Number of Intersecting Interval Pairs I](https://leetcode.com/problems/number-of-intersecting-interval-pairs-i)

[中文文档](/solution/4000-4099/4056.Number%20of%20Intersecting%20Interval%20Pairs%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>intervals</code> gồm <code>n</code> phần tử, trong đó <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> biểu diễn đoạn <strong>đóng</strong> từ <code>start<sub>i</sub></code> đến <code>end<sub>i</sub></code>.</p>

<p>Trả về số cặp chỉ số <code>(i, j)</code> thỏa mãn <code>0 &lt;= i &lt; j &lt; n</code> và <code>intervals[i]</code> và <code>intervals[j]</code> <strong>giao nhau</strong>.</p>

<p>Hai đoạn <strong>giao nhau</strong> nếu chúng có ít nhất một điểm chung, kể cả khi chúng chỉ chung một đầu mút.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">intervals = [[1,2],[2,3],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 2 cặp đoạn giao nhau:</p>

<ul>
	<li>Các đoạn <code>[1, 2]</code> và <code>[2, 3]</code> giao nhau tại điểm 2.</li>
	<li>Các đoạn <code>[2, 3]</code> và <code>[3, 4]</code> giao nhau tại điểm 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">intervals = [[1,5],[2,4],[3,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 3 cặp đoạn giao nhau:</p>

<ul>
	<li>Phần giao của <code>[1, 5]</code> và <code>[2, 4]</code> là <code>[2, 4]</code>.</li>
	<li>Phần giao của <code>[1, 5]</code> và <code>[3, 6]</code> là <code>[3, 5]</code>.</li>
	<li>Phần giao của <code>[2, 4]</code> và <code>[3, 6]</code> là <code>[3, 4]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">intervals = [[1,2],[3,4],[5,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cặp đoạn nào giao nhau. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == intervals.length &lt;= 100</code></li>
	<li><code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n \le 100$, việc liệt kê mọi cặp chỉ số và kiểm tra giao nhau là đủ đáp ứng giới hạn. Hai đoạn đóng không giao nhau khi và chỉ khi một đầu mút phải nhỏ hơn đầu mút trái của đoạn kia.
>
> Kiểm tra từng cặp cần xét cả hai phía của điều kiện đó. Ta có thể bắt đầu từ $\frac{n(n-1)}{2}$ rồi trừ đi số cặp không giao nhau: với mỗi đầu mút trái, đếm số đoạn đã kết thúc trước khi đoạn hiện tại bắt đầu.
>
> Sau khi sắp xếp riêng các đầu mút trái và phải, một con trỏ chỉ di chuyển sang phải có thể đếm các đoạn đã kết thúc trong một lần duyệt.

<!-- thinking:end -->

Hai đoạn đóng $[l_1, r_1]$ và $[l_2, r_2]$ không giao nhau khi và chỉ khi $r_1 < l_2$ hoặc $r_2 < l_1$.

Tổng số cặp là $\frac{n(n-1)}{2}$. Ta đếm số cặp không giao nhau rồi lấy tổng số cặp trừ đi số đó.

Sắp xếp tất cả đầu mút trái và tất cả đầu mút phải theo thứ tự tăng dần. Duyệt từng đầu mút trái $s$ từ trái sang phải, đồng thời duy trì một con trỏ $i$ biểu thị số đoạn có $\textit{ends}[i] < s$. Các đoạn đó không giao nhau với đoạn hiện tại, nên ta trừ số lượng này khỏi đáp án.

Mỗi cặp không giao nhau được đếm đúng một lần: đoạn có đầu mút phải nhỏ hơn được tính khi ta duyệt đầu mút trái của đoạn còn lại.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số đoạn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countIntersectingIntervals(self, intervals: list[list[int]]) -> int:
        n = len(intervals)
        starts = sorted(s for s, _ in intervals)
        ends = sorted(e for _, e in intervals)
        ans = n * (n - 1) // 2
        i = 0
        for start in starts:
            while i < n and ends[i] < start:
                i += 1
            ans -= i
        return ans
```

#### Java

```java
class Solution {
    public int countIntersectingIntervals(int[][] intervals) {
        int n = intervals.length;
        int[] starts = new int[n];
        int[] ends = new int[n];
        for (int i = 0; i < n; i++) {
            starts[i] = intervals[i][0];
            ends[i] = intervals[i][1];
        }
        Arrays.sort(starts);
        Arrays.sort(ends);
        int ans = n * (n - 1) / 2;
        int i = 0;
        for (int start : starts) {
            while (i < n && ends[i] < start) {
                i++;
            }
            ans -= i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countIntersectingIntervals(vector<vector<int>>& intervals) {
        int n = intervals.size();
        vector<int> starts(n), ends(n);
        for (int i = 0; i < n; i++) {
            starts[i] = intervals[i][0];
            ends[i] = intervals[i][1];
        }
        sort(starts.begin(), starts.end());
        sort(ends.begin(), ends.end());
        int ans = n * (n - 1) / 2;
        int i = 0;
        for (int start : starts) {
            while (i < n && ends[i] < start) {
                i++;
            }
            ans -= i;
        }
        return ans;
    }
};
```

#### Go

```go
func countIntersectingIntervals(intervals [][]int) int {
	n := len(intervals)
	starts := make([]int, n)
	ends := make([]int, n)
	for i, p := range intervals {
		starts[i] = p[0]
		ends[i] = p[1]
	}
	slices.Sort(starts)
	slices.Sort(ends)
	ans := n * (n - 1) / 2
	i := 0
	for _, start := range starts {
		for i < n && ends[i] < start {
			i++
		}
		ans -= i
	}
	return ans
}
```

#### TypeScript

```ts
function countIntersectingIntervals(intervals: number[][]): number {
    const n = intervals.length;
    const starts = intervals.map(([s]) => s).sort((a, b) => a - b);
    const ends = intervals.map(([, e]) => e).sort((a, b) => a - b);
    let ans = (n * (n - 1)) / 2;
    let i = 0;
    for (const start of starts) {
        while (i < n && ends[i] < start) {
            i++;
        }
        ans -= i;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
