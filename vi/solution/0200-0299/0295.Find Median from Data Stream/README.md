---
comments: true
difficulty: Hard
tags:
    - Design
    - Two Pointers
    - Data Stream
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream)

[中文文档](/solution/0200-0299/0295.Find%20Median%20from%20Data%20Stream/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Median</strong> là giá trị ở giữa trong danh sách số nguyên đã sắp xếp. Nếu danh sách có số phần tử chẵn thì không có giá trị ở giữa, và median là trung bình cộng của hai giá trị ở giữa.</p>

<ul>
	<li>Ví dụ, với <code>arr = [2,3,4]</code>, median là <code>3</code>.</li>
	<li>Ví dụ, với <code>arr = [2,3]</code>, median là <code>(2 + 3) / 2 = 2.5</code>.</li>
</ul>

<p>Hãy triển khai class MedianFinder:</p>

<ul>
	<li><code>MedianFinder()</code> khởi tạo đối tượng <code>MedianFinder</code>.</li>
	<li><code>void addNum(int num)</code> thêm số nguyên <code>num</code> từ data stream vào cấu trúc dữ liệu.</li>
	<li><code>double findMedian()</code> trả về median của tất cả phần tử đã nhận được. Đáp án sai lệch tối đa <code>10<sup>-5</sup></code> so với kết quả chính xác vẫn được chấp nhận.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MedianFinder&quot;, &quot;addNum&quot;, &quot;addNum&quot;, &quot;findMedian&quot;, &quot;addNum&quot;, &quot;findMedian&quot;]
[[], [1], [2], [], [3], []]
<strong>Đầu ra</strong>
[null, null, null, 1.5, null, 2.0]

<strong>Giải thích</strong>
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // trả về 1.5 (tức (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // trả về 2.0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-10<sup>5</sup> &lt;= num &lt;= 10<sup>5</sup></code></li>
	<li>Cấu trúc dữ liệu sẽ có ít nhất một phần tử trước khi gọi <code>findMedian</code>.</li>
	<li>Sẽ có tối đa <code>5 * 10<sup>4</sup></code> lời gọi đến <code>addNum</code> và <code>findMedian</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Nếu tất cả số nguyên trong data stream nằm trong đoạn <code>[0, 100]</code>, bạn sẽ tối ưu lời giải như thế nào?</li>
	<li>Nếu <code>99%</code> số nguyên trong data stream nằm trong đoạn <code>[0, 100]</code>, bạn sẽ tối ưu lời giải như thế nào?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Min Heap và Max Heap (Priority Queue)

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp lại sau mỗi lần thêm quá chậm khi cần cập nhật median liên tục. Max-heap lưu nửa dưới, min-heap lưu nửa trên; hai phần tử ở đỉnh heap xác định median.
>
> Giá trị mới được thêm vào max-heap rồi chuyển sang min-heap, đảm bảo min-heap nhiều hơn max-heap không quá một phần tử. Khi tổng số phần tử chẵn, lấy trung bình cộng hai đỉnh; khi lẻ, lấy đỉnh của min-heap.

<!-- thinking:end -->

Ta dùng hai heap để lưu các phần tử: min-heap $\textit{minQ}$ và max-heap $\textit{maxQ}$. Min-heap $\textit{minQ}$ lưu nửa lớn hơn, còn max-heap $\textit{maxQ}$ lưu nửa nhỏ hơn.

Khi gọi method `addNum`, trước tiên ta thêm phần tử vào max-heap $\textit{maxQ}$, sau đó lấy phần tử ở đỉnh $\textit{maxQ}$ ra và thêm vào min-heap $\textit{minQ}$. Nếu lúc này chênh lệch kích thước giữa $\textit{minQ}$ và $\textit{maxQ}$ lớn hơn $1$, ta lấy phần tử ở đỉnh $\textit{minQ}$ ra và chuyển sang $\textit{maxQ}$. Độ phức tạp thời gian là $O(\log n)$.

Khi gọi method `findMedian`, nếu $\textit{minQ}$ và $\textit{maxQ}$ có cùng kích thước thì tổng số phần tử là chẵn, ta trả về trung bình cộng hai phần tử ở đỉnh; nếu không, ta trả về phần tử ở đỉnh của $\textit{minQ}$. Độ phức tạp thời gian là $O(1)$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là số phần tử.

<!-- tabs:start -->

#### Python3

