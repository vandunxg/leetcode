---
comments: true
difficulty: Easy
tags:
    - Design
    - Array
    - Hash Table
    - Linked List
    - Hash Function
---

<!-- problem:start -->

# [705. Design HashSet](https://leetcode.com/problems/design-hashset)

[中文文档](/solution/0700-0799/0705.Design%20HashSet/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế HashSet mà không dùng thư viện hash table có sẵn.</p>

<p>Hãy triển khai class <code>MyHashSet</code>:</p>

<ul>
	<li><code>void add(key)</code> Chèn giá trị <code>key</code> vào HashSet.</li>
	<li><code>bool contains(key)</code> Trả về liệu giá trị <code>key</code> có tồn tại trong HashSet hay không.</li>
	<li><code>void remove(key)</code> Xóa giá trị <code>key</code> khỏi HashSet. Nếu <code>key</code> không tồn tại, không làm gì.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MyHashSet&quot;, &quot;add&quot;, &quot;add&quot;, &quot;contains&quot;, &quot;contains&quot;, &quot;add&quot;, &quot;contains&quot;, &quot;remove&quot;, &quot;contains&quot;]
[[], [1], [2], [1], [3], [2], [2], [2], [2]]
<strong>Đầu ra</strong>
[null, null, null, true, false, null, true, null, false]

<strong>Giải thích</strong>
MyHashSet myHashSet = new MyHashSet();
myHashSet.add(1);      // set = [1]
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(1); // return True
myHashSet.contains(3); // return False, (not found)
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(2); // return True
myHashSet.remove(2);   // set = [1]
myHashSet.contains(2); // return False, (already removed)</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= key &lt;= 10<sup>6</sup></code></li>
	<li>Sẽ có tối đa <code>10<sup>4</sup></code> lần gọi <code>add</code>, <code>remove</code> và <code>contains</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Triển khai bằng mảng tĩnh

<!-- thinking:start -->

> **Tư duy**
>
> Cần triển khai set chứa các key trong $[0, 10^6]$ với tối đa $10^4$ thao tác. Dùng map có sẵn cũng được, nhưng mục tiêu ở đây là tự thiết kế cấu trúc lưu trữ.
>
> Key vốn là số nguyên không âm có giới hạn cố định nên có thể dùng làm chỉ số. Mảng boolean độ dài $10^6+1$ giúp mỗi thao tác add, remove và contains chỉ cần một lần ghi hoặc đọc.
>
> Bộ nhớ sử dụng phụ thuộc vào miền giá trị thay vì số key; với giới hạn này thì vẫn chấp nhận được.

<!-- thinking:end -->

Tạo trực tiếp một mảng kích thước $1000001$, ban đầu mọi phần tử đều là `false`, biểu thị giá trị đó chưa có trong hash set.

Khi thêm phần tử vào hash set, đặt vị trí tương ứng trong mảng thành `true`; khi xóa phần tử, đặt vị trí đó thành `false`; khi kiểm tra phần tử có tồn tại không, trả về trực tiếp giá trị tại vị trí tương ứng trong mảng.

Các thao tác trên có độ phức tạp thời gian $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class MyHashSet:
    def __init__(self):
        self.data = [False] * 1000001

    def add(self, key: int) -> None:
        self.data[key] = True

    def remove(self, key: int) -> None:
        self.data[key] = False

    def contains(self, key: int) -> bool:
        return self.data[key]


# Your MyHashSet object will be instantiated and called as such:
# obj = MyHashSet()
# obj.add(key)
# obj.remove(key)
# param_3 = obj.contains(key)
```

#### Java

```java
class MyHashSet {
    private boolean[] data = new boolean[1000001];

    public MyHashSet() {
    }

    public void add(int key) {
        data[key] = true;
    }

    public void remove(int key) {
        data[key] = false;
    }

    public boolean contains(int key) {
        return data[key];
    }
}

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet obj = new MyHashSet();
 * obj.add(key);
 * obj.remove(key);
 * boolean param_3 = obj.contains(key);
 */
```

#### C++

```cpp
class MyHashSet {
public:
    bool data[1000001];

    MyHashSet() {
        memset(data, false, sizeof data);
    }

    void add(int key) {
        data[key] = true;
    }

    void remove(int key) {
        data[key] = false;
    }

    bool contains(int key) {
        return data[key];
    }
};

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */
```

#### Go

```go
type MyHashSet struct {
	data []bool
}

func Constructor() MyHashSet {
	data := make([]bool, 1000010)
	return MyHashSet{data}
}

func (this *MyHashSet) Add(key int) {
	this.data[key] = true
}

func (this *MyHashSet) Remove(key int) {
	this.data[key] = false
}

func (this *MyHashSet) Contains(key int) bool {
	return this.data[key]
}

/**
 * Your MyHashSet object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Add(key);
 * obj.Remove(key);
 * param_3 := obj.Contains(key);
 */
