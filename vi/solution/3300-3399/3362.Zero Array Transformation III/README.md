---
comments: true
difficulty: Medium
rating: 2423
source: Biweekly Contest 144 Q3
tags:
    - Greedy
    - Array
    - Two Pointers
    - Prefix Sum
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3362. Zero Array Transformation III](https://leetcode.com/problems/zero-array-transformation-iii)

[中文文档](/solution/3300-3399/3362.Zero%20Array%20Transformation%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng 2D <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.</p>

<p>Mỗi <code>queries[i]</code> biểu diễn thao tác sau trên <code>nums</code>:</p>

<ul>
	<li>Giảm giá trị tại mỗi chỉ số trong đoạn <code>[l<sub>i</sub>, r<sub>i</sub>]</code> của <code>nums</code> đi <strong>tối đa</strong><strong> </strong>1.</li>
	<li>Lượng giảm có thể được chọn <strong>độc lập</strong> cho từng chỉ số.</li>
</ul>

<p><strong>Mảng 0</strong> là một mảng mà tất cả phần tử đều bằng 0.</p>

<p>Hãy trả về <strong>số lượng phần tử lớn nhất</strong> có thể xóa khỏi <code>queries</code>, sao cho <code>nums</code> vẫn có thể được chuyển thành một <strong>mảng 0</strong> bằng các query <em>còn lại</em>. Nếu không thể chuyển <code>nums</code> thành một <strong>mảng 0</strong>, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,0,2], queries = [[0,2],[0,2],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi xóa <code>queries[2]</code>, <code>nums</code> vẫn có thể được chuyển thành một mảng 0.</p>

<ul>
	<li>Sử dụng <code>queries[0]</code>, giảm <code>nums[0]</code> và <code>nums[2]</code> đi 1, còn <code>nums[1]</code> đi 0.</li>
	<li>Sử dụng <code>queries[1]</code>, giảm <code>nums[0]</code> và <code>nums[2]</code> đi 1, còn <code>nums[1]</code> đi 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1], queries = [[1,3],[0,2],[1,3],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể xóa <code>queries[2]</code> và <code>queries[3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], queries = [[0,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> không thể được chuyển thành một mảng 0 ngay cả khi sử dụng tất cả query.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Mảng hiệu + Hàng đợi ưu tiên

<!-- thinking:start -->

> **Tư duy**
>
> Khác với hai bài đầu tiên, ta có thể chọn bất kỳ tập con query nào và muốn để lại càng nhiều query chưa dùng càng tốt. Chỉ số $i$ cần ít nhất $\textit{nums}[i]$ đoạn phủ lên nó.
>
> Sắp xếp theo đầu trái rồi duyệt qua $i$, đưa các đầu phải đã bắt đầu vào max-heap. Chỉ khi số đoạn phủ còn thiếu, ta mới chọn đoạn có đầu phải lớn nhất nhưng vẫn phủ lên $i$.
>
> Các đoạn kết thúc muộn sẽ có ích cho những chỉ số phía sau, nên ta giữ chúng lại cho đến khi cần. Mảng hiệu giảm số đoạn phủ tại $r+1$. Những gì còn lại trong heap là các query chưa dùng.

<!-- thinking:end -->

Ta muốn "xóa" càng nhiều query đoạn càng tốt, đồng thời đảm bảo rằng với mỗi vị trí $i$, số query được chọn phủ lên vị trí đó, $s(i)$, ít nhất bằng giá trị ban đầu $\textit{nums}[i]$, để giá trị tại vị trí đó có thể được giảm về 0 hoặc thấp hơn. Nếu không thể thỏa mãn $s(i) \ge \textit{nums}[i]$ tại một vị trí $i$, thì dù có chọn thêm bao nhiêu query cũng không thể đưa vị trí đó về 0, nên ta trả về $-1$.

Để thực hiện điều này, ta duyệt các query theo thứ tự của đầu trái và duy trì:

1. **Mảng hiệu** `d`: Dùng để ghi nhận phạm vi tác động của các query hiện đang được áp dụng. Khi "áp dụng" một query trên đoạn $[l, r]$, ta lập tức thực hiện `d[l] += 1` và `d[r+1] -= 1`. Nhờ đó, khi duyệt đến chỉ số $i$, tổng tiền tố cho biết có bao nhiêu query phủ lên $i$.
2. **Max heap** `pq`: Lưu các đầu phải của những query đoạn đang là "ứng viên" (lưu dưới dạng số âm để mô phỏng max heap bằng min heap của Python). Tại sao chọn đoạn kết thúc muộn nhất? Vì nó có thể phủ đến các vị trí xa hơn. Chiến lược tham lam của ta là: **tại mỗi $i$, chỉ chọn đoạn dài nhất trong heap khi cần tăng số đoạn phủ**, để có nhiều đoạn hơn cho các vị trí tiếp theo.

Các bước cụ thể như sau:

1. Sắp xếp `queries` theo đầu trái `l` tăng dần;
2. Khởi tạo mảng hiệu `d` có độ dài `n+1` (để xử lý việc giảm tại `r+1`), đồng thời đặt số đoạn phủ hiện tại `s=0`, con trỏ heap `j=0`;
3. Với $i=0$ đến $n-1$:
    - Trước tiên, cộng `d[i]` vào `s` để cập nhật số đoạn phủ hiện tại;
    - Đưa mọi query $[l, r]$ có đầu trái $\le i$ vào max heap `pq` (lưu `-r`), rồi tăng `j`;
    - Trong khi số đoạn phủ hiện tại `s` nhỏ hơn giá trị yêu cầu `nums[i]`, heap không rỗng và đoạn ở đỉnh heap vẫn phủ lên $i$ (tức là $-pq[0] \ge i$):
        1. Lấy phần tử ở đỉnh heap ra (tương đương với việc "áp dụng" query này);
        2. Tăng `s` lên 1 và thực hiện `d[r+1] -= 1` (để sau khi vượt qua $r$, số đoạn phủ tự động giảm);

    - Lặp lại các bước trên cho đến khi `s \ge nums[i]` hoặc không còn query nào có thể được chọn;
    - Nếu lúc này `s < nums[i]`, nghĩa là không thể đưa vị trí $i$ về 0, nên trả về $-1$.

4. Sau khi duyệt qua mọi vị trí, các đoạn còn lại trong heap là những đoạn **chưa bị lấy ra**, tức là các query thực sự được **giữ lại** (không dùng cho nhiệm vụ "đưa về 0"). Kích thước heap là đáp án.

Độ phức tạp thời gian là $O(n + m \times \log m)$, và độ phức tạp không gian là $O(n + m)$, trong đó $n$ là độ dài mảng và $m$ là số lượng query.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxRemoval(self, nums: List[int], queries: List[List[int]]) -> int:
        queries.sort()
        pq = []
        d = [0] * (len(nums) + 1)
        s = j = 0
        for i, x in enumerate(nums):
            s += d[i]
            while j < len(queries) and queries[j][0] <= i:
                heappush(pq, -queries[j][1])
                j += 1
            while s < x and pq and -pq[0] >= i:
                s += 1
                d[-heappop(pq) + 1] -= 1
            if s < x:
                return -1
        return len(pq)
```

#### Java

```java
class Solution {
    public int maxRemoval(int[] nums, int[][] queries) {
        Arrays.sort(queries, (a, b) -> Integer.compare(a[0], b[0]));
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        int n = nums.length;
        int[] d = new int[n + 1];
        int s = 0, j = 0;
        for (int i = 0; i < n; i++) {
            s += d[i];
            while (j < queries.length && queries[j][0] <= i) {
                pq.offer(queries[j][1]);
                j++;
            }
            while (s < nums[i] && !pq.isEmpty() && pq.peek() >= i) {
                s++;
                d[pq.poll() + 1]--;
            }
            if (s < nums[i]) {
                return -1;
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
    int maxRemoval(vector<int>& nums, vector<vector<int>>& queries) {
        sort(queries.begin(), queries.end());
        priority_queue<int> pq;
        int n = nums.size();
        vector<int> d(n + 1, 0);
        int s = 0, j = 0;
        for (int i = 0; i < n; ++i) {
            s += d[i];
            while (j < queries.size() && queries[j][0] <= i) {
                pq.push(queries[j][1]);
                ++j;
            }
            while (s < nums[i] && !pq.empty() && pq.top() >= i) {
                ++s;
                int end = pq.top();
                pq.pop();
                --d[end + 1];
            }
            if (s < nums[i]) {
                return -1;
            }
        }
        return pq.size();
    }
};
```

#### Go

```go
func maxRemoval(nums []int, queries [][]int) int {
	sort.Slice(queries, func(i, j int) bool {
		return queries[i][0] < queries[j][0]
	})

	var h hp
	heap.Init(&h)

	n := len(nums)
	d := make([]int, n+1)
	s, j := 0, 0

	for i := 0; i < n; i++ {
		s += d[i]
		for j < len(queries) && queries[j][0] <= i {
			heap.Push(&h, queries[j][1])
			j++
		}
		for s < nums[i] && h.Len() > 0 && h.IntSlice[0] >= i {
			s++
			end := heap.Pop(&h).(int)
			if end+1 < len(d) {
				d[end+1]--
			}
		}
		if s < nums[i] {
			return -1
		}
	}

	return h.Len()
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
```

#### TypeScript

```ts
function maxRemoval(nums: number[], queries: number[][]): number {
    queries.sort((a, b) => a[0] - b[0]);
    const pq = new MaxPriorityQueue<number>();
    const n = nums.length;
    const d: number[] = Array(n + 1).fill(0);
    let [s, j] = [0, 0];
    for (let i = 0; i < n; i++) {
        s += d[i];
        while (j < queries.length && queries[j][0] <= i) {
            pq.enqueue(queries[j][1]);
            j++;
        }
        while (s < nums[i] && !pq.isEmpty() && pq.front() >= i) {
            s++;
            d[pq.dequeue() + 1]--;
        }
        if (s < nums[i]) {
            return -1;
        }
    }
    return pq.size();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
