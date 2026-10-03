---
comments: true
difficulty: Hard
rating: 2158
source: Biweekly Contest 67 Q4
tags:
    - Design
    - Data Stream
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2102. Sequentially Ordinal Rank Tracker](https://leetcode.com/problems/sequentially-ordinal-rank-tracker)

[中文文档](/solution/2100-2199/2102.Sequentially%20Ordinal%20Rank%20Tracker/README.md)

## Mô tả

<!-- description:start -->

<p>Một địa điểm phong cảnh được biểu diễn bởi <code>name</code> và <code>score</code>, trong đó <code>name</code> là một chuỗi <strong>duy nhất</strong> trong tất cả các địa điểm và <code>score</code> là một số nguyên. Các địa điểm có thể được xếp hạng từ tốt nhất đến kém nhất. <strong>Điểm</strong> càng cao thì địa điểm càng tốt. Nếu điểm của hai địa điểm bằng nhau, địa điểm có <strong>tên nhỏ hơn theo thứ tự từ điển</strong> sẽ tốt hơn.</p>

<p>Bạn đang xây dựng một hệ thống theo dõi thứ hạng của các địa điểm, ban đầu hệ thống không có địa điểm nào. Hệ thống hỗ trợ:</p>

<ul>
	<li><strong>Thêm</strong> các địa điểm phong cảnh, <strong>từng địa điểm một</strong>.</li>
	<li><strong>Truy vấn</strong> địa điểm <code>i<sup>th</sup></code> <strong>tốt nhất</strong> trong <strong>tất cả các địa điểm đã được thêm</strong>, trong đó <code>i</code> là số lần hệ thống đã được truy vấn (bao gồm cả truy vấn hiện tại).
	<ul>
		<li>Ví dụ, khi hệ thống được truy vấn lần thứ <code>4<sup>th</sup></code>, hệ thống trả về địa điểm tốt thứ <code>4<sup>th</sup></code> trong tất cả các địa điểm đã được thêm.</li>
	</ul>
	</li>
</ul>

<p>Lưu ý rằng dữ liệu kiểm thử được tạo sao cho <strong>tại mọi thời điểm</strong>, số lần truy vấn <strong>không vượt quá</strong> số địa điểm đã được thêm vào hệ thống.</p>

<p>Hãy triển khai lớp <code>SORTracker</code>:</p>

<ul>
	<li><code>SORTracker()</code> Khởi tạo hệ thống theo dõi.</li>
	<li><code>void add(string name, int score)</code> Thêm một địa điểm phong cảnh có <code>name</code> và <code>score</code> vào hệ thống.</li>
	<li><code>string get()</code> Truy vấn và trả về địa điểm tốt thứ <code>i<sup>th</sup></code>, trong đó <code>i</code> là số lần phương thức này được gọi (bao gồm cả lần gọi hiện tại).</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;SORTracker&quot;, &quot;add&quot;, &quot;add&quot;, &quot;get&quot;, &quot;add&quot;, &quot;get&quot;, &quot;add&quot;, &quot;get&quot;, &quot;add&quot;, &quot;get&quot;, &quot;add&quot;, &quot;get&quot;, &quot;get&quot;]
[[], [&quot;bradford&quot;, 2], [&quot;branford&quot;, 3], [], [&quot;alps&quot;, 2], [], [&quot;orland&quot;, 2], [], [&quot;orlando&quot;, 3], [], [&quot;alpine&quot;, 2], [], []]
<strong>Đầu ra</strong>
[null, null, null, &quot;branford&quot;, null, &quot;alps&quot;, null, &quot;bradford&quot;, null, &quot;bradford&quot;, null, &quot;bradford&quot;, &quot;orland&quot;]

<strong>Giải thích</strong>
SORTracker tracker = new SORTracker(); // Khởi tạo hệ thống theo dõi.
tracker.add(&quot;bradford&quot;, 2); // Thêm địa điểm có name=&quot;bradford&quot; và score=2 vào hệ thống.
tracker.add(&quot;branford&quot;, 3); // Thêm địa điểm có name=&quot;branford&quot; và score=3 vào hệ thống.
tracker.get();              // Các địa điểm được sắp xếp từ tốt nhất đến kém nhất là: branford, bradford.
                            // Lưu ý rằng branford đứng trước bradford do có <strong>điểm cao hơn</strong> (3 &gt; 2).
                            // Đây là lần thứ <sup>1</sup> get() được gọi, nên trả về địa điểm tốt nhất: &quot;branford&quot;.
