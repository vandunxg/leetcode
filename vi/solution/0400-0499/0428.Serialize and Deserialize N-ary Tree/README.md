---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - String
---

<!-- problem:start -->

# [428. Serialize and Deserialize N-ary Tree 🔒](https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree)

[中文文档](/solution/0400-0499/0428.Serialize%20and%20Deserialize%20N-ary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Serialization là quá trình chuyển đổi cấu trúc dữ liệu hoặc object thành một dãy bit để lưu trong file hoặc memory buffer, hoặc truyền qua kết nối mạng và khôi phục lại sau đó trong cùng môi trường máy tính hay một môi trường khác.</p>

<p>Hãy thiết kế thuật toán serialize và deserialize một cây N-ary. Cây N-ary là cây có gốc, trong đó mỗi node có không quá N node con. Không có giới hạn về cách thuật toán serialize/deserialize hoạt động; bạn chỉ cần đảm bảo có thể serialize cây N-ary thành chuỗi và deserialize chuỗi đó về đúng cấu trúc cây ban đầu.</p>

<p>Ví dụ, bạn có thể serialize cây <code>3-ary</code> sau</p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0428.Serialize%20and%20Deserialize%20N-ary%20Tree/images/narytreeexample.png" style="width: 500px; max-width: 300px; height: 321px;" />
<p>&nbsp;</p>

<p>thành <code>[1 [3[5 6] 2 4]]</code>. Đây chỉ là ví dụ; bạn không nhất thiết phải dùng định dạng này.</p>

<p>Bạn cũng có thể dùng định dạng serialize theo level order traversal của LeetCode, trong đó mỗi nhóm node con được phân tách bằng giá trị null.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0428.Serialize%20and%20Deserialize%20N-ary%20Tree/images/sample_4_964.png" style="width: 500px; height: 454px;" />
<p>&nbsp;</p>

<p>Ví dụ, cây trên có thể được serialize thành <code>[1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]</code>.</p>

<p>Bạn không nhất thiết phải dùng các định dạng gợi ý ở trên; còn nhiều định dạng khác cũng phù hợp, vì vậy hãy tự chọn cách tiếp cận sáng tạo.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
<strong>Đầu ra:</strong> [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,3,2,4,null,5,6]
<strong>Đầu ra:</strong> [1,null,3,2,4,null,5,6]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> root = []
<strong>Đầu ra:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
	<li>Chiều cao của cây N-ary không vượt quá <code>1000</code>.</li>
	<li>Không dùng biến member của class, biến global hoặc static để lưu trạng thái. Thuật toán encode và decode phải stateless.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt theo level order

<!-- thinking:start -->

> **Tư duy**
>
> Node trong cây $N$-ary có số lượng node con thay đổi, nên preorder chỉ ghi giá trị không thể cho biết danh sách node con kết thúc ở đâu. Cần có ký hiệu kết thúc sau mỗi danh sách.
>
> Serialize theo level order: ghi root; mỗi khi lấy một node khỏi queue thì ghi các node con và đưa chúng vào queue, cuối cùng ghi $\#$. Khi deserialize, dùng queue tương tự: lần lượt đọc các token tiếp theo làm node con cho đến khi gặp $\#$.
>
> Sentinel được đặt tương ứng với thứ tự dequeue, nhờ đó có thể dựng lại danh sách node con của từng node; cây rỗng được biểu diễn bằng chuỗi rỗng.

<!-- thinking:end -->

Có thể serialize cây N-ary bằng level order traversal. Bắt đầu từ root, thêm giá trị của nó vào kết quả rồi enqueue node đó. Mỗi lần dequeue một node, thêm giá trị của tất cả node con vào kết quả và enqueue chúng, sau đó thêm ký tự đặc biệt `#` để đánh dấu kết thúc danh sách node con của node đó. Cuối cùng, nối các giá trị bằng dấu phẩy.

Khi deserialize, tách chuỗi theo delimiter. Tạo root từ giá trị đầu tiên rồi enqueue node đó. Với mỗi node được dequeue, tiếp tục đọc các giá trị tiếp theo làm node con cho đến khi gặp `#`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây N-ary.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val: Optional[int] = None, children: Optional[List['Node']] = None):
        self.val = val
        self.children = children
