---
comments: true
difficulty: Medium
tags:
    - Trie
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [648. Replace Words](https://leetcode.com/problems/replace-words)

[中文文档](/solution/0600-0699/0648.Replace%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Trong tiếng Anh, có một khái niệm gọi là <strong>root</strong>; khi thêm một từ khác vào sau root, ta tạo thành một từ dài hơn, gọi là <strong>từ phái sinh</strong>. Ví dụ, thêm từ <code>&quot;ful&quot;</code> sau <strong>root</strong> <code>&quot;help&quot;</code> sẽ tạo thành từ phái sinh <code>&quot;helpful&quot;</code>.</p>

<p>Cho một <code>dictionary</code> gồm nhiều <strong>root</strong> và một <code>sentence</code> gồm các từ được phân tách bằng dấu cách. Hãy thay mỗi từ phái sinh trong câu bằng <strong>root</strong> tạo ra từ đó. Nếu một từ phái sinh có thể được thay bằng nhiều <strong>root</strong>, hãy chọn <strong>root ngắn nhất</strong>.</p>

<p>Trả về <em><code>sentence</code></em> sau khi thay thế.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> dictionary = [&quot;cat&quot;,&quot;bat&quot;,&quot;rat&quot;], sentence = &quot;the cattle was rattled by the battery&quot;
<strong>Đầu ra:</strong> &quot;the cat was rat by the bat&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> dictionary = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;], sentence = &quot;aadsfasf absbs bbab cadsfafs&quot;
<strong>Đầu ra:</strong> &quot;a a b c&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= dictionary.length &lt;= 1000</code></li>
	<li><code>1 &lt;= dictionary[i].length &lt;= 100</code></li>
	<li><code>dictionary[i]</code> chỉ gồm các chữ cái viết thường.</li>
	<li><code>1 &lt;= sentence.length &lt;= 10<sup>6</sup></code></li>
	<li><code>sentence</code> chỉ gồm các chữ cái viết thường và dấu cách.</li>
	<li>Số từ trong <code>sentence</code> nằm trong khoảng <code>[1, 1000]</code></li>
	<li>Độ dài mỗi từ trong <code>sentence</code> nằm trong khoảng <code>[1, 1000]</code></li>
	<li>Mỗi cặp từ liên tiếp trong <code>sentence</code> được phân tách bằng đúng một dấu cách.</li>
	<li><code>sentence</code> không có dấu cách ở đầu hoặc cuối.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi từ trong câu cần được thay bằng root ngắn nhất trong dictionary. Nếu thử từng root với từng từ, ta sẽ lặp lại việc kiểm tra các tiền tố.
>
> Đưa các root vào trie rồi duyệt từng từ cho đến khi gặp `is_end` đầu tiên; tiền tố đó chính là root ngắn nhất.

<!-- thinking:end -->

Ta có thể dùng trie để lưu tất cả root trong dictionary. Định nghĩa class node của trie là $\text{Trie}$, gồm một mảng $\text{children}$ có độ dài $26$ để lưu các node con và một biến boolean $\text{is\_end}$ để đánh dấu node có kết thúc một root hay không.

Với mỗi root, ta chèn nó vào trie. Với mỗi từ trong câu, ta tìm root ngắn nhất của từ đó trong trie. Nếu tìm thấy, ta thay từ bằng root; nếu không, giữ nguyên từ.

Độ phức tạp thời gian là $O(n \times |w| + L)$, còn độ phức tạp không gian là $O(n \times |w|)$, trong đó $n$ là số root trong dictionary, $|w|$ là độ dài trung bình của các root và $L$ là tổng độ dài các từ trong câu.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26
        self.is_end = False

    def insert(self, w: str) -> None:
        node = self
        for c in w:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True

    def search(self, w: str) -> str:
        node = self
        for i, c in enumerate(w, 1):
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                return w
            node = node.children[idx]
            if node.is_end:
                return w[:i]
        return w


class Solution:
    def replaceWords(self, dictionary: List[str], sentence: str) -> str:
        trie = Trie()
        for w in dictionary:
            trie.insert(w)
        return " ".join(trie.search(w) for w in sentence.split())
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];
    boolean isEnd = false;

    void insert(String w) {
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

    String search(String w) {
        Trie node = this;
        for (int i = 0; i < w.length(); i++) {
            int idx = w.charAt(i) - 'a';
            if (node.children[idx] == null) {
                return w;
            }
            node = node.children[idx];
            if (node.isEnd) {
                return w.substring(0, i + 1);
            }
        }
        return w;
    }
}

