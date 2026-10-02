---
comments: true
difficulty: Hard
tags:
    - Design
    - Hash Table
    - Linked List
    - Doubly-Linked List
---

<!-- problem:start -->

# [460. LFU Cache](https://leetcode.com/problems/lfu-cache)

[中文文档](/solution/0400-0499/0460.LFU%20Cache/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế và cài đặt cấu trúc dữ liệu cho cache <a href="https://en.wikipedia.org/wiki/Least_frequently_used" target="_blank">Least Frequently Used (LFU)</a>.</p>

<p>Hãy cài đặt class <code>LFUCache</code>:</p>

<ul>
	<li><code>LFUCache(int capacity)</code> Khởi tạo đối tượng với <code>capacity</code> của cấu trúc dữ liệu.</li>
	<li><code>int get(int key)</code> Lấy giá trị của <code>key</code> nếu <code>key</code> có trong cache. Nếu không, trả về <code>-1</code>.</li>
	<li><code>void put(int key, int value)</code> Cập nhật giá trị của <code>key</code> nếu đã tồn tại, hoặc chèn <code>key</code> nếu chưa có. Khi cache đạt <code>capacity</code>, cần vô hiệu hóa và xóa key <strong>ít được sử dụng thường xuyên nhất</strong> trước khi chèn phần tử mới. Nếu có <strong>nhiều key cùng tần suất</strong>, sẽ vô hiệu hóa <code>key</code> <strong>ít được sử dụng gần đây nhất</strong>.</li>
</ul>

<p>Để xác định key ít được sử dụng thường xuyên nhất, cache duy trì một <strong>bộ đếm số lần sử dụng</strong> cho mỗi key. Key có bộ đếm nhỏ nhất là key ít được sử dụng thường xuyên nhất.</p>

<p>Khi một key được chèn vào cache lần đầu, <strong>bộ đếm số lần sử dụng</strong> của nó được đặt thành <code>1</code> (do thao tác <code>put</code>). Bộ đếm của key trong cache tăng lên mỗi khi gọi <code>get</code> hoặc <code>put</code> trên key đó.</p>

<p>Các hàm&nbsp;<code data-stringify-type="code">get</code>&nbsp;và&nbsp;<code data-stringify-type="code">put</code>&nbsp;phải có độ phức tạp thời gian trung bình <code>O(1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;LFUCache&quot;, &quot;put&quot;, &quot;put&quot;, &quot;get&quot;, &quot;put&quot;, &quot;get&quot;, &quot;get&quot;, &quot;put&quot;, &quot;get&quot;, &quot;get&quot;, &quot;get&quot;]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
<strong>Đầu ra</strong>
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

<strong>Giải thích</strong>
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
&nbsp;                // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[4,3], cnt(4)=2, cnt(3)=3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= capacity&nbsp;&lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= key &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= value &lt;= 10<sup>9</sup></code></li>
	<li>Sẽ có tối đa <code>2 * 10<sup>5</sup></code>&nbsp;lần gọi <code>get</code> và <code>put</code>.</li>
</ul>

<p>&nbsp;</p>
<span style="display: none;">&nbsp;</span>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> LFU cần hỗ trợ get và put trong $O(1)$; khi cache đầy, phải loại key có tần suất thấp nhất, nếu hòa thì loại key ít được dùng gần đây nhất. Một hash map có thể tìm giá trị nhưng không thể đồng thời theo dõi tần suất và độ gần đây.
>
> Map từ key đến node hỗ trợ tra cứu. Map từ tần suất đến list lưu các node có cùng tần suất, với node mới dùng nhất ở đầu. $\textit{minFreq}$ trỏ đến list chứa key cần loại bỏ, tại phần tử cuối list.
>
> Khi truy cập key, chuyển node sang list có tần suất $\textit{freq}+1$; nếu list cũ rỗng và có tần suất nhỏ nhất thì tăng $\textit{minFreq}$. Key mới có tần suất $1$ và đặt lại giá trị nhỏ nhất thành $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Node:
    def __init__(self, key: int, value: int) -> None:
        self.key = key
        self.value = value
        self.freq = 1
        self.prev = None
        self.next = None


class DoublyLinkedList:
    def __init__(self) -> None:
        self.head = Node(-1, -1)
        self.tail = Node(-1, -1)
        self.head.next = self.tail
        self.tail.prev = self.head

    def add_first(self, node: Node) -> None:
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node

    def remove(self, node: Node) -> Node:
        node.next.prev = node.prev
        node.prev.next = node.next
        node.next, node.prev = None, None
        return node

    def remove_last(self) -> Node:
        return self.remove(self.tail.prev)

    def is_empty(self) -> bool:
        return self.head.next == self.tail


class LFUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.min_freq = 0
        self.map = defaultdict(Node)
        self.freq_map = defaultdict(DoublyLinkedList)

    def get(self, key: int) -> int:
        if self.capacity == 0 or key not in self.map:
            return -1
        node = self.map[key]
        self.incr_freq(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if self.capacity == 0:
            return
        if key in self.map:
            node = self.map[key]
            node.value = value
            self.incr_freq(node)
            return
        if len(self.map) == self.capacity:
            ls = self.freq_map[self.min_freq]
            node = ls.remove_last()
            self.map.pop(node.key)
        node = Node(key, value)
        self.add_node(node)
        self.map[key] = node
        self.min_freq = 1

    def incr_freq(self, node: Node) -> None:
        freq = node.freq
        ls = self.freq_map[freq]
        ls.remove(node)
        if ls.is_empty():
            self.freq_map.pop(freq)
            if freq == self.min_freq:
                self.min_freq += 1
        node.freq += 1
        self.add_node(node)

    def add_node(self, node: Node) -> None:
        freq = node.freq
        ls = self.freq_map[freq]
        ls.add_first(node)
        self.freq_map[freq] = ls


# Your LFUCache object will be instantiated and called as such:
# obj = LFUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```

#### Java

```java
class LFUCache {

    private final Map<Integer, Node> map;
    private final Map<Integer, DoublyLinkedList> freqMap;
    private final int capacity;
    private int minFreq;

    public LFUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>(capacity, 1);
        freqMap = new HashMap<>();
    }

    public int get(int key) {
        if (capacity == 0) {
            return -1;
        }
        if (!map.containsKey(key)) {
            return -1;
        }
        Node node = map.get(key);
        incrFreq(node);
        return node.value;
    }

    public void put(int key, int value) {
        if (capacity == 0) {
            return;
        }
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.value = value;
            incrFreq(node);
            return;
        }
        if (map.size() == capacity) {
            DoublyLinkedList list = freqMap.get(minFreq);
            map.remove(list.removeLast().key);
        }
        Node node = new Node(key, value);
        addNode(node);
        map.put(key, node);
        minFreq = 1;
    }

    private void incrFreq(Node node) {
        int freq = node.freq;
        DoublyLinkedList list = freqMap.get(freq);
        list.remove(node);
        if (list.isEmpty()) {
            freqMap.remove(freq);
            if (freq == minFreq) {
                minFreq++;
            }
        }
        node.freq++;
        addNode(node);
    }

    private void addNode(Node node) {
        int freq = node.freq;
        DoublyLinkedList list = freqMap.getOrDefault(freq, new DoublyLinkedList());
        list.addFirst(node);
        freqMap.put(freq, list);
    }

    private static class Node {
        int key;
        int value;
        int freq;
        Node prev;
        Node next;

        Node(int key, int value) {
            this.key = key;
            this.value = value;
            this.freq = 1;
        }
    }

    private static class DoublyLinkedList {

        private final Node head;
        private final Node tail;

        public DoublyLinkedList() {
            head = new Node(-1, -1);
            tail = new Node(-1, -1);
            head.next = tail;
            tail.prev = head;
        }

        public void addFirst(Node node) {
            node.prev = head;
            node.next = head.next;
            head.next.prev = node;
            head.next = node;
        }

        public Node remove(Node node) {
            node.next.prev = node.prev;
            node.prev.next = node.next;
            node.next = null;
            node.prev = null;
            return node;
        }

        public Node removeLast() {
            return remove(tail.prev);
        }

        public boolean isEmpty() {
            return head.next == tail;
        }
    }
}
```

#### C++

```cpp
class Node {
public:
    int key;
    int value;
    int freq;
    Node* prev;
    Node* next;
    Node(int key, int value) {
        this->key = key;
        this->value = value;
        this->freq = 1;
        this->prev = nullptr;
        this->next = nullptr;
    }
};

class DoublyLinkedList {
public:
    Node* head;
    Node* tail;
    DoublyLinkedList() {
        this->head = new Node(-1, -1);
        this->tail = new Node(-1, -1);
        this->head->next = this->tail;
        this->tail->prev = this->head;
    }
    void addFirst(Node* node) {
        node->prev = this->head;
        node->next = this->head->next;
        this->head->next->prev = node;
        this->head->next = node;
    }
    Node* remove(Node* node) {
        node->next->prev = node->prev;
        node->prev->next = node->next;
        node->next = nullptr;
        node->prev = nullptr;
        return node;
    }
    Node* removeLast() {
        return remove(this->tail->prev);
    }
    bool isEmpty() {
        return this->head->next == this->tail;
    }
};

class LFUCache {
public:
    LFUCache(int capacity) {
        this->capacity = capacity;
        this->minFreq = 0;
    }

    int get(int key) {
        if (capacity == 0 || map.find(key) == map.end()) {
            return -1;
        }
        Node* node = map[key];
        incrFreq(node);
        return node->value;
    }

    void put(int key, int value) {
        if (capacity == 0) {
            return;
        }
        if (map.find(key) != map.end()) {
            Node* node = map[key];
            node->value = value;
            incrFreq(node);
            return;
        }
        if (map.size() == capacity) {
            DoublyLinkedList* list = freqMap[minFreq];
            Node* node = list->removeLast();
            map.erase(node->key);
        }
        Node* node = new Node(key, value);
        addNode(node);
        map[key] = node;
        minFreq = 1;
    }

private:
    int capacity;
    int minFreq;
    unordered_map<int, Node*> map;
    unordered_map<int, DoublyLinkedList*> freqMap;

    void incrFreq(Node* node) {
        int freq = node->freq;
        DoublyLinkedList* list = freqMap[freq];
        list->remove(node);
        if (list->isEmpty()) {
            freqMap.erase(freq);
            if (freq == minFreq) {
                minFreq++;
            }
        }
        node->freq++;
        addNode(node);
    }

    void addNode(Node* node) {
        int freq = node->freq;
        if (freqMap.find(freq) == freqMap.end()) {
            freqMap[freq] = new DoublyLinkedList();
        }
        DoublyLinkedList* list = freqMap[freq];
        list->addFirst(node);
        freqMap[freq] = list;
    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

#### Go

```go
type LFUCache struct {
	cache    map[int]*node
	freqMap  map[int]*list
	minFreq  int
	capacity int
}

func Constructor(capacity int) LFUCache {
	return LFUCache{
		cache:    make(map[int]*node),
		freqMap:  make(map[int]*list),
		capacity: capacity,
	}
}

func (this *LFUCache) Get(key int) int {
	if this.capacity == 0 {
		return -1
	}

	n, ok := this.cache[key]
	if !ok {
		return -1
	}

	this.incrFreq(n)
	return n.val
}

func (this *LFUCache) Put(key int, value int) {
	if this.capacity == 0 {
		return
	}

	n, ok := this.cache[key]
	if ok {
		n.val = value
		this.incrFreq(n)
		return
	}

	if len(this.cache) == this.capacity {
		l := this.freqMap[this.minFreq]
		delete(this.cache, l.removeBack().key)
	}
	n = &node{key: key, val: value, freq: 1}
	this.addNode(n)
	this.cache[key] = n
	this.minFreq = 1
}

func (this *LFUCache) incrFreq(n *node) {
	l := this.freqMap[n.freq]
	l.remove(n)
	if l.empty() {
		delete(this.freqMap, n.freq)
		if n.freq == this.minFreq {
			this.minFreq++
		}
	}
	n.freq++
	this.addNode(n)
}

func (this *LFUCache) addNode(n *node) {
	l, ok := this.freqMap[n.freq]
	if !ok {
		l = newList()
		this.freqMap[n.freq] = l
	}
	l.pushFront(n)
}

type node struct {
	key  int
	val  int
	freq int
	prev *node
	next *node
}

type list struct {
	head *node
	tail *node
}

func newList() *list {
	head := new(node)
	tail := new(node)
	head.next = tail
	tail.prev = head
	return &list{
		head: head,
		tail: tail,
	}
}

func (l *list) pushFront(n *node) {
	n.prev = l.head
	n.next = l.head.next
	l.head.next.prev = n
	l.head.next = n
}

func (l *list) remove(n *node) {
	n.prev.next = n.next
	n.next.prev = n.prev
	n.next = nil
	n.prev = nil
}

func (l *list) removeBack() *node {
	n := l.tail.prev
	l.remove(n)
	return n
}

func (l *list) empty() bool {
	return l.head.next == l.tail
}
```

#### Rust

```rust
use std::cell::RefCell;
use std::collections::HashMap;
use std::rc::Rc;

struct Node {
    key: i32,
    value: i32,
    freq: i32,
    prev: Option<Rc<RefCell<Node>>>,
    next: Option<Rc<RefCell<Node>>>,
}

impl Node {
    fn new(key: i32, value: i32) -> Self {
        Self {
            key,
            value,
            freq: 1,
            prev: None,
            next: None,
        }
    }
}

struct LinkedList {
    head: Option<Rc<RefCell<Node>>>,
    tail: Option<Rc<RefCell<Node>>>,
}

impl LinkedList {
    fn new() -> Self {
        Self {
            head: None,
            tail: None,
        }
    }

    fn push_front(&mut self, node: &Rc<RefCell<Node>>) {
        match self.head.take() {
            Some(head) => {
                head.borrow_mut().prev = Some(Rc::clone(node));
                node.borrow_mut().prev = None;
                node.borrow_mut().next = Some(head);
                self.head = Some(Rc::clone(node));
            }
            None => {
                node.borrow_mut().prev = None;
                node.borrow_mut().next = None;
                self.head = Some(Rc::clone(node));
                self.tail = Some(Rc::clone(node));
            }
        };
    }

    fn remove(&mut self, node: &Rc<RefCell<Node>>) {
        match (node.borrow().prev.as_ref(), node.borrow().next.as_ref()) {
            (None, None) => {
                self.head = None;
                self.tail = None;
            }
            (None, Some(next)) => {
                self.head = Some(Rc::clone(next));
                next.borrow_mut().prev = None;
            }
            (Some(prev), None) => {
                self.tail = Some(Rc::clone(prev));
                prev.borrow_mut().next = None;
            }
            (Some(prev), Some(next)) => {
                next.borrow_mut().prev = Some(Rc::clone(prev));
                prev.borrow_mut().next = Some(Rc::clone(next));
            }
        };
    }

    fn pop_back(&mut self) -> Option<Rc<RefCell<Node>>> {
        match self.tail.take() {
            Some(tail) => {
                self.remove(&tail);
                Some(tail)
            }
            None => None,
        }
    }

    fn is_empty(&self) -> bool {
        self.head.is_none()
    }
}

struct LFUCache {
    cache: HashMap<i32, Rc<RefCell<Node>>>,
    freq_map: HashMap<i32, LinkedList>,
    min_freq: i32,
    capacity: usize,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl LFUCache {
    fn new(capacity: i32) -> Self {
        Self {
            cache: HashMap::new(),
            freq_map: HashMap::new(),
            min_freq: 0,
            capacity: capacity as usize,
        }
    }

    fn get(&mut self, key: i32) -> i32 {
        if self.capacity == 0 {
            return -1;
        }

        match self.cache.get(&key) {
            Some(node) => {
                let node = Rc::clone(node);
                self.incr_freq(&node);
                let value = node.borrow().value;
                value
            }
            None => -1,
        }
    }

    fn put(&mut self, key: i32, value: i32) {
        if self.capacity == 0 {
            return;
        }

        match self.cache.get(&key) {
            Some(node) => {
                let node = Rc::clone(node);
                node.borrow_mut().value = value;
                self.incr_freq(&node);
            }
            None => {
                if self.cache.len() == self.capacity {
                    let list = self.freq_map.get_mut(&self.min_freq).unwrap();
                    self.cache.remove(&list.pop_back().unwrap().borrow().key);
                }
                let node = Rc::new(RefCell::new(Node::new(key, value)));
                self.add_node(&node);
                self.cache.insert(key, node);
                self.min_freq = 1;
            }
        };
    }

    fn incr_freq(&mut self, node: &Rc<RefCell<Node>>) {
        let freq = node.borrow().freq;
        let list = self.freq_map.get_mut(&freq).unwrap();
        list.remove(node);
        if list.is_empty() {
            self.freq_map.remove(&freq);
            if freq == self.min_freq {
                self.min_freq += 1;
            }
        }
        node.borrow_mut().freq += 1;
        self.add_node(node);
    }

    fn add_node(&mut self, node: &Rc<RefCell<Node>>) {
        let freq = node.borrow().freq;
        match self.freq_map.get_mut(&freq) {
            Some(list) => {
                list.push_front(node);
            }
            None => {
                let mut list = LinkedList::new();
                list.push_front(node);
                self.freq_map.insert(node.borrow().freq, list);
            }
        };
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
