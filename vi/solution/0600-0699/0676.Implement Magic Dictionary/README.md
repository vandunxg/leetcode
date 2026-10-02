---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Design
    - Trie
    - Hash Table
    - String
---

<!-- problem:start -->

# [676. Implement Magic Dictionary](https://leetcode.com/problems/implement-magic-dictionary)

[中文文档](/solution/0600-0699/0676.Implement%20Magic%20Dictionary/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một data structure được khởi tạo từ danh sách các từ <strong>khác nhau</strong>. Khi được cung cấp một chuỗi, hãy xác định liệu có thể thay đổi chính xác một ký tự trong chuỗi đó để khớp với một từ bất kỳ trong data structure hay không.</p>

<p>Hãy triển khai class&nbsp;<code>MagicDictionary</code>:</p>

<ul>
	<li><code>MagicDictionary()</code>&nbsp;Khởi tạo object.</li>
	<li><code>void buildDict(String[]&nbsp;dictionary)</code>&nbsp;Khởi tạo data structure bằng mảng các chuỗi khác nhau <code>dictionary</code>.</li>
	<li><code>bool search(String searchWord)</code> Trả về <code>true</code> nếu có thể thay đổi <strong>chính xác một ký tự</strong> trong <code>searchWord</code> để khớp với một chuỗi bất kỳ trong data structure; nếu không thì trả về <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MagicDictionary&quot;, &quot;buildDict&quot;, &quot;search&quot;, &quot;search&quot;, &quot;search&quot;, &quot;search&quot;]
[[], [[&quot;hello&quot;, &quot;leetcode&quot;]], [&quot;hello&quot;], [&quot;hhllo&quot;], [&quot;hell&quot;], [&quot;leetcoded&quot;]]
<strong>Đầu ra</strong>
[null, null, false, true, false, false]

<strong>Giải thích</strong>
MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict([&quot;hello&quot;, &quot;leetcode&quot;]);
magicDictionary.search(&quot;hello&quot;); // return False
magicDictionary.search(&quot;hhllo&quot;); // We can change the second &#39;h&#39; to &#39;e&#39; to match &quot;hello&quot; so we return True
magicDictionary.search(&quot;hell&quot;); // return False
magicDictionary.search(&quot;leetcoded&quot;); // return False
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;dictionary.length &lt;= 100</code></li>
	<li><code>1 &lt;=&nbsp;dictionary[i].length &lt;= 100</code></li>
	<li><code>dictionary[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả chuỗi trong&nbsp;<code>dictionary</code>&nbsp;đều <strong>khác nhau</strong>.</li>
	<li><code>1 &lt;=&nbsp;searchWord.length &lt;= 100</code></li>
	<li><code>searchWord</code>&nbsp;chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>buildDict</code>&nbsp;chỉ được gọi một lần trước <code>search</code>.</li>
	<li><code>search</code> được gọi nhiều nhất <code>100</code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Một query phải khác một từ trong dictionary đúng một ký tự. Quét từng từ cho mỗi query sẽ lặp lại việc xử lý các prefix.
>
> Lưu các từ trong trie. DFS đi theo nhánh có cùng ký tự nếu có thể, hoặc dùng lần thay đổi duy nhất để chọn một node con khác. Tìm thấy từ khi kết thúc từ với $\textit{diff}=1$.

<!-- thinking:end -->

Ta có thể dùng trie để lưu các từ trong dictionary. Với mỗi từ cần tìm, ta dùng depth-first search (DFS), bắt đầu từ root của trie. Ở mỗi ký tự, trước tiên kiểm tra xem có node con tương ứng với chính ký tự đó hay không. Nếu có, tiếp tục duyệt xuống node con. Nếu không, kiểm tra xem còn lượt thay đổi nào không. Nếu không còn thì không thể khớp và trả về false. Nếu vẫn còn lượt thay đổi, thử thay ký tự hiện tại rồi tiếp tục duyệt xuống. Nếu tồn tại node con tương ứng với ký tự thay thế thì có thể khớp; ngược lại, trả về false. Nếu duyệt đến cuối từ và số lần thay đổi đúng bằng 1 thì trả về true.

Độ phức tạp thời gian là $O(n \times l + q \times l \times |\Sigma|)$, độ phức tạp không gian là $O(n \times l)$; trong đó $n$ là số từ trong dictionary, $l$ là độ dài trung bình của các từ và $q$ là số từ cần tìm. Ngoài ra, $|\Sigma|$ là kích thước của bộ ký tự. Ở đây bộ ký tự gồm các chữ cái tiếng Anh viết thường nên $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    __slots__ = "children", "is_end"

    def __init__(self):
        self.children: List[Optional[Trie]] = [None] * 26
        self.is_end = False

    def insert(self, w: str) -> None:
        node = self
        for c in w:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True

    def search(self, w: str) -> bool:
        def dfs(i: int, node: Optional[Trie], diff: int) -> bool:
            if i == len(w):
                return diff == 1 and node.is_end
            j = ord(w[i]) - ord("a")
            if node.children[j] and dfs(i + 1, node.children[j], diff):
                return True
            return diff == 0 and any(
                node.children[k] and dfs(i + 1, node.children[k], 1)
                for k in range(26)
                if k != j
            )

        return dfs(0, self, 0)


class MagicDictionary:
    def __init__(self):
        self.trie = Trie()

    def buildDict(self, dictionary: List[str]) -> None:
        for w in dictionary:
            self.trie.insert(w)

    def search(self, searchWord: str) -> bool:
        return self.trie.search(searchWord)


# Your MagicDictionary object will be instantiated and called as such:
# obj = MagicDictionary()
# obj.buildDict(dictionary)
# param_2 = obj.search(searchWord)
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private boolean isEnd;

    public void insert(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            int i = c - 'a';
            if (node.children[i] == null) {
                node.children[i] = new Trie();
            }
            node = node.children[i];
        }
        node.isEnd = true;
    }

    public boolean search(String w) {
        return dfs(w, 0, this, 0);
    }

    private boolean dfs(String w, int i, Trie node, int diff) {
        if (i == w.length()) {
            return diff == 1 && node.isEnd;
        }
        int j = w.charAt(i) - 'a';
        if (node.children[j] != null) {
            if (dfs(w, i + 1, node.children[j], diff)) {
                return true;
            }
        }
        if (diff == 0) {
            for (int k = 0; k < 26; k++) {
                if (k != j && node.children[k] != null) {
                    if (dfs(w, i + 1, node.children[k], 1)) {
                        return true;
                    }
                }
            }
        }
        return false;
    }
}

