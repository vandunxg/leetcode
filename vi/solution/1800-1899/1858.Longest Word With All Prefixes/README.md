---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Trie
    - Array
    - String
---

<!-- problem:start -->

# [1858. Longest Word With All Prefixes 🔒](https://leetcode.com/problems/longest-word-with-all-prefixes)

[中文文档](/solution/1800-1899/1858.Longest%20Word%20With%20All%20Prefixes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code>, tìm chuỗi <strong>dài nhất</strong> trong <code>words</code> sao cho <strong>mọi tiền tố</strong> của nó cũng nằm trong <code>words</code>.</p>

<ul>
	<li>Ví dụ, với <code>words = [&quot;a&quot;, &quot;app&quot;, &quot;ap&quot;]</code>. Chuỗi <code>&quot;app&quot;</code> có các tiền tố là <code>&quot;ap&quot;</code> và <code>&quot;a&quot;</code>, tất cả đều nằm trong <code>words</code>.</li>
</ul>

<p>Trả về <em>chuỗi được mô tả ở trên. Nếu có nhiều chuỗi có cùng độ dài, trả về chuỗi <strong>nhỏ nhất theo thứ tự từ điển</strong>; nếu không tồn tại chuỗi nào, trả về </em><code>&quot;&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;k&quot;,&quot;ki&quot;,&quot;kir&quot;,&quot;kira&quot;, &quot;kiran&quot;]
<strong>Đầu ra:</strong> &quot;kiran&quot;
<strong>Giải thích:</strong> &quot;kiran&quot; có các tiền tố &quot;kira&quot;, &quot;kir&quot;, &quot;ki&quot; và &quot;k&quot;, tất cả đều xuất hiện trong words.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;, &quot;banana&quot;, &quot;app&quot;, &quot;appl&quot;, &quot;ap&quot;, &quot;apply&quot;, &quot;apple&quot;]
<strong>Đầu ra:</strong> &quot;apple&quot;
<strong>Giải thích:</strong> Cả &quot;apple&quot; và &quot;apply&quot; đều có tất cả tiền tố nằm trong words.
Tuy nhiên, &quot;apple&quot; nhỏ hơn theo thứ tự từ điển, nên ta trả về chuỗi này.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abc&quot;, &quot;bc&quot;, &quot;ab&quot;, &quot;qwe&quot;]
<strong>Đầu ra:</strong> &quot;&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= sum(words[i].length) &lt;= 10<sup>5</sup></code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie

<!-- thinking:start -->

> **Tư duy**
>
> Ta muốn tìm từ dài nhất mà mọi tiền tố của nó cũng nằm trong danh sách, nếu hòa thì chọn theo thứ tự từ điển. Kiểm tra lại danh sách cho từng tiền tố sẽ khiến tổng độ dài bị bình phương.
>
> Chèn mọi từ vào trie và đánh dấu các node kết thúc. Một từ hợp lệ khi và chỉ khi mọi node trên đường đi của nó đều là node kết thúc. So sánh các từ hợp lệ theo độ dài, sau đó theo thứ tự từ điển.

<!-- thinking:end -->

Ta định nghĩa một Trie, trong đó mỗi node có hai thuộc tính: mảng node con $\textit{children}$ có độ dài $26$ và cờ $\textit{isEnd}$ cho biết node có đánh dấu kết thúc một từ hay không.

Ta duyệt $\textit{words}$; với mỗi từ $w$, ta đi từ node gốc. Nếu mảng node con của node hiện tại không chứa ký tự đầu tiên của $w$, ta tạo một node mới, sau đó tiếp tục đi qua ký tự tiếp theo của $w$. Sau khi đi qua tất cả ký tự của $w$, ta đặt cờ $\textit{isEnd}$ của node hiện tại thành $\texttt{true}$.

Tiếp theo, ta duyệt $\textit{words}$ một lần nữa. Với mỗi từ $w$, ta đi từ node gốc. Nếu trường $\textit{isEnd}$ của một node trong mảng node con là $\texttt{false}$, điều đó có nghĩa một tiền tố nào đó của $w$ không nằm trong $\textit{words}$, và ta trả về $\texttt{false}$. Ngược lại, ta tiếp tục đi qua ký tự tiếp theo của $w$; sau khi đi qua tất cả ký tự, ta trả về $\texttt{true}$.

Độ phức tạp thời gian là $O(\sum_{w \in \textit{words}} |w|)$, độ phức tạp không gian là $O(\sum_{w \in \textit{words}} |w|)$, trong đó $|w|$ là độ dài của từ $w$.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    __slots__ = ["children", "is_end"]

    def __init__(self):
        self.children: List[Trie | None] = [None] * 26
        self.is_end: bool = False

    def insert(self, w: str) -> None:
        node = self
        for c in w:
            idx = ord(c) - ord("a")
            if not node.children[idx]:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True

    def search(self, w: str) -> bool:
        node = self
        for c in w:
            idx = ord(c) - ord("a")
            node = node.children[idx]
            if not node.is_end:
                return False
        return True


class Solution:
    def longestWord(self, words: List[str]) -> str:
        trie = Trie()
        for w in words:
            trie.insert(w)
        ans = ""
        for w in words:
            if (len(w) > len(ans) or len(w) == len(ans) and w < ans) and trie.search(w):
                ans = w
        return ans
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private boolean isEnd;

    public Trie() {
    }

    public void insert(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    public boolean search(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            int idx = c - 'a';
            node = node.children[idx];
            if (!node.isEnd) {
                return false;
            }
        }
        return true;
    }
}