class Solution {
    public String replaceWords(List<String> dictionary, String sentence) {
        Trie trie = new Trie();
        for (String w : dictionary) {
            trie.insert(w);
        }

        String[] words = sentence.split(" ");
        for (int i = 0; i < words.length; i++) {
            words[i] = trie.search(words[i]);
        }
        return String.join(" ", words);
    }
}
```

#### C++

```cpp
class Trie {
public:
    Trie* children[26]{};
    bool isEnd = false;

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

    string search(const string& w) {
        Trie* node = this;
        for (int i = 0; i < w.size(); ++i) {
            int idx = w[i] - 'a';
            if (!node->children[idx]) {
                return w;
            }
            node = node->children[idx];
            if (node->isEnd) {
                return w.substr(0, i + 1);
            }
        }
        return w;
    }
};

class Solution {
public:
    string replaceWords(vector<string>& dictionary, string sentence) {
        Trie trie;
        for (auto& w : dictionary) {
            trie.insert(w);
        }

        stringstream ss(sentence);
        string word, res;
        while (ss >> word) {
            if (!res.empty()) res += " ";
            res += trie.search(word);
        }
        return res;
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
	for _, c := range w {
		idx := c - 'a'
		if node.children[idx] == nil {
			node.children[idx] = &Trie{}
		}
		node = node.children[idx]
	}
	node.isEnd = true
}

func (t *Trie) search(w string) string {
	node := t
	for i, c := range w {
		idx := c - 'a'
		if node.children[idx] == nil {
			return w
		}
		node = node.children[idx]
		if node.isEnd {
			return w[:i+1]
		}
	}
	return w
}

func replaceWords(dictionary []string, sentence string) string {
	trie := &Trie{}
	for _, w := range dictionary {
		trie.insert(w)
	}

	words := strings.Split(sentence, " ")
	for i, w := range words {
		words[i] = trie.search(w)
	}
	return strings.Join(words, " ")
}
```

#### TypeScript

```ts
class Trie {
    children: Array<Trie | null>;
    isEnd: boolean;

    constructor() {
        this.children = new Array(26).fill(null);
        this.isEnd = false;
    }

    insert(w: string): void {
        let node: Trie = this;
        for (const c of w) {
            const idx = c.charCodeAt(0) - 97;
            if (!node.children[idx]) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx]!;
        }
        node.isEnd = true;
    }

    search(w: string): string {
        let node: Trie = this;
        for (let i = 0; i < w.length; i++) {
            const idx = w.charCodeAt(i) - 97;
            if (!node.children[idx]) {
                return w;
            }
            node = node.children[idx]!;
            if (node.isEnd) {
                return w.slice(0, i + 1);
            }
        }
        return w;
    }
}

function replaceWords(dictionary: string[], sentence: string): string {
    const trie = new Trie();
    for (const w of dictionary) {
        trie.insert(w);
    }

    return sentence
        .split(' ')
        .map(w => trie.search(w))
        .join(' ');
}
```

#### Rust

```rust
struct Trie {
    children: Vec<Option<Box<Trie>>>,
    is_end: bool,
}

impl Trie {
    fn new() -> Self {
        Self {
            children: (0..26).map(|_| None).collect(),
            is_end: false,
        }
    }

    fn insert(&mut self, w: String) {
        let mut node = self;
        for c in w.chars() {
            let idx = (c as u8 - b'a') as usize;
            node = node.children[idx].get_or_insert(Box::new(Trie::new()));
        }
        node.is_end = true;
    }

    fn search(&self, w: &str) -> String {
        let mut node = self;
        for (i, c) in w.chars().enumerate() {
            let idx = (c as u8 - b'a') as usize;
            if node.children[idx].is_none() {
                return w.to_string();
            }
            node = node.children[idx].as_ref().unwrap();
            if node.is_end {
                return w[..i + 1].to_string();
            }
        }
        w.to_string()
    }
}

impl Solution {
    pub fn replace_words(dictionary: Vec<String>, sentence: String) -> String {
        let mut trie = Trie::new();
        for w in dictionary {
            trie.insert(w);
        }

        sentence
            .split_whitespace()
            .map(|w| trie.search(w))
            .collect::<Vec<_>>()
            .join(" ")
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
