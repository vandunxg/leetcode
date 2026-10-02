---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [630. Course Schedule III](https://leetcode.com/problems/course-schedule-iii)

[中文文档](/solution/0600-0699/0630.Course%20Schedule%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> khóa học online khác nhau, được đánh số từ <code>1</code> đến <code>n</code>. Bạn được cho một mảng <code>courses</code>, trong đó <code>courses[i] = [duration<sub>i</sub>, lastDay<sub>i</sub>]</code> cho biết khóa học thứ <code>i<sup>th</sup></code> cần được học <b>liên tục</b> trong <code>duration<sub>i</sub></code> ngày và phải hoàn thành vào hoặc trước <code>lastDay<sub>i</sub></code>.</p>

<p>Bạn bắt đầu học vào ngày thứ <code>1<sup>st</sup></code> và không thể học đồng thời từ hai khóa trở lên.</p>

<p>Hãy trả về <em>số khóa học tối đa mà bạn có thể tham gia</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]
<strong>Đầu ra:</strong> 3
Giải thích: 
Có tổng cộng 4 khóa học, nhưng bạn chỉ có thể tham gia tối đa 3 khóa:
Đầu tiên, tham gia khóa học thứ 1<sup>st</sup>. Khóa học kéo dài 100 ngày nên bạn sẽ hoàn thành vào ngày thứ 100<sup>th</sup> và có thể bắt đầu khóa tiếp theo vào ngày thứ 101<sup>st</sup>.
Tiếp theo, tham gia khóa học thứ 3<sup>rd</sup>. Khóa học kéo dài 1000 ngày nên bạn sẽ hoàn thành vào ngày thứ 1100<sup>th</sup> và có thể bắt đầu khóa tiếp theo vào ngày thứ 1101<sup>st</sup>. 
Sau đó, tham gia khóa học thứ 2<sup>nd</sup>. Khóa học kéo dài 200 ngày nên bạn sẽ hoàn thành vào ngày thứ 1300<sup>th</sup>. 
Không thể tham gia khóa học thứ 4<sup>th</sup> lúc này vì bạn sẽ hoàn thành vào ngày thứ 3300<sup>th</sup>, vượt quá hạn chót.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> courses = [[1,2]]
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> courses = [[3,2],[4,3]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= courses.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= duration<sub>i</sub>, lastDay<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue (Max-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi khóa học có thời lượng và deadline; với $n\le 10^4$, không thể thử mọi tập hợp khóa học.
>
> Sắp xếp theo deadline rồi lần lượt chọn từng khóa học. Nếu tổng thời gian vượt quá deadline, bỏ khóa học dài nhất đã chọn (dùng max-heap) để dành chỗ cho các khóa sau có deadline sớm hơn. Kích thước heap chính là đáp án.

<!-- thinking:end -->

Ta sắp xếp các khóa học theo thời điểm kết thúc tăng dần, rồi lần lượt chọn khóa có deadline sớm nhất.

Nếu tổng thời gian $s$ của các khóa đã chọn vượt quá thời điểm kết thúc $last$ của khóa hiện tại, ta loại khóa có thời lượng dài nhất trong số các khóa đã chọn trước đó cho đến khi thỏa mãn deadline của khóa hiện tại. Ta dùng priority queue (max-heap) $pq$ để lưu thời lượng các khóa đang được chọn, và mỗi lần lấy ra rồi loại khóa có thời lượng dài nhất khỏi priority queue.

Cuối cùng, số phần tử trong priority queue là số khóa học tối đa ta có thể tham gia.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số khóa học.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        courses.sort(key=lambda x: x[1])
        pq = []
        s = 0
        for duration, last in courses:
            heappush(pq, -duration)
            s += duration
            while s > last:
                s += heappop(pq)
        return len(pq)
```

#### Java

```java
class Solution {
    public int scheduleCourse(int[][] courses) {
        Arrays.sort(courses, (a, b) -> a[1] - b[1]);
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        int s = 0;
        for (var e : courses) {
            int duration = e[0], last = e[1];
            pq.offer(duration);
            s += duration;
            while (s > last) {
                s -= pq.poll();
            }
        }
        return pq.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        sort(courses.begin(), courses.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[1] < b[1];
        });
        priority_queue<int> pq;
        int s = 0;
        for (auto& e : courses) {
            int duration = e[0], last = e[1];
            pq.push(duration);
            s += duration;
            while (s > last) {
                s -= pq.top();
                pq.pop();
            }
        }
        return pq.size();
    }
};
```

#### Go

```go
func scheduleCourse(courses [][]int) int {
	sort.Slice(courses, func(i, j int) bool { return courses[i][1] < courses[j][1] })
	pq := &hp{}
	s := 0
	for _, e := range courses {
		duration, last := e[0], e[1]
		s += duration
		pq.push(duration)
		for s > last {
			s -= pq.pop()
		}
	}
	return pq.Len()
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
func (h *hp) push(v int) { heap.Push(h, v) }
func (h *hp) pop() int   { return heap.Pop(h).(int) }
```

#### TypeScript

```ts
function scheduleCourse(courses: number[][]): number {
    courses.sort((a, b) => a[1] - b[1]);
    const pq = new MaxPriorityQueue<number>();
    let s = 0;
    for (const [duration, last] of courses) {
        pq.enqueue(duration);
        s += duration;
        while (s > last) {
            s -= pq.dequeue();
        }
    }
    return pq.size();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
