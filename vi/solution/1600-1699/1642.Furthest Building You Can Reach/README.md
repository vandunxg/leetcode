---
comments: true
difficulty: Medium
rating: 1962
source: Weekly Contest 213 Q3
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1642. Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach)

[中文文档](/solution/1600-1699/1642.Furthest%20Building%20You%20Can%20Reach/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho mảng số nguyên <code>heights</code> biểu diễn chiều cao các tòa nhà, một số <code>bricks</code> và một số <code>ladders</code>.</p>

<p>Bạn bắt đầu từ tòa nhà <code>0</code> và di chuyển đến tòa nhà tiếp theo, có thể dùng gạch hoặc thang.</p>

<p>Khi di chuyển từ tòa nhà <code>i</code> đến tòa nhà <code>i+1</code> (<strong>đánh chỉ số từ 0</strong>),</p>

<ul>
	<li>Nếu chiều cao tòa nhà hiện tại <strong>lớn hơn hoặc bằng</strong> tòa nhà tiếp theo, bạn <strong>không</strong> cần dùng thang hay gạch.</li>
	<li>Nếu chiều cao tòa nhà hiện tại <b>nhỏ hơn</b> tòa nhà tiếp theo, bạn có thể dùng <strong>một thang</strong> hoặc <strong>gạch</strong> <code>(h[i+1] - h[i])</code>.</li>
</ul>

<p><em>Trả về chỉ số tòa nhà xa nhất (đánh chỉ số từ 0) có thể đến được nếu sử dụng thang và gạch một cách tối ưu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1642.Furthest%20Building%20You%20Can%20Reach/images/q4.gif" style="width: 562px; height: 561px;" />
<pre>
<strong>Input:</strong> heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
<strong>Output:</strong> 4
<strong>Explanation:</strong> Bắt đầu từ tòa nhà 0, ta có thể thực hiện các bước sau:
- Đến tòa nhà 1 mà không dùng thang hay gạch vì 4 &gt;= 2.
- Đến tòa nhà 2 bằng 5 viên gạch. Phải dùng gạch hoặc thang vì 2 &lt; 7.
- Đến tòa nhà 3 mà không dùng thang hay gạch vì 7 &gt;= 6.
- Đến tòa nhà 4 bằng chiếc thang duy nhất. Phải dùng gạch hoặc thang vì 6 &lt; 9.
Không thể đi xa hơn tòa nhà 4 vì không còn gạch hay thang.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
<strong>Output:</strong> 7
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> heights = [14,3,19,3], bricks = 17, ladders = 0
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= heights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= bricks &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= ladders &lt;= heights.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ các đoạn leo lên mới tốn gạch hoặc thang, và thang nên dành cho các đoạn leo lớn nhất. Dữ liệu có thể rất lớn nên quyết định phải được đưa ra online.
>
> Min-heap lưu các đoạn leo hiện đang được dùng thang. Khi số phần tử trong heap vượt số thang, dùng gạch trả cho đoạn leo nhỏ nhất. Nếu hết gạch thì dừng lại.
>
> Nếu gạch đủ dùng suốt hành trình, ta đến được tòa nhà cuối cùng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def furthestBuilding(self, heights: List[int], bricks: int, ladders: int) -> int:
        h = []
        for i, a in enumerate(heights[:-1]):
            b = heights[i + 1]
            d = b - a
            if d > 0:
                heappush(h, d)
                if len(h) > ladders:
                    bricks -= heappop(h)
                    if bricks < 0:
                        return i
        return len(heights) - 1
```

#### Java

```java
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        PriorityQueue<Integer> q = new PriorityQueue<>();
        int n = heights.length;
        for (int i = 0; i < n - 1; ++i) {
            int a = heights[i], b = heights[i + 1];
            int d = b - a;
            if (d > 0) {
                q.offer(d);
                if (q.size() > ladders) {
                    bricks -= q.poll();
                    if (bricks < 0) {
                        return i;
                    }
                }
            }
        }
        return n - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        priority_queue<int, vector<int>, greater<int>> q;
        int n = heights.size();
        for (int i = 0; i < n - 1; ++i) {
            int a = heights[i], b = heights[i + 1];
            int d = b - a;
            if (d > 0) {
                q.push(d);
                if (q.size() > ladders) {
                    bricks -= q.top();
                    q.pop();
                    if (bricks < 0) {
                        return i;
                    }
                }
            }
        }
        return n - 1;
    }
};
```

#### Go

```go
func furthestBuilding(heights []int, bricks int, ladders int) int {
	q := hp{}
	n := len(heights)
	for i, a := range heights[:n-1] {
		b := heights[i+1]
		d := b - a
		if d > 0 {
			heap.Push(&q, d)
			if q.Len() > ladders {
				bricks -= heap.Pop(&q).(int)
				if bricks < 0 {
					return i
				}
			}
		}
	}
	return n - 1
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
