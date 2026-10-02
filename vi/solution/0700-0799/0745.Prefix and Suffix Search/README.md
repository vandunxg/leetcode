---
comments: true
difficulty: Hard
tags:
    - Design
    - Trie
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [745. Prefix and Suffix Search](https://leetcode.com/problems/prefix-and-suffix-search)

[中文文档](/solution/0700-0799/0745.Prefix%20and%20Suffix%20Search/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một dictionary đặc biệt để tìm từ dựa trên prefix và suffix.</p>

<p>Hãy triển khai class <code>WordFilter</code>:</p>

<ul>
	<li><code>WordFilter(string[] words)</code> Khởi tạo object với các từ <code>words</code> trong dictionary.</li>
	<li><code>f(string pref, string suff)</code> Trả về <em>chỉ số của từ trong dictionary</em> có prefix <code>pref</code> và suffix <code>suff</code>. Nếu có nhiều chỉ số thỏa mãn, trả về chỉ số <strong>lớn nhất</strong>. Nếu không có từ phù hợp trong dictionary, trả về <code>-1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;WordFilter&quot;, &quot;f&quot;]
[[[&quot;apple&quot;]], [&quot;a&quot;, &quot;e&quot;]]
<strong>Đầu ra</strong>
[null, 0]
<strong>Giải thích</strong>
WordFilter wordFilter = new WordFilter([&quot;apple&quot;]);
wordFilter.f(&quot;a&quot;, &quot;e&quot;); // return 0, because the word at index 0 has prefix = &quot;a&quot; and suffix = &quot;e&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 7</code></li>
	<li><code>1 &lt;= pref.length, suff.length &lt;= 7</code></li>
	<li><code>words[i]</code>, <code>pref</code> và <code>suff</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Hàm <code>f</code> được gọi nhiều nhất <code>10<sup>4</sup></code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trả về chỉ số lớn nhất có từ tương ứng chứa prefix và suffix đã cho. Mỗi từ dài tối đa $7$, nên có thể lưu mọi cặp prefix–suffix của từng từ và mỗi query chỉ mất $O(1)$.
>
> Từ xuất hiện sau ghi đè từ trước, nên map giữ chỉ số lớn nhất. Cũng lưu cả prefix và suffix rỗng.
>
> Key là $(pref,suff)$. $\textit{f}$ tra cứu key này và trả về giá trị tương ứng, hoặc $-1$ nếu không tìm thấy.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class WordFilter:
    def __init__(self, words: List[str]):
        self.d = {}
        for k, w in enumerate(words):
            n = len(w)
            for i in range(n + 1):
                a = w[:i]
                for j in range(n + 1):
                    b = w[j:]
                    self.d[(a, b)] = k

    def f(self, pref: str, suff: str) -> int:
        return self.d.get((pref, suff), -1)


# Your WordFilter object will be instantiated and called as such:
# obj = WordFilter(words)
# param_1 = obj.f(pref,suff)
```

#### Java

```java
class WordFilter {
    private Map<String, Integer> d = new HashMap<>();

    public WordFilter(String[] words) {
        for (int k = 0; k < words.length; ++k) {
            String w = words[k];
            int n = w.length();
            for (int i = 0; i <= n; ++i) {
                String a = w.substring(0, i);
                for (int j = 0; j <= n; ++j) {
                    String b = w.substring(j);
                    d.put(a + "." + b, k);
                }
            }
        }
    }

    public int f(String pref, String suff) {
        return d.getOrDefault(pref + "." + suff, -1);
    }
}

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter obj = new WordFilter(words);
 * int param_1 = obj.f(pref,suff);
 */
```

#### C++

```cpp
class WordFilter {
public:
    unordered_map<string, int> d;

    WordFilter(vector<string>& words) {
        for (int k = 0; k < words.size(); ++k) {
            string w = words[k];
            int n = w.size();
            for (int i = 0; i <= n; ++i) {
                string a = w.substr(0, i);
                for (int j = 0; j <= n; ++j) {
                    string b = w.substr(j, n - j);
                    d[a + "." + b] = k;
                }
            }
        }
    }

    int f(string pref, string suff) {
        string key = pref + "." + suff;
        if (d.count(key)) return d[key];
        return -1;
    }
};

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter* obj = new WordFilter(words);
 * int param_1 = obj->f(pref,suff);
 */
```

#### Go

```go
type WordFilter struct {
	d map[string]int
}

func Constructor(words []string) WordFilter {
	d := map[string]int{}
	for k, w := range words {
		n := len(w)
		for i := 0; i <= n; i++ {
			a := w[:i]
			for j := 0; j <= n; j++ {
				b := w[j:]
				d[a+"."+b] = k
			}
		}
	}
	return WordFilter{d}
}

func (this *WordFilter) F(pref string, suff string) int {
	if v, ok := this.d[pref+"."+suff]; ok {
		return v
	}
	return -1
}

/**
 * Your WordFilter object will be instantiated and called as such:
 * obj := Constructor(words);
 * param_1 := obj.F(pref,suff);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu mọi cặp prefix–suffix. Thay vào đó, dùng hai trie—một trie cho từ, một trie cho từ đảo ngược—để lưu các chỉ số đi qua từng node.
>
> Mỗi query lấy hai danh sách chỉ số (được sắp tăng theo thứ tự thêm vào), rồi duyệt từ cuối danh sách để tìm chỉ số chung lớn nhất.
>
> Vì các từ ngắn nên danh sách cũng ngắn, đồng thời không cần tạo trước mọi cặp prefix–suffix.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26
        self.indexes = []

    def insert(self, word, i):
        node = self
        for c in word:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
            node.indexes.append(i)

    def search(self, pref):
        node = self
        for c in pref:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                return []
            node = node.children[idx]
        return node.indexes


class WordFilter:
    def __init__(self, words: List[str]):
        self.p = Trie()
        self.s = Trie()
        for i, w in enumerate(words):
            self.p.insert(w, i)
            self.s.insert(w[::-1], i)

    def f(self, pref: str, suff: str) -> int:
        a = self.p.search(pref)
        b = self.s.search(suff[::-1])
        if not a or not b:
            return -1
        i, j = len(a) - 1, len(b) - 1
        while ~i and ~j:
            if a[i] == b[j]:
                return a[i]
            if a[i] > b[j]:
                i -= 1
            else:
                j -= 1
        return -1


# Your WordFilter object will be instantiated and called as such:
# obj = WordFilter(words)
# param_1 = obj.f(pref,suff)
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];
    List<Integer> indexes = new ArrayList<>();

    void insert(String word, int i) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
            node.indexes.add(i);
        }
    }

    List<Integer> search(String pref) {
        Trie node = this;
        for (char c : pref.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                return Collections.emptyList();
            }
            node = node.children[c];
        }
        return node.indexes;
    }
}

class WordFilter {
    private Trie p = new Trie();
    private Trie s = new Trie();

    public WordFilter(String[] words) {
        for (int i = 0; i < words.length; ++i) {
            String w = words[i];
            p.insert(w, i);
            s.insert(new StringBuilder(w).reverse().toString(), i);
        }
    }

    public int f(String pref, String suff) {
        suff = new StringBuilder(suff).reverse().toString();
        List<Integer> a = p.search(pref);
        List<Integer> b = s.search(suff);
        if (a.isEmpty() || b.isEmpty()) {
            return -1;
        }
        int i = a.size() - 1, j = b.size() - 1;
        while (i >= 0 && j >= 0) {
            int c = a.get(i), d = b.get(j);
            if (c == d) {
                return c;
            }
            if (c > d) {
                --i;
            } else {
                --j;
            }
        }
        return -1;
    }
}

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter obj = new WordFilter(words);
 * int param_1 = obj.f(pref,suff);
 */
```

#### Go

```go
type Trie struct {
	children [26]*Trie
	indexes  []int
}

func newTrie() *Trie {
	return &Trie{indexes: []int{}}
}
func (this *Trie) insert(word string, i int) {
	node := this
	for _, c := range word {
		idx := c - 'a'
		if node.children[idx] == nil {
			node.children[idx] = newTrie()
		}
		node = node.children[idx]
		node.indexes = append(node.indexes, i)
	}
}

func (this *Trie) search(pref string) []int {
	node := this
	for _, c := range pref {
		idx := c - 'a'
		if node.children[idx] == nil {
			return []int{}
		}
		node = node.children[idx]
	}
	return node.indexes
}

type WordFilter struct {
	p *Trie
	s *Trie
}

func Constructor(words []string) WordFilter {
	p := newTrie()
	s := newTrie()
	for i, w := range words {
		p.insert(w, i)
		s.insert(reverse(w), i)
	}
	return WordFilter{p, s}
}

func (this *WordFilter) F(pref string, suff string) int {
	a := this.p.search(pref)
	b := this.s.search(reverse(suff))
	if len(a) == 0 || len(b) == 0 {
		return -1
	}
	i, j := len(a)-1, len(b)-1
	for i >= 0 && j >= 0 {
		if a[i] == b[j] {
			return a[i]
		}
		if a[i] > b[j] {
			i--
		} else {
			j--
		}
	}
	return -1
}

func reverse(w string) string {
	ww := []byte(w)
	for i, j := 0, len(w)-1; i < j; i++ {
		ww[i], ww[j] = ww[j], ww[i]
		j--
	}
	return string(ww)
}

/**
 * Your WordFilter object will be instantiated and called as such:
 * obj := Constructor(words);
 * param_1 := obj.F(pref,suff);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