```

#### TypeScript

```ts
class MyHashSet {
    data: Array<boolean>;
    constructor() {
        this.data = new Array(10 ** 6 + 1).fill(false);
    }

    add(key: number): void {
        this.data[key] = true;
    }

    remove(key: number): void {
        this.data[key] = false;
    }

    contains(key: number): boolean {
        return this.data[key];
    }
}

/**
 * Your MyHashSet object will be instantiated and called as such:
 * var obj = new MyHashSet()
 * obj.add(key)
 * obj.remove(key)
 * var param_3 = obj.contains(key)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mảng các linked list

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đạt $O(1)$ bằng cách dùng mảng có kích thước bằng toàn bộ miền key; nếu chỉ có ít key thì phần lớn bộ nhớ sẽ không được dùng.
>
> Dùng modulo nhỏ hơn để hash key vào số bucket cố định, rồi lưu các key va chạm trong một list. Mỗi thao tác tính hash rồi duyệt một bucket ngắn.
>
> Với $\textit{SIZE}=1000$, bộ nhớ phụ thuộc vào số bucket và chi phí kỳ vọng vẫn gần hằng số.

<!-- thinking:end -->

Ta cũng có thể tạo mảng kích thước $SIZE=1000$, trong đó mỗi vị trí chứa một linked list.

<!-- tabs:start -->

#### Python3

```python
class MyHashSet:
    def __init__(self):
        self.size = 1000
        self.data = [[] for _ in range(self.size)]

    def add(self, key: int) -> None:
        if self.contains(key):
            return
        idx = self.hash(key)
        self.data[idx].append(key)

    def remove(self, key: int) -> None:
        if not self.contains(key):
            return
        idx = self.hash(key)
        self.data[idx].remove(key)

    def contains(self, key: int) -> bool:
        idx = self.hash(key)
        return any(v == key for v in self.data[idx])

    def hash(self, key) -> int:
        return key % self.size


# Your MyHashSet object will be instantiated and called as such:
# obj = MyHashSet()
# obj.add(key)
# obj.remove(key)
# param_3 = obj.contains(key)
```

#### Java

```java
class MyHashSet {
    private static final int SIZE = 1000;
    private LinkedList[] data;

    public MyHashSet() {
        data = new LinkedList[SIZE];
        for (int i = 0; i < SIZE; ++i) {
            data[i] = new LinkedList<Integer>();
        }
    }

    public void add(int key) {
        if (contains(key)) {
            return;
        }
        int idx = hash(key);
        data[idx].addFirst(key);
    }

    public void remove(int key) {
        if (!contains(key)) {
            return;
        }
        int idx = hash(key);
        data[idx].remove(Integer.valueOf(key));
    }

    public boolean contains(int key) {
        int idx = hash(key);
        Iterator<Integer> it = data[idx].iterator();
        while (it.hasNext()) {
            Integer e = it.next();
            if (e == key) {
                return true;
            }
        }
        return false;
    }

    private int hash(int key) {
        return key % SIZE;
    }
}

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet obj = new MyHashSet();
 * obj.add(key);
 * obj.remove(key);
 * boolean param_3 = obj.contains(key);
 */
```

#### C++

```cpp
class MyHashSet {
private:
    int size = 1000;
    vector<list<int>> data;

public:
    MyHashSet()
        : data(size) {
    }

    void add(int key) {
        if (contains(key)) {
            return;
        }
        int idx = hash(key);
        data[idx].push_back(key);
    }

    void remove(int key) {
        if (!contains(key)) {
            return;
        }
        int idx = hash(key);
        data[idx].remove(key);
    }

    bool contains(int key) {
        int idx = hash(key);
        for (auto it = data[idx].begin(); it != data[idx].end(); it++) {
            if ((*it) == key) {
                return true;
            }
        }
        return false;
    }

    int hash(int key) {
        return key % size;
    }
};

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */
```

#### Go

```go
type MyHashSet struct {
	data []list.List
}

func Constructor() MyHashSet {
	return MyHashSet{make([]list.List, 1000)}
}

func (this *MyHashSet) Add(key int) {
	if this.Contains(key) {
		return
	}
	idx := this.hash(key)
	this.data[idx].PushBack(key)
}

func (this *MyHashSet) Remove(key int) {
	idx := this.hash(key)
	for e := this.data[idx].Front(); e != nil; e = e.Next() {
		if e.Value.(int) == key {
			this.data[idx].Remove(e)
		}
	}
}

func (this *MyHashSet) Contains(key int) bool {
	idx := this.hash(key)
	for e := this.data[idx].Front(); e != nil; e = e.Next() {
		if e.Value.(int) == key {
			return true
		}
	}
	return false
}

func (this *MyHashSet) hash(key int) int {
	return key % len(this.data)
}

/**
 * Your MyHashSet object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Add(key);
 * obj.Remove(key);
 * param_3 := obj.Contains(key);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
