---
comments: true
difficulty: Medium
rating: 1544
source: Weekly Contest 159 Q2
tags:
    - Depth-First Search
    - Trie
    - Array
    - String
---

<!-- problem:start -->

# [1233. Remove Sub-Folders from the Filesystem](https://leetcode.com/problems/remove-sub-folders-from-the-filesystem)

[中文文档](/solution/1200-1299/1233.Remove%20Sub-Folders%20from%20the%20Filesystem/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách thư mục <code>folder</code>, hãy trả về các thư mục sau khi loại bỏ mọi <strong>thư mục con</strong>. Bạn có thể trả lời theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Nếu <code>folder[i]</code> nằm bên trong <code>folder[j]</code>, thì nó được gọi là <strong>thư mục con</strong> của <code>folder[j]</code>. Đường dẫn thư mục con của <code>folder[j]</code> phải bắt đầu bằng <code>folder[j]</code>, theo sau là <code>&quot;/&quot;</code>. Ví dụ, <code>&quot;/a/b&quot;</code> là thư mục con của <code>&quot;/a&quot;</code>, nhưng <code>&quot;/b&quot;</code> không phải là thư mục con của <code>&quot;/a/b/c&quot;</code>.</p>

<p>Đường dẫn có định dạng gồm một hoặc nhiều chuỗi nối tiếp nhau, mỗi chuỗi có dạng: <code>&#39;/&#39;</code> theo sau bởi một hoặc nhiều chữ cái tiếng Anh viết thường.</p>

<ul>
	<li>Ví dụ, <code>&quot;/leetcode&quot;</code> và <code>&quot;/leetcode/problems&quot;</code> là các đường dẫn hợp lệ, còn chuỗi rỗng và <code>&quot;/&quot;</code> thì không.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> folder = [&quot;/a&quot;,&quot;/a/b&quot;,&quot;/c/d&quot;,&quot;/c/d/e&quot;,&quot;/c/f&quot;]
<strong>Đầu ra:</strong> [&quot;/a&quot;,&quot;/c/d&quot;,&quot;/c/f&quot;]
<strong>Giải thích:</strong> Thư mục &quot;/a/b&quot; là thư mục con của &quot;/a&quot;, còn &quot;/c/d/e&quot; nằm bên trong thư mục &quot;/c/d&quot; trong filesystem.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> folder = [&quot;/a&quot;,&quot;/a/b/c&quot;,&quot;/a/b/d&quot;]
<strong>Đầu ra:</strong> [&quot;/a&quot;]
<strong>Giải thích:</strong> Các thư mục &quot;/a/b/c&quot; và &quot;/a/b/d&quot; sẽ bị loại bỏ vì chúng là thư mục con của &quot;/a&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> folder = [&quot;/a/b/c&quot;,&quot;/a/b/ca&quot;,&quot;/a/b/d&quot;]
<strong>Đầu ra:</strong> [&quot;/a/b/c&quot;,&quot;/a/b/ca&quot;,&quot;/a/b/d&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= folder.length &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>2 &lt;= folder[i].length &lt;= 100</code></li>
	<li><code>folder[i]</code> contains only lowercase letters and <code>&#39;/&#39;</code>.</li>
	<li><code>folder[i]</code> always starts with the character <code>&#39;/&#39;</code>.</li>
	<li>Tên mỗi thư mục là <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Thư mục con đứng sau thư mục cha theo thứ tự từ điển và bắt đầu bằng đường dẫn thư mục cha cộng thêm $/$. $n$ có thể lên đến $4\times 10^4$, nên kiểm tra tiền tố theo từng cặp sẽ quá chậm.
>
> Sau khi sắp xếp, ta chỉ cần so sánh với thư mục được giữ lại gần nhất: bỏ qua thư mục hậu duệ, còn nếu không thì đường dẫn đó là một thư mục gốc mới. Việc sắp xếp đưa quan hệ cha-con về quan hệ tiền tố giữa các phần tử liền kề, nên chỉ cần duyệt một lần.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng `folder` theo thứ tự từ điển rồi duyệt mảng. Với thư mục hiện tại $f$, nếu độ dài của nó lớn hơn hoặc bằng độ dài của thư mục cuối cùng trong mảng kết quả, đồng thời tiền tố của nó gồm thư mục cuối cùng đó theo sau bởi `/`, thì $f$ là thư mục con và không cần thêm vào kết quả. Nếu không, ta thêm $f$ vào mảng kết quả.

Sau khi duyệt xong, các thư mục trong mảng kết quả chính là đáp án.

Độ phức tạp thời gian là $O(n \times \log n \times m)$, còn độ phức tạp không gian là $O(m)$. Trong đó, $n$ là độ dài mảng `folder`, còn $m$ là độ dài lớn nhất của các chuỗi trong `folder`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeSubfolders(self, folder: List[str]) -> List[str]:
        folder.sort()
        ans = [folder[0]]
        for f in folder[1:]:
            m, n = len(ans[-1]), len(f)
            if m >= n or not (ans[-1] == f[:m] and f[m] == '/'):
                ans.append(f)
        return ans
```

#### Java

```java
class Solution {
    public List<String> removeSubfolders(String[] folder) {
        Arrays.sort(folder);
        List<String> ans = new ArrayList<>();
        ans.add(folder[0]);
        for (int i = 1; i < folder.length; ++i) {
            int m = ans.get(ans.size() - 1).length();
            int n = folder[i].length();
            if (m >= n
                || !(ans.get(ans.size() - 1).equals(folder[i].substring(0, m))
                    && folder[i].charAt(m) == '/')) {
                ans.add(folder[i]);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        sort(folder.begin(), folder.end());
        vector<string> ans = {folder[0]};
        for (int i = 1; i < folder.size(); ++i) {
            int m = ans.back().size();
            int n = folder[i].size();
            if (m >= n || !(ans.back() == folder[i].substr(0, m) && folder[i][m] == '/')) {
                ans.emplace_back(folder[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeSubfolders(folder []string) []string {
	sort.Strings(folder)
	ans := []string{folder[0]}
	for _, f := range folder[1:] {
		m, n := len(ans[len(ans)-1]), len(f)
		if m >= n || !(ans[len(ans)-1] == f[:m] && f[m] == '/') {
			ans = append(ans, f)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function removeSubfolders(folder: string[]): string[] {
    let s = folder[1];
    return folder.sort().filter(x => !x.startsWith(s + '/') && (s = x));
}
```

#### JavaScript

```js
function removeSubfolders(folder) {
    let s = folder[1];
    return folder.sort().filter(x => !x.startsWith(s + '/') && (s = x));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Trie

<!-- thinking:start -->

> **Tư duy**
>
> Cách sắp xếp phụ thuộc vào thứ tự từ điển toàn cục và phải so sánh tiền tố của chuỗi. Trie chèn từng segment của đường dẫn và đánh dấu điểm kết thúc thư mục; khi tìm kiếm gặp một điểm kết thúc, ta có thể bỏ qua toàn bộ subtree bên dưới vì đó là các thư mục lồng bên trong. Độ phức tạp thời gian phụ thuộc vào tổng độ dài các đường dẫn và không cần sắp xếp.

<!-- thinking:end -->

Ta có thể dùng trie để lưu tất cả thư mục trong mảng `folder`. Mỗi node của trie có trường `children` để lưu các node con và trường `fid` để lưu chỉ số của thư mục tương ứng với node hiện tại trong mảng `folder`.

Với mỗi thư mục $f$ trong mảng `folder`, trước tiên ta tách $f$ thành các chuỗi con theo dấu `/`, sau đó bắt đầu từ node gốc và lần lượt thêm các chuỗi con vào trie. Tiếp theo, ta tìm kiếm trie từ node gốc. Nếu trường `fid` của node hiện tại khác `-1`, thư mục tương ứng với node đó thuộc kết quả; ta thêm nó vào mảng kết quả rồi dừng nhánh tìm kiếm này. Nếu không, ta đệ quy tìm kiếm tất cả node con của node hiện tại, rồi trả về mảng kết quả.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(n \times m)$. Trong đó, $n$ là độ dài mảng `folder`, còn $m$ là độ dài lớn nhất của các chuỗi trong `folder`.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = {}
        self.fid = -1

    def insert(self, i, f):
        node = self
        ps = f.split('/')
        for p in ps[1:]:
            if p not in node.children:
                node.children[p] = Trie()
            node = node.children[p]
        node.fid = i

    def search(self):
        def dfs(root):
            if root.fid != -1:
                ans.append(root.fid)
                return
            for child in root.children.values():
                dfs(child)

        ans = []
        dfs(self)
        return ans


class Solution:
    def removeSubfolders(self, folder: List[str]) -> List[str]:
        trie = Trie()
        for i, f in enumerate(folder):
            trie.insert(i, f)
        return [folder[i] for i in trie.search()]
```

#### Java

```java
class Trie {
    private Map<String, Trie> children = new HashMap<>();
    private int fid = -1;

    public void insert(int fid, String f) {
        Trie node = this;
        String[] ps = f.split("/");
        for (int i = 1; i < ps.length; ++i) {
            String p = ps[i];
            if (!node.children.containsKey(p)) {
                node.children.put(p, new Trie());
            }
            node = node.children.get(p);
        }
        node.fid = fid;
    }

    public List<Integer> search() {
        List<Integer> ans = new ArrayList<>();
        dfs(this, ans);
        return ans;
    }

    private void dfs(Trie root, List<Integer> ans) {
        if (root.fid != -1) {
            ans.add(root.fid);
            return;
        }
        for (var child : root.children.values()) {
            dfs(child, ans);
        }
    }
}

class Solution {
    public List<String> removeSubfolders(String[] folder) {
        Trie trie = new Trie();
        for (int i = 0; i < folder.length; ++i) {
            trie.insert(i, folder[i]);
        }
        List<String> ans = new ArrayList<>();
        for (int i : trie.search()) {
            ans.add(folder[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
public:
    void insert(int fid, string& f) {
        Trie* node = this;
        vector<string> ps = split(f, '/');
        for (int i = 1; i < ps.size(); ++i) {
            auto& p = ps[i];
            if (!node->children.count(p)) {
                node->children[p] = new Trie();
            }
            node = node->children[p];
        }
        node->fid = fid;
    }

    vector<int> search() {
        vector<int> ans;
        function<void(Trie*)> dfs = [&](Trie* root) {
            if (root->fid != -1) {
                ans.push_back(root->fid);
                return;
            }
            for (auto& [_, child] : root->children) {
                dfs(child);
            }
        };
        dfs(this);
        return ans;
    }

    vector<string> split(string& s, char delim) {
        stringstream ss(s);
        string item;
        vector<string> res;
        while (getline(ss, item, delim)) {
            res.emplace_back(item);
        }
        return res;
    }

private:
    unordered_map<string, Trie*> children;
    int fid = -1;
};

class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        Trie* trie = new Trie();
        for (int i = 0; i < folder.size(); ++i) {
            trie->insert(i, folder[i]);
        }
        vector<string> ans;
        for (int i : trie->search()) {
            ans.emplace_back(folder[i]);
        }
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children map[string]*Trie
	fid      int
}

func newTrie() *Trie {
	return &Trie{map[string]*Trie{}, -1}
}

func (this *Trie) insert(fid int, f string) {
	node := this
	ps := strings.Split(f, "/")
	for _, p := range ps[1:] {
		if _, ok := node.children[p]; !ok {
			node.children[p] = newTrie()
		}
		node = node.children[p]
	}
	node.fid = fid
}

func (this *Trie) search() (ans []int) {
	var dfs func(*Trie)
	dfs = func(root *Trie) {
		if root.fid != -1 {
			ans = append(ans, root.fid)
			return
		}
		for _, child := range root.children {
			dfs(child)
		}
	}
	dfs(this)
	return
}

func removeSubfolders(folder []string) (ans []string) {
	trie := newTrie()
	for i, f := range folder {
		trie.insert(i, f)
	}
	for _, i := range trie.search() {
		ans = append(ans, folder[i])
	}
	return
}
```

#### TypeScript

```ts
class Trie {
    children: Record<string, Trie>;
    fid: number;

    constructor() {
        this.children = {};
        this.fid = -1;
    }

    insert(i: number, f: string): void {
        let node: Trie = this;
        const ps = f.split('/');
        for (let j = 1; j < ps.length; ++j) {
            const p = ps[j];
            if (!(p in node.children)) {
                node.children[p] = new Trie();
            }
            node = node.children[p];
        }
        node.fid = i;
    }

    search(): number[] {
        const ans: number[] = [];
        const dfs = (root: Trie): void => {
            if (root.fid !== -1) {
                ans.push(root.fid);
                return;
            }
            for (const child of Object.values(root.children)) {
                dfs(child);
            }
        };
        dfs(this);
        return ans;
    }
}

function removeSubfolders(folder: string[]): string[] {
    const trie = new Trie();
    for (let i = 0; i < folder.length; ++i) {
        trie.insert(i, folder[i]);
    }
    return trie.search().map(i => folder[i]);
}
```

#### JavaScript

```js
class Trie {
    constructor() {
        this.children = {};
        this.fid = -1;
    }

    insert(i, f) {
        let node = this;
        const ps = f.split('/');
        for (let j = 1; j < ps.length; ++j) {
            const p = ps[j];
            if (!(p in node.children)) {
                node.children[p] = new Trie();
            }
            node = node.children[p];
        }
        node.fid = i;
    }

    search() {
        const ans = [];
        const dfs = root => {
            if (root.fid !== -1) {
                ans.push(root.fid);
                return;
            }
            for (const child of Object.values(root.children)) {
                dfs(child);
            }
        };
        dfs(this);
        return ans;
    }
}

/**
 * @param {string[]} folder
 * @return {string[]}
 */
var removeSubfolders = function (folder) {
    const trie = new Trie();
    for (let i = 0; i < folder.length; ++i) {
        trie.insert(i, folder[i]);
    }
    return trie.search().map(i => folder[i]);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
