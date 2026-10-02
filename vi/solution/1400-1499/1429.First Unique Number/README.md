---
comments: true
difficulty: Medium
tags:
    - Design
    - Queue
    - Array
    - Hash Table
    - Data Stream
---

<!-- problem:start -->

# [1429. First Unique Number 🔒](https://leetcode.com/problems/first-unique-number)

[中文文档](/solution/1400-1499/1429.First%20Unique%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một hàng đợi các số nguyên và cần lấy ra số nguyên duy nhất đầu tiên trong hàng đợi.</p>

<p>Hãy triển khai lớp <code>FirstUnique</code>:</p>

<ul>
	<li><code>FirstUnique(int[] nums)</code> khởi tạo đối tượng với các số trong hàng đợi.</li>
	<li><code>int showFirstUnique()</code>&nbsp;trả về giá trị của số nguyên <strong>duy nhất đầu tiên</strong> trong hàng đợi, và trả về <strong>-1</strong> nếu không có số nào như vậy.</li>
	<li><code>void add(int value)</code>&nbsp;thêm value&nbsp;vào hàng đợi.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<b>Đầu vào: </b>
[&quot;FirstUnique&quot;,&quot;showFirstUnique&quot;,&quot;add&quot;,&quot;showFirstUnique&quot;,&quot;add&quot;,&quot;showFirstUnique&quot;,&quot;add&quot;,&quot;showFirstUnique&quot;]
[[[2,3,5]],[],[5],[],[2],[],[3],[]]
<b>Đầu ra: </b>
[null,2,null,2,null,3,null,-1]
<b>Giải thích: </b>
FirstUnique firstUnique = new FirstUnique([2,3,5]);
firstUnique.showFirstUnique(); // return 2
firstUnique.add(5);            // the queue is now [2,3,5,5]
firstUnique.showFirstUnique(); // return 2
firstUnique.add(2);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; // the queue is now [2,3,5,5,2]
firstUnique.showFirstUnique(); // return 3
firstUnique.add(3);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; // the queue is now [2,3,5,5,2,3]
firstUnique.showFirstUnique(); // return -1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<b>Đầu vào: </b>
[&quot;FirstUnique&quot;,&quot;showFirstUnique&quot;,&quot;add&quot;,&quot;add&quot;,&quot;add&quot;,&quot;add&quot;,&quot;add&quot;,&quot;showFirstUnique&quot;]
[[[7,7,7,7,7,7]],[],[7],[3],[3],[7],[17],[]]
<b>Đầu ra: </b>
[null,-1,null,null,null,null,null,17]
<b>Giải thích: </b>
FirstUnique firstUnique = new FirstUnique([7,7,7,7,7,7]);
firstUnique.showFirstUnique(); // return -1
firstUnique.add(7);            // the queue is now [7,7,7,7,7,7,7]
firstUnique.add(3);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; // the queue is now [7,7,7,7,7,7,7,3]
firstUnique.add(3);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; // the queue is now [7,7,7,7,7,7,7,3,3]
firstUnique.add(7);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; // the queue is now [7,7,7,7,7,7,7,3,3,7]
firstUnique.add(17);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;// the queue is now [7,7,7,7,7,7,7,3,3,7,17]
firstUnique.showFirstUnique(); // return 17
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<b>Đầu vào: </b>
[&quot;FirstUnique&quot;,&quot;showFirstUnique&quot;,&quot;add&quot;,&quot;showFirstUnique&quot;]
[[[809]],[],[809],[]]
<b>Đầu ra: </b>
[null,809,null,-1]
<b>Giải thích: </b>
FirstUnique firstUnique = new FirstUnique([809]);
firstUnique.showFirstUnique(); // return 809
firstUnique.add(809);          // the queue is now [809,809]
firstUnique.showFirstUnique(); // return -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10^5</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10^8</code></li>
	<li><code>1 &lt;= value &lt;= 10^8</code></li>
	<li>Có nhiều nhất <code>50000</code>&nbsp;lần gọi đến <code>showFirstUnique</code>&nbsp;và <code>add</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau nhiều lần gọi `add`, ta phải trả về giá trị cũ nhất vẫn chỉ xuất hiện một lần. Chỉ dùng bộ đếm sẽ làm mất thứ tự; chỉ dùng queue thì không thể xóa một giá trị vừa trở thành trùng lặp.
>
> Hãy đếm tần suất và giữ các giá trị duy nhất hiện tại trong một `OrderedDict`. Khi `add`, thêm giá trị khi số lần xuất hiện trở thành $1$ và xóa khi trở thành $2$. `showFirstUnique` trả về key đầu tiên còn lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class FirstUnique:
    def __init__(self, nums: List[int]):
        self.cnt = Counter(nums)
        self.unique = OrderedDict({v: 1 for v in nums if self.cnt[v] == 1})

    def showFirstUnique(self) -> int:
        return -1 if not self.unique else next(v for v in self.unique.keys())

    def add(self, value: int) -> None:
        self.cnt[value] += 1
        if self.cnt[value] == 1:
            self.unique[value] = 1
        elif value in self.unique:
            self.unique.pop(value)


# Your FirstUnique object will be instantiated and called as such:
# obj = FirstUnique(nums)
# param_1 = obj.showFirstUnique()
# obj.add(value)
```

#### Java

```java
class FirstUnique {
    private Map<Integer, Integer> cnt = new HashMap<>();
    private Set<Integer> unique = new LinkedHashSet<>();

    public FirstUnique(int[] nums) {
        for (int v : nums) {
            cnt.put(v, cnt.getOrDefault(v, 0) + 1);
        }
        for (int v : nums) {
            if (cnt.get(v) == 1) {
                unique.add(v);
            }
        }
    }

    public int showFirstUnique() {
        return unique.isEmpty() ? -1 : unique.iterator().next();
    }

    public void add(int value) {
        cnt.put(value, cnt.getOrDefault(value, 0) + 1);
        if (cnt.get(value) == 1) {
            unique.add(value);
        } else {
            unique.remove(value);
        }
    }
}

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique obj = new FirstUnique(nums);
 * int param_1 = obj.showFirstUnique();
 * obj.add(value);
 */
```

#### C++

```cpp
class FirstUnique {
public:
    FirstUnique(vector<int>& nums) {
        for (int& v : nums) {
            ++cnt[v];
            q.push_back(v);
        }
    }

    int showFirstUnique() {
        while (q.size() && cnt[q.front()] != 1) q.pop_front();
        return q.size() ? q.front() : -1;
    }

    void add(int value) {
        ++cnt[value];
        q.push_back(value);
    }

private:
    unordered_map<int, int> cnt;
    deque<int> q;
};

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique* obj = new FirstUnique(nums);
 * int param_1 = obj->showFirstUnique();
 * obj->add(value);
 */
```

#### Go

```go
type FirstUnique struct {
	cnt map[int]int
	q   []int
}

func Constructor(nums []int) FirstUnique {
	cnt := map[int]int{}
	for _, v := range nums {
		cnt[v]++
	}
	return FirstUnique{cnt, nums}
}

func (this *FirstUnique) ShowFirstUnique() int {
	for len(this.q) > 0 && this.cnt[this.q[0]] != 1 {
		this.q = this.q[1:]
	}
	if len(this.q) > 0 {
		return this.q[0]
	}
	return -1
}

func (this *FirstUnique) Add(value int) {
	this.cnt[value]++
	this.q = append(this.q, value)
}

/**
 * Your FirstUnique object will be instantiated and called as such:
 * obj := Constructor(nums);
 * param_1 := obj.ShowFirstUnique();
 * obj.Add(value);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 xóa khỏi ordered map ngay khi một giá trị lặp lại. Thay vào đó, ta có thể thêm các giá trị vào deque và khi truy vấn, pop các phần tử ở đầu có số lần xuất hiện không còn bằng một. Cách xóa lười này vẫn cho cùng đáp án nhưng cần ít thao tác quản lý hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class FirstUnique:
    def __init__(self, nums: List[int]):
        self.cnt = Counter(nums)
        self.q = deque(nums)

    def showFirstUnique(self) -> int:
        while self.q and self.cnt[self.q[0]] != 1:
            self.q.popleft()
        return -1 if not self.q else self.q[0]

    def add(self, value: int) -> None:
        self.cnt[value] += 1
        self.q.append(value)


# Your FirstUnique object will be instantiated and called as such:
# obj = FirstUnique(nums)
# param_1 = obj.showFirstUnique()
# obj.add(value)
```

#### Java

```java
class FirstUnique {
    private Map<Integer, Integer> cnt = new HashMap<>();
    private Deque<Integer> q = new ArrayDeque<>();

    public FirstUnique(int[] nums) {
        for (int v : nums) {
            cnt.put(v, cnt.getOrDefault(v, 0) + 1);
            q.offer(v);
        }
    }

    public int showFirstUnique() {
        while (!q.isEmpty() && cnt.get(q.peekFirst()) != 1) {
            q.poll();
        }
        return q.isEmpty() ? -1 : q.peekFirst();
    }

    public void add(int value) {
        cnt.put(value, cnt.getOrDefault(value, 0) + 1);
        q.offer(value);
    }
}

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique obj = new FirstUnique(nums);
 * int param_1 = obj.showFirstUnique();
 * obj.add(value);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
