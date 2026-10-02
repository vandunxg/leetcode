---
comments: true
difficulty: Medium
rating: 1479
source: Biweekly Contest 7 Q2
tags:
    - Design
    - Trie
    - Hash Table
    - String
---

<!-- problem:start -->

# [1166. Design File System 🔒](https://leetcode.com/problems/design-file-system)

[中文文档](/solution/1100-1199/1166.Design%20File%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế một file system cho phép tạo các path mới và gắn mỗi path với một giá trị.</p>

<p>Path có định dạng gồm một hoặc nhiều chuỗi nối tiếp nhau, mỗi chuỗi bắt đầu bằng <code>/</code>, theo sau là một hoặc nhiều chữ cái tiếng Anh viết thường. Ví dụ, &quot;<code>/leetcode&quot;</code> và &quot;<code>/leetcode/problems&quot;</code> là các path hợp lệ, còn chuỗi rỗng <code>&quot;&quot;</code> và <code>&quot;/&quot;</code> thì không.</p>

<p>Hãy triển khai class <code>FileSystem</code>:</p>

<ul>
	<li><code>bool createPath(string path, int value)</code> tạo <code>path</code> mới và gắn với <code>value</code> nếu có thể, sau đó trả về <code>true</code>. Trả về <code>false</code> nếu path <strong>đã tồn tại</strong> hoặc path cha của nó <strong>không tồn tại</strong>.</li>
	<li><code>int get(string path)</code> trả về giá trị được gắn với <code>path</code>, hoặc trả về <code>-1</code> nếu path không tồn tại.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
[&quot;FileSystem&quot;,&quot;createPath&quot;,&quot;get&quot;]
[[],[&quot;/a&quot;,1],[&quot;/a&quot;]]
<strong>Output:</strong> 
[null,true,1]
<strong>Giải thích:</strong> 
FileSystem fileSystem = new FileSystem();

fileSystem.createPath(&quot;/a&quot;, 1); // return true
fileSystem.get(&quot;/a&quot;); // return 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> 
[&quot;FileSystem&quot;,&quot;createPath&quot;,&quot;createPath&quot;,&quot;get&quot;,&quot;createPath&quot;,&quot;get&quot;]
[[],[&quot;/leet&quot;,1],[&quot;/leet/code&quot;,2],[&quot;/leet/code&quot;],[&quot;/c/d&quot;,1],[&quot;/c&quot;]]
<strong>Output:</strong> 
[null,true,true,2,false,-1]
<strong>Giải thích:</strong> 
FileSystem fileSystem = new FileSystem();

fileSystem.createPath(&quot;/leet&quot;, 1); // return true
fileSystem.createPath(&quot;/leet/code&quot;, 2); // return true
fileSystem.get(&quot;/leet/code&quot;); // return 2
fileSystem.createPath(&quot;/c/d&quot;, 1); // return false because the parent path &quot;/c&quot; doesn&#39;t exist.
fileSystem.get(&quot;/c&quot;); // return -1 because this path doesn&#39;t exist.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= path.length &lt;= 100</code></li>
	<li><code>1 &lt;= value &lt;= 10<sup>9</sup></code></li>
	<li>Mỗi <code>path</code> đều <strong>hợp lệ</strong> và chỉ gồm chữ cái tiếng Anh viết thường cùng ký tự <code>&#39;/&#39;</code>.</li>
	<li>Tổng cộng có tối đa <code>10<sup>4</sup></code> lần gọi <code>createPath</code> và <code>get</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie

<!-- thinking:start -->

> **Tư duy**
>
> Tách path theo `/`. Khi tạo, path cha phải tồn tại và path mới chưa được tạo; khi lấy giá trị, duyệt qua từng segment. Trie lưu các segment dưới dạng cạnh: `insert` yêu cầu mọi segment tiền tố trừ segment cuối đã có node con, đồng thời từ chối nếu segment cuối bị trùng; `get` trả về $-1$ nếu thiếu bất kỳ segment nào. Lưu các node con bằng hash map giúp mỗi bước tra cứu có thời gian kỳ vọng hằng số.

<!-- thinking:end -->

Ta có thể dùng trie để lưu các path, trong đó mỗi node chứa giá trị tương ứng với path của node đó.

Cấu trúc của một node trong trie được định nghĩa như sau:

- `children`: Các node con được lưu trong hash table; key là path của node con và value là tham chiếu đến node con đó.
- `v`: Giá trị của path tương ứng với node hiện tại.

Các method của trie được định nghĩa như sau:

- `insert(w, v)`: Thêm path $w$ và gán giá trị tương ứng là $v$. Nếu path $w$ đã tồn tại hoặc path cha không tồn tại thì trả về `false`, ngược lại trả về `true`. Độ phức tạp thời gian là $O(|w|)$, với $|w|$ là độ dài path $w$.
- `search(w)`: Trả về giá trị tương ứng với path $w$. Nếu path $w$ không tồn tại thì trả về $-1$. Độ phức tạp thời gian là $O(|w|)$.

Tổng độ phức tạp thời gian là $O(\sum_{w \in W}|w|)$ và tổng độ phức tạp không gian là $O(\sum_{w \in W}|w|)$, với $W$ là tập hợp tất cả path đã được thêm.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self, v: int = -1):
        self.children = {}
        self.v = v

    def insert(self, w: str, v: int) -> bool:
        node = self
        ps = w.split("/")
        for p in ps[1:-1]:
            if p not in node.children:
                return False
            node = node.children[p]
        if ps[-1] in node.children:
            return False
        node.children[ps[-1]] = Trie(v)
        return True

    def search(self, w: str) -> int:
        node = self
        for p in w.split("/")[1:]:
            if p not in node.children:
                return -1
            node = node.children[p]
        return node.v


