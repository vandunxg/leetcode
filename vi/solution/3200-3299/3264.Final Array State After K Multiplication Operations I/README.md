---
comments: true
difficulty: Easy
rating: 1177
source: Weekly Contest 412 Q1
tags:
    - Array
    - Math
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3264. Final Array State After K Multiplication Operations I](https://leetcode.com/problems/final-array-state-after-k-multiplication-operations-i)

[中文文档](/solution/3200-3299/3264.Final%20Array%20State%20After%20K%20Multiplication%20Operations%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, một số nguyên <code>k</code> và một số nguyên <code>multiplier</code>.</p>

<p>Bạn cần thực hiện <code>k</code> phép toán trên <code>nums</code>. Trong mỗi phép toán:</p>

<ul>
	<li>Tìm giá trị <strong>nhỏ nhất</strong> <code>x</code> trong <code>nums</code>. Nếu giá trị nhỏ nhất xuất hiện nhiều lần, chọn phần tử xuất hiện <strong>đầu tiên</strong>.</li>
	<li>Thay giá trị nhỏ nhất được chọn <code>x</code> bằng <code>x * multiplier</code>.</li>
</ul>

<p>Trả về một mảng số nguyên biểu diễn <em>trạng thái cuối cùng</em> của <code>nums</code> sau khi thực hiện tất cả <code>k</code> phép toán.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3,5,6], k = 5, multiplier = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[8,4,6,5,6]</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Phép toán</th>
			<th>Kết quả</th>
		</tr>
		<tr>
			<td>Sau phép toán 1</td>
			<td>[2, 2, 3, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau phép toán 2</td>
			<td>[4, 2, 3, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau phép toán 3</td>
			<td>[4, 4, 3, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau phép toán 4</td>
			<td>[4, 4, 6, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau phép toán 5</td>
			<td>[8, 4, 6, 5, 6]</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2], k = 3, multiplier = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[16,8]</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Phép toán</th>
			<th>Kết quả</th>
		</tr>
		<tr>
			<td>Sau phép toán 1</td>
			<td>[4, 2]</td>
		</tr>
		<tr>
			<td>Sau phép toán 2</td>
			<td>[4, 8]</td>
		</tr>
		<tr>
			<td>Sau phép toán 3</td>
			<td>[16, 8]</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 10</code></li>
	<li><code>1 &lt;= multiplier &lt;= 5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min-Heap) + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước nhân giá trị nhỏ nhất hiện tại với $\textit{multiplier}$, nếu bằng nhau thì ưu tiên chỉ số nhỏ hơn. $n$ và $k$ đều rất nhỏ, nên chỉ cần mô phỏng $k$ lần. Duyệt tuyến tính để tìm giá trị nhỏ nhất cũng đủ, nhưng heap phù hợp với quy tắc này.
>
> Lưu $(value,index)$, lấy phần tử nhỏ nhất ra, nhân ngay trong mảng rồi đưa trở lại heap. Sau $k$ vòng lặp, ta thu được mảng kết quả.

<!-- thinking:end -->

Ta có thể sử dụng một min-heap để duy trì các phần tử trong mảng $\textit{nums}$. Ở mỗi bước, ta lấy giá trị nhỏ nhất ra khỏi min-heap, nhân nó với $\textit{multiplier}$, rồi đưa trở lại min-heap. Khi triển khai, ta đưa chỉ số của các phần tử vào min-heap và định nghĩa một hàm so sánh tùy chỉnh để sắp xếp min-heap, trong đó giá trị của các phần tử trong $\textit{nums}$ là khóa chính và chỉ số là khóa phụ.

Cuối cùng, ta trả về mảng $\textit{nums}$.

Độ phức tạp thời gian là $O((n + k) \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getFinalState(self, nums: List[int], k: int, multiplier: int) -> List[int]:
        pq = [(x, i) for i, x in enumerate(nums)]
        heapify(pq)
        for _ in range(k):
            _, i = heappop(pq)
            nums[i] *= multiplier
            heappush(pq, (nums[i], i))
        return nums
```

#### Java

```java
class Solution {
    public int[] getFinalState(int[] nums, int k, int multiplier) {
        PriorityQueue<Integer> pq
            = new PriorityQueue<>((i, j) -> nums[i] - nums[j] == 0 ? i - j : nums[i] - nums[j]);
        for (int i = 0; i < nums.length; i++) {
            pq.offer(i);
        }
        while (k-- > 0) {
            int i = pq.poll();
            nums[i] *= multiplier;
            pq.offer(i);
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getFinalState(vector<int>& nums, int k, int multiplier) {
        auto cmp = [&nums](int i, int j) {
            return nums[i] == nums[j] ? i > j : nums[i] > nums[j];
        };
        priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);

        for (int i = 0; i < nums.size(); ++i) {
            pq.push(i);
        }

        while (k--) {
            int i = pq.top();
            pq.pop();
            nums[i] *= multiplier;
            pq.push(i);
        }

        return nums;
    }
};
```

#### Go

```go
func getFinalState(nums []int, k int, multiplier int) []int {
	h := &hp{nums: nums}
	for i := range nums {
		heap.Push(h, i)
	}

	for k > 0 {
		i := heap.Pop(h).(int)
		nums[i] *= multiplier
		heap.Push(h, i)
		k--
	}

	return nums
}

type hp struct {
	sort.IntSlice
	nums []int
}

func (h *hp) Less(i, j int) bool {
	if h.nums[h.IntSlice[i]] == h.nums[h.IntSlice[j]] {
		return h.IntSlice[i] < h.IntSlice[j]
	}
	return h.nums[h.IntSlice[i]] < h.nums[h.IntSlice[j]]
}

func (h *hp) Pop() any {
	old := h.IntSlice
	n := len(old)
	x := old[n-1]
	h.IntSlice = old[:n-1]
	return x
}

func (h *hp) Push(x any) {
	h.IntSlice = append(h.IntSlice, x.(int))
}
```

#### TypeScript

```ts
function getFinalState(nums: number[], k: number, multiplier: number): number[] {
    const pq = new PriorityQueue<number>((i, j) =>
        nums[i] === nums[j] ? i - j : nums[i] - nums[j],
    );

    for (let i = 0; i < nums.length; ++i) {
        pq.enqueue(i);
    }
    while (k--) {
        const i = pq.dequeue()!;
        nums[i] *= multiplier;
        pq.enqueue(i);
    }
    return nums;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
