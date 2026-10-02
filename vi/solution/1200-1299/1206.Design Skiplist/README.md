---
comments: true
difficulty: Hard
tags:
    - Design
    - Linked List
---

<!-- problem:start -->

# [1206. Design Skiplist](https://leetcode.com/problems/design-skiplist)

[中文文档](/solution/1200-1299/1206.Design%20Skiplist/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế một <strong>Skiplist</strong> mà không dùng thư viện dựng sẵn.</p>

<p><strong>Skiplist</strong> là cấu trúc dữ liệu hỗ trợ thêm, xóa và tìm kiếm trong thời gian <code>O(log(n))</code>. So với treap và red-black tree có cùng chức năng và hiệu năng, code của Skiplist tương đối ngắn; ý tưởng nền tảng chỉ là các linked list đơn giản.</p>

<p>Ví dụ, ta có Skiplist chứa <code>[30,40,50,60,70,90]</code> và muốn thêm <code>80</code> cùng <code>45</code>. Skiplist hoạt động như sau:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1206.Design%20Skiplist/images/1506_skiplist.gif" style="width: 500px; height: 173px;" /><br />
<small>Artyom Kalinin [CC BY-SA 3.0], nguồn: <a href="https://commons.wikimedia.org/wiki/File:Skip_list_add_element-en.gif" target="_blank" title="Artyom Kalinin [CC BY-SA 3.0 (https://creativecommons.org/licenses/by-sa/3.0)], via Wikimedia Commons">Wikimedia Commons</a></small></p>

<p>Có thể thấy Skiplist gồm nhiều tầng. Mỗi tầng là một linked list đã sắp xếp. Nhờ các tầng phía trên, thao tác thêm, xóa và tìm kiếm có thể nhanh hơn <code>O(n)</code>. Có thể chứng minh độ phức tạp thời gian trung bình của mỗi thao tác là <code>O(log(n))</code>, còn độ phức tạp không gian là <code>O(n)</code>.</p>

<p>Tìm hiểu thêm về Skiplist: <a href="https://en.wikipedia.org/wiki/Skip_list" target="_blank">https://en.wikipedia.org/wiki/Skip_list</a></p>

<p>Hãy triển khai class <code>Skiplist</code>:</p>

<ul>
	<li><code>Skiplist()</code> Khởi tạo đối tượng skiplist.</li>
	<li><code>bool search(int target)</code> Trả về <code>true</code> nếu số nguyên <code>target</code> có trong Skiplist, nếu không thì trả về <code>false</code>.</li>
	<li><code>void add(int num)</code> Chèn giá trị <code>num</code> vào SkipList.</li>
	<li><code>bool erase(int num)</code> Xóa giá trị <code>num</code> khỏi Skiplist và trả về <code>true</code>. Nếu <code>num</code> không tồn tại, không làm gì và trả về <code>false</code>. Nếu có nhiều giá trị <code>num</code>, xóa giá trị nào cũng được.</li>
</ul>

<p>Lưu ý rằng Skiplist có thể chứa giá trị trùng lặp; code của bạn cần xử lý trường hợp này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Skiplist&quot;, &quot;add&quot;, &quot;add&quot;, &quot;add&quot;, &quot;search&quot;, &quot;add&quot;, &quot;search&quot;, &quot;erase&quot;, &quot;erase&quot;, &quot;search&quot;]
[[], [1], [2], [3], [0], [4], [1], [0], [1], [1]]
<strong>Đầu ra</strong>
[null, null, null, null, false, null, true, false, true, false]

<strong>Giải thích</strong>
Skiplist skiplist = new Skiplist();
skiplist.add(1);
skiplist.add(2);
skiplist.add(3);
skiplist.search(0); // return False
skiplist.add(4);
skiplist.search(1); // return True
skiplist.erase(0);  // return False, 0 is not in skiplist.
skiplist.erase(1);  // return True
skiplist.search(1); // return False, 1 has already been erased.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num, target &lt;= 2 * 10<sup>4</sup></code></li>
	<li>Sẽ có nhiều nhất <code>5 * 10<sup>4</sup></code> lần gọi các phương thức <code>search</code>, <code>add</code> và <code>erase</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cấu trúc dữ liệu

<!-- thinking:start -->