class FileSystem:
    def __init__(self):
        self.trie = Trie()

    def createPath(self, path: str, value: int) -> bool:
        return self.trie.insert(path, value)

    def get(self, path: str) -> int:
        return self.trie.search(path)


# Your FileSystem object will be instantiated and called as such:
# obj = FileSystem()
# param_1 = obj.createPath(path,value)
# param_2 = obj.get(path)
```

#### Java

```java
class Trie {
    Map<String, Trie> children = new HashMap<>();
    int v;

    Trie(int v) {
        this.v = v;
    }

    boolean insert(String w, int v) {
        Trie node = this;
        var ps = w.split("/");
        for (int i = 1; i < ps.length - 1; ++i) {
            var p = ps[i];
            if (!node.children.containsKey(p)) {
                return false;
            }
            node = node.children.get(p);
        }
        if (node.children.containsKey(ps[ps.length - 1])) {
            return false;
        }
        node.children.put(ps[ps.length - 1], new Trie(v));
        return true;
    }

    int search(String w) {
        Trie node = this;
        var ps = w.split("/");
        for (int i = 1; i < ps.length; ++i) {
            var p = ps[i];
            if (!node.children.containsKey(p)) {
                return -1;
            }
            node = node.children.get(p);
        }
        return node.v;
    }
}

class FileSystem {
    private Trie trie = new Trie(-1);

    public FileSystem() {
    }

    public boolean createPath(String path, int value) {
        return trie.insert(path, value);
    }

    public int get(String path) {
        return trie.search(path);
    }
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem obj = new FileSystem();
 * boolean param_1 = obj.createPath(path,value);
 * int param_2 = obj.get(path);
 */
```

#### C++

```cpp
class Trie {
public:
    unordered_map<string, Trie*> children;
    int v;

    Trie(int v) {
        this->v = v;
    }

    bool insert(string& w, int v) {
        Trie* node = this;
        auto ps = split(w, '/');
        for (int i = 1; i < ps.size() - 1; ++i) {
            auto p = ps[i];
            if (!node->children.count(p)) {
                return false;
            }
            node = node->children[p];
        }
        if (node->children.count(ps.back())) {
            return false;
        }
        node->children[ps.back()] = new Trie(v);
        return true;
    }

    int search(string& w) {
        Trie* node = this;
        auto ps = split(w, '/');
        for (int i = 1; i < ps.size(); ++i) {
            auto p = ps[i];
            if (!node->children.count(p)) {
                return -1;
            }
            node = node->children[p];
        }
        return node->v;
    }

private:
    vector<string> split(string& s, char delim) {
        stringstream ss(s);
        string item;
        vector<string> res;
        while (getline(ss, item, delim)) {
            res.emplace_back(item);
        }
        return res;
    }
};

class FileSystem {
public:
    FileSystem() {
        trie = new Trie(-1);
    }

    bool createPath(string path, int value) {
        return trie->insert(path, value);
    }

    int get(string path) {
        return trie->search(path);
    }

private:
    Trie* trie;
};

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem* obj = new FileSystem();
 * bool param_1 = obj->createPath(path,value);
 * int param_2 = obj->get(path);
 */
```

#### Go

```go
type trie struct {
	children map[string]*trie
	v        int
}

func newTrie(v int) *trie {
	return &trie{map[string]*trie{}, v}
}

func (t *trie) insert(w string, v int) bool {
	node := t
	ps := strings.Split(w, "/")
	for _, p := range ps[1 : len(ps)-1] {
		if _, ok := node.children[p]; !ok {
			return false
		}
		node = node.children[p]
	}
	if _, ok := node.children[ps[len(ps)-1]]; ok {
		return false
	}
	node.children[ps[len(ps)-1]] = newTrie(v)
	return true
}

func (t *trie) search(w string) int {
	node := t
	ps := strings.Split(w, "/")
	for _, p := range ps[1:] {
		if _, ok := node.children[p]; !ok {
			return -1
		}
		node = node.children[p]
	}
	return node.v
}

type FileSystem struct {
	trie *trie
}

func Constructor() FileSystem {
	return FileSystem{trie: newTrie(-1)}
}

func (this *FileSystem) CreatePath(path string, value int) bool {
	return this.trie.insert(path, value)
}

func (this *FileSystem) Get(path string) int {
	return this.trie.search(path)
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.CreatePath(path,value);
 * param_2 := obj.Get(path);
 */
```

#### TypeScript

```ts
class Trie {
    children: Map<string, Trie>;
    v: number;

    constructor(v: number) {
        this.children = new Map<string, Trie>();
        this.v = v;
    }

    insert(w: string, v: number): boolean {
        let node: Trie = this;
        const ps = w.split('/').slice(1);
        for (let i = 0; i < ps.length - 1; ++i) {
            const p = ps[i];
            if (!node.children.has(p)) {
                return false;
            }
            node = node.children.get(p)!;
        }
        if (node.children.has(ps[ps.length - 1])) {
            return false;
        }
        node.children.set(ps[ps.length - 1], new Trie(v));
        return true;
    }

    search(w: string): number {
        let node: Trie = this;
        const ps = w.split('/').slice(1);
        for (const p of ps) {
            if (!node.children.has(p)) {
                return -1;
            }
            node = node.children.get(p)!;
        }
        return node.v;
    }
}

class FileSystem {
    trie: Trie;

    constructor() {
        this.trie = new Trie(-1);
    }

    createPath(path: string, value: number): boolean {
        return this.trie.insert(path, value);
    }

    get(path: string): number {
        return this.trie.search(path);
    }
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * var obj = new FileSystem()
 * var param_1 = obj.createPath(path,value)
 * var param_2 = obj.get(path)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
