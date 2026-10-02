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

# [432. All O`one Data Structure](https://leetcode.com/problems/all-oone-data-structure)

[中文文档](/solution/0400-0499/0432.All%20O%60one%20Data%20Structure/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế cấu trúc dữ liệu lưu số lần xuất hiện của các chuỗi và có thể trả về chuỗi có số lần xuất hiện nhỏ nhất hoặc lớn nhất.</p>

<p>Hãy cài đặt class <code>AllOne</code>:</p>

<ul>
	<li><code>AllOne()</code> Khởi tạo đối tượng cấu trúc dữ liệu.</li>
	<li><code>inc(String key)</code> Tăng số lần xuất hiện của chuỗi <code>key</code> thêm <code>1</code>. Nếu <code>key</code> chưa có trong cấu trúc dữ liệu, thêm nó với số lần xuất hiện là <code>1</code>.</li>
	<li><code>dec(String key)</code> Giảm số lần xuất hiện của chuỗi <code>key</code> đi <code>1</code>. Nếu số lần xuất hiện của <code>key</code> bằng <code>0</code> sau khi giảm, xóa nó khỏi cấu trúc dữ liệu. Đảm bảo <code>key</code> tồn tại trong cấu trúc dữ liệu trước khi giảm.</li>
	<li><code>getMaxKey()</code> Trả về một trong các key có số lần xuất hiện lớn nhất. Nếu không có phần tử nào, trả về chuỗi rỗng <code>&quot;&quot;</code>.</li>
	<li><code>getMinKey()</code> Trả về một trong các key có số lần xuất hiện nhỏ nhất. Nếu không có phần tử nào, trả về chuỗi rỗng <code>&quot;&quot;</code>.</li>
</ul>

<p><strong>Lưu ý</strong>: mỗi hàm phải có độ phức tạp thời gian trung bình <code>O(1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;AllOne&quot;, &quot;inc&quot;, &quot;inc&quot;, &quot;getMaxKey&quot;, &quot;getMinKey&quot;, &quot;inc&quot;, &quot;getMaxKey&quot;, &quot;getMinKey&quot;]
[[], [&quot;hello&quot;], [&quot;hello&quot;], [], [], [&quot;leet&quot;], [], []]
<strong>Đầu ra</strong>
[null, null, null, &quot;hello&quot;, &quot;hello&quot;, null, &quot;hello&quot;, &quot;leet&quot;]

<strong>Giải thích</strong>
AllOne allOne = new AllOne();
allOne.inc(&quot;hello&quot;);
allOne.inc(&quot;hello&quot;);
allOne.getMaxKey(); // return &quot;hello&quot;
allOne.getMinKey(); // return &quot;hello&quot;
allOne.inc(&quot;leet&quot;);
allOne.getMaxKey(); // return &quot;hello&quot;
allOne.getMinKey(); // return &quot;leet&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= key.length &lt;= 10</code></li>
	<li><code>key</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Đảm bảo <code>key</code> tồn tại trong cấu trúc dữ liệu ở mỗi lần gọi <code>dec</code>.</li>
	<li>Sẽ có tối đa <code>5 * 10<sup>4</sup></code>&nbsp;lần gọi <code>inc</code>, <code>dec</code>, <code>getMaxKey</code> và <code>getMinKey</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các thao tác $\textit{inc}$, $\textit{dec}$, $\textit{getMaxKey}$ và $\textit{getMinKey}$ đều cần chạy trong thời gian khấu hao $O(1)$. Hash map có thể cập nhật số lần xuất hiện trong $O(1)$ nhưng không tìm được giá trị lớn nhất, nhỏ nhất; ordered set thì cần thời gian logarithmic.
>
> Các key có cùng số lần xuất hiện được gom vào một bucket trong doubly linked list; các bucket tạo thành một vòng được sắp theo số lần xuất hiện. Map ánh xạ mỗi key tới bucket tương ứng. Khi tăng hoặc giảm, key chỉ di chuyển sang bucket có số đếm liền kề; nếu cần thì tạo hoặc xóa bucket rỗng.
>
> Node kế tiếp và node trước của vòng tương ứng với số lần xuất hiện nhỏ nhất và lớn nhất. Vì key không bao giờ bỏ qua bucket, ta không cần duyệt toàn bộ list.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Node:
    def __init__(self, key='', cnt=0):
        self.prev = None
        self.next = None
        self.cnt = cnt
        self.keys = {key}

    def insert(self, node):
        node.prev = self
        node.next = self.next
        node.prev.next = node
        node.next.prev = node
        return node

    def remove(self):
        self.prev.next = self.next
        self.next.prev = self.prev


class AllOne:
    def __init__(self):
        self.root = Node()
        self.root.next = self.root
        self.root.prev = self.root
        self.nodes = {}

    def inc(self, key: str) -> None:
        root, nodes = self.root, self.nodes
        if key not in nodes:
            if root.next == root or root.next.cnt > 1:
                nodes[key] = root.insert(Node(key, 1))
            else:
                root.next.keys.add(key)
                nodes[key] = root.next
        else:
            curr = nodes[key]
            next = curr.next
            if next == root or next.cnt > curr.cnt + 1:
                nodes[key] = curr.insert(Node(key, curr.cnt + 1))
            else:
                next.keys.add(key)
                nodes[key] = next
            curr.keys.discard(key)
            if not curr.keys:
                curr.remove()

    def dec(self, key: str) -> None:
        root, nodes = self.root, self.nodes
        curr = nodes[key]
        if curr.cnt == 1:
            nodes.pop(key)
        else:
            prev = curr.prev
            if prev == root or prev.cnt < curr.cnt - 1:
                nodes[key] = prev.insert(Node(key, curr.cnt - 1))
            else:
                prev.keys.add(key)
                nodes[key] = prev
        curr.keys.discard(key)
        if not curr.keys:
            curr.remove()

    def getMaxKey(self) -> str:
        return next(iter(self.root.prev.keys))

    def getMinKey(self) -> str:
        return next(iter(self.root.next.keys))


# Your AllOne object will be instantiated and called as such:
# obj = AllOne()
# obj.inc(key)
# obj.dec(key)
# param_3 = obj.getMaxKey()
# param_4 = obj.getMinKey()
```

#### Java

```java
class AllOne {
    Node root = new Node();
    Map<String, Node> nodes = new HashMap<>();

    public AllOne() {
        root.next = root;
        root.prev = root;
    }

    public void inc(String key) {
        if (!nodes.containsKey(key)) {
            if (root.next == root || root.next.cnt > 1) {
                nodes.put(key, root.insert(new Node(key, 1)));
            } else {
                root.next.keys.add(key);
                nodes.put(key, root.next);
            }
        } else {
            Node curr = nodes.get(key);
            Node next = curr.next;
            if (next == root || next.cnt > curr.cnt + 1) {
                nodes.put(key, curr.insert(new Node(key, curr.cnt + 1)));
            } else {
                next.keys.add(key);
                nodes.put(key, next);
            }
            curr.keys.remove(key);
            if (curr.keys.isEmpty()) {
                curr.remove();
            }
        }
    }

    public void dec(String key) {
        Node curr = nodes.get(key);
        if (curr.cnt == 1) {
            nodes.remove(key);
        } else {
            Node prev = curr.prev;
            if (prev == root || prev.cnt < curr.cnt - 1) {
                nodes.put(key, prev.insert(new Node(key, curr.cnt - 1)));
            } else {
                prev.keys.add(key);
                nodes.put(key, prev);
            }
        }

        curr.keys.remove(key);
        if (curr.keys.isEmpty()) {
            curr.remove();
        }
    }

    public String getMaxKey() {
        return root.prev.keys.iterator().next();
    }

    public String getMinKey() {
        return root.next.keys.iterator().next();
    }
}

class Node {
    Node prev;
    Node next;
    int cnt;
    Set<String> keys = new HashSet<>();

    public Node() {
        this("", 0);
    }

    public Node(String key, int cnt) {
        this.cnt = cnt;
        keys.add(key);
    }

    public Node insert(Node node) {
        node.prev = this;
        node.next = this.next;
        node.prev.next = node;
        node.next.prev = node;
        return node;
    }

    public void remove() {
        this.prev.next = this.next;
        this.next.prev = this.prev;
    }
}

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne obj = new AllOne();
 * obj.inc(key);
 * obj.dec(key);
 * String param_3 = obj.getMaxKey();
 * String param_4 = obj.getMinKey();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