tracker.add(&quot;alps&quot;, 2);     // Thêm địa điểm có name=&quot;alps&quot; và score=2 vào hệ thống.
tracker.get();              // Các địa điểm được sắp xếp là: branford, alps, bradford.
                            // Lưu ý rằng alps đứng trước bradford dù chúng có cùng điểm (2).
                            // Điều này là vì &quot;alps&quot; có thứ tự từ điển <strong>nhỏ hơn</strong> &quot;bradford&quot;.
                            // Trả về địa điểm tốt thứ <sup>2</sup> &quot;alps&quot;, vì đây là lần thứ <sup>2</sup> get() được gọi.
tracker.add(&quot;orland&quot;, 2);   // Thêm địa điểm có name=&quot;orland&quot; và score=2 vào hệ thống.
tracker.get();              // Các địa điểm được sắp xếp là: branford, alps, bradford, orland.
                            // Trả về &quot;bradford&quot;, vì đây là lần thứ <sup>3</sup> get() được gọi.
tracker.add(&quot;orlando&quot;, 3);  // Thêm địa điểm có name=&quot;orlando&quot; và score=3 vào hệ thống.
tracker.get();              // Các địa điểm được sắp xếp là: branford, orlando, alps, bradford, orland.
                            // Trả về &quot;bradford&quot;.
tracker.add(&quot;alpine&quot;, 2);   // Thêm địa điểm có name=&quot;alpine&quot; và score=2 vào hệ thống.
tracker.get();              // Các địa điểm được sắp xếp là: branford, orlando, alpine, alps, bradford, orland.
                            // Trả về &quot;bradford&quot;.
tracker.get();              // Các địa điểm được sắp xếp là: branford, orlando, alpine, alps, bradford, orland.
                            // Trả về &quot;orland&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>name</code> chỉ gồm các chữ cái tiếng Anh viết thường và là duy nhất trong tất cả các địa điểm.</li>
	<li><code>1 &lt;= name.length &lt;= 10</code></li>
	<li><code>1 &lt;= score &lt;= 10<sup>5</sup></code></li>
	<li>Tại mọi thời điểm, số lần gọi <code>get</code> không vượt quá số lần gọi <code>add</code>.</li>
	<li>Tổng cộng có nhiều nhất <code>4 * 10<sup>4</sup></code> lần gọi <strong>tất cả</strong> các phương thức <code>add</code> và <code>get</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Tracker phải thêm các địa điểm trong khi trả lời các truy vấn theo thứ hạng tăng dần nghiêm ngặt. Việc sắp xếp toàn bộ danh sách trong mỗi truy vấn sẽ tốn khoảng $O(n\log n)$ cho mỗi lần và quá nặng khi có một chuỗi thao tác dài.
>
> Chỉ số truy vấn chỉ tăng, còn khóa so sánh là $(-\textit{score},\textit{name})$, nên một dãy có thứ tự và truy cập trực tiếp theo chỉ số là đủ.
>
> Vì vậy, ta lưu tất cả địa điểm trong một danh sách được sắp xếp theo khóa này và một bộ đếm $i$ cho biết số lần $\texttt{get}$ đã được gọi, rồi trả về tên ở chỉ số $i$ sau khi tăng bộ đếm.

<!-- thinking:end -->

Ta có thể dùng một ordered set để lưu các địa điểm và một biến $i$ để ghi lại số lần truy vấn hiện tại, ban đầu $i = -1$.

Khi gọi phương thức `add`, ta lấy số đối của điểm của địa điểm để ordered set có thể sắp xếp theo điểm giảm dần. Nếu điểm bằng nhau, sắp xếp tên địa điểm theo thứ tự từ điển tăng dần.

Khi gọi phương thức `get`, ta tăng $i$ lên một, sau đó trả về tên của địa điểm ở vị trí thứ $i$ trong ordered set.

Độ phức tạp thời gian của mỗi thao tác là $O(\log n)$, trong đó $n$ là số địa điểm đã được thêm. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class SORTracker:

    def __init__(self):
        self.sl = SortedList()
        self.i = -1

    def add(self, name: str, score: int) -> None:
        self.sl.add((-score, name))

    def get(self) -> str:
        self.i += 1
        return self.sl[self.i][1]


# Your SORTracker object will be instantiated and called as such:
# obj = SORTracker()
# obj.add(name,score)
# param_2 = obj.get()
```

#### C++

```cpp
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/hash_policy.hpp>
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

class SORTracker {
public:
    SORTracker() {
    }

    void add(string name, int score) {
        st.insert({-score, name});
    }