```python
class MedianFinder:

    def __init__(self):
        self.minq = []
        self.maxq = []

    def addNum(self, num: int) -> None:
        heappush(self.minq, -heappushpop(self.maxq, -num))
        if len(self.minq) - len(self.maxq) > 1:
            heappush(self.maxq, -heappop(self.minq))

    def findMedian(self) -> float:
        if len(self.minq) == len(self.maxq):
            return (self.minq[0] - self.maxq[0]) / 2
        return self.minq[0]


# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```

#### Java

```java
class MedianFinder {
    private PriorityQueue<Integer> minQ = new PriorityQueue<>();
    private PriorityQueue<Integer> maxQ = new PriorityQueue<>(Collections.reverseOrder());

    public MedianFinder() {
    }

    public void addNum(int num) {
        maxQ.offer(num);
        minQ.offer(maxQ.poll());
        if (minQ.size() - maxQ.size() > 1) {
            maxQ.offer(minQ.poll());
        }
    }

    public double findMedian() {
        return minQ.size() == maxQ.size() ? (minQ.peek() + maxQ.peek()) / 2.0 : minQ.peek();
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

#### C++

```cpp
class MedianFinder {
public:
    MedianFinder() {
    }

    void addNum(int num) {
        maxQ.push(num);
        minQ.push(maxQ.top());
        maxQ.pop();

        if (minQ.size() > maxQ.size() + 1) {
            maxQ.push(minQ.top());
            minQ.pop();
        }
    }