> **Tư duy**
>
> Với tối đa $5\times 10^4$ thao tác, chèn/xóa tuyến tính trên mảng đã sắp xếp sẽ quá chậm. Cây cân bằng có độ phức tạp $O(\log n)$ nhưng triển khai khá phức tạp. Skiplist mô phỏng các tầng index: tầng dưới cùng chứa mọi key, các tầng trên lấy mẫu thưa hơn.
>
> Tìm kiếm, chèn và xóa bắt đầu từ tầng cao nhất, đi xuống node gần nhất không vượt quá target rồi hạ một tầng. Số bước nhảy kỳ vọng là logarithmic. Chiều cao của node mới tăng với xác suất $p$, nhờ đó các tầng trên vẫn thưa.
>
> Ta dùng danh sách nhiều tầng với một head giả. $find\_closest$ di chuyển trong một tầng; $random\_level$ chọn chiều cao mới. Mỗi thao tác có độ phức tạp kỳ vọng $O(\log n)$, còn không gian tỷ lệ với số node và chiều cao.

<!-- thinking:end -->

Ý tưởng cốt lõi của skiplist là dùng nhiều “tầng” để lưu dữ liệu, trong đó mỗi tầng đóng vai trò như một index. Dữ liệu bắt đầu ở linked list tầng dưới cùng rồi dần xuất hiện ở các tầng cao hơn, tạo thành cấu trúc linked list nhiều tầng. Mỗi tầng chỉ chứa một phần dữ liệu, nhờ đó có thể nhảy qua các node để giảm thời gian tìm kiếm.

Trong bài này, ta dùng class $\textit{Node}$ để biểu diễn các node của skiplist. Mỗi node có trường $\textit{val}$ và mảng $\textit{next}$. Độ dài mảng là $\textit{level}$, cho biết node kế tiếp ở mỗi tầng. Ta dùng class $\textit{Skiplist}$ để triển khai các thao tác của skiplist.

Skiplist gồm node đầu $\textit{head}$ và số tầng tối đa hiện tại $\textit{level}$. Giá trị của node đầu được đặt là $-1$ để đánh dấu vị trí bắt đầu danh sách. Ta dùng mảng động $\textit{next}$ để lưu pointer đến các node kế tiếp.

Với thao tác $\textit{search}$, ta bắt đầu từ tầng cao nhất của skiplist rồi lần lượt đi xuống cho đến khi tìm được node đích hoặc xác định node đó không tồn tại. Ở mỗi tầng, dùng phương thức $\textit{find\_closest}$ để nhảy đến node gần target nhất.

Với thao tác $\textit{add}$, trước tiên ta chọn ngẫu nhiên số tầng của node mới. Sau đó, bắt đầu từ tầng cao nhất, tìm node gần giá trị mới nhất ở mỗi tầng và chèn node mới vào vị trí phù hợp. Nếu node được chèn có số tầng lớn hơn số tầng tối đa hiện tại của skiplist, cần cập nhật số tầng của skiplist.

Với thao tác $\textit{erase}$, tương tự thao tác tìm kiếm, ta duyệt từng tầng để tìm và xóa node đích. Khi xóa node, cần cập nhật các pointer $\textit{next}$ ở từng tầng. Nếu tầng cao nhất không còn node nào, ta giảm số tầng của skiplist.

Ngoài ra, ta định nghĩa phương thức $\textit{random\_level}$ để chọn ngẫu nhiên số tầng cho node mới. Phương thức này sinh số ngẫu nhiên trong khoảng $[1, \textit{max\_level}]$ cho đến khi số được tạo lớn hơn hoặc bằng $\textit{p}$. Ta cũng có phương thức $\textit{find\_closest}$ để tìm node gần target nhất ở mỗi tầng.

Độ phức tạp thời gian của các thao tác trên là $O(\log n)$, trong đó $n$ là số node trong skiplist. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = ['val', 'next']

    def __init__(self, val: int, level: int):
        self.val = val
        self.next = [None] * level


class Skiplist:
    max_level = 32
    p = 0.25

    def __init__(self):
        self.head = Node(-1, self.max_level)
        self.level = 0

    def search(self, target: int) -> bool:
        curr = self.head
        for i in range(self.level - 1, -1, -1):
            curr = self.find_closest(curr, i, target)
            if curr.next[i] and curr.next[i].val == target:
                return True
        return False

    def add(self, num: int) -> None:
        curr = self.head
        level = self.random_level()
        node = Node(num, level)
        self.level = max(self.level, level)
        for i in range(self.level - 1, -1, -1):
            curr = self.find_closest(curr, i, num)
            if i < level:
                node.next[i] = curr.next[i]
                curr.next[i] = node

    def erase(self, num: int) -> bool:
        curr = self.head
        ok = False
        for i in range(self.level - 1, -1, -1):
            curr = self.find_closest(curr, i, num)
            if curr.next[i] and curr.next[i].val == num:
                curr.next[i] = curr.next[i].next[i]
                ok = True
        while self.level > 1 and self.head.next[self.level - 1] is None:
            self.level -= 1
        return ok

    def find_closest(self, curr: Node, level: int, target: int) -> Node:
        while curr.next[level] and curr.next[level].val < target:
            curr = curr.next[level]
        return curr

    def random_level(self) -> int:
        level = 1
        while level < self.max_level and random.random() < self.p:
            level += 1
        return level