    string get() {
        return st.find_by_order(++i)->second;
    }

private:
    ordered_set<pair<int, string>> st;
    int i = -1;
};

/**
 * Your SORTracker object will be instantiated and called as such:
 * SORTracker* obj = new SORTracker();
 * obj->add(name,score);
 * string param_2 = obj->get();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Double Priority Queue (Min-Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dựa trên việc truy cập ngẫu nhiên vào ordered set. Các ngôn ngữ không có container cây cân bằng cần một cấu trúc khác, khai thác việc các truy vấn tăng nghiêm ngặt.
>
> Tương tự bài toán median trong data stream, hai heap có thể chia các địa điểm tốt nhất đã được công bố và phần còn lại: min-heap $\textit{good}$ lưu các phần tử tốt hơn đã được truy vấn, còn max-heap $\textit{bad}$ lưu phần còn lại.
>
> $\texttt{add}$ đẩy phần tử mới qua $\textit{good}$ rồi chuyển phần tử tệ nhất của $\textit{good}$ sang $\textit{bad}$; $\texttt{get}$ đưa phần tử tốt nhất của $\textit{bad}$ vào $\textit{good}$, mà phần tử đầu của nó chính là thứ hạng hiện tại. Tên được lưu theo thứ tự ngược để điểm cao hơn và thứ tự từ điển nhỏ hơn được ưu tiên.

<!-- thinking:end -->

Ta nhận thấy các thao tác truy vấn trong bài toán này được thực hiện theo thứ tự tăng dần nghiêm ngặt. Do đó, ta có thể dùng phương pháp tương tự như bài toán median trong data stream. Ta định nghĩa hai priority queue `good` và `bad`. `good` là một min-heap, lưu các địa điểm tốt nhất hiện tại, còn `bad` là một max-heap, lưu địa điểm tốt thứ $i$ hiện tại.

Mỗi khi gọi phương thức `add`, ta thêm điểm và tên của địa điểm vào `good`, sau đó thêm địa điểm tệ nhất trong `good` vào `bad`.

Mỗi khi gọi phương thức `get`, ta thêm địa điểm tốt nhất trong `bad` vào `good`, sau đó trả về địa điểm tệ nhất trong `good`.

Độ phức tạp thời gian của mỗi thao tác là $O(\log n)$, trong đó $n$ là số địa điểm đã được thêm. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Node:
    def __init__(self, s: str):
        self.s = s

    def __lt__(self, other):
        return self.s > other.s


class SORTracker:

    def __init__(self):
        self.good = []
        self.bad = []

    def add(self, name: str, score: int) -> None:
        score, node = heappushpop(self.good, (score, Node(name)))
        heappush(self.bad, (-score, node.s))

    def get(self) -> str:
        score, name = heappop(self.bad)
        heappush(self.good, (-score, Node(name)))
        return self.good[0][1].s


# Your SORTracker object will be instantiated and called as such:
# obj = SORTracker()
# obj.add(name,score)
# param_2 = obj.get()
```

#### Java

```java
class SORTracker {
    private PriorityQueue<Map.Entry<Integer, String>> good = new PriorityQueue<>(
        (a, b)
            -> a.getKey().equals(b.getKey()) ? b.getValue().compareTo(a.getValue())
                                             : a.getKey() - b.getKey());
    private PriorityQueue<Map.Entry<Integer, String>> bad = new PriorityQueue<>(
        (a, b)
            -> a.getKey().equals(b.getKey()) ? a.getValue().compareTo(b.getValue())
                                             : b.getKey() - a.getKey());

    public SORTracker() {
    }

    public void add(String name, int score) {
        good.offer(Map.entry(score, name));
        bad.offer(good.poll());
    }

    public String get() {
        good.offer(bad.poll());
        return good.peek().getValue();
    }
}

/**
 * Your SORTracker object will be instantiated and called as such:
 * SORTracker obj = new SORTracker();
 * obj.add(name,score);
 * String param_2 = obj.get();
 */
```

#### C++

```cpp
using pis = pair<int, string>;

class SORTracker {
public:
    SORTracker() {
    }

    void add(string name, int score) {
        good.push({-score, name});
        bad.push(good.top());
        good.pop();
    }

    string get() {
        good.push(bad.top());
        bad.pop();
        return good.top().second;
    }

private:
    priority_queue<pis, vector<pis>, less<pis>> good;
    priority_queue<pis, vector<pis>, greater<pis>> bad;
};

/**
 * Your SORTracker object will be instantiated and called as such:
 * SORTracker* obj = new SORTracker();
 * obj->add(name,score);
 * string param_2 = obj->get();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
