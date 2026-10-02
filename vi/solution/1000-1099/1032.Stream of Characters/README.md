---
comments: true
difficulty: Hard
rating: 1970
source: Weekly Contest 133 Q4
tags:
    - Design
    - Trie
    - Array
    - String
    - Data Stream
    - Aho-Corasick
---

<!-- problem:start -->

# [1032. Stream of Characters](https://leetcode.com/problems/stream-of-characters)

[中文文档](/solution/1000-1099/1032.Stream%20of%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một thuật toán nhận vào luồng ký tự và kiểm tra xem hậu tố của luồng có trùng với một chuỗi trong mảng <code>words</code> cho trước hay không.</p>

<p>Ví dụ, nếu <code>words = [&quot;abc&quot;, &quot;xyz&quot;]</code> và luồng lần lượt nhận bốn ký tự <code>&#39;a&#39;</code>, <code>&#39;x&#39;</code>, <code>&#39;y&#39;</code>, <code>&#39;z&#39;</code>, thuật toán cần nhận ra hậu tố <code>&quot;xyz&quot;</code> của chuỗi <code>&quot;axyz&quot;</code> khớp với <code>&quot;xyz&quot;</code> trong <code>words</code>.</p>

<p>Triển khai class <code>StreamChecker</code>:</p>

<ul>
	<li><code>StreamChecker(String[] words)</code> Khởi tạo đối tượng bằng mảng chuỗi <code>words</code>.</li>
	<li><code>boolean query(char letter)</code> Nhận một ký tự mới từ luồng và trả về <code>true</code> nếu có hậu tố không rỗng nào của luồng tạo thành một từ nằm trong <code>words</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;StreamChecker&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;, &quot;query&quot;]
[[[&quot;cd&quot;, &quot;f&quot;, &quot;kl&quot;]], [&quot;a&quot;], [&quot;b&quot;], [&quot;c&quot;], [&quot;d&quot;], [&quot;e&quot;], [&quot;f&quot;], [&quot;g&quot;], [&quot;h&quot;], [&quot;i&quot;], [&quot;j&quot;], [&quot;k&quot;], [&quot;l&quot;]]
<strong>Đầu ra</strong>
[null, false, false, false, true, false, true, false, false, false, false, false, true]

<strong>Giải thích</strong>
StreamChecker streamChecker = new StreamChecker([&quot;cd&quot;, &quot;f&quot;, &quot;kl&quot;]);
streamChecker.query(&quot;a&quot;); // return False
streamChecker.query(&quot;b&quot;); // return False
streamChecker.query(&quot;c&quot;); // return False
streamChecker.query(&quot;d&quot;); // return True, because &#39;cd&#39; is in the wordlist
streamChecker.query(&quot;e&quot;); // return False
streamChecker.query(&quot;f&quot;); // return True, because &#39;f&#39; is in the wordlist
streamChecker.query(&quot;g&quot;); // return False
streamChecker.query(&quot;h&quot;); // return False
streamChecker.query(&quot;i&quot;); // return False
streamChecker.query(&quot;j&quot;); // return False
streamChecker.query(&quot;k&quot;); // return False
streamChecker.query(&quot;l&quot;); // return True, because &#39;kl&#39; is in the wordlist
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 2000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 200</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>letter</code> là một chữ cái tiếng Anh viết thường.</li>
	<li>Có tối đa <code>4 * 10<sup>4</sup></code> lời gọi đến query.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> So khớp toàn bộ luồng với từng từ ở mỗi lần query sẽ lặp lại nhiều công việc: có thể có tới $4\times 10^4$ query và tổng độ dài pattern lớn. Ta chỉ cần biết luồng hiện tại có kết thúc bằng một từ nào đó hay không.
>
> Chèn các từ theo thứ tự đảo ngược vào trie sẽ biến việc kiểm tra hậu tố thành quá trình đi theo prefix. Độ dài mỗi từ tối đa là $200$, nên chỉ cần xét một đoạn đuôi có giới hạn của luồng.
>
> Constructor xây dựng trie đảo ngược này; mỗi query thêm ký tự mới rồi đi tối đa $201$ bước, và thành công nếu gặp dấu kết thúc từ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26
        self.is_end = False

    def insert(self, w: str):
        node = self
        for c in w[::-1]:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True

    def search(self, w: List[str]) -> bool:
        node = self
        for c in w[::-1]:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                return False
            node = node.children[idx]
            if node.is_end:
                return True
        return False


