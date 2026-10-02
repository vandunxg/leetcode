---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [757. Set Intersection Size At Least Two](https://leetcode.com/problems/set-intersection-size-at-least-two)

[中文文档](/solution/0700-0799/0757.Set%20Intersection%20Size%20At%20Least%20Two/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên 2 chiều <code>intervals</code>, trong đó <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> biểu diễn tất cả số nguyên từ <code>start<sub>i</sub></code> đến <code>end<sub>i</sub></code>, bao gồm cả hai đầu mút.</p>

<p><strong>Tập bao phủ</strong> là một mảng <code>nums</code> sao cho mỗi interval trong <code>intervals</code> chứa <strong>ít nhất hai</strong> số nguyên thuộc <code>nums</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>intervals = [[1,3], [3,7], [8,9]]</code>, thì <code>[1,2,4,7,8,9]</code> và <code>[2,3,4,8,9]</code> đều là <strong>tập bao phủ</strong>.</li>
</ul>

<p>Hãy trả về <em>kích thước nhỏ nhất có thể của một tập bao phủ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,3],[3,7],[8,9]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Chọn nums = [2, 3, 4, 8, 9].
Có thể chứng minh rằng không tồn tại mảng bao phủ nào có kích thước 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,3],[1,4],[2,5],[3,5]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chọn nums = [2, 3, 4].
Có thể chứng minh rằng không tồn tại mảng bao phủ nào có kích thước 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,2],[2,3],[2,4],[4,5]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Chọn nums = [1, 2, 3, 4, 5].
Có thể chứng minh rằng không tồn tại mảng bao phủ nào có kích thước 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= intervals.length &lt;= 3000</code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Chọn ít điểm nhất có thể sao cho mỗi interval chứa ít nhất hai điểm. Với $3000$ interval, không thể xét các tập con. Nên đặt các điểm càng về bên phải càng tốt để chúng có thể được dùng lại.
>
> Sắp xếp theo điểm kết thúc tăng dần, sau đó theo điểm bắt đầu giảm dần để xét các interval hẹp hơn trước. Theo dõi hai điểm được chọn gần nhất là điểm áp cuối $s$ và điểm cuối cùng $e$, với $s<e$.
>
> Bỏ qua nếu cả hai điểm đều nằm trong interval; nếu chỉ có $e$, thêm $b$; nếu không có điểm nào, thêm $b-1$ và $b$. Mỗi bước đều thêm số điểm ít nhất có thể và ưu tiên các điểm nằm bên phải.

<!-- thinking:end -->

Ta cần chọn ít điểm nguyên nhất có thể trên trục số sao cho mỗi interval chứa ít nhất hai điểm. Một chiến lược hiệu quả là sắp xếp các interval theo điểm kết thúc, rồi ưu tiên đặt các điểm được chọn về phía bên phải để chúng có thể bao phủ thêm nhiều interval tiếp theo.

Trước tiên, sắp xếp tất cả interval theo các quy tắc sau:

1. Sắp xếp theo điểm kết thúc tăng dần;
2. Nếu điểm kết thúc bằng nhau, sắp xếp theo điểm bắt đầu giảm dần.

Lý do là các interval có điểm kết thúc nhỏ hơn có ít khoảng lựa chọn hơn nên cần được xử lý trước; khi điểm kết thúc bằng nhau, interval có điểm bắt đầu lớn hơn thì hẹp hơn và nên được ưu tiên.

Tiếp theo, dùng hai biến $s$ và $e$ để lưu **điểm được chọn áp cuối** và **điểm được chọn cuối cùng** trong các interval đã xử lý. Ban đầu, $s = e = -1$, nghĩa là chưa chọn điểm nào.

Sau đó, lần lượt xử lý các interval đã sắp xếp $[a, b]$ và xét ba trường hợp dựa trên quan hệ của chúng với $\{s, e\}$:

1. **Nếu $a \leq s$**:
   Interval hiện tại đã chứa cả hai điểm $s$ và $e$, nên không cần thêm điểm nào.

2. **Nếu $s < a \leq e$**:
   Interval hiện tại chỉ chứa một điểm (tức là $e$), nên cần thêm một điểm. Để điểm mới hữu ích nhất cho các interval tiếp theo, ta chọn điểm ngoài cùng bên phải $b$ trong interval. Cập nhật $\textit{ans} = \textit{ans} + 1$ và đặt hai điểm mới là $\{e, b\}$.

3. **Nếu $a > e$**:
   Interval hiện tại không chứa điểm nào trong hai điểm đã chọn, nên cần thêm hai điểm. Lựa chọn tối ưu là đặt $\{b - 1, b\}$ ở phía ngoài cùng bên phải của interval. Cập nhật $\textit{ans} = \textit{ans} + 2$ và đặt hai điểm mới là $\{b - 1, b\}$.

Cuối cùng, trả về tổng số điểm đã chọn, $\textit{ans}$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số lượng interval.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def intersectionSizeTwo(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: (x[1], -x[0]))
        s = e = -1
        ans = 0
        for a, b in intervals:
            if a <= s:
                continue
            if a > e:
                ans += 2
                s, e = b - 1, b
            else:
                ans += 1
                s, e = e, b
        return ans
```

#### Java

```java
class Solution {
    public int intersectionSizeTwo(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[1] == b[1] ? b[0] - a[0] : a[1] - b[1]);
        int ans = 0;
        int s = -1, e = -1;
        for (int[] v : intervals) {
            int a = v[0], b = v[1];
            if (a <= s) {
                continue;
            }
            if (a > e) {
                ans += 2;
                s = b - 1;
                e = b;
            } else {
                ans += 1;
                s = e;
                e = b;
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
    int intersectionSizeTwo(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [&](vector<int>& a, vector<int>& b) {
            return a[1] == b[1] ? a[0] > b[0] : a[1] < b[1];
        });
        int ans = 0;
        int s = -1, e = -1;
        for (auto& v : intervals) {
            int a = v[0], b = v[1];
            if (a <= s) continue;
            if (a > e) {
                ans += 2;
                s = b - 1;
                e = b;
            } else {
                ans += 1;
                s = e;
                e = b;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func intersectionSizeTwo(intervals [][]int) int {
	sort.Slice(intervals, func(i, j int) bool {
		a, b := intervals[i], intervals[j]
		if a[1] == b[1] {
			return a[0] > b[0]
		}
		return a[1] < b[1]
	})
	ans := 0
	s, e := -1, -1
	for _, v := range intervals {
		a, b := v[0], v[1]
		if a <= s {
			continue
		}
		if a > e {
			ans += 2
			s, e = b-1, b
		} else {
			ans += 1
			s, e = e, b
		}
	}
	return ans
}
```

#### TypeScript

```ts
function intersectionSizeTwo(intervals: number[][]): number {
    intervals.sort((a, b) => (a[1] !== b[1] ? a[1] - b[1] : b[0] - a[0]));
    let s = -1;
    let e = -1;
    let ans = 0;
    for (const [a, b] of intervals) {
        if (a <= s) {
            continue;
        }
        if (a > e) {
            ans += 2;
            s = b - 1;
            e = b;
        } else {
            ans += 1;
            s = e;
            e = b;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
