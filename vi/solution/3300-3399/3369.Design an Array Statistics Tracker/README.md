---
comments: true
difficulty: Hard
tags:
    - Design
    - Queue
    - Hash Table
    - Binary Search
    - Data Stream
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3369. Design an Array Statistics Tracker 🔒](https://leetcode.com/problems/design-an-array-statistics-tracker)

[中文文档](/solution/3300-3399/3369.Design%20an%20Array%20Statistics%20Tracker/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một cấu trúc dữ liệu theo dõi các giá trị trong đó và trả lời một số truy vấn về mean, median và mode của chúng.</p>

<p>Hãy cài đặt lớp <code>StatisticsTracker</code>.</p>

<ul>
	<li><code>StatisticsTracker()</code>: Khởi tạo đối tượng <code>StatisticsTracker</code> với một mảng rỗng.</li>
	<li><code>void addNumber(int number)</code>: Thêm <code>number</code> vào cấu trúc dữ liệu.</li>
	<li><code>void removeFirstAddedNumber()</code>: Xóa số được thêm vào sớm nhất khỏi cấu trúc dữ liệu.</li>
	<li><code>int getMean()</code>: Trả về <strong>mean</strong> lấy phần nguyên của các số trong cấu trúc dữ liệu.</li>
	<li><code>int getMedian()</code>: Trả về <strong>median</strong> của các số trong cấu trúc dữ liệu.</li>
	<li><code>int getMode()</code>: Trả về <strong>mode</strong> của các số trong cấu trúc dữ liệu. Nếu có nhiều mode, trả về mode nhỏ nhất.</li>
</ul>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li><strong>Mean</strong> của một mảng là tổng của tất cả các giá trị chia cho số lượng giá trị trong mảng.</li>
	<li><strong>Median</strong> của một mảng là phần tử ở giữa mảng khi mảng được sắp xếp theo thứ tự không giảm. Nếu có hai lựa chọn cho median, chọn giá trị lớn hơn trong hai giá trị đó.</li>
	<li><strong>Mode</strong> của một mảng là phần tử xuất hiện nhiều nhất trong mảng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;StatisticsTracker&quot;, &quot;addNumber&quot;, &quot;addNumber&quot;, &quot;addNumber&quot;, &quot;addNumber&quot;, &quot;getMean&quot;, &quot;getMedian&quot;, &quot;getMode&quot;, &quot;removeFirstAddedNumber&quot;, &quot;getMode&quot;]<br />
[[], [4], [4], [2], [3], [], [], [], [], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, null, null, null, 3, 4, 4, null, 2] </span></p>

<p><strong>Giải thích</strong></p>
StatisticsTracker statisticsTracker = new StatisticsTracker();<br />
statisticsTracker.addNumber(4); // The data structure now contains [4]<br />
statisticsTracker.addNumber(4); // The data structure now contains [4, 4]<br />
statisticsTracker.addNumber(2); // The data structure now contains [4, 4, 2]<br />
statisticsTracker.addNumber(3); // The data structure now contains [4, 4, 2, 3]<br />
statisticsTracker.getMean(); // return 3<br />
statisticsTracker.getMedian(); // return 4<br />
statisticsTracker.getMode(); // return 4<br />
statisticsTracker.removeFirstAddedNumber(); // The data structure now contains [4, 2, 3]<br />
statisticsTracker.getMode(); // return 2</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;StatisticsTracker&quot;, &quot;addNumber&quot;, &quot;addNumber&quot;, &quot;getMean&quot;, &quot;removeFirstAddedNumber&quot;, &quot;addNumber&quot;, &quot;addNumber&quot;, &quot;removeFirstAddedNumber&quot;, &quot;getMedian&quot;, &quot;addNumber&quot;, &quot;getMode&quot;]<br />
[[], [9], [5], [], [], [5], [6], [], [], [8], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, null, 7, null, null, null, null, 6, null, 5] </span></p>

<p><strong>Giải thích</strong></p>
StatisticsTracker statisticsTracker = new StatisticsTracker();<br />
statisticsTracker.addNumber(9); // The data structure now contains [9]<br />
statisticsTracker.addNumber(5); // The data structure now contains [9, 5]<br />
statisticsTracker.getMean(); // return 7<br />
statisticsTracker.removeFirstAddedNumber(); // The data structure now contains [5]<br />
statisticsTracker.addNumber(5); // The data structure now contains [5, 5]<br />
statisticsTracker.addNumber(6); // The data structure now contains [5, 5, 6]<br />
statisticsTracker.removeFirstAddedNumber(); // The data structure now contains [5, 6]<br />
statisticsTracker.getMedian(); // return 6<br />
statisticsTracker.addNumber(8); // The data structure now contains [5, 6, 8]<br />
statisticsTracker.getMode(); // return 5</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= number &lt;= 10<sup>9</sup></code></li>
	<li>Tổng số lần gọi <code>addNumber</code>, <code>removeFirstAddedNumber</code>, <code>getMean</code>, <code>getMedian</code> và <code>getMode</code> nhiều nhất là <code>10<sup>5</sup></code>.</li>
	<li><code>removeFirstAddedNumber</code>, <code>getMean</code>, <code>getMedian</code> và <code>getMode</code> chỉ được gọi khi cấu trúc dữ liệu có ít nhất một phần tử.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Queue + Hash Table + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Với $10^5$ thao tác, ta cần thêm phần tử vào queue, xóa giá trị cũ nhất, đồng thời truy vấn mean, median và mode trong $O(\log n)$.
>
> Queue giữ thứ tự thêm vào; $s$ là tổng dùng cho mean; $\textit{sl}$ là danh sách đã sắp xếp cho median; một danh sách đã sắp xếp khác, được sắp theo $(-\textit{cnt},\textit{value})$, sẽ cho mode.
>
> Khi cập nhật tần suất, ta xóa pair cũ trước khi chèn pair mới để tập mode luôn nhất quán.

<!-- thinking:end -->

Ta định nghĩa một queue $\textit{q}$ để lưu các số được thêm vào, một biến $\textit{s}$ để lưu tổng của tất cả các số, một hash table $\textit{cnt}$ để lưu số lần xuất hiện của mỗi số, một ordered set $\textit{sl}$ để lưu tất cả các số, và một ordered set $\textit{sl2}$ để lưu tất cả các số cùng số lần xuất hiện của chúng, được sắp xếp theo số lần xuất hiện giảm dần rồi theo giá trị tăng dần.

Trong phương thức `addNumber`, ta thêm số vào queue $\textit{q}$, thêm số vào ordered set $\textit{sl}$, sau đó xóa số và số lần xuất hiện của nó khỏi ordered set $\textit{sl2}$, cập nhật số lần xuất hiện của số đó, cuối cùng thêm số và số lần xuất hiện đã cập nhật vào ordered set $\textit{sl2}$, đồng thời cập nhật tổng của tất cả các số. Độ phức tạp thời gian là $O(\log n)$.

Trong phương thức `removeFirstAddedNumber`, ta xóa số được thêm vào sớm nhất khỏi queue $\textit{q}$, xóa số đó khỏi ordered set $\textit{sl}$, sau đó xóa số và số lần xuất hiện của nó khỏi ordered set $\textit{sl2}$, cập nhật số lần xuất hiện của số đó, cuối cùng thêm số và số lần xuất hiện đã cập nhật vào ordered set $\textit{sl2}$, đồng thời cập nhật tổng của tất cả các số. Độ phức tạp thời gian là $O(\log n)$.

Trong phương thức `getMean`, ta trả về tổng của tất cả các số chia cho số lượng số. Độ phức tạp thời gian là $O(1)$.

Trong phương thức `getMedian`, ta trả về số thứ $\textit{len}(\textit{q}) / 2$ trong ordered set $\textit{sl}$. Độ phức tạp thời gian là $O(1)$ hoặc $O(\log n)$.

Trong phương thức `getMode`, ta trả về số đầu tiên trong ordered set $\textit{sl2}$. Độ phức tạp thời gian là $O(1)$.

> Trong Python, ta có thể truy cập trực tiếp các phần tử trong ordered set bằng chỉ số. Trong các ngôn ngữ khác, ta có thể cài đặt cấu trúc này bằng heap.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng số đã được thêm vào.

<!-- tabs:start -->

#### Python3

```python
class StatisticsTracker:

    def __init__(self):
        self.q = deque()
        self.s = 0
        self.cnt = defaultdict(int)
        self.sl = SortedList()
        self.sl2 = SortedList(key=lambda x: (-x[1], x[0]))

    def addNumber(self, number: int) -> None:
        self.q.append(number)
        self.sl.add(number)
        self.sl2.discard((number, self.cnt[number]))
        self.cnt[number] += 1
        self.sl2.add((number, self.cnt[number]))
        self.s += number

    def removeFirstAddedNumber(self) -> None:
        number = self.q.popleft()
        self.sl.remove(number)
        self.sl2.discard((number, self.cnt[number]))
        self.cnt[number] -= 1
        self.sl2.add((number, self.cnt[number]))
        self.s -= number

    def getMean(self) -> int:
        return self.s // len(self.q)

    def getMedian(self) -> int:
        return self.sl[len(self.q) // 2]

    def getMode(self) -> int:
        return self.sl2[0][0]


# Your StatisticsTracker object will be instantiated and called as such:
# obj = StatisticsTracker()
# obj.addNumber(number)
# obj.removeFirstAddedNumber()
# param_3 = obj.getMean()
# param_4 = obj.getMedian()
# param_5 = obj.getMode()
```

#### Java

```java
class MedianFinder {
    private final PriorityQueue<Integer> small = new PriorityQueue<>(Comparator.reverseOrder());
    private final PriorityQueue<Integer> large = new PriorityQueue<>();
    private final Map<Integer, Integer> delayed = new HashMap<>();
    private int smallSize;
    private int largeSize;

    public void addNum(int num) {
        if (small.isEmpty() || num <= small.peek()) {
            small.offer(num);
            ++smallSize;
        } else {
            large.offer(num);
            ++largeSize;
        }
        rebalance();
    }

    public Integer findMedian() {
        return smallSize == largeSize ? large.peek() : small.peek();
    }

    public void removeNum(int num) {
        delayed.merge(num, 1, Integer::sum);
        if (num <= small.peek()) {
            --smallSize;
            if (num == small.peek()) {
                prune(small);
            }
        } else {
            --largeSize;
            if (num == large.peek()) {
                prune(large);
            }
        }
        rebalance();
    }

    private void prune(PriorityQueue<Integer> pq) {
        while (!pq.isEmpty() && delayed.containsKey(pq.peek())) {
            if (delayed.merge(pq.peek(), -1, Integer::sum) == 0) {
                delayed.remove(pq.peek());
            }
            pq.poll();
        }
    }

    private void rebalance() {
        if (smallSize > largeSize + 1) {
            large.offer(small.poll());
            --smallSize;
            ++largeSize;
            prune(small);
        } else if (smallSize < largeSize) {
            small.offer(large.poll());
            --largeSize;
            ++smallSize;
            prune(large);
        }
    }
}

class StatisticsTracker {
    private final Deque<Integer> q = new ArrayDeque<>();
    private long s;
    private final Map<Integer, Integer> cnt = new HashMap<>();
    private final MedianFinder medianFinder = new MedianFinder();
    private final TreeSet<int[]> ts
        = new TreeSet<>((a, b) -> a[1] == b[1] ? a[0] - b[0] : b[1] - a[1]);

    public StatisticsTracker() {
    }

    public void addNumber(int number) {
        q.offerLast(number);
        s += number;
        ts.remove(new int[] {number, cnt.getOrDefault(number, 0)});
        cnt.merge(number, 1, Integer::sum);
        medianFinder.addNum(number);
        ts.add(new int[] {number, cnt.get(number)});
    }

    public void removeFirstAddedNumber() {
        int number = q.pollFirst();
        s -= number;
        ts.remove(new int[] {number, cnt.get(number)});
        cnt.merge(number, -1, Integer::sum);
        medianFinder.removeNum(number);
        ts.add(new int[] {number, cnt.get(number)});
    }

    public int getMean() {
        return (int) (s / q.size());
    }

    public int getMedian() {
        return medianFinder.findMedian();
    }

    public int getMode() {
        return ts.first()[0];
    }
}
```

#### C++

```cpp
class MedianFinder {
public:
    void addNum(int num) {
        if (small.empty() || num <= small.top()) {
            small.push(num);
            ++smallSize;
        } else {
            large.push(num);
            ++largeSize;
        }
        reblance();
    }

    void removeNum(int num) {
        ++delayed[num];
        if (num <= small.top()) {
            --smallSize;
            if (num == small.top()) {
                prune(small);
            }
        } else {
            --largeSize;
            if (num == large.top()) {
                prune(large);
            }
        }
        reblance();
    }

    int findMedian() {
        return smallSize == largeSize ? large.top() : small.top();
    }

private:
    priority_queue<int> small;
    priority_queue<int, vector<int>, greater<int>> large;
    unordered_map<int, int> delayed;
    int smallSize = 0;
    int largeSize = 0;

    template <typename T>
    void prune(T& pq) {
        while (!pq.empty() && delayed[pq.top()]) {
            if (--delayed[pq.top()] == 0) {
                delayed.erase(pq.top());
            }
            pq.pop();
        }
    }

    void reblance() {
        if (smallSize > largeSize + 1) {
            large.push(small.top());
            small.pop();
            --smallSize;
            ++largeSize;
            prune(small);
        } else if (smallSize < largeSize) {
            small.push(large.top());
            large.pop();
            ++smallSize;
            --largeSize;
            prune(large);
        }
    }
};

class StatisticsTracker {
private:
    queue<int> q;
    long long s = 0;
    unordered_map<int, int> cnt;
    MedianFinder medianFinder;
    set<pair<int, int>> ts;

public:
    StatisticsTracker() {}

    void addNumber(int number) {
        q.push(number);
        s += number;
        ts.erase({-cnt[number], number});
        cnt[number]++;
        medianFinder.addNum(number);
        ts.insert({-cnt[number], number});
    }

    void removeFirstAddedNumber() {
        int number = q.front();
        q.pop();
        s -= number;
        ts.erase({-cnt[number], number});
        cnt[number]--;
        if (cnt[number] > 0) {
            ts.insert({-cnt[number], number});
        }
        medianFinder.removeNum(number);
    }

    int getMean() {
        return static_cast<int>(s / q.size());
    }

    int getMedian() {
        return medianFinder.findMedian();
    }

    int getMode() {
        return ts.begin()->second;
    }
};
```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
