---
comments: true
difficulty: Easy
tags:
    - Tree
    - Design
    - Binary Search Tree
    - Binary Tree
    - Data Stream
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream)

[中文文档](/solution/0700-0799/0703.Kth%20Largest%20Element%20in%20a%20Stream/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn làm việc tại văn phòng tuyển sinh của một trường đại học và cần theo dõi điểm thi cao thứ <code>kth</code> của các ứng viên theo thời gian thực. Thông tin này giúp xác định linh hoạt điểm chuẩn phỏng vấn và tuyển sinh khi có ứng viên mới nộp điểm.</p>

<p>Hãy triển khai một class nhận số nguyên <code>k</code>, duy trì stream điểm thi và liên tục trả về điểm cao thứ <code>k</code> <strong>sau khi</strong> có điểm mới được nộp. Cụ thể, ta cần tìm điểm cao thứ <code>k</code> trong danh sách đã sắp xếp gồm tất cả điểm.</p>

<p>Hãy triển khai class <code>KthLargest</code>:</p>

<ul>
	<li><code>KthLargest(int k, int[] nums)</code> Khởi tạo object với số nguyên <code>k</code> và stream điểm thi <code>nums</code>.</li>
	<li><code>int add(int val)</code> Thêm điểm thi mới <code>val</code> vào stream và trả về phần tử lớn thứ <code>k<sup>th</sup></code> trong tập điểm hiện có.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;KthLargest&quot;, &quot;add&quot;, &quot;add&quot;, &quot;add&quot;, &quot;add&quot;, &quot;add&quot;]<br />
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[null, 4, 5, 5, 8, 8]</span></p>

<p><strong>Giải thích:</strong></p>

<p>KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);<br />
kthLargest.add(3); // return 4<br />
kthLargest.add(5); // return 5<br />
kthLargest.add(10); // return 5<br />
kthLargest.add(9); // return 8<br />
kthLargest.add(4); // return 8</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;KthLargest&quot;, &quot;add&quot;, &quot;add&quot;, &quot;add&quot;, &quot;add&quot;]<br />
[[4, [7, 7, 7, 7, 8, 3]], [2], [10], [9], [9]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[null, 7, 7, 7, 8]</span></p>

<p><strong>Giải thích:</strong></p>
KthLargest kthLargest = new KthLargest(4, [7, 7, 7, 7, 8, 3]);<br />
kthLargest.add(2); // return 7<br />
kthLargest.add(10); // return 7<br />
kthLargest.add(9); // return 7<br />
kthLargest.add(9); // return 8</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length + 1</code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= val &lt;= 10<sup>4</sup></code></li>
	<li>Sẽ có tối đa <code>10<sup>4</sup></code> lần gọi <code>add</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Ta thêm điểm vào stream và phải trả về phần tử lớn thứ $k$ sau mỗi lần thêm. Sắp xếp hoặc quét toàn bộ lịch sử ở mỗi query sẽ quá tốn kém khi cả $n$ và số query đều có thể lên tới $10^4$.
>
> Chỉ cần quan tâm đến $k$ giá trị lớn nhất hiện tại; các giá trị nhỏ hơn không thể là đáp án. Trong min-heap kích thước $k$, phần tử trên cùng là giá trị nhỏ nhất trong nhóm này, tức phần tử lớn thứ $k$ trong toàn bộ dữ liệu.
>
> Push mỗi giá trị mới vào heap và pop nếu heap lớn hơn $k$. Khởi tạo từ $\textit{nums}$ cũng dùng cùng thao tác $\textit{add}$. Mỗi lần cập nhật mất $O(\log k)$.

<!-- thinking:end -->

Ta duy trì một priority queue (min heap) $\textit{minQ}$.

Ban đầu, ta lần lượt thêm các phần tử của mảng $\textit{nums}$ vào $\textit{minQ}$, đồng thời đảm bảo kích thước $\textit{minQ}$ không vượt quá $k$. Độ phức tạp thời gian là $O(n \times \log k)$.

Mỗi khi thêm phần tử mới, nếu kích thước $\textit{minQ}$ vượt quá $k$, ta pop phần tử trên cùng để giữ kích thước $\textit{minQ}$ bằng $k$. Độ phức tạp thời gian là $O(\log k)$.

Như vậy, các phần tử trong $\textit{minQ}$ là $k$ phần tử lớn nhất trong mảng $\textit{nums}$, và phần tử trên cùng của heap là phần tử lớn thứ $k$.

Độ phức tạp không gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.min_q = []
        for x in nums:
            self.add(x)

    def add(self, val: int) -> int:
        heappush(self.min_q, val)
        if len(self.min_q) > self.k:
            heappop(self.min_q)
        return self.min_q[0]


# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```

#### Java

```java
class KthLargest {
    private PriorityQueue<Integer> minQ;
    private int k;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        minQ = new PriorityQueue<>(k);
        for (int x : nums) {
            add(x);
        }
    }

    public int add(int val) {
        minQ.offer(val);
        if (minQ.size() > k) {
            minQ.poll();
        }
        return minQ.peek();
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.add(val);
 */
```

#### C++

```cpp
class KthLargest {
public:
    KthLargest(int k, vector<int>& nums) {
        this->k = k;
        for (int x : nums) {
            add(x);
        }
    }

    int add(int val) {
        minQ.push(val);
        if (minQ.size() > k) {
            minQ.pop();
        }
        return minQ.top();
    }

private:
    int k;
    priority_queue<int, vector<int>, greater<int>> minQ;
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

#### Go

```go
type KthLargest struct {
	k    int
	minQ hp
}

func Constructor(k int, nums []int) KthLargest {
	minQ := hp{}
	this := KthLargest{k, minQ}
	for _, x := range nums {
		this.Add(x)
	}
	return this
}

func (this *KthLargest) Add(val int) int {
	heap.Push(&this.minQ, val)
	if this.minQ.Len() > this.k {
		heap.Pop(&this.minQ)
	}
	return this.minQ.IntSlice[0]
}

type hp struct{ sort.IntSlice }

func (h *hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
func (h *hp) Pop() interface{} {
	old := h.IntSlice
	n := len(old)
	x := old[n-1]
	h.IntSlice = old[0 : n-1]
	return x
}
func (h *hp) Push(x interface{}) {
	h.IntSlice = append(h.IntSlice, x.(int))
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * obj := Constructor(k, nums);
 * param_1 := obj.Add(val);
 */
```

#### TypeScript

```ts
class KthLargest {
    #k: number = 0;
    #minQ = new MinPriorityQueue<number>();

    constructor(k: number, nums: number[]) {
        this.#k = k;
        for (const x of nums) {
            this.add(x);
        }
    }

    add(val: number): number {
        this.#minQ.enqueue(val);
        if (this.#minQ.size() > this.#k) {
            this.#minQ.dequeue();
        }
        return this.#minQ.front();
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * var obj = new KthLargest(k, nums)
 * var param_1 = obj.add(val)
 */
```

#### JavaScript

```js
/**
 * @param {number} k
 * @param {number[]} nums
 */
var KthLargest = function (k, nums) {
    this.k = k;
    this.minQ = new MinPriorityQueue();
    for (const x of nums) {
        this.add(x);
    }
};

/**
 * @param {number} val
 * @return {number}
 */
KthLargest.prototype.add = function (val) {
    this.minQ.enqueue(val);
    if (this.minQ.size() > this.k) {
        this.minQ.dequeue();
    }
    return this.minQ.front();
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * var obj = new KthLargest(k, nums)
 * var param_1 = obj.add(val)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
