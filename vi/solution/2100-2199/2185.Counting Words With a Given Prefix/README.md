---
comments: true
difficulty: Easy
rating: 1167
source: Weekly Contest 282 Q1
tags:
    - Array
    - String
    - String Matching
---

<!-- problem:start -->

# [2185. Counting Words With a Given Prefix](https://leetcode.com/problems/counting-words-with-a-given-prefix)

[中文文档](/solution/2100-2199/2185.Counting%20Words%20With%20a%20Given%20Prefix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> và một chuỗi <code>pref</code>.</p>

<p>Trả về <em>số chuỗi trong </em><code>words</code><em> có </em><code>pref</code><em> làm <strong>tiền tố</strong></em>.</p>

<p><strong>Tiền tố</strong> của một chuỗi <code>s</code> là một chuỗi con liên tiếp đứng ở đầu của <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;pay&quot;,&quot;<strong><u>at</u></strong>tention&quot;,&quot;practice&quot;,&quot;<u><strong>at</strong></u>tend&quot;], <code>pref </code>= &quot;at&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 2 chuỗi có &quot;at&quot; làm tiền tố là: &quot;<u><strong>at</strong></u>tention&quot; và &quot;<u><strong>at</strong></u>tend&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;leetcode&quot;,&quot;win&quot;,&quot;loops&quot;,&quot;success&quot;], <code>pref </code>= &quot;code&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có chuỗi nào có &quot;code&quot; làm tiền tố.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length, pref.length &lt;= 100</code></li>
	<li><code>words[i]</code> và <code>pref</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các từ bắt đầu bằng $\textit{pref}$. Vì số lượng từ và độ dài các từ đều nhỏ, chỉ cần kiểm tra tiền tố trực tiếp cho từng từ.
>
> $\texttt{startswith}$ so sánh nhiều nhất $|\textit{pref}|$ ký tự.
>
> Đáp án là số lần phép kiểm tra trả về đúng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def prefixCount(self, words: List[str], pref: str) -> int:
        return sum(w.startswith(pref) for w in words)
```

#### Java

```java
class Solution {
    public int prefixCount(String[] words, String pref) {
        int ans = 0;
        for (String w : words) {
            if (w.startsWith(pref)) {
                ++ans;
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
    int prefixCount(vector<string>& words, string pref) {
        int ans = 0;
        for (auto& w : words) ans += w.find(pref) == 0;
        return ans;
    }
};
```

#### Go

```go
func prefixCount(words []string, pref string) (ans int) {
	for _, w := range words {
		if strings.HasPrefix(w, pref) {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function prefixCount(words: string[], pref: string): number {
    return words.reduce((r, s) => (r += s.startsWith(pref) ? 1 : 0), 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn prefix_count(words: Vec<String>, pref: String) -> i32 {
        words.iter().filter(|s| s.starts_with(&pref)).count() as i32
    }
}
```

#### C

```c
int prefixCount(char** words, int wordsSize, char* pref) {
    int ans = 0;
    int n = strlen(pref);
    for (int i = 0; i < wordsSize; i++) {
        if (strncmp(words[i], pref, n) == 0) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã duyệt qua mọi ký tự. Nếu cần thực hiện nhiều truy vấn trên cùng một từ điển, có thể dùng trie để lưu số lượng tiền tố tại các node.
>
> Khi chèn, tăng $\textit{cnt}$ trên đường đi; khi truy vấn, duyệt qua $\textit{pref}$ và đọc $\textit{cnt}$, hoặc trả về $0$ nếu thiếu edge.
>
> Bài toán này chỉ có một truy vấn; trie là lựa chọn có thể mở rộng hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26
        self.cnt = 0

    def insert(self, w):
        node = self
        for c in w:
            i = ord(c) - ord('a')
            if node.children[i] is None:
                node.children[i] = Trie()
            node = node.children[i]
            node.cnt += 1

    def search(self, pref):
        node = self
        for c in pref:
            i = ord(c) - ord('a')
            if node.children[i] is None:
                return 0
            node = node.children[i]
        return node.cnt


class Solution:
    def prefixCount(self, words: List[str], pref: str) -> int:
        tree = Trie()
        for w in words:
            tree.insert(w)
        return tree.search(pref)
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private int cnt;

    public void insert(String w) {
        Trie node = this;
        for (int i = 0; i < w.length(); ++i) {
            int j = w.charAt(i) - 'a';
            if (node.children[j] == null) {
                node.children[j] = new Trie();
            }
            node = node.children[j];
            ++node.cnt;
        }
    }

    public int search(String pref) {
        Trie node = this;
        for (int i = 0; i < pref.length(); ++i) {
            int j = pref.charAt(i) - 'a';
            if (node.children[j] == null) {
                return 0;
            }
            node = node.children[j];
        }
        return node.cnt;
    }
}

class Solution {
    public int prefixCount(String[] words, String pref) {
        Trie tree = new Trie();
        for (String w : words) {
            tree.insert(w);
        }
        return tree.search(pref);
    }
}
```

#### C++

```cpp
class Trie {
public:
    Trie()
        : children(26)
        , cnt(0) {}

    void insert(string w) {
        Trie* node = this;
        for (auto& c : w) {
            int i = c - 'a';
            if (!node->children[i]) {
                node->children[i] = new Trie();
            }
            node = node->children[i];
            ++node->cnt;
        }
    }

    int search(string pref) {
        Trie* node = this;
        for (auto& c : pref) {
            int i = c - 'a';
            if (!node->children[i]) {
                return 0;
            }
            node = node->children[i];
        }
        return node->cnt;
    }

private:
    vector<Trie*> children;
    int cnt;
};

class Solution {
public:
    int prefixCount(vector<string>& words, string pref) {
        Trie* tree = new Trie();
        for (auto& w : words) {
            tree->insert(w);
        }
        return tree->search(pref);
    }
};
```

#### Go

```go
type Trie struct {
	children [26]*Trie
	cnt      int
}

func newTrie() *Trie {
	return &Trie{}
}

func (this *Trie) insert(w string) {
	node := this
	for _, c := range w {
		c -= 'a'
		if node.children[c] == nil {
			node.children[c] = newTrie()
		}
		node = node.children[c]
		node.cnt++
	}
}

func (this *Trie) search(pref string) int {
	node := this
	for _, c := range pref {
		c -= 'a'
		if node.children[c] == nil {
			return 0
		}
		node = node.children[c]
	}
	return node.cnt
}

func prefixCount(words []string, pref string) int {
	tree := newTrie()
	for _, w := range words {
		tree.insert(w)
	}
	return tree.search(pref)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
