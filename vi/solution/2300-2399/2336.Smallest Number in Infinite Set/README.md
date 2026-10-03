---
comments: true
difficulty: Medium
rating: 1375
source: Weekly Contest 301 Q2
tags:
    - Design
    - Hash Table
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2336. Smallest Number in Infinite Set](https://leetcode.com/problems/smallest-number-in-infinite-set)

[中文文档](/solution/2300-2399/2336.Smallest%20Number%20in%20Infinite%20Set/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một tập hợp chứa tất cả các số nguyên dương <code>[1, 2, 3, 4, 5, ...]</code>.</p>

<p>Hãy cài đặt lớp <code>SmallestInfiniteSet</code>:</p>

<ul>
	<li><code>SmallestInfiniteSet()</code> Khởi tạo đối tượng <strong>SmallestInfiniteSet</strong> chứa <strong>tất cả</strong> các số nguyên dương.</li>
	<li><code>int popSmallest()</code> <strong>Xóa</strong> và trả về số nguyên nhỏ nhất có trong tập hợp vô hạn.</li>
	<li><code>void addBack(int num)</code> <strong>Thêm</strong> số nguyên dương <code>num</code> trở lại tập hợp vô hạn nếu số đó <strong>chưa</strong> có trong tập hợp.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;SmallestInfiniteSet&quot;, &quot;addBack&quot;, &quot;popSmallest&quot;, &quot;popSmallest&quot;, &quot;popSmallest&quot;, &quot;addBack&quot;, &quot;popSmallest&quot;, &quot;popSmallest&quot;, &quot;popSmallest&quot;]
[[], [2], [], [], [], [1], [], [], []]
<strong>Đầu ra</strong>
[null, null, 1, 2, 3, null, 1, 4, 5]

<strong>Giải thích</strong>
SmallestInfiniteSet smallestInfiniteSet = new SmallestInfiniteSet();
smallestInfiniteSet.addBack(2);    // 2 is already in the set, so no change is made.
smallestInfiniteSet.popSmallest(); // return 1, since 1 is the smallest number, and remove it from the set.
smallestInfiniteSet.popSmallest(); // return 2, and remove it from the set.
smallestInfiniteSet.popSmallest(); // return 3, and remove it from the set.
smallestInfiniteSet.addBack(1);    // 1 is added back to the set.
smallestInfiniteSet.popSmallest(); // return 1, since 1 was added back to the set and
                                   // is the smallest number, and remove it from the set.
smallestInfiniteSet.popSmallest(); // return 4, and remove it from the set.
smallestInfiniteSet.popSmallest(); // return 5, and remove it from the set.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 1000</code></li>
	<li><strong>Tổng cộng</strong> có nhiều nhất <code>1000</code> lần gọi đến <code>popSmallest</code> và <code>addBack</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tập hợp có thứ tự + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Tập hợp ban đầu chứa tất cả các số nguyên dương, nhưng vì các giá trị và thao tác đều không vượt quá $1000$, chỉ cần xét $[1,1000]$.
>
> Lưu các số hiện có trong một tập hợp có thứ tự. Thao tác lấy ra sẽ xóa phần tử nhỏ nhất; thao tác thêm lại sẽ chèn phần tử đó vào tập hợp. Nhờ được sắp xếp, ta lấy được phần tử nhỏ nhất trong $O(\log n)$.

<!-- thinking:end -->

Ta nhận thấy miền giá trị của các phần tử trong tập hợp theo đề bài là $[1, 1000]$, và các thao tác cần hỗ trợ gồm:

- `popSmallest`: Lấy phần tử nhỏ nhất ra khỏi tập hợp
- `addBack`: Thêm một phần tử trở lại tập hợp

Do đó, ta có thể dùng một tập hợp có thứ tự để mô phỏng. Gọi tập hợp có thứ tự là $s$, các phần tử trong tập hợp là $s_1, s_2, \cdots, s_n$, trong đó $n$ là số lượng phần tử trong tập hợp. Trong bài này, $n \le 1000$.

Khi khởi tạo, ta thêm tất cả các phần tử trong $[1, 1000]$ vào tập hợp có thứ tự. Độ phức tạp thời gian là $O(n \times \log n)$.

Với thao tác `popSmallest`, ta chỉ cần lấy phần tử đầu tiên ra khỏi tập hợp có thứ tự. Độ phức tạp thời gian cho một thao tác là $O(\log n)$.

Với thao tác `addBack`, ta chỉ cần thêm phần tử đó trở lại tập hợp có thứ tự. Độ phức tạp thời gian cho một thao tác là $O(\log n)$.

Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class SmallestInfiniteSet:
    def __init__(self):
        self.s = SortedSet(range(1, 1001))

    def popSmallest(self) -> int:
        x = self.s[0]
        self.s.remove(x)
        return x

    def addBack(self, num: int) -> None:
        self.s.add(num)


# Your SmallestInfiniteSet object will be instantiated and called as such:
# obj = SmallestInfiniteSet()
# param_1 = obj.popSmallest()
# obj.addBack(num)
```

#### Java

```java
class SmallestInfiniteSet {
    private TreeSet<Integer> s = new TreeSet<>();

    public SmallestInfiniteSet() {
        for (int i = 1; i <= 1000; ++i) {
            s.add(i);
        }
    }

    public int popSmallest() {
        return s.pollFirst();
    }

    public void addBack(int num) {
        s.add(num);
    }
}

/**
 * Your SmallestInfiniteSet object will be instantiated and called as such:
 * SmallestInfiniteSet obj = new SmallestInfiniteSet();
 * int param_1 = obj.popSmallest();
 * obj.addBack(num);
 */
```

#### C++

```cpp
class SmallestInfiniteSet {
public:
    SmallestInfiniteSet() {
        for (int i = 1; i <= 1000; ++i) {
            s.insert(i);
        }
    }

    int popSmallest() {
        int x = *s.begin();
        s.erase(s.begin());
        return x;
    }

    void addBack(int num) {
        s.insert(num);
    }

private:
    set<int> s;
};

/**
 * Your SmallestInfiniteSet object will be instantiated and called as such:
 * SmallestInfiniteSet* obj = new SmallestInfiniteSet();
 * int param_1 = obj->popSmallest();
 * obj->addBack(num);
 */
```

#### Go

```go
type SmallestInfiniteSet struct {
	s *treemap.Map
}

func Constructor() SmallestInfiniteSet {
	s := treemap.NewWithIntComparator()
	for i := 1; i <= 1000; i++ {
		s.Put(i, nil)
	}
	return SmallestInfiniteSet{s}
}

func (this *SmallestInfiniteSet) PopSmallest() int {
	x, _ := this.s.Min()
	this.s.Remove(x.(int))
	return x.(int)
}

func (this *SmallestInfiniteSet) AddBack(num int) {
	this.s.Put(num, nil)
}

/**
 * Your SmallestInfiniteSet object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.PopSmallest();
 * obj.AddBack(num);
 */
```

#### TypeScript

```ts
class SmallestInfiniteSet {
    private pq = new MinPriorityQueue<number>();
    private s = new Set<number>();

    constructor() {
        for (let i = 1; i <= 1000; i++) {
            this.pq.enqueue(i);
            this.s.add(i);
        }
    }

    popSmallest(): number {
        const x = this.pq.dequeue();
        this.s.delete(x);
        return x;
    }

    addBack(num: number): void {
        if (!this.s.has(num)) {
            this.pq.enqueue(num);
            this.s.add(num);
        }
    }
}

/**
 * Your SmallestInfiniteSet object will be instantiated and called as such:
 * var obj = new SmallestInfiniteSet()
 * var param_1 = obj.popSmallest()
 * obj.addBack(num)
 */
```

#### Rust

```rust
use std::collections::BTreeSet;

struct SmallestInfiniteSet {
    s: BTreeSet<i32>,
}

impl SmallestInfiniteSet {
    fn new() -> Self {
        let mut set = BTreeSet::new();
        for i in 1..=1000 {
            set.insert(i);
        }
        SmallestInfiniteSet { s: set }
    }

    fn pop_smallest(&mut self) -> i32 {
        let x = *self.s.iter().next().unwrap();
        self.s.remove(&x);
        x
    }

    fn add_back(&mut self, num: i32) {
        self.s.insert(num);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
