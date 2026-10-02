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

# [820. Short Encoding of Words](https://leetcode.com/problems/short-encoding-of-words)

[中文文档](/solution/0800-0899/0820.Short%20Encoding%20of%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>mã hóa hợp lệ</strong> của mảng <code>words</code> là chuỗi tham chiếu <code>s</code> và mảng chỉ số <code>indices</code> thỏa mãn:</p>

<ul>
	<li><code>words.length == indices.length</code></li>
	<li>Chuỗi tham chiếu <code>s</code> kết thúc bằng ký tự <code>&#39;#&#39;</code>.</li>
	<li>Với mỗi chỉ số <code>indices[i]</code>, <strong>chuỗi con</strong> của <code>s</code> bắt đầu tại <code>indices[i]</code> và kéo dài đến (nhưng không bao gồm) ký tự <code>&#39;#&#39;</code> tiếp theo phải bằng <code>words[i]</code>.</li>
</ul>

<p>Cho mảng <code>words</code>, hãy trả về <em><strong>độ dài chuỗi tham chiếu ngắn nhất</strong> </em><code>s</code><em> có thể tạo ra trong mọi cách <strong>mã hóa hợp lệ</strong> của </em><code>words</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;time&quot;, &quot;me&quot;, &quot;bell&quot;]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Một cách mã hóa hợp lệ là s = <code>&quot;time#bell#&quot; and indices = [0, 2, 5</code>].
words[0] = &quot;time&quot;, chuỗi con của s bắt đầu tại indices[0] = 0 và đến ký tự &#39;#&#39; tiếp theo được gạch chân trong &quot;<u>time</u>#bell#&quot;
words[1] = &quot;me&quot;, chuỗi con của s bắt đầu tại indices[1] = 2 và đến ký tự &#39;#&#39; tiếp theo được gạch chân trong &quot;ti<u>me</u>#bell#&quot;
words[2] = &quot;bell&quot;, chuỗi con của s bắt đầu tại indices[2] = 5 và đến ký tự &#39;#&#39; tiếp theo được gạch chân trong &quot;time#<u>bell</u>#&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;t&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Một cách mã hóa hợp lệ là s = &quot;t#&quot; và indices = [0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 2000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 7</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mã hóa nối các từ bằng `#`. Từ ngắn hơn nếu là suffix của từ dài hơn thì đã được bao phủ. Kiểm tra từng cặp suffix sẽ tốn công không cần thiết.
>
> Chèn các từ theo thứ tự đảo ngược vào trie để các suffix chung dùng chung một nhánh. Chỉ các node lá cần được mã hóa riêng (độ dài cộng một); các node bên trong đã được bao phủ bởi một từ dài hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self) -> None:
        self.children = [None] * 26


class Solution:
    def minimumLengthEncoding(self, words: List[str]) -> int:
        root = Trie()
        for w in words:
            cur = root
            for c in w[::-1]:
                idx = ord(c) - ord("a")
                if cur.children[idx] == None:
                    cur.children[idx] = Trie()
                cur = cur.children[idx]
        return self.dfs(root, 1)

    def dfs(self, cur: Trie, l: int) -> int:
        isLeaf, ans = True, 0
        for i in range(26):
            if cur.children[i] != None:
                isLeaf = False
                ans += self.dfs(cur.children[i], l + 1)
        if isLeaf:
            ans += l
        return ans
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];
}

class Solution {
    public int minimumLengthEncoding(String[] words) {
        Trie root = new Trie();
        for (String w : words) {
            Trie cur = root;
            for (int i = w.length() - 1; i >= 0; i--) {
                int idx = w.charAt(i) - 'a';
                if (cur.children[idx] == null) {
                    cur.children[idx] = new Trie();
                }
                cur = cur.children[idx];
            }
        }
        return dfs(root, 1);
    }

    private int dfs(Trie cur, int l) {
        boolean isLeaf = true;
        int ans = 0;
        for (int i = 0; i < 26; i++) {
            if (cur.children[i] != null) {
                isLeaf = false;
                ans += dfs(cur.children[i], l + 1);
            }
        }
        if (isLeaf) {
            ans += l;
        }
        return ans;
    }
}
```

#### C++

```cpp
struct Trie {
    Trie* children[26] = {nullptr};
};