class MagicDictionary {
    private Trie trie = new Trie();

    public MagicDictionary() {
    }

    public void buildDict(String[] dictionary) {
        for (String w : dictionary) {
            trie.insert(w);
        }
    }

    public boolean search(String searchWord) {
        return trie.search(searchWord);
    }
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary obj = new MagicDictionary();
 * obj.buildDict(dictionary);
 * boolean param_2 = obj.search(searchWord);
 */
```

#### C++

```cpp
class Trie {
private:
    Trie* children[26];
    bool isEnd = false;

public:
    Trie() {
        fill(begin(children), end(children), nullptr);
    }

    void insert(const string& w) {
        Trie* node = this;
        for (char c : w) {
            int i = c - 'a';
            if (!node->children[i]) {
                node->children[i] = new Trie();
            }
            node = node->children[i];
        }
        node->isEnd = true;
    }

    bool search(const string& w) {
        function<bool(int, Trie*, int)> dfs = [&](int i, Trie* node, int diff) {
            if (i >= w.size()) {
                return diff == 1 && node->isEnd;
            }
            int j = w[i] - 'a';
            if (node->children[j] && dfs(i + 1, node->children[j], diff)) {
                return true;
            }
            if (diff == 0) {
                for (int k = 0; k < 26; ++k) {
                    if (k != j && node->children[k]) {
                        if (dfs(i + 1, node->children[k], 1)) {
                            return true;
                        }
                    }
                }
            }
            return false;
        };
        return dfs(0, this, 0);
    }
};

class MagicDictionary {
public:
    MagicDictionary() {
        trie = new Trie();
    }

    void buildDict(vector<string> dictionary) {
        for (auto& w : dictionary) {
            trie->insert(w);
        }
    }

