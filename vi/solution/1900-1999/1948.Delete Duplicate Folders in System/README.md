---
comments: true
difficulty: Hard
rating: 2533
source: Weekly Contest 251 Q4
tags:
    - Depth-First Search
    - Trie
    - Array
    - Hash Table
    - String
    - Sorting
    - Hash Function
---

<!-- problem:start -->

# [1948. Delete Duplicate Folders in System](https://leetcode.com/problems/delete-duplicate-folders-in-system)

[中文文档](/solution/1900-1999/1948.Delete%20Duplicate%20Folders%20in%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Do một lỗi, hệ thống file có rất nhiều thư mục bị trùng lặp. Cho một mảng 2 chiều <code>paths</code>, trong đó <code>paths[i]</code> là một mảng biểu diễn đường dẫn tuyệt đối đến thư mục thứ <code>i<sup>th</sup></code> trong hệ thống file.</p>

<ul>
	<li>Ví dụ, <code>[&quot;one&quot;, &quot;two&quot;, &quot;three&quot;]</code> biểu diễn đường dẫn <code>&quot;/one/two/three&quot;</code>.</li>
</ul>

<p>Hai thư mục (không nhất thiết ở cùng một cấp) được xem là <strong>giống hệt</strong> nếu chúng chứa cùng một tập <strong>không rỗng</strong> các thư mục con giống hệt nhau và có cùng cấu trúc thư mục con. Các thư mục <strong>không</strong> cần nằm ở cấp gốc mới được xem là giống hệt. Nếu có từ hai thư mục <strong>giống hệt</strong> trở lên, hãy <strong>đánh dấu</strong> các thư mục đó cùng toàn bộ thư mục con của chúng.</p>

<ul>
	<li>Ví dụ, các thư mục <code>&quot;/a&quot;</code> và <code>&quot;/b&quot;</code> trong cấu trúc file dưới đây là giống hệt nhau. Chúng (cùng toàn bộ thư mục con) đều phải được <strong>đánh dấu</strong>:

    <ul>
    <li><code>/a</code></li>
    <li><code>/a/x</code></li>
    <li><code>/a/x/y</code></li>
    <li><code>/a/z</code></li>
    <li><code>/b</code></li>
    <li><code>/b/x</code></li>
    <li><code>/b/x/y</code></li>
    <li><code>/b/z</code></li>
    </ul>
    </li>
    <li>Tuy nhiên, nếu cấu trúc file cũng có đường dẫn <code>&quot;/b/w&quot;</code>, thì các thư mục <code>&quot;/a&quot;</code> và <code>&quot;/b&quot;</code> sẽ không còn giống hệt nhau. Lưu ý rằng <code>&quot;/a/x&quot;</code> và <code>&quot;/b/x&quot;</code> vẫn được xem là giống hệt nhau dù có thêm thư mục này.</li>

</ul>

<p>Sau khi tất cả các thư mục giống hệt và thư mục con của chúng được đánh dấu, hệ thống file sẽ <strong>xóa</strong> tất cả các thư mục đó. Hệ thống file chỉ thực hiện thao tác xóa một lần, vì vậy những thư mục trở nên giống hệt nhau sau lần xóa ban đầu sẽ không bị xóa.</p>

<p>Trả về <em>mảng 2 chiều </em><code>ans</code> <em>chứa đường dẫn của các thư mục <strong>còn lại</strong> sau khi xóa tất cả thư mục đã đánh dấu. Có thể trả về các đường dẫn theo <strong>thứ tự bất kỳ</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1948.Delete%20Duplicate%20Folders%20in%20System/images/lc-dupfolder1.jpg" style="width: 200px; height: 218px;" />
<pre>
<strong>Đầu vào:</strong> paths = [[&quot;a&quot;],[&quot;c&quot;],[&quot;d&quot;],[&quot;a&quot;,&quot;b&quot;],[&quot;c&quot;,&quot;b&quot;],[&quot;d&quot;,&quot;a&quot;]]
<strong>Đầu ra:</strong> [[&quot;d&quot;],[&quot;d&quot;,&quot;a&quot;]]
<strong>Giải thích:</strong> Cấu trúc file như hình.
Các thư mục &quot;/a&quot; và &quot;/c&quot; (cùng các thư mục con của chúng) bị đánh dấu để xóa vì cả hai đều chứa
một thư mục rỗng có tên &quot;b&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1948.Delete%20Duplicate%20Folders%20in%20System/images/lc-dupfolder2.jpg" style="width: 200px; height: 355px;" />
<pre>
<strong>Đầu vào:</strong> paths = [[&quot;a&quot;],[&quot;c&quot;],[&quot;a&quot;,&quot;b&quot;],[&quot;c&quot;,&quot;b&quot;],[&quot;a&quot;,&quot;b&quot;,&quot;x&quot;],[&quot;a&quot;,&quot;b&quot;,&quot;x&quot;,&quot;y&quot;],[&quot;w&quot;],[&quot;w&quot;,&quot;y&quot;]]
<strong>Đầu ra:</strong> [[&quot;c&quot;],[&quot;c&quot;,&quot;b&quot;],[&quot;a&quot;],[&quot;a&quot;,&quot;b&quot;]]
<strong>Giải thích: </strong>Cấu trúc file như hình.
Các thư mục &quot;/a/b/x&quot; và &quot;/w&quot; (cùng các thư mục con của chúng) bị đánh dấu để xóa vì cả hai đều chứa một thư mục rỗng có tên &quot;y&quot;.
Lưu ý rằng các thư mục &quot;/a&quot; và &quot;/c&quot; trở nên giống hệt nhau sau khi xóa, nhưng không bị xóa vì trước đó chúng chưa được đánh dấu.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1948.Delete%20Duplicate%20Folders%20in%20System/images/lc-dupfolder3.jpg" style="width: 200px; height: 201px;" />
<pre>
<strong>Đầu vào:</strong> paths = [[&quot;a&quot;,&quot;b&quot;],[&quot;c&quot;,&quot;d&quot;],[&quot;c&quot;],[&quot;a&quot;]]
<strong>Đầu ra:</strong> [[&quot;c&quot;],[&quot;c&quot;,&quot;d&quot;],[&quot;a&quot;],[&quot;a&quot;,&quot;b&quot;]]
<strong>Giải thích:</strong> Tất cả các thư mục trong hệ thống file đều khác nhau.
Lưu ý rằng mảng trả về có thể có thứ tự khác, vì thứ tự không quan trọng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= paths.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= paths[i].length &lt;= 500</code></li>
	<li><code>1 &lt;= paths[i][j].length &lt;= 10</code></li>
	<li><code>1 &lt;= sum(paths[i][j].length) &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>path[i][j]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Không có hai đường dẫn nào trỏ đến cùng một thư mục.</li>
	<li>Với mọi thư mục không nằm ở cấp gốc, thư mục cha của nó cũng xuất hiện trong input.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Tất cả các cây con thư mục giống hệt nhau (bao gồm cả tên) đều phải bị xóa. So sánh từng cặp chuỗi biểu diễn sẽ quá chậm.
>
> Chèn mọi đường dẫn vào một trie, sau đó dùng DFS để mã hóa mỗi node thành phép nối đã sắp xếp của tên các node con và các chuỗi mã hóa tương ứng. Những node có cùng chuỗi mã hóa sẽ đánh dấu lẫn nhau để xóa.
>
> Dùng DFS lần thứ hai để bỏ qua các node đã bị xóa và tạo ra các đường dẫn từ gốc đến node còn lại.

<!-- thinking:end -->

Ta có thể dùng trie để lưu cấu trúc thư mục, trong đó mỗi node của trie chứa các dữ liệu sau:

- `children`: Một dictionary, trong đó key là tên thư mục con và value là node con tương ứng.
- `deleted`: Một giá trị boolean cho biết node có được đánh dấu để xóa hay không.

Ta chèn tất cả đường dẫn vào trie, sau đó dùng DFS để duyệt trie và xây dựng chuỗi biểu diễn cho mỗi cây con. Với mỗi cây con, nếu chuỗi biểu diễn của nó đã tồn tại trong một dictionary toàn cục, ta đánh dấu node hiện tại và node tương ứng trong dictionary toàn cục để xóa. Cuối cùng, ta lại dùng DFS để duyệt trie và thêm đường dẫn của các node chưa bị đánh dấu vào danh sách kết quả.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children: Dict[str, "Trie"] = defaultdict(Trie)
        self.deleted: bool = False


class Solution:
    def deleteDuplicateFolder(self, paths: List[List[str]]) -> List[List[str]]:
        root = Trie()
        for path in paths:
            cur = root
            for name in path:
                if cur.children[name] is None:
                    cur.children[name] = Trie()
                cur = cur.children[name]

        g: Dict[str, Trie] = {}

        def dfs(node: Trie) -> str:
            if not node.children:
                return ""
            subs: List[str] = []
            for name, child in node.children.items():
                subs.append(f"{name}({dfs(child)})")
            s = "".join(sorted(subs))
            if s in g:
                node.deleted = g[s].deleted = True
            else:
                g[s] = node
            return s

        def dfs2(node: Trie) -> None:
            if node.deleted:
                return
            if path:
                ans.append(path[:])
            for name, child in node.children.items():
                path.append(name)
                dfs2(child)
                path.pop()

        dfs(root)
        ans: List[List[str]] = []
        path: List[str] = []
        dfs2(root)
        return ans
```

#### Java

```java
class Trie {
    Map<String, Trie> children;
    boolean deleted;

    public Trie() {
        children = new HashMap<>();
        deleted = false;
    }
}

class Solution {
    public List<List<String>> deleteDuplicateFolder(List<List<String>> paths) {
        Trie root = new Trie();
        for (List<String> path : paths) {
            Trie cur = root;
            for (String name : path) {
                if (!cur.children.containsKey(name)) {
                    cur.children.put(name, new Trie());
                }
                cur = cur.children.get(name);
            }
        }

        Map<String, Trie> g = new HashMap<>();

        var dfs = new Function<Trie, String>() {
            @Override
            public String apply(Trie node) {
                if (node.children.isEmpty()) {
                    return "";
                }
                List<String> subs = new ArrayList<>();
                for (var entry : node.children.entrySet()) {
                    subs.add(entry.getKey() + "(" + apply(entry.getValue()) + ")");
                }
                Collections.sort(subs);
                String s = String.join("", subs);
                if (g.containsKey(s)) {
                    node.deleted = true;
                    g.get(s).deleted = true;
                } else {
                    g.put(s, node);
                }
                return s;
            }
        };

        dfs.apply(root);

        List<List<String>> ans = new ArrayList<>();
        List<String> path = new ArrayList<>();

        var dfs2 = new Function<Trie, Void>() {
            @Override
            public Void apply(Trie node) {
                if (node.deleted) {
                    return null;
                }
                if (!path.isEmpty()) {
                    ans.add(new ArrayList<>(path));
                }
                for (Map.Entry<String, Trie> entry : node.children.entrySet()) {
                    path.add(entry.getKey());
                    apply(entry.getValue());
                    path.remove(path.size() - 1);
                }
                return null;
            }
        };

        dfs2.apply(root);

        return ans;
    }
}
```

#### C++

```cpp
class Trie {
public:
    unordered_map<string, Trie*> children;
    bool deleted = false;
};

class Solution {
public:
    vector<vector<string>> deleteDuplicateFolder(vector<vector<string>>& paths) {
        Trie* root = new Trie();

        for (auto& path : paths) {
            Trie* cur = root;
            for (auto& name : path) {
                if (cur->children.find(name) == cur->children.end()) {
                    cur->children[name] = new Trie();
                }
                cur = cur->children[name];
            }
        }

        unordered_map<string, Trie*> g;

        auto dfs = [&](this auto&& dfs, Trie* node) -> string {
            if (node->children.empty()) return "";

            vector<string> subs;
            for (auto& child : node->children) {
                subs.push_back(child.first + "(" + dfs(child.second) + ")");
            }
            sort(subs.begin(), subs.end());
            string s = "";
            for (auto& sub : subs) s += sub;

            if (g.contains(s)) {
                node->deleted = true;
                g[s]->deleted = true;
            } else {
                g[s] = node;
            }
            return s;
        };

        dfs(root);

        vector<vector<string>> ans;
        vector<string> path;

        auto dfs2 = [&](this auto&& dfs2, Trie* node) -> void {
            if (node->deleted) return;
            if (!path.empty()) {
                ans.push_back(path);
            }
            for (auto& child : node->children) {
                path.push_back(child.first);
                dfs2(child.second);
                path.pop_back();
            }
        };

        dfs2(root);

        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children map[string]*Trie
	deleted  bool
}

func NewTrie() *Trie {
	return &Trie{
		children: make(map[string]*Trie),
	}
}

func deleteDuplicateFolder(paths [][]string) (ans [][]string) {
	root := NewTrie()
	for _, path := range paths {
		cur := root
		for _, name := range path {
			if _, exists := cur.children[name]; !exists {
				cur.children[name] = NewTrie()
			}
			cur = cur.children[name]
		}
	}

	g := make(map[string]*Trie)

	var dfs func(*Trie) string
	dfs = func(node *Trie) string {
		if len(node.children) == 0 {
			return ""
		}
		var subs []string
		for name, child := range node.children {
			subs = append(subs, name+"("+dfs(child)+")")
		}
		sort.Strings(subs)
		s := strings.Join(subs, "")
		if existingNode, exists := g[s]; exists {
			node.deleted = true
			existingNode.deleted = true
		} else {
			g[s] = node
		}
		return s
	}

	var dfs2 func(*Trie, []string)
	dfs2 = func(node *Trie, path []string) {
		if node.deleted {
			return
		}
		if len(path) > 0 {
			ans = append(ans, append([]string{}, path...))
		}
		for name, child := range node.children {
			dfs2(child, append(path, name))
		}
	}

	dfs(root)
	dfs2(root, []string{})
	return ans
}
```

#### TypeScript

```ts
function deleteDuplicateFolder(paths: string[][]): string[][] {
    class Trie {
        children: { [key: string]: Trie } = {};
        deleted: boolean = false;
    }

    const root = new Trie();

    for (const path of paths) {
        let cur = root;
        for (const name of path) {
            if (!cur.children[name]) {
                cur.children[name] = new Trie();
            }
            cur = cur.children[name];
        }
    }

    const g: { [key: string]: Trie } = {};

    const dfs = (node: Trie): string => {
        if (Object.keys(node.children).length === 0) return '';

        const subs: string[] = [];
        for (const [name, child] of Object.entries(node.children)) {
            subs.push(`${name}(${dfs(child)})`);
        }
        subs.sort();
        const s = subs.join('');

        if (g[s]) {
            node.deleted = true;
            g[s].deleted = true;
        } else {
            g[s] = node;
        }
        return s;
    };

    dfs(root);

    const ans: string[][] = [];
    const path: string[] = [];

    const dfs2 = (node: Trie): void => {
        if (node.deleted) return;
        if (path.length > 0) {
            ans.push([...path]);
        }
        for (const [name, child] of Object.entries(node.children)) {
            path.push(name);
            dfs2(child);
            path.pop();
        }
    };

    dfs2(root);

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