# Your Skiplist object will be instantiated and called as such:
# obj = Skiplist()
# param_1 = obj.search(target)
# obj.add(num)
# param_3 = obj.erase(num)
```

#### Java

```java
class Skiplist {
    private static final int MAX_LEVEL = 32;
    private static final double P = 0.25;
    private static final Random RANDOM = new Random();
    private final Node head = new Node(-1, MAX_LEVEL);
    private int level = 0;

    public Skiplist() {
    }

    public boolean search(int target) {
        Node curr = head;
        for (int i = level - 1; i >= 0; --i) {
            curr = findClosest(curr, i, target);
            if (curr.next[i] != null && curr.next[i].val == target) {
                return true;
            }
        }
        return false;
    }

    public void add(int num) {
        Node curr = head;
        int lv = randomLevel();
        Node node = new Node(num, lv);
        level = Math.max(level, lv);
        for (int i = level - 1; i >= 0; --i) {
            curr = findClosest(curr, i, num);
            if (i < lv) {
                node.next[i] = curr.next[i];
                curr.next[i] = node;
            }
        }
    }

    public boolean erase(int num) {
        Node curr = head;
        boolean ok = false;
        for (int i = level - 1; i >= 0; --i) {
            curr = findClosest(curr, i, num);
            if (curr.next[i] != null && curr.next[i].val == num) {
                curr.next[i] = curr.next[i].next[i];
                ok = true;
            }
        }
        while (level > 1 && head.next[level - 1] == null) {
            --level;
        }
        return ok;
    }

    private Node findClosest(Node curr, int level, int target) {
        while (curr.next[level] != null && curr.next[level].val < target) {
            curr = curr.next[level];
        }
        return curr;
    }

    private static int randomLevel() {
        int level = 1;
        while (level < MAX_LEVEL && RANDOM.nextDouble() < P) {
            ++level;
        }
        return level;
    }

    static class Node {
        int val;
        Node[] next;

        Node(int val, int level) {
            this.val = val;
            next = new Node[level];
        }
    }
}

/**
 * Your Skiplist object will be instantiated and called as such:
 * Skiplist obj = new Skiplist();
 * boolean param_1 = obj.search(target);
 * obj.add(num);
 * boolean param_3 = obj.erase(num);
 */
```

#### C++

```cpp
struct Node {
    int val;
    vector<Node*> next;
    Node(int v, int level)
        : val(v)
        , next(level, nullptr) {}
};

class Skiplist {
public:
    const int p = RAND_MAX / 4;
    const int maxLevel = 32;
    Node* head;
    int level;

    Skiplist() {
        head = new Node(-1, maxLevel);
        level = 0;
    }

    bool search(int target) {
        Node* curr = head;
        for (int i = level - 1; ~i; --i) {
            curr = findClosest(curr, i, target);
            if (curr->next[i] && curr->next[i]->val == target) return true;
        }
        return false;
    }

    void add(int num) {
        Node* curr = head;
        int lv = randomLevel();
        Node* node = new Node(num, lv);
        level = max(level, lv);
        for (int i = level - 1; ~i; --i) {
            curr = findClosest(curr, i, num);
            if (i < lv) {
                node->next[i] = curr->next[i];
                curr->next[i] = node;
            }
        }
    }

    bool erase(int num) {
        Node* curr = head;
        bool ok = false;
        for (int i = level - 1; ~i; --i) {
            curr = findClosest(curr, i, num);
            if (curr->next[i] && curr->next[i]->val == num) {
                curr->next[i] = curr->next[i]->next[i];
                ok = true;
            }
        }
        while (level > 1 && !head->next[level - 1]) --level;
        return ok;
    }

    Node* findClosest(Node* curr, int level, int target) {
        while (curr->next[level] && curr->next[level]->val < target) curr = curr->next[level];
        return curr;
    }