class Solution {
    public String longestWord(String[] words) {
        Trie trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        String ans = "";
        for (String w : words) {
            if ((w.length() > ans.length() || (w.length() == ans.length() && w.compareTo(ans) < 0))
                && trie.search(w)) {
                ans = w;
            }
        }
        return ans;
    }
}
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
            int idx = c - 'a';
            if (!node->children[idx]) {
                node->children[idx] = new Trie();
            }
            node = node->children[idx];
        }
        node->isEnd = true;
    }

    bool search(const string& w) {
        Trie* node = this;
        for (char c : w) {
            int idx = c - 'a';
            node = node->children[idx];
            if (!node->isEnd) {
                return false;
            }
        }
        return true;
    }
};

class Solution {
public:
    string longestWord(vector<string>& words) {
        Trie trie;
        for (const string& w : words) {
            trie.insert(w);
        }
        string ans = "";
        for (const string& w : words) {
            if ((w.size() > ans.size() || (w.size() == ans.size() && w < ans)) && trie.search(w)) {
                ans = w;
            }
        }
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children [26]*Trie
	isEnd    bool
}

func newTrie() *Trie {
	return &Trie{}
}

func (t *Trie) insert(w string) {
	node := t
	for _, c := range w {
		idx := c - 'a'
		if node.children[idx] == nil {
			node.children[idx] = newTrie()
		}
		node = node.children[idx]
	}
	node.isEnd = true
}

func (t *Trie) search(w string) bool {
	node := t
	for _, c := range w {
		idx := c - 'a'
		node = node.children[idx]
		if !node.isEnd {
			return false
		}
	}
	return true
}

func longestWord(words []string) string {
	trie := newTrie()
	for _, w := range words {
		trie.insert(w)
	}
	ans := ""
	for _, w := range words {
		if (len(w) > len(ans) || (len(w) == len(ans) && w < ans)) && trie.search(w) {
			ans = w
		}
	}
	return ans
}
```

#### TypeScript

```ts
class Trie {
    private children: (Trie | null)[] = Array(26).fill(null);
    private isEnd: boolean = false;

    insert(w: string): void {
        let node: Trie = this;
        for (const c of w) {
            const idx: number = c.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[idx]) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx] as Trie;
        }
        node.isEnd = true;
    }

    search(w: string): boolean {
        let node: Trie = this;
        for (const c of w) {
            const idx: number = c.charCodeAt(0) - 'a'.charCodeAt(0);
            node = node.children[idx] as Trie;
            if (!node.isEnd) {
                return false;
            }
        }
        return true;
    }
}

function longestWord(words: string[]): string {
    const trie: Trie = new Trie();
    for (const w of words) {
        trie.insert(w);
    }
    let ans: string = '';
    for (const w of words) {
        if ((w.length > ans.length || (w.length === ans.length && w < ans)) && trie.search(w)) {
            ans = w;
        }
    }
    return ans;
}
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
            let idx = (c as usize) - ('a' as usize);
            node = node.children[idx].get_or_insert_with(|| Box::new(Trie::new()));
        }
        node.is_end = true;
    }

    fn search(&self, w: &str) -> bool {
        let mut node = self;
        for c in w.chars() {
            let idx = (c as usize) - ('a' as usize);
            if let Some(next_node) = &node.children[idx] {
                node = next_node.as_ref();
                if !node.is_end {
                    return false;
                }
            }
        }
        true
    }
}

impl Solution {
    pub fn longest_word(words: Vec<String>) -> String {
        let mut trie = Trie::new();
        for w in &words {
            trie.insert(w);
        }
        let mut ans = String::new();
        for w in &words {
            if (w.len() > ans.len() || (w.len() == ans.len() && w < &ans)) && trie.search(w) {
                ans = w.clone();
            }
        }
        ans
    }
}
```

#### JavaScript

```js
class Trie {
    constructor() {
        this.children = new Array(26).fill(null);
        this.isEnd = false;
    }

    insert(w) {
        let node = this;
        for (const c of w) {
            const idx = c.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[idx]) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    search(w) {
        let node = this;
        for (const c of w) {
            const idx = c.charCodeAt(0) - 'a'.charCodeAt(0);
            node = node.children[idx];
            if (!node || !node.isEnd) {
                return false;
            }
        }
        return true;
    }
}

/**
 * @param {string[]} words
 * @return {string}
 */
var longestWord = function (words) {
    const trie = new Trie();
    for (const w of words) {
        trie.insert(w);
    }

    let ans = '';
    for (const w of words) {
        if ((w.length > ans.length || (w.length === ans.length && w < ans)) && trie.search(w)) {
            ans = w;
        }
    }
    return ans;
};
```

#### C#

```cs
public class Trie {
    private Trie[] children = new Trie[26];
    private bool isEnd;

    public Trie() { }

    public void Insert(string w) {
        Trie node = this;
        foreach (char c in w.ToCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    public bool Search(string w) {
        Trie node = this;
        foreach (char c in w.ToCharArray()) {
            int idx = c - 'a';
            node = node.children[idx];
            if (!node.isEnd) {
                return false;
            }
        }
        return true;
    }
}

public class Solution {
    public string LongestWord(string[] words) {
        Trie trie = new Trie();
        foreach (string w in words) {
            trie.Insert(w);
        }

        string ans = "";
        foreach (string w in words) {
            if ((w.Length > ans.Length || (w.Length == ans.Length && string.Compare(w, ans) < 0)) && trie.Search(w)) {
                ans = w;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
