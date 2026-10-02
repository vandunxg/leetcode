---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Trie
    - Array
    - String
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [472. Concatenated Words](https://leetcode.com/problems/concatenated-words)

[中文文档](/solution/0400-0499/0472.Concatenated%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code> (<strong>không có phần tử trùng lặp</strong>). Hãy trả về <em>tất cả <strong>chuỗi ghép</strong> trong danh sách</em> <code>words</code>.</p>

<p><strong>Chuỗi ghép</strong> được định nghĩa là chuỗi tạo thành hoàn toàn từ ít nhất hai chuỗi ngắn hơn (không nhất thiết khác nhau) trong mảng đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;cat&quot;,&quot;cats&quot;,&quot;catsdogcats&quot;,&quot;dog&quot;,&quot;dogcatsdog&quot;,&quot;hippopotamuses&quot;,&quot;rat&quot;,&quot;ratcatdogcat&quot;]
<strong>Đầu ra:</strong> [&quot;catsdogcats&quot;,&quot;dogcatsdog&quot;,&quot;ratcatdogcat&quot;]
<strong>Giải thích:</strong> Có thể ghép &quot;catsdogcats&quot; từ &quot;cats&quot;, &quot;dog&quot; và &quot;cats&quot;; 
Có thể ghép &quot;dogcatsdog&quot; từ &quot;dog&quot;, &quot;cats&quot; và &quot;dog&quot;; 
Có thể ghép &quot;ratcatdogcat&quot; từ &quot;rat&quot;, &quot;cat&quot;, &quot;dog&quot; và &quot;cat&quot;.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;cat&quot;,&quot;dog&quot;,&quot;catdog&quot;]
<strong>Đầu ra:</strong> [&quot;catdog&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 30</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả chuỗi trong <code>words</code> đều <strong>khác nhau</strong>.</li>
	<li><code>1 &lt;= sum(words[i].length) &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi ghép gồm ít nhất hai từ ngắn hơn trong từ điển. Thử mọi cách tách rồi tra cứu trong set sẽ tốn thời gian theo cả số từ $n$ lẫn độ dài chuỗi.
>
> Sắp xếp theo độ dài rồi thêm các từ ngắn hơn vào trie trước. Dùng DFS để dò từ hiện tại trên trie: khi gặp node kết thúc một từ, đệ quy kiểm tra hậu tố; nếu tách được toàn bộ từ thì đó là chuỗi ghép, nếu không thì thêm từ này vào trie.
>
> Không thêm các chuỗi ghép vào trie, vì một từ dài hơn có thể được tạo từ các thành phần cơ bản của chúng. Hậu tố rỗng là trường hợp cơ sở thành công.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26
        self.is_end = False

    def insert(self, w):
        node = self
        for c in w:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True


class Solution:
    def findAllConcatenatedWordsInADict(self, words: List[str]) -> List[str]:
        def dfs(w):
            if not w:
                return True
            node = trie
            for i, c in enumerate(w):
                idx = ord(c) - ord('a')
                if node.children[idx] is None:
                    return False
                node = node.children[idx]
                if node.is_end and dfs(w[i + 1 :]):
                    return True
            return False

        trie = Trie()
        ans = []
        words.sort(key=lambda x: len(x))
        for w in words:
            if dfs(w):
                ans.append(w)
            else:
                trie.insert(w)
        return ans
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];
    boolean isEnd;

    void insert(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
        }
        node.isEnd = true;
    }
}

class Solution {
    private Trie trie = new Trie();

    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        Arrays.sort(words, (a, b) -> a.length() - b.length());
        List<String> ans = new ArrayList<>();
        for (String w : words) {
            if (dfs(w)) {
                ans.add(w);
            } else {
                trie.insert(w);
            }
        }
        return ans;
    }

    private boolean dfs(String w) {
        if ("".equals(w)) {
            return true;
        }
        Trie node = trie;
        for (int i = 0; i < w.length(); ++i) {
            int idx = w.charAt(i) - 'a';
            if (node.children[idx] == null) {
                return false;
            }
            node = node.children[idx];
            if (node.isEnd && dfs(w.substring(i + 1))) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Trie {
public:
    vector<Trie*> children;
    bool isEnd;
    Trie()
        : children(26)
        , isEnd(false) {}

    void insert(string w) {
        Trie* node = this;
        for (char c : w) {
            c -= 'a';
            if (!node->children[c]) node->children[c] = new Trie();
            node = node->children[c];
        }
        node->isEnd = true;
    }
};

class Solution {
public:
    Trie* trie = new Trie();

    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        sort(words.begin(), words.end(), [&](const string& a, const string& b) {
            return a.size() < b.size();
        });
        vector<string> ans;
        for (auto& w : words) {
            if (dfs(w))
                ans.push_back(w);
            else
                trie->insert(w);
        }
        return ans;
    }

    bool dfs(string w) {
        if (w == "") return true;
        Trie* node = trie;
        for (int i = 0; i < w.size(); ++i) {
            int idx = w[i] - 'a';
            if (!node->children[idx]) return false;
            node = node->children[idx];
            if (node->isEnd && dfs(w.substr(i + 1))) return true;
        }
        return false;
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
func (this *Trie) insert(word string) {
	node := this
	for _, c := range word {
		c -= 'a'
		if node.children[c] == nil {
			node.children[c] = newTrie()
		}
		node = node.children[c]
	}
	node.isEnd = true
}

func findAllConcatenatedWordsInADict(words []string) (ans []string) {
	sort.Slice(words, func(i, j int) bool { return len(words[i]) < len(words[j]) })
	trie := newTrie()
	var dfs func(string) bool
	dfs = func(w string) bool {
		if w == "" {
			return true
		}
		node := trie
		for i, c := range w {
			c -= 'a'
			if node.children[c] == nil {
				return false
			}
			node = node.children[c]
			if node.isEnd && dfs(w[i+1:]) {
				return true
			}
		}
		return false
	}
	for _, w := range words {
		if dfs(w) {
			ans = append(ans, w)
		} else {
			trie.insert(w)
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