    int randomLevel() {
        int lv = 1;
        while (lv < maxLevel && rand() < p) ++lv;
        return lv;
    }
};

/**
 * Your Skiplist object will be instantiated and called as such:
 * Skiplist* obj = new Skiplist();
 * bool param_1 = obj->search(target);
 * obj->add(num);
 * bool param_3 = obj->erase(num);
 */
```

#### Go

```go
func init() { rand.Seed(time.Now().UnixNano()) }

const (
	maxLevel = 16
	p        = 0.5
)

type node struct {
	val  int
	next []*node
}

func newNode(val, level int) *node {
	return &node{
		val:  val,
		next: make([]*node, level),
	}
}

type Skiplist struct {
	head  *node
	level int
}

func Constructor() Skiplist {
	return Skiplist{
		head:  newNode(-1, maxLevel),
		level: 1,
	}
}

func (this *Skiplist) Search(target int) bool {
	p := this.head
	for i := this.level - 1; i >= 0; i-- {
		p = findClosest(p, i, target)
		if p.next[i] != nil && p.next[i].val == target {
			return true
		}
	}
	return false
}

func (this *Skiplist) Add(num int) {
	level := randomLevel()
	if level > this.level {
		this.level = level
	}
	node := newNode(num, level)
	p := this.head
	for i := this.level - 1; i >= 0; i-- {
		p = findClosest(p, i, num)
		if i < level {
			node.next[i] = p.next[i]
			p.next[i] = node
		}
	}
}

func (this *Skiplist) Erase(num int) bool {
	ok := false
	p := this.head
	for i := this.level - 1; i >= 0; i-- {
		p = findClosest(p, i, num)
		if p.next[i] != nil && p.next[i].val == num {
			p.next[i] = p.next[i].next[i]
			ok = true
		}
	}
	for this.level > 1 && this.head.next[this.level-1] == nil {
		this.level--
	}
	return ok
}

func findClosest(p *node, level, target int) *node {
	for p.next[level] != nil && p.next[level].val < target {
		p = p.next[level]
	}
	return p
}

func randomLevel() int {
	level := 1
	for level < maxLevel && rand.Float64() < p {
		level++
	}
	return level
}

/**
 * Your Skiplist object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Search(target);
 * obj.Add(num);
 * param_3 := obj.Erase(num);
 */
```

#### TypeScript

```ts
class Node {
    val: number;
    next: (Node | null)[];

    constructor(val: number, level: number) {
        this.val = val;
        this.next = Array(level).fill(null);
    }
}

class Skiplist {
    private static maxLevel: number = 32;
    private static p: number = 0.25;
    private head: Node;
    private level: number;

    constructor() {
        this.head = new Node(-1, Skiplist.maxLevel);
        this.level = 0;
    }

    search(target: number): boolean {
        let curr = this.head;
        for (let i = this.level - 1; i >= 0; i--) {
            curr = this.findClosest(curr, i, target);
            if (curr.next[i] && curr.next[i]!.val === target) {
                return true;
            }
        }
        return false;
    }

    add(num: number): void {
        let curr = this.head;
        const level = this.randomLevel();
        const node = new Node(num, level);
        this.level = Math.max(this.level, level);

        for (let i = this.level - 1; i >= 0; i--) {
            curr = this.findClosest(curr, i, num);
            if (i < level) {
                node.next[i] = curr.next[i];
                curr.next[i] = node;
            }
        }
    }

    erase(num: number): boolean {
        let curr = this.head;
        let ok = false;

        for (let i = this.level - 1; i >= 0; i--) {
            curr = this.findClosest(curr, i, num);
            if (curr.next[i] && curr.next[i]!.val === num) {
                curr.next[i] = curr.next[i]!.next[i];
                ok = true;
            }
        }

        while (this.level > 1 && this.head.next[this.level - 1] === null) {
            this.level--;
        }

        return ok;
    }

    private findClosest(curr: Node, level: number, target: number): Node {
        while (curr.next[level] && curr.next[level]!.val < target) {
            curr = curr.next[level]!;
        }
        return curr;
    }

    private randomLevel(): number {
        let level = 1;
        while (level < Skiplist.maxLevel && Math.random() < Skiplist.p) {
            level++;
        }
        return level;
    }
}

/**
 * Your Skiplist object will be instantiated and called as such:
 * var obj = new Skiplist()
 * var param_1 = obj.search(target)
 * obj.add(num)
 * var param_3 = obj.erase(num)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
