---
comments: true
difficulty: Medium
tags:
    - Trie
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [720. Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary)

[中文文档](/solution/0700-0799/0720.Longest%20Word%20in%20Dictionary/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code> đại diện cho một từ điển tiếng Anh. Hãy trả về <em>từ dài nhất trong</em> <code>words</code> <em>có thể được tạo lần lượt từng ký tự từ các từ khác trong</em> <code>words</code>.</p>

<p>Nếu có nhiều đáp án, chọn từ dài nhất có thứ tự từ điển nhỏ nhất. Nếu không có đáp án, trả về chuỗi rỗng.</p>

<p>Lưu ý, từ phải được tạo từ trái sang phải, mỗi ký tự mới được thêm vào cuối từ trước đó.&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;w&quot;,&quot;wo&quot;,&quot;wor&quot;,&quot;worl&quot;,&quot;world&quot;]
<strong>Đầu ra:</strong> &quot;world&quot;
<strong>Giải thích:</strong> Từ &quot;world&quot; có thể được tạo lần lượt từng ký tự từ &quot;w&quot;, &quot;wo&quot;, &quot;wor&quot; và &quot;worl&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;banana&quot;,&quot;app&quot;,&quot;appl&quot;,&quot;ap&quot;,&quot;apply&quot;,&quot;apple&quot;]
<strong>Đầu ra:</strong> &quot;apple&quot;
<strong>Giải thích:</strong> Cả &quot;apply&quot; và &quot;apple&quot; đều có thể được tạo từ các từ khác trong từ điển. Tuy nhiên, &quot;apple&quot; có thứ tự từ điển nhỏ hơn &quot;apply&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 30</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm từ dài nhất có thể được tạo từng ký tự một từ các từ khác trong từ điển; nếu hòa thì chọn từ có thứ tự từ điển nhỏ hơn. Dữ liệu đầu vào nhỏ, nhưng việc kiểm tra từng prefix bằng set dễ trở nên rườm rà.
>
> Điều kiện này tương đương với việc mọi prefix của từ đều có trong từ điển. Sau khi chèn tất cả từ vào trie, chỉ cần duyệt một lượt và yêu cầu mỗi node đi qua đều kết thúc một từ.
>
> Xây dựng trie, sau đó dùng $\textit{search}$ kiểm tra từng từ và giữ lại từ dài hơn, hoặc từ có thứ tự từ điển nhỏ hơn nếu hai từ bằng độ dài. Thời gian chạy tuyến tính theo tổng số ký tự.

<!-- thinking:end -->

Ta có thể dùng trie để lưu tất cả các từ, rồi duyệt từng từ để xác định liệu từ đó có thể được tạo từng ký tự một từ các từ khác trong trie hay không. Tìm từ dài nhất thỏa mãn điều kiện; nếu hòa, chọn từ có thứ tự từ điển nhỏ nhất.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các từ.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children: List[Optional[Trie]] = [None] * 26
        self.is_end = False

    def insert(self, w: str):
        node = self
        for c in w:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True

    def search(self, w: str) -> bool:
        node = self
        for c in w:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                return False
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
            if trie.search(w) and (
                len(ans) < len(w) or (len(ans) == len(w) and ans > w)
            ):
                ans = w
        return ans
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private boolean isEnd = false;

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
            if (node.children[idx] == null || !node.children[idx].isEnd) {
                return false;
            }
            node = node.children[idx];
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
            if (trie.search(w)
                && (ans.length() < w.length()
                    || (ans.length() == w.length() && w.compareTo(ans) < 0))) {
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
public:
    Trie* children[26] = {nullptr};
    bool isEnd = false;

    void insert(const string& w) {
        Trie* node = this;
        for (char c : w) {
            int idx = c - 'a';
            if (node->children[idx] == nullptr) {
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
            if (node->children[idx] == nullptr || !node->children[idx]->isEnd) {
                return false;
            }
            node = node->children[idx];
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
            if (trie.search(w) && (ans.length() < w.length() || (ans.length() == w.length() && w < ans))) {
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

func (t *Trie) insert(w string) {
	node := t
	for i := 0; i < len(w); i++ {
		idx := w[i] - 'a'
		if node.children[idx] == nil {
			node.children[idx] = &Trie{}
		}
		node = node.children[idx]
	}
	node.isEnd = true
}

func (t *Trie) search(w string) bool {
	node := t
	for i := 0; i < len(w); i++ {
		idx := w[i] - 'a'
		if node.children[idx] == nil || !node.children[idx].isEnd {
			return false
		}
		node = node.children[idx]
	}
	return true
}

func longestWord(words []string) string {
	trie := &Trie{}
	for _, w := range words {
		trie.insert(w)
	}

	ans := ""
	for _, w := range words {
		if trie.search(w) && (len(ans) < len(w) || (len(ans) == len(w) && w < ans)) {
			ans = w
		}
	}
	return ans
}
```

#### TypeScript

```ts
class Trie {
    children: (Trie | null)[] = new Array(26).fill(null);
    isEnd: boolean = false;

    insert(w: string): void {
        let node: Trie = this;
        for (let i = 0; i < w.length; i++) {
            const idx: number = w.charCodeAt(i) - 'a'.charCodeAt(0);
            if (node.children[idx] === null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx]!;
        }
        node.isEnd = true;
    }

    search(w: string): boolean {
        let node: Trie = this;
        for (let i = 0; i < w.length; i++) {
            const idx: number = w.charCodeAt(i) - 'a'.charCodeAt(0);
            if (node.children[idx] === null || !node.children[idx]!.isEnd) {
                return false;
            }
            node = node.children[idx]!;
        }
        return true;
    }
}

function longestWord(words: string[]): string {
    const trie = new Trie();
    for (const w of words) {
        trie.insert(w);
    }

    let ans = '';
    for (const w of words) {
        if (trie.search(w) && (ans.length < w.length || (ans.length === w.length && w < ans))) {
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
            if node.children[idx].is_none() {
                node.children[idx] = Some(Box::new(Trie::new()));
            }
            node = node.children[idx].as_mut().unwrap();
        }
        node.is_end = true;
    }

    fn search(&self, w: &str) -> bool {
        let mut node = self;
        for c in w.chars() {
            let idx = (c as usize) - ('a' as usize);
            if node.children[idx].is_none() || !node.children[idx].as_ref().unwrap().is_end {
                return false;
            }
            node = node.children[idx].as_ref().unwrap();
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
        for w in words {
            if trie.search(&w) && (ans.len() < w.len() || (ans.len() == w.len() && w < ans)) {
                ans = w;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
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
        if (trie.search(w) && (ans.length < w.length || (ans.length === w.length && w < ans))) {
            ans = w;
        }
    }
    return ans;
};

class Trie {
    constructor() {
        this.children = Array(26).fill(null);
        this.isEnd = false;
    }

    insert(w) {
        let node = this;
        for (let i = 0; i < w.length; i++) {
            const idx = w.charCodeAt(i) - 'a'.charCodeAt(0);
            if (node.children[idx] === null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    search(w) {
        let node = this;
        for (let i = 0; i < w.length; i++) {
            const idx = w.charCodeAt(i) - 'a'.charCodeAt(0);
            if (node.children[idx] === null || !node.children[idx].isEnd) {
                return false;
            }
            node = node.children[idx];
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
