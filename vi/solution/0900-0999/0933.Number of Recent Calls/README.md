---
comments: true
difficulty: Easy
tags:
    - Design
    - Queue
    - Data Stream
---

<!-- problem:start -->

# [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls)

[中文文档](/solution/0900-0999/0933.Number%20of%20Recent%20Calls/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có class <code>RecentCounter</code> dùng để đếm số request gần đây trong một khoảng thời gian nhất định.</p>

<p>Cài đặt class <code>RecentCounter</code>:</p>

<ul>
	<li><code>RecentCounter()</code> Khởi tạo counter với số request gần đây bằng 0.</li>
	<li><code>int ping(int t)</code> Thêm request mới tại thời điểm <code>t</code>, trong đó <code>t</code> tính bằng mili giây, rồi trả về số request xảy ra trong <code>3000</code> mili giây gần nhất (bao gồm request vừa thêm). Cụ thể, trả về số request có thời điểm nằm trong khoảng đóng <code>[t - 3000, t]</code>.</li>
</ul>

<p><strong>Đảm bảo</strong> mỗi lần gọi <code>ping</code>, giá trị <code>t</code> lớn hơn nghiêm ngặt so với lần gọi trước.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;RecentCounter&quot;, &quot;ping&quot;, &quot;ping&quot;, &quot;ping&quot;, &quot;ping&quot;]
[[], [1], [100], [3001], [3002]]
<strong>Đầu ra</strong>
[null, 1, 2, 3, 3]

<strong>Giải thích</strong>
RecentCounter recentCounter = new RecentCounter();
recentCounter.ping(1);     // requests = [<u>1</u>], range is [-2999,1], return 1
recentCounter.ping(100);   // requests = [<u>1</u>, <u>100</u>], range is [-2900,100], return 2
recentCounter.ping(3001);  // requests = [<u>1</u>, <u>100</u>, <u>3001</u>], range is [1,3001], return 3
recentCounter.ping(3002);  // requests = [1, <u>100</u>, <u>3001</u>, <u>3002</u>], range is [2,3002], return 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= t &lt;= 10<sup>9</sup></code></li>
	<li>Mỗi test case gọi <code>ping</code> với các giá trị <code>t</code> <strong>tăng nghiêm ngặt</strong>.</li>
	<li>Số lần gọi <code>ping</code> không quá <code>10<sup>4</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $t$ tăng nghiêm ngặt nên request có thời điểm nhỏ hơn $t-3000$ sẽ không bao giờ nằm trong phạm vi truy vấn về sau. Lưu thời điểm vào queue và lấy khỏi đầu queue khi đã hết hạn; kích thước queue chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class RecentCounter:
    def __init__(self):
        self.q = deque()

    def ping(self, t: int) -> int:
        self.q.append(t)
        while self.q[0] < t - 3000:
            self.q.popleft()
        return len(self.q)


# Your RecentCounter object will be instantiated and called as such:
# obj = RecentCounter()
# param_1 = obj.ping(t)
```

#### Java

```java
class RecentCounter {
    private int[] s = new int[10010];
    private int idx;

    public RecentCounter() {
    }

    public int ping(int t) {
        s[idx++] = t;
        return idx - search(t - 3000);
    }

    private int search(int x) {
        int left = 0, right = idx;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (s[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter obj = new RecentCounter();
 * int param_1 = obj.ping(t);
 */
```

#### C++

```cpp
class RecentCounter {
public:
    queue<int> q;

    RecentCounter() {
    }

    int ping(int t) {
        q.push(t);
        while (q.front() < t - 3000) q.pop();
        return q.size();
    }
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
 */
```

#### Go

```go
type RecentCounter struct {
	q []int
}

func Constructor() RecentCounter {
	return RecentCounter{[]int{}}
}

func (this *RecentCounter) Ping(t int) int {
	this.q = append(this.q, t)
	for this.q[0] < t-3000 {
		this.q = this.q[1:]
	}
	return len(this.q)
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Ping(t);
 */
```

#### TypeScript

```ts
class RecentCounter {
    private queue: number[];

    constructor() {
        this.queue = [];
    }

    ping(t: number): number {
        this.queue.push(t);
        while (this.queue[0] < t - 3000) {
            this.queue.shift();
        }
        return this.queue.length;
    }
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * var obj = new RecentCounter()
 * var param_1 = obj.ping(t)
 */
```

#### Rust

```rust
use std::collections::VecDeque;
struct RecentCounter {
    queue: VecDeque<i32>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl RecentCounter {
    fn new() -> Self {
        Self {
            queue: VecDeque::new(),
        }
    }

    fn ping(&mut self, t: i32) -> i32 {
        self.queue.push_back(t);
        while let Some(&v) = self.queue.front() {
            if v >= t - 3000 {
                break;
            }
            self.queue.pop_front();
        }
        self.queue.len() as i32
    }
}
```

#### JavaScript

```js
var RecentCounter = function () {
    this.q = [];
};

/**
 * @param {number} t
 * @return {number}
 */
RecentCounter.prototype.ping = function (t) {
    this.q.push(t);
    while (this.q[0] < t - 3000) {
        this.q.shift();
    }
    return this.q.length;
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * var obj = new RecentCounter()
 * var param_1 = obj.ping(t)
 */
```

#### C#

```cs
public class RecentCounter {
    private Queue<int> q = new Queue<int>();

    public RecentCounter() {

    }

    public int Ping(int t) {
        q.Enqueue(t);
        while (q.Peek() < t - 3000) {
            q.Dequeue();
        }
        return q.Count;
    }
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter obj = new RecentCounter();
 * int param_1 = obj.Ping(t);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Queue có tổng độ phức tạp tuyến tính theo số thao tác. Vì các thời điểm đã được sắp xếp, ta có thể lưu chúng trong mảng và tìm kiếm nhị phân chỉ số đầu tiên thỏa $\ge t-3000$; độ dài phần đuôi chính là số request. Bộ nhớ phụ vẫn tuyến tính, còn mỗi query mất thời gian logarithmic.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class RecentCounter:
    def __init__(self):
        self.s = []

    def ping(self, t: int) -> int:
        self.s.append(t)
        return len(self.s) - bisect_left(self.s, t - 3000)


# Your RecentCounter object will be instantiated and called as such:
# obj = RecentCounter()
# param_1 = obj.ping(t)
```

#### C++

```cpp
class RecentCounter {
public:
    vector<int> s;

    RecentCounter() {
    }

    int ping(int t) {
        s.push_back(t);
        return s.size() - (lower_bound(s.begin(), s.end(), t - 3000) - s.begin());
    }
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
 */
```

#### Go

```go
type RecentCounter struct {
	s []int
}

func Constructor() RecentCounter {
	return RecentCounter{[]int{}}
}

func (this *RecentCounter) Ping(t int) int {
	this.s = append(this.s, t)
	search := func(x int) int {
		left, right := 0, len(this.s)
		for left < right {
			mid := (left + right) >> 1
			if this.s[mid] >= x {
				right = mid
			} else {
				left = mid + 1
			}
		}
		return left
	}
	return len(this.s) - search(t-3000)
}

/**
 * Your RecentCounter object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Ping(t);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
