---
comments: true
difficulty: Hard
tags:
    - Queue
    - Array
    - Sliding Window
    - Monotonic Queue
    - Heap (Priority Queue)
    - Range Query
---

<!-- problem:start -->

# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum)

[中文文档](/solution/0200-0299/0239.Sliding%20Window%20Maximum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên&nbsp;<code>nums</code>, có một cửa sổ trượt kích thước <code>k</code> di chuyển từ ngoài cùng bên trái của mảng đến ngoài cùng bên phải. Bạn chỉ có thể nhìn thấy <code>k</code> số trong cửa sổ. Mỗi lần cửa sổ trượt sang phải một vị trí.</p>

<p>Trả về <em>giá trị lớn nhất của mỗi cửa sổ trượt</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,-1,-3,5,3,6,7], k = 3
<strong>Đầu ra:</strong> [3,3,5,5,6,7]
<strong>Giải thích:</strong>
Vị trí cửa sổ                 Giá trị lớn nhất
---------------               ----------------
[1  3  -1] -3  5  3  6  7       <strong>3</strong>
 1 [3  -1  -3] 5  3  6  7       <strong>3</strong>
 1  3 [-1  -3  5] 3  6  7      <strong> 5</strong>
 1  3  -1 [-3  5  3] 6  7       <strong>5</strong>
 1  3  -1  -3 [5  3  6] 7       <strong>6</strong>
 1  3  -1  -3  5 [3  6  7]      <strong>7</strong>
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1], k = 1
<strong>Đầu ra:</strong> [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Max-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt tuyến tính trong mỗi cửa sổ sẽ tốn $O(nk)$. Max-heap cho phép lấy giá trị lớn nhất hiện tại trong thời gian logarithmic; các chỉ số đã hết hạn chỉ bị loại khi chúng nằm ở đỉnh heap.
>
> Nạp $k-1$ giá trị đầu tiên, sau đó thêm từng chỉ số mới, loại các phần tử ở đỉnh đã hết hạn và ghi nhận phần tử ở đỉnh.

<!-- thinking:end -->

Ta có thể dùng priority queue (max-heap) để duy trì giá trị lớn nhất trong cửa sổ trượt.

Trước tiên, thêm $k-1$ phần tử đầu tiên vào priority queue. Sau đó, bắt đầu từ phần tử thứ $k$, thêm phần tử mới vào priority queue và kiểm tra xem phần tử ở đỉnh heap có nằm ngoài cửa sổ hay không. Nếu có, xóa phần tử ở đỉnh. Tiếp theo, thêm phần tử ở đỉnh heap vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log k)$, còn độ phức tạp không gian là $O(k)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        q = [(-v, i) for i, v in enumerate(nums[: k - 1])]
        heapify(q)
        ans = []
        for i in range(k - 1, len(nums)):
            heappush(q, (-nums[i], i))
            while q[0][1] <= i - k:
                heappop(q)
            ans.append(-q[0][0])
        return ans
```

#### Java

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        PriorityQueue<int[]> q
            = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);
        int n = nums.length;
        for (int i = 0; i < k - 1; ++i) {
            q.offer(new int[] {nums[i], i});
        }
        int[] ans = new int[n - k + 1];
        for (int i = k - 1, j = 0; i < n; ++i) {
            q.offer(new int[] {nums[i], i});
            while (q.peek()[1] <= i - k) {
                q.poll();
            }
            ans[j++] = q.peek()[0];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        priority_queue<pair<int, int>> q;
        int n = nums.size();
        for (int i = 0; i < k - 1; ++i) {
            q.push({nums[i], -i});
        }
        vector<int> ans;
        for (int i = k - 1; i < n; ++i) {
            q.push({nums[i], -i});
            while (-q.top().second <= i - k) {
                q.pop();
            }
            ans.emplace_back(q.top().first);
        }
        return ans;
    }
};
```

#### Go