class Solution {
public:
    int minimumLengthEncoding(vector<string>& words) {
        auto root = new Trie();
        for (auto& w : words) {
            auto cur = root;
            for (int i = w.size() - 1; i >= 0; --i) {
                if (cur->children[w[i] - 'a'] == nullptr) {
                    cur->children[w[i] - 'a'] = new Trie();
                }
                cur = cur->children[w[i] - 'a'];
            }
        }
        return dfs(root, 1);
    }

private:
    int dfs(Trie* cur, int l) {
        bool isLeaf = true;
        int ans = 0;
        for (int i = 0; i < 26; ++i) {
            if (cur->children[i] != nullptr) {
                isLeaf = false;
                ans += dfs(cur->children[i], l + 1);
            }
        }
        if (isLeaf) {
            ans += l;
        }
        return ans;
    }
};
```

#### Go

```go
type trie struct {
	children [26]*trie
}

func minimumLengthEncoding(words []string) int {
	root := new(trie)
	for _, w := range words {
		cur := root
		for i := len(w) - 1; i >= 0; i-- {
			if cur.children[w[i]-'a'] == nil {
				cur.children[w[i]-'a'] = new(trie)
			}
			cur = cur.children[w[i]-'a']
		}
	}
	return dfs(root, 1)
}

func dfs(cur *trie, l int) int {
	isLeaf, ans := true, 0
	for i := 0; i < 26; i++ {
		if cur.children[i] != nil {
			isLeaf = false
			ans += dfs(cur.children[i], l+1)
		}
	}
	if isLeaf {
		ans += l
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo toàn bộ trie rồi dùng DFS đếm các node lá. Nếu chèn từ dài đến ngắn theo thứ tự đảo ngược, các từ dài sẽ chiếm các nhánh trước. Nếu đường đi của từ được chèn sau đã tồn tại, từ đó là suffix của một từ dài hơn nên đóng góp $0$; nếu không, nó đóng góp độ dài cộng một.
>
> Ta cộng dồn đáp án ngay trong lúc chèn, nên không cần DFS lần hai.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26

    def insert(self, w):
        node = self
        pref = True
        for c in w:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                node.children[idx] = Trie()
                pref = False
            node = node.children[idx]
        return 0 if pref else len(w) + 1


class Solution:
    def minimumLengthEncoding(self, words: List[str]) -> int:
        words.sort(key=lambda x: -len(x))
        trie = Trie()
        return sum(trie.insert(w[::-1]) for w in words)
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];

    int insert(String w) {
        Trie node = this;
        boolean pref = true;
        for (int i = w.length() - 1; i >= 0; --i) {
            int idx = w.charAt(i) - 'a';
            if (node.children[idx] == null) {
                pref = false;
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        return pref ? 0 : w.length() + 1;
    }
}

class Solution {
    public int minimumLengthEncoding(String[] words) {
        Arrays.sort(words, (a, b) -> b.length() - a.length());
        int ans = 0;
        Trie trie = new Trie();
        for (String w : words) {
            ans += trie.insert(w);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
public:
    vector<Trie*> children;
    Trie()
        : children(26) {}

    int insert(string w) {
        Trie* node = this;
        bool pref = true;
        for (char c : w) {
            c -= 'a';
            if (!node->children[c]) {
                pref = false;
                node->children[c] = new Trie();
            }
            node = node->children[c];
        }
        return pref ? 0 : w.size() + 1;
    }
};

class Solution {
public:
    int minimumLengthEncoding(vector<string>& words) {
        sort(words.begin(), words.end(), [](string& a, string& b) { return a.size() > b.size(); });
        Trie* trie = new Trie();
        int ans = 0;
        for (auto& w : words) {
            reverse(w.begin(), w.end());
            ans += trie->insert(w);
        }
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children [26]*Trie
}

func newTrie() *Trie {
	return &Trie{}
}

func (this *Trie) insert(w string) int {
	node := this
	pref := true
	for i := len(w) - 1; i >= 0; i-- {
		idx := w[i] - 'a'
		if node.children[idx] == nil {
			pref = false
			node.children[idx] = newTrie()
		}
		node = node.children[idx]
	}
	if pref {
		return 0
	}
	return len(w) + 1
}

func minimumLengthEncoding(words []string) int {
	sort.Slice(words, func(i, j int) bool { return len(words[i]) > len(words[j]) })
	trie := newTrie()
	ans := 0
	for _, w := range words {
		ans += trie.insert(w)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