"""


class Codec:
    def serialize(self, root: 'Node') -> str:
        if root is None:
            return ''
        ans = [str(root.val)]
        q = deque([root])
        while q:
            node = q.popleft()
            for child in node.children or []:
                ans.append(str(child.val))
                q.append(child)
            ans.append('#')
        return ','.join(ans)

    def deserialize(self, data: str) -> 'Node':
        if not data:
            return None
        vals = data.split(',')
        root = Node(int(vals[0]), [])
        q = deque([root])
        i = 1
        while q:
            node = q.popleft()
            while vals[i] != '#':
                child = Node(int(vals[i]), [])
                node.children.append(child)
                q.append(child)
                i += 1
            i += 1
        return root


# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Codec {
    public String serialize(Node root) {
        if (root == null) {
            return "";
        }
        List<String> ans = new ArrayList<>();
        Deque<Node> q = new ArrayDeque<>();
        ans.add(String.valueOf(root.val));
        q.offer(root);
        while (!q.isEmpty()) {
            Node node = q.poll();
            if (node.children != null) {
                for (Node child : node.children) {
                    ans.add(String.valueOf(child.val));
                    q.offer(child);
                }
            }
            ans.add("#");
        }
        return String.join(",", ans);
    }

    public Node deserialize(String data) {
        if ("".equals(data)) {
            return null;
        }
        String[] vals = data.split(",");
        Node root = new Node(Integer.parseInt(vals[0]), new ArrayList<>());
        Deque<Node> q = new ArrayDeque<>();
        q.offer(root);
        int i = 1;
        while (!q.isEmpty()) {
            Node node = q.poll();
            while (!"#".equals(vals[i])) {
                Node child = new Node(Integer.parseInt(vals[i++]), new ArrayList<>());
                node.children.add(child);
                q.offer(child);
            }
            ++i;
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Codec {
public:
    string serialize(Node* root) {
        if (!root) {
            return "";
        }
        queue<Node*> q{{root}};
        string ans = to_string(root->val);
        while (!q.empty()) {
            auto node = q.front();
            q.pop();
            for (auto child : node->children) {
                ans += "," + to_string(child->val);
                q.push(child);
            }
            ans += ",#";
        }
        return ans;
    }

    Node* deserialize(string data) {
        if (data.empty()) {
            return nullptr;
        }
        stringstream ss(data);
        string t;
        getline(ss, t, ',');
        Node* root = new Node(stoi(t), {});
        queue<Node*> q{{root}};
        while (!q.empty()) {
            auto node = q.front();
            q.pop();
            while (getline(ss, t, ',') && t != "#") {
                Node* child = new Node(stoi(t), {});
                node->children.push_back(child);
                q.push(child);
            }
        }
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

type Codec struct {
}

func Constructor() *Codec {
	return &Codec{}
}

func (this *Codec) serialize(root *Node) string {
	if root == nil {
		return ""
	}
	ans := []string{strconv.Itoa(root.Val)}
	q := []*Node{root}
	for len(q) > 0 {
		node := q[0]
		q = q[1:]
		for _, child := range node.Children {
			ans = append(ans, strconv.Itoa(child.Val))
			q = append(q, child)
		}
		ans = append(ans, "#")
	}
	return strings.Join(ans, ",")
}

func (this *Codec) deserialize(data string) *Node {
	if data == "" {
		return nil
	}
	vals := strings.Split(data, ",")
	v, _ := strconv.Atoi(vals[0])
	root := &Node{Val: v}
	q := []*Node{root}
	i := 1
	for len(q) > 0 {
		node := q[0]
		q = q[1:]
		for i < len(vals) && vals[i] != "#" {
			v, _ = strconv.Atoi(vals[i])
			child := &Node{Val: v}
			node.Children = append(node.Children, child)
			q = append(q, child)
			i++
		}
		i++
	}
	return root
}

/**
 * Your Codec object will be instantiated and called as such:
 * obj := Constructor();
 * data := obj.serialize(root);
 * ans := obj.deserialize(data);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