    double findMedian() {
        return minQ.size() == maxQ.size() ? (minQ.top() + maxQ.top()) / 2.0 : minQ.top();
    }

private:
    priority_queue<int> maxQ;
    priority_queue<int, vector<int>, greater<int>> minQ;
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

#### Go

```go
type MedianFinder struct {
	minq hp
	maxq hp
}

func Constructor() MedianFinder {
	return MedianFinder{hp{}, hp{}}
}

func (this *MedianFinder) AddNum(num int) {
	minq, maxq := &this.minq, &this.maxq
	heap.Push(maxq, -num)
	heap.Push(minq, -heap.Pop(maxq).(int))
	if minq.Len()-maxq.Len() > 1 {
		heap.Push(maxq, -heap.Pop(minq).(int))
	}
}

func (this *MedianFinder) FindMedian() float64 {
	minq, maxq := this.minq, this.maxq
	if minq.Len() == maxq.Len() {
		return float64(minq.IntSlice[0]-maxq.IntSlice[0]) / 2
	}
	return float64(minq.IntSlice[0])
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddNum(num);
 * param_2 := obj.FindMedian();
 */
```

#### TypeScript

```ts
class MedianFinder {
    #minQ = new MinPriorityQueue<number>();
    #maxQ = new MaxPriorityQueue<number>();

    addNum(num: number): void {
        const [minQ, maxQ] = [this.#minQ, this.#maxQ];
        maxQ.enqueue(num);
        minQ.enqueue(maxQ.dequeue());
        if (minQ.size() - maxQ.size() > 1) {
            maxQ.enqueue(minQ.dequeue());
        }
    }

    findMedian(): number {
        const [minQ, maxQ] = [this.#minQ, this.#maxQ];
        if (minQ.size() === maxQ.size()) {
            return (minQ.front() + maxQ.front()) / 2;
        }
        return minQ.front();
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * var obj = new MedianFinder()
 * obj.addNum(num)
 * var param_2 = obj.findMedian()
 */
```

#### Rust

```rust
use std::cmp::Reverse;
use std::collections::BinaryHeap;

struct MedianFinder {
    minQ: BinaryHeap<Reverse<i32>>,
    maxQ: BinaryHeap<i32>,
}

impl MedianFinder {
    fn new() -> Self {
        MedianFinder {
            minQ: BinaryHeap::new(),
            maxQ: BinaryHeap::new(),
        }
    }

    fn add_num(&mut self, num: i32) {
        self.maxQ.push(num);
        self.minQ.push(Reverse(self.maxQ.pop().unwrap()));

        if self.minQ.len() > self.maxQ.len() + 1 {
            self.maxQ.push(self.minQ.pop().unwrap().0);
        }
    }

    fn find_median(&self) -> f64 {
        if self.minQ.len() == self.maxQ.len() {
            let min_top = self.minQ.peek().unwrap().0;
            let max_top = *self.maxQ.peek().unwrap();
            (min_top + max_top) as f64 / 2.0
        } else {
            self.minQ.peek().unwrap().0 as f64
        }
    }
}
```

#### JavaScript

```js
var MedianFinder = function () {
    this.minQ = new MinPriorityQueue();
    this.maxQ = new MaxPriorityQueue();
};

/**
 * @param {number} num
 * @return {void}
 */
MedianFinder.prototype.addNum = function (num) {
    this.maxQ.enqueue(num);
    this.minQ.enqueue(this.maxQ.dequeue());
    if (this.minQ.size() - this.maxQ.size() > 1) {
        this.maxQ.enqueue(this.minQ.dequeue());
    }
};

/**
 * @return {number}
 */
MedianFinder.prototype.findMedian = function () {
    if (this.minQ.size() === this.maxQ.size()) {
        return (this.minQ.front() + this.maxQ.front()) / 2;
    }
    return this.minQ.front();
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * var obj = new MedianFinder()
 * obj.addNum(num)
 * var param_2 = obj.findMedian()
 */
```

#### C#

```cs
public class MedianFinder {
    private PriorityQueue<int, int> minQ = new PriorityQueue<int, int>();
    private PriorityQueue<int, int> maxQ = new PriorityQueue<int, int>(Comparer<int>.Create((a, b) => b.CompareTo(a)));

    public MedianFinder() {

    }

    public void AddNum(int num) {
        maxQ.Enqueue(num, num);
        minQ.Enqueue(maxQ.Peek(), maxQ.Dequeue());
        if (minQ.Count > maxQ.Count + 1) {
            maxQ.Enqueue(minQ.Peek(), minQ.Dequeue());
        }
    }

    public double FindMedian() {
        return minQ.Count == maxQ.Count ? (minQ.Peek() + maxQ.Peek()) / 2.0 : minQ.Peek();
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.AddNum(num);
 * double param_2 = obj.FindMedian();
 */
```

#### Swift

```swift
class MedianFinder {
    private var minQ = Heap<Int>(sort: <)
    private var maxQ = Heap<Int>(sort: >)

    init() {
    }

    func addNum(_ num: Int) {
        maxQ.insert(num)
        minQ.insert(maxQ.remove()!)
        if maxQ.count < minQ.count {
            maxQ.insert(minQ.remove()!)
        }
    }

    func findMedian() -> Double {
        if maxQ.count > minQ.count {
            return Double(maxQ.peek()!)
        }
        return (Double(maxQ.peek()!) + Double(minQ.peek()!)) / 2.0
    }
}

struct Heap<T> {
    var elements: [T]
    let sort: (T, T) -> Bool

    init(sort: @escaping (T, T) -> Bool, elements: [T] = []) {
        self.sort = sort
        self.elements = elements
        if !elements.isEmpty {
            for i in stride(from: elements.count / 2 - 1, through: 0, by: -1) {
                siftDown(from: i)
            }
        }
    }

    var isEmpty: Bool {
        return elements.isEmpty
    }

    var count: Int {
        return elements.count
    }

    func peek() -> T? {
        return elements.first
    }

    mutating func insert(_ value: T) {
        elements.append(value)
        siftUp(from: elements.count - 1)
    }

    mutating func remove() -> T? {
        guard !elements.isEmpty else { return nil }
        elements.swapAt(0, elements.count - 1)
        let removedValue = elements.removeLast()
        siftDown(from: 0)
        return removedValue
    }

    private mutating func siftUp(from index: Int) {
        var child = index
        var parent = parentIndex(ofChildAt: child)
        while child > 0 && sort(elements[child], elements[parent]) {
            elements.swapAt(child, parent)
            child = parent
            parent = parentIndex(ofChildAt: child)
        }
    }

    private mutating func siftDown(from index: Int) {
        var parent = index
        while true {
            let left = leftChildIndex(ofParentAt: parent)
            let right = rightChildIndex(ofParentAt: parent)
            var candidate = parent
            if left < count && sort(elements[left], elements[candidate]) {
                candidate = left
            }
            if right < count && sort(elements[right], elements[candidate]) {
                candidate = right
            }
            if candidate == parent {
                return
            }
            elements.swapAt(parent, candidate)
            parent = candidate
        }
    }

    private func parentIndex(ofChildAt index: Int) -> Int {
        return (index - 1) / 2
    }

    private func leftChildIndex(ofParentAt index: Int) -> Int {
        return 2 * index + 1
    }

    private func rightChildIndex(ofParentAt index: Int) -> Int {
        return 2 * index + 2
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * let obj = MedianFinder()
 * obj.addNum(num)
 * let ret_2: Double = obj.findMedian()
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
