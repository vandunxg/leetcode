---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [17.20. Continuous Median](https://leetcode.cn/problems/continuous-median-lcci)

[中文文档](/lcci/17.20.Continuous%20Median/README.md)

## Mô tả

<!-- description:start -->

<p>Các số được tạo ngẫu nhiên và truyền vào một method. Hãy viết một chương trình để tìm và duy trì giá trị median khi các giá trị mới được tạo ra.</p>

<p>Median là giá trị ở giữa trong một danh sách số nguyên đã được sắp xếp. Nếu kích thước danh sách là số chẵn thì không có giá trị ở giữa. Khi đó, median là trung bình cộng của hai giá trị ở giữa.</p>

<p>Ví dụ:</p>

<p>[2,3,4], median là&nbsp;3</p>

<p>[2,3], median là (2 + 3) / 2 = 2.5</p>

<p>Hãy thiết kế một data structure hỗ trợ hai thao tác sau:</p>

<ul>
	<li>void addNum(int num) - Thêm một số nguyên từ data stream vào data structure.</li>
	<li>double findMedian() - Trả về median của tất cả phần tử tính đến thời điểm hiện tại.</li>
</ul>

<p><strong>Ví dụ: </strong></p>

<pre>

addNum(1)

addNum(2)

findMedian() -&gt; 1.5

addNum(3)

findMedian() -&gt; 2

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Min Heap và Max Heap (Priority Queue)

<!-- thinking:start -->

> **Tư duy**
>
> Với một data stream, nếu mỗi lần đều sắp xếp thì độ phức tạp là $O(n\log n)$; dùng một balanced BST thì khá nặng.
>
> Median là max của nửa dưới và min của nửa trên — dùng hai heap.
>
> Một max-heap $maxq$ (đổi dấu) lưu nửa dưới; một min-heap $minq$ lưu nửa trên. Chèn vào max-heap, lấy phần tử đầu ra rồi đưa vào min-heap, và cân bằng lại khi kích thước chênh nhau quá một. Với số lượng phần tử chẵn, lấy trung bình của hai phần tử đầu.

<!-- thinking:end -->

Ta có thể dùng hai heap để duy trì tất cả phần tử: một min heap $\textit{minQ}$ và một max heap $\textit{maxQ}$, trong đó min heap $\textit{minQ}$ lưu nửa lớn hơn, còn max heap $\textit{maxQ}$ lưu nửa nhỏ hơn.

Khi gọi method `addNum`, trước tiên ta thêm phần tử vào max heap $\textit{maxQ}$, sau đó lấy phần tử đầu của $\textit{maxQ}$ ra và thêm vào min heap $\textit{minQ}$. Nếu lúc này độ chênh lệch kích thước giữa $\textit{minQ}$ và $\textit{maxQ}$ lớn hơn $1$, ta lấy phần tử đầu của $\textit{minQ}$ ra và thêm vào $\textit{maxQ}$. Độ phức tạp thời gian là $O(\log n)$.

Khi gọi method `findMedian`, nếu kích thước của $\textit{minQ}$ bằng kích thước của $\textit{maxQ}$ thì tổng số phần tử là chẵn, và ta có thể trả về trung bình của các phần tử đầu trong $\textit{minQ}$ và $\textit{maxQ}$; nếu không, ta trả về phần tử đầu của $\textit{minQ}$. Độ phức tạp thời gian là $O(1)$.

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