class StreamChecker:
    def __init__(self, words: List[str]):
        self.trie = Trie()
        self.cs = []
        self.limit = 201
        for w in words:
            self.trie.insert(w)

    def query(self, letter: str) -> bool:
        self.cs.append(letter)
        return self.trie.search(self.cs[-self.limit :])


# Your StreamChecker object will be instantiated and called as such:
# obj = StreamChecker(words)
# param_1 = obj.query(letter)
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];
    boolean isEnd = false;

    public void insert(String w) {
        Trie node = this;
        for (int i = w.length() - 1; i >= 0; --i) {
            int idx = w.charAt(i) - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    public boolean query(StringBuilder s) {
        Trie node = this;
        for (int i = s.length() - 1; i >= 0; --i) {
            int idx = s.charAt(i) - 'a';
            if (node.children[idx] == null) {
                return false;
            }
            node = node.children[idx];
            if (node.isEnd) {
                return true;
            }
        }
        return false;
    }
}

class StreamChecker {
    private StringBuilder sb = new StringBuilder();
    private Trie trie = new Trie();

    public StreamChecker(String[] words) {
        for (String w : words) {
            trie.insert(w);
        }
    }

    public boolean query(char letter) {
        sb.append(letter);
        return trie.query(sb);
    }
}

/**
 * Your StreamChecker object will be instantiated and called as such:
 * StreamChecker obj = new StreamChecker(words);
 * boolean param_1 = obj.query(letter);
 */
```

#### C++

```cpp
class Trie {
private:
    Trie* children[26]{};
    bool isEnd = false;

public:
    void insert(string& w) {
        Trie* node = this;
        reverse(w.begin(), w.end());
        for (char& c : w) {
            int idx = c - 'a';
            if (!node->children[idx]) {
                node->children[idx] = new Trie();
            }
            node = node->children[idx];
        }
        node->isEnd = true;
    }

    bool search(string& w) {
        Trie* node = this;
        for (int i = w.size() - 1, j = 0; ~i && j < 201; --i, ++j) {
            int idx = w[i] - 'a';
            if (!node->children[idx]) {
                return false;
            }
            node = node->children[idx];
            if (node->isEnd) {
                return true;
            }
        }
        return false;
    }
};

class StreamChecker {
public:
    Trie* trie = new Trie();
    string s;

    StreamChecker(vector<string>& words) {
        for (auto& w : words) {
            trie->insert(w);
        }
    }

    bool query(char letter) {
        s += letter;
        return trie->search(s);
    }
};

/**
 * Your StreamChecker object will be instantiated and called as such:
 * StreamChecker* obj = new StreamChecker(words);
 * bool param_1 = obj->query(letter);
 */
```

#### Go

```go
type Trie struct {
	children [26]*Trie
	isEnd    bool
}

func newTrie() Trie {
	return Trie{}
}

func (this *Trie) Insert(word string) {
	node := this
	for i := len(word) - 1; i >= 0; i-- {
		idx := word[i] - 'a'
		if node.children[idx] == nil {
			node.children[idx] = &Trie{}
		}
		node = node.children[idx]
	}
	node.isEnd = true
}

func (this *Trie) Search(word []byte) bool {
	node := this
	for i := len(word) - 1; i >= 0; i-- {
		idx := word[i] - 'a'
		if node.children[idx] == nil {
			return false
		}
		node = node.children[idx]
		if node.isEnd {
			return true
		}
	}
	return false
}

type StreamChecker struct {
	trie Trie
	s    []byte
}

func Constructor(words []string) StreamChecker {
	trie := newTrie()
	for _, w := range words {
		trie.Insert(w)
	}
	return StreamChecker{trie, []byte{}}
}

func (this *StreamChecker) Query(letter byte) bool {
	this.s = append(this.s, letter)
	return this.trie.Search(this.s)
}

/**
 * Your StreamChecker object will be instantiated and called as such:
 * obj := Constructor(words);
 * param_1 := obj.Query(letter);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