```go
func maxSlidingWindow(nums []int, k int) (ans []int) {
	q := hp{}
	for i, v := range nums[:k-1] {
		heap.Push(&q, pair{v, i})
	}
	for i := k - 1; i < len(nums); i++ {
		heap.Push(&q, pair{nums[i], i})
		for q[0].i <= i-k {
			heap.Pop(&q)
		}
		ans = append(ans, q[0].v)
	}
	return
}

type pair struct{ v, i int }

type hp []pair

func (h hp) Len() int { return len(h) }
func (h hp) Less(i, j int) bool {
	a, b := h[i], h[j]
	return a.v > b.v || (a.v == b.v && i < j)
}
func (h hp) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)   { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Monotonic Queue

<!-- thinking:start -->

> **Tư duy**
>
> Heap có thể chứa các chỉ số đã hết hạn và vẫn phải trả thêm chi phí log. Chỉ một dãy giảm dần các ứng viên mới có thể trở thành giá trị lớn nhất của cửa sổ.
>
> Một monotonic queue gồm các chỉ số sẽ xóa phần tử ở đầu khi phần tử đó rời khỏi cửa sổ và xóa phần tử ở cuối khi nó $\le$ giá trị mới; phần tử ở đầu là giá trị lớn nhất, với thời gian tuyến tính.

<!-- thinking:end -->

Để tìm giá trị lớn nhất trong một cửa sổ trượt, một phương pháp phổ biến là sử dụng monotonic queue.

Ta có thể duy trì một queue $q$ giảm dần từ đầu đến cuối, lưu các chỉ số của các phần tử. Khi duyệt mảng $\textit{nums}$, với phần tử hiện tại $\textit{nums}[i]$, trước tiên ta kiểm tra xem phần tử ở đầu queue có nằm ngoài cửa sổ hay không. Nếu có, ta xóa phần tử ở đầu. Sau đó, ta so sánh phần tử hiện tại $\textit{nums}[i]$ với các phần tử ở cuối queue. Nếu các phần tử ở cuối nhỏ hơn hoặc bằng phần tử hiện tại, ta xóa chúng cho đến khi phần tử ở cuối lớn hơn phần tử hiện tại hoặc queue rỗng. Tiếp theo, ta thêm chỉ số của phần tử hiện tại vào queue. Lúc này, phần tử ở đầu queue là giá trị lớn nhất của cửa sổ trượt hiện tại. Lưu ý rằng ta thêm phần tử ở đầu queue vào mảng kết quả khi chỉ số $i$ lớn hơn hoặc bằng $k-1$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(k)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        q = deque()
        ans = []
        for i, x in enumerate(nums):
            if q and i - q[0] >= k:
                q.popleft()
            while q and nums[q[-1]] <= x:
                q.pop()
            q.append(i)
            if i >= k - 1:
                ans.append(nums[q[0]])
        return ans
```

#### Java

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (!q.isEmpty() && i - q.peekFirst() >= k) {
                q.pollFirst();
            }
            while (!q.isEmpty() && nums[q.peekLast()] <= nums[i]) {
                q.pollLast();
            }
            q.offerLast(i);
            if (i >= k - 1) {
                ans[i - k + 1] = nums[q.peekFirst()];
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
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> q;
        vector<int> ans;
        for (int i = 0; i < nums.size(); ++i) {
            if (q.size() && i - q.front() >= k) {
                q.pop_front();
            }
            while (q.size() && nums[q.back()] <= nums[i]) {
                q.pop_back();
            }
            q.push_back(i);
            if (i >= k - 1) {
                ans.push_back(nums[q.front()]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxSlidingWindow(nums []int, k int) (ans []int) {
	q := []int{}
	for i, x := range nums {
		if len(q) > 0 && i-q[0] >= k {
			q = q[1:]
		}
		for len(q) > 0 && nums[q[len(q)-1]] <= x {
			q = q[:len(q)-1]
		}
		q = append(q, i)
		if i >= k-1 {
			ans = append(ans, nums[q[0]])
		}
	}
	return
}
```

#### TypeScript

```ts
function maxSlidingWindow(nums: number[], k: number): number[] {
    const ans: number[] = [];
    const q = new Deque();
    for (let i = 0; i < nums.length; ++i) {
        if (!q.isEmpty() && i - q.front()! >= k) {
            q.popFront();
        }
        while (!q.isEmpty() && nums[q.back()!] <= nums[i]) {
            q.popBack();
        }
        q.pushBack(i);
        if (i >= k - 1) {
            ans.push(nums[q.front()!]);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn max_sliding_window(nums: Vec<i32>, k: i32) -> Vec<i32> {
        let k = k as usize;
        let mut ans = Vec::new();
        let mut q: VecDeque<usize> = VecDeque::new();

        for i in 0..nums.len() {
            if let Some(&front) = q.front() {
                if i >= front + k {
                    q.pop_front();
                }
            }
            while let Some(&back) = q.back() {
                if nums[back] <= nums[i] {
                    q.pop_back();
                } else {
                    break;
                }
            }
            q.push_back(i);
            if i >= k - 1 {
                ans.push(nums[*q.front().unwrap()]);
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function (nums, k) {
    const ans = [];
    const q = new Deque();
    for (let i = 0; i < nums.length; ++i) {
        if (!q.isEmpty() && i - q.front() >= k) {
            q.popFront();
        }
        while (!q.isEmpty() && nums[q.back()] <= nums[i]) {
            q.popBack();
        }
        q.pushBack(i);
        if (i >= k - 1) {
            ans.push(nums[q.front()]);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