    bool search(string searchWord) {
        return trie->search(searchWord);
    }

private:
    Trie* trie;
};

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary* obj = new MagicDictionary();
 * obj->buildDict(dictionary);
 * bool param_2 = obj->search(searchWord);
 */
```

#### Go

```go
type Trie struct {
	children [26]*Trie
	isEnd    bool
}

func NewTrie() *Trie {
	return &Trie{}
}

func (t *Trie) Insert(w string) {
	node := t
	for _, c := range w {
		i := c - 'a'
		if node.children[i] == nil {
			node.children[i] = NewTrie()
		}
		node = node.children[i]
	}
	node.isEnd = true
}

func (t *Trie) Search(w string) bool {
	var dfs func(int, *Trie, int) bool
	dfs = func(i int, node *Trie, diff int) bool {
		if i >= len(w) {
			return diff == 1 && node.isEnd
		}
		j := int(w[i] - 'a')
		if node.children[j] != nil && dfs(i+1, node.children[j], diff) {
			return true
		}
		if diff == 0 {
			for k := 0; k < 26; k++ {
				if k != j && node.children[k] != nil && dfs(i+1, node.children[k], 1) {
					return true
				}
			}
		}
		return false
	}
	return dfs(0, t, 0)
}

type MagicDictionary struct {
	trie *Trie
}

func Constructor() MagicDictionary {
	return MagicDictionary{trie: NewTrie()}
}

func (md *MagicDictionary) BuildDict(dictionary []string) {
	for _, w := range dictionary {
		md.trie.Insert(w)
	}
}

func (md *MagicDictionary) Search(searchWord string) bool {
	return md.trie.Search(searchWord)
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * obj := Constructor();
 * obj.BuildDict(dictionary);
 * param_2 := obj.Search(searchWord);
 */
```

#### TypeScript

```ts
class Trie {
    private children: Trie[] = Array(26).fill(null);
    private isEnd: boolean = false;

    constructor() {}

    insert(w: string): void {
        let node: Trie = this;
        for (const c of w) {
            const i: number = c.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[i]) {
                node.children[i] = new Trie();
            }
            node = node.children[i];
        }
        node.isEnd = true;
    }

    search(w: string): boolean {
        const dfs = (i: number, node: Trie, diff: number): boolean => {
            if (i >= w.length) {
                return diff === 1 && node.isEnd;
            }
            const j: number = w.charCodeAt(i) - 'a'.charCodeAt(0);
            if (node.children[j] && dfs(i + 1, node.children[j], diff)) {
                return true;
            }
            if (diff === 0) {
                for (let k = 0; k < 26; k++) {
                    if (k !== j && node.children[k] && dfs(i + 1, node.children[k], 1)) {
                        return true;
                    }
                }
            }
            return false;
        };
        return dfs(0, this, 0);
    }
}

class MagicDictionary {
    private trie: Trie;

    constructor() {
        this.trie = new Trie();
    }

    buildDict(dictionary: string[]): void {
        for (const w of dictionary) {
            this.trie.insert(w);
        }
    }

    search(searchWord: string): boolean {
        return this.trie.search(searchWord);
    }
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * var obj = new MagicDictionary()
 * obj.buildDict(dictionary)
 * var param_2 = obj.search(searchWord)
 */
```

#### Rust

```rust
struct Trie {
    children: [Option<Box<Trie>>; 26],
    is_end: bool,
}

impl Trie {
    fn new() -> Self {
        Trie {
            children: Default::default(),
            is_end: false,
        }
    }

    fn insert(&mut self, w: &str) {
        let mut node = self;
        for c in w.chars() {
            let i = (c as usize) - ('a' as usize);
            if node.children[i].is_none() {
                node.children[i] = Some(Box::new(Trie::new()));
            }
            node = node.children[i].as_mut().unwrap();
        }
        node.is_end = true;
    }

    fn search(&self, w: &str) -> bool {
        self.dfs(w, 0, 0)
    }

    fn dfs(&self, w: &str, i: usize, diff: usize) -> bool {
        if i == w.len() {
            return diff == 1 && self.is_end;
        }

        let j = (w.chars().nth(i).unwrap() as usize) - ('a' as usize);

        if let Some(child) = &self.children[j] {
            if child.dfs(w, i + 1, diff) {
                return true;
            }
        }

        if diff == 0 {
            for k in 0..26 {
                if k != j {
                    if let Some(child) = &self.children[k] {
                        if child.dfs(w, i + 1, 1) {
                            return true;
                        }
                    }
                }
            }
        }
        false
    }
}

struct MagicDictionary {
    trie: Trie,
}

impl MagicDictionary {
    fn new() -> Self {
        MagicDictionary {
            trie: Trie::new(),
        }
    }

    fn build_dict(&mut self, dictionary: Vec<String>) {
        for w in dictionary {
            self.trie.insert(&w);
        }
    }

    fn search(&self, search_word: String) -> bool {
        self.trie.search(&search_word)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
