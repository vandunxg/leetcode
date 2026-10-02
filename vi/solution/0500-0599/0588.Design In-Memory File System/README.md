---
comments: true
difficulty: Hard
tags:
    - Design
    - Trie
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [588. Design In-Memory File System 🔒](https://leetcode.com/problems/design-in-memory-file-system)

[中文文档](/solution/0500-0599/0588.Design%20In-Memory%20File%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế một cấu trúc dữ liệu mô phỏng file system trong bộ nhớ.</p>

<p>Hãy triển khai class FileSystem:</p>

<ul>
	<li><code>FileSystem()</code> Khởi tạo đối tượng của hệ thống.</li>
	<li><code>List&lt;String&gt; ls(String path)</code>
	<ul>
		<li>Nếu <code>path</code> là đường dẫn đến file, trả về danh sách chỉ chứa tên file đó.</li>
		<li>Nếu <code>path</code> là đường dẫn đến thư mục, trả về danh sách tên các file và thư mục <strong>trong thư mục này</strong>.</li>
	</ul>
	Kết quả phải theo <strong>thứ tự từ điển</strong>.</li>
	<li><code>void mkdir(String path)</code> Tạo thư mục mới theo <code>path</code> đã cho. Đường dẫn thư mục này chưa tồn tại. Nếu các thư mục trung gian trong đường dẫn chưa tồn tại, bạn cũng cần tạo chúng.</li>
	<li><code>void addContentToFile(String filePath, String content)</code>
	<ul>
		<li>Nếu <code>filePath</code> chưa tồn tại, tạo file đó với nội dung <code>content</code> đã cho.</li>
		<li>Nếu <code>filePath</code> đã tồn tại, nối <code>content</code> vào cuối nội dung hiện có.</li>
	</ul>
	</li>
	<li><code>String readContentFromFile(String filePath)</code> Trả về nội dung của file tại <code>filePath</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0588.Design%20In-Memory%20File%20System/images/filesystem.png" style="width: 650px; height: 315px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;FileSystem&quot;, &quot;ls&quot;, &quot;mkdir&quot;, &quot;addContentToFile&quot;, &quot;ls&quot;, &quot;readContentFromFile&quot;]
[[], [&quot;/&quot;], [&quot;/a/b/c&quot;], [&quot;/a/b/c/d&quot;, &quot;hello&quot;], [&quot;/&quot;], [&quot;/a/b/c/d&quot;]]
<strong>Đầu ra</strong>
[null, [], null, null, [&quot;a&quot;], &quot;hello&quot;]

<strong>Giải thích</strong>
FileSystem fileSystem = new FileSystem();
fileSystem.ls(&quot;/&quot;); // return []
fileSystem.mkdir(&quot;/a/b/c&quot;);
fileSystem.addContentToFile(&quot;/a/b/c/d&quot;, &quot;hello&quot;);
fileSystem.ls(&quot;/&quot;); // return [&quot;a&quot;]
fileSystem.readContentFromFile(&quot;/a/b/c/d&quot;); // return &quot;hello&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= path.length,&nbsp;filePath.length &lt;= 100</code></li>
	<li><code>path</code> và <code>filePath</code> là đường dẫn tuyệt đối, bắt đầu bằng <code>&#39;/&#39;</code> và không kết thúc bằng <code>&#39;/&#39;</code>, trừ trường hợp đường dẫn chỉ là <code>&quot;/&quot;</code>.</li>
	<li>Có thể giả định tên thư mục và tên file chỉ gồm chữ cái viết thường, và không có hai tên trùng nhau trong cùng một thư mục.</li>
	<li>Có thể giả định mọi thao tác đều nhận tham số hợp lệ; người dùng sẽ không yêu cầu đọc nội dung file hoặc liệt kê thư mục hay file không tồn tại.</li>
	<li>Có thể giả định thư mục cha của file được truyền vào <code>addContentToFile</code> luôn tồn tại.</li>
	<li><code>1 &lt;= content.length &lt;= 50</code></li>
	<li>Tổng số lần gọi <code>ls</code>, <code>mkdir</code>, <code>addContentToFile</code> và <code>readContentFromFile</code> không quá <code>300</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> File system có cấu trúc phân cấp với các thao tác liệt kê, tạo thư mục, nối nội dung và đọc file. Dùng map phẳng lưu toàn bộ đường dẫn vẫn được, nhưng khó chia sẻ prefix và liệt kê nội dung thư mục.
>
> Trie dùng các segment của đường dẫn làm key để lưu node con, cờ đánh dấu file và các phần nội dung. `insert` tạo node; `search` duyệt đến đích. `ls` trả về tên file hoặc danh sách tên node con đã sắp xếp. Chi phí phụ thuộc vào độ sâu của đường dẫn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.name = None
        self.isFile = False
        self.content = []
        self.children = {}

    def insert(self, path, isFile):
        node = self
        ps = path.split('/')
        for p in ps[1:]:
            if p not in node.children:
                node.children[p] = Trie()
            node = node.children[p]
        node.isFile = isFile
        if isFile:
            node.name = ps[-1]
        return node

    def search(self, path):
        node = self
        if path == '/':
            return node
        ps = path.split('/')
        for p in ps[1:]:
            if p not in node.children:
                return None
            node = node.children[p]
        return node


class FileSystem:
    def __init__(self):
        self.root = Trie()

    def ls(self, path: str) -> List[str]:
        node = self.root.search(path)
        if node is None:
            return []
        if node.isFile:
            return [node.name]
        return sorted(node.children.keys())

    def mkdir(self, path: str) -> None:
        self.root.insert(path, False)

    def addContentToFile(self, filePath: str, content: str) -> None:
        node = self.root.insert(filePath, True)
        node.content.append(content)

    def readContentFromFile(self, filePath: str) -> str:
        node = self.root.search(filePath)
        return ''.join(node.content)


# Your FileSystem object will be instantiated and called as such:
# obj = FileSystem()
# param_1 = obj.ls(path)
# obj.mkdir(path)
# obj.addContentToFile(filePath,content)
# param_4 = obj.readContentFromFile(filePath)
```

#### Java

```java
class Trie {
    String name;
    boolean isFile;
    StringBuilder content = new StringBuilder();
    Map<String, Trie> children = new HashMap<>();

    Trie insert(String path, boolean isFile) {
        Trie node = this;
        String[] ps = path.split("/");
        for (int i = 1; i < ps.length; ++i) {
            String p = ps[i];
            if (!node.children.containsKey(p)) {
                node.children.put(p, new Trie());
            }
            node = node.children.get(p);
        }
        node.isFile = isFile;
        if (isFile) {
            node.name = ps[ps.length - 1];
        }
        return node;
    }

    Trie search(String path) {
        Trie node = this;
        String[] ps = path.split("/");
        for (int i = 1; i < ps.length; ++i) {
            String p = ps[i];
            if (!node.children.containsKey(p)) {
                return null;
            }
            node = node.children.get(p);
        }
        return node;
    }
}

class FileSystem {
    private Trie root = new Trie();

    public FileSystem() {
    }

    public List<String> ls(String path) {
        List<String> ans = new ArrayList<>();
        Trie node = root.search(path);
        if (node == null) {
            return ans;
        }
        if (node.isFile) {
            ans.add(node.name);
            return ans;
        }
        for (String v : node.children.keySet()) {
            ans.add(v);
        }
        Collections.sort(ans);
        return ans;
    }

    public void mkdir(String path) {
        root.insert(path, false);
    }

    public void addContentToFile(String filePath, String content) {
        Trie node = root.insert(filePath, true);
        node.content.append(content);
    }

    public String readContentFromFile(String filePath) {
        Trie node = root.search(filePath);
        return node.content.toString();
    }
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem obj = new FileSystem();
 * List<String> param_1 = obj.ls(path);
 * obj.mkdir(path);
 * obj.addContentToFile(filePath,content);
 * String param_4 = obj.readContentFromFile(filePath);
 */
```

#### Go

```go
type Trie struct {
	name     string
	isFile   bool
	content  strings.Builder
	children map[string]*Trie
}

func newTrie() *Trie {
	m := map[string]*Trie{}
	return &Trie{children: m}
}

func (this *Trie) insert(path string, isFile bool) *Trie {
	node := this
	ps := strings.Split(path, "/")
	for _, p := range ps[1:] {
		if _, ok := node.children[p]; !ok {
			node.children[p] = newTrie()
		}
		node, _ = node.children[p]
	}
	node.isFile = isFile
	if isFile {
		node.name = ps[len(ps)-1]
	}
	return node
}

func (this *Trie) search(path string) *Trie {
	if path == "/" {
		return this
	}
	node := this
	ps := strings.Split(path, "/")
	for _, p := range ps[1:] {
		if _, ok := node.children[p]; !ok {
			return nil
		}
		node, _ = node.children[p]
	}
	return node
}

type FileSystem struct {
	root *Trie
}

func Constructor() FileSystem {
	root := newTrie()
	return FileSystem{root}
}

func (this *FileSystem) Ls(path string) []string {
	var ans []string
	node := this.root.search(path)
	if node == nil {
		return ans
	}
	if node.isFile {
		ans = append(ans, node.name)
		return ans
	}
	for v := range node.children {
		ans = append(ans, v)
	}
	sort.Strings(ans)
	return ans
}

func (this *FileSystem) Mkdir(path string) {
	this.root.insert(path, false)
}

func (this *FileSystem) AddContentToFile(filePath string, content string) {
	node := this.root.insert(filePath, true)
	node.content.WriteString(content)
}

func (this *FileSystem) ReadContentFromFile(filePath string) string {
	node := this.root.search(filePath)
	return node.content.String()
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Ls(path);
 * obj.Mkdir(path);
 * obj.AddContentToFile(filePath,content);
 * param_4 := obj.ReadContentFromFile(filePath);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
