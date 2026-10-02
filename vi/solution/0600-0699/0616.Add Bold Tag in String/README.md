---
comments: true
difficulty: Medium
tags:
    - Trie
    - Array
    - Hash Table
    - String
    - String Matching
    - Aho-Corasick
---

<!-- problem:start -->

# [616. Add Bold Tag in String 🔒](https://leetcode.com/problems/add-bold-tag-in-string)

[中文文档](/solution/0600-0699/0616.Add%20Bold%20Tag%20in%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho chuỗi <code>s</code> và mảng các chuỗi <code>words</code>.</p>

<p>Hãy thêm cặp thẻ in đậm đóng/mở <code>&lt;b&gt;</code> và <code>&lt;/b&gt;</code> để bao quanh các chuỗi con trong <code>s</code> có trong <code>words</code>.</p>

<ul>
	<li>Nếu hai chuỗi con như vậy chồng lấn, hãy gộp chúng và chỉ dùng một cặp thẻ in đậm.</li>
	<li>Nếu hai chuỗi con được bọc thẻ in đậm nằm liền nhau, hãy gộp chúng lại.</li>
</ul>

<p>Trả về <code>s</code> <em>sau khi thêm các thẻ in đậm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcxyz123&quot;, words = [&quot;abc&quot;,&quot;123&quot;]
<strong>Đầu ra:</strong> &quot;&lt;b&gt;abc&lt;/b&gt;xyz&lt;b&gt;123&lt;/b&gt;&quot;
<strong>Giải thích:</strong> Hai chuỗi trong words xuất hiện dưới dạng chuỗi con của s như sau: &quot;<u>abc</u>xyz<u>123</u>&quot;.
Ta thêm &lt;b&gt; trước mỗi chuỗi con và &lt;/b&gt; sau mỗi chuỗi con.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabbb&quot;, words = [&quot;aa&quot;,&quot;b&quot;]
<strong>Đầu ra:</strong> &quot;&lt;b&gt;aaabbb&lt;/b&gt;&quot;
<strong>Giải thích:</strong> 
&quot;aa&quot; xuất hiện hai lần dưới dạng chuỗi con: &quot;<u>aa</u>abbb&quot; và &quot;a<u>aa</u>bbb&quot;.
&quot;b&quot; xuất hiện ba lần dưới dạng chuỗi con: &quot;aaa<u>b</u>bb&quot;, &quot;aaab<u>b</u>b&quot; và &quot;aaabb<u>b</u>&quot;.
Ta thêm &lt;b&gt; trước mỗi chuỗi con và &lt;/b&gt; sau mỗi chuỗi con: &quot;&lt;b&gt;a&lt;b&gt;a&lt;/b&gt;a&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&quot;.
Vì hai thẻ &lt;b&gt; đầu tiên chồng lấn, ta gộp chúng lại: &quot;&lt;b&gt;aaa&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&quot;.
Sau đó, vì bốn thẻ &lt;b&gt; liền nhau, ta gộp chúng lại: &quot;&lt;b&gt;aaabbb&lt;/b&gt;&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>0 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 1000</code></li>
	<li><code>s</code> và <code>words[i]</code> chỉ gồm chữ cái tiếng Anh và chữ số.</li>
	<li>Mọi giá trị trong <code>words</code> đều <strong>khác nhau</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với <a href="https://leetcode.com/problems/bold-words-in-string/description/" target="_blank">758. Bold Words in String</a>.</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần bọc mọi lần xuất hiện của từ trong $s$, và các đoạn có thể chồng lấn. Tìm kiếm từng từ một sẽ lặp lại công việc, sau đó vẫn cần gộp các đoạn.
>
> Chèn các từ vào trie rồi duyệt từ mỗi chỉ số bắt đầu để thu thập vị trí kết thúc. Gộp các đoạn chồng lấn, sau đó chèn `<b>` bao quanh các đoạn đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 128
        self.is_end = False

    def insert(self, word):
        node = self
        for c in word:
            idx = ord(c)
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True


class Solution:
    def addBoldTag(self, s: str, words: List[str]) -> str:
        trie = Trie()
        for w in words:
            trie.insert(w)
        n = len(s)
        pairs = []
        for i in range(n):
            node = trie
            for j in range(i, n):
                idx = ord(s[j])
                if node.children[idx] is None:
                    break
                node = node.children[idx]
                if node.is_end:
                    pairs.append([i, j])
        if not pairs:
            return s
        st, ed = pairs[0]
        t = []
        for a, b in pairs[1:]:
            if ed + 1 < a:
                t.append([st, ed])
                st, ed = a, b
            else:
                ed = max(ed, b)
        t.append([st, ed])

        ans = []
        i = j = 0
        while i < n:
            if j == len(t):
                ans.append(s[i:])
                break
            st, ed = t[j]
            if i < st:
                ans.append(s[i:st])
            ans.append('<b>')
            ans.append(s[st : ed + 1])
            ans.append('</b>')
            j += 1
            i = ed + 1

        return ''.join(ans)
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[128];
    boolean isEnd;

    public void insert(String word) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            if (node.children[c] == null) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
        }
        node.isEnd = true;
    }
}

class Solution {
    public String addBoldTag(String s, String[] words) {
        Trie trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        List<int[]> pairs = new ArrayList<>();
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            Trie node = trie;
            for (int j = i; j < n; ++j) {
                int idx = s.charAt(j);
                if (node.children[idx] == null) {
                    break;
                }
                node = node.children[idx];
                if (node.isEnd) {
                    pairs.add(new int[] {i, j});
                }
            }
        }
        if (pairs.isEmpty()) {
            return s;
        }
        List<int[]> t = new ArrayList<>();
        int st = pairs.get(0)[0], ed = pairs.get(0)[1];
        for (int j = 1; j < pairs.size(); ++j) {
            int a = pairs.get(j)[0], b = pairs.get(j)[1];
            if (ed + 1 < a) {
                t.add(new int[] {st, ed});
                st = a;
                ed = b;
            } else {
                ed = Math.max(ed, b);
            }
        }
        t.add(new int[] {st, ed});
        int i = 0, j = 0;
        StringBuilder ans = new StringBuilder();
        while (i < n) {
            if (j == t.size()) {
                ans.append(s.substring(i));
                break;
            }
            st = t.get(j)[0];
            ed = t.get(j)[1];
            if (i < st) {
                ans.append(s.substring(i, st));
            }
            ++j;
            ans.append("<b>");
            ans.append(s.substring(st, ed + 1));
            ans.append("</b>");
            i = ed + 1;
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Trie {
public:
    vector<Trie*> children;
    bool isEnd;

    Trie() {
        children.resize(128);
        isEnd = false;
    }

    void insert(string word) {
        Trie* node = this;
        for (char c : word) {
            if (!node->children[c]) node->children[c] = new Trie();
            node = node->children[c];
        }
        node->isEnd = true;
    }
};

class Solution {
public:
    string addBoldTag(string s, vector<string>& words) {
        Trie* trie = new Trie();
        for (string w : words) trie->insert(w);
        int n = s.size();
        vector<pair<int, int>> pairs;
        for (int i = 0; i < n; ++i) {
            Trie* node = trie;
            for (int j = i; j < n; ++j) {
                int idx = s[j];
                if (!node->children[idx]) break;
                node = node->children[idx];
                if (node->isEnd) pairs.push_back({i, j});
            }
        }
        if (pairs.empty()) return s;
        vector<pair<int, int>> t;
        int st = pairs[0].first, ed = pairs[0].second;
        for (int i = 1; i < pairs.size(); ++i) {
            int a = pairs[i].first, b = pairs[i].second;
            if (ed + 1 < a) {
                t.push_back({st, ed});
                st = a, ed = b;
            } else
                ed = max(ed, b);
        }
        t.push_back({st, ed});
        string ans = "";
        int i = 0, j = 0;
        while (i < n) {
            if (j == t.size()) {
                ans += s.substr(i);
                break;
            }
            st = t[j].first, ed = t[j].second;
            if (i < st) ans += s.substr(i, st - i);
            ans += "<b>";
            ans += s.substr(st, ed - st + 1);
            ans += "</b>";
            i = ed + 1;
            ++j;
        }
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children [128]*Trie
	isEnd    bool
}

func newTrie() *Trie {
	return &Trie{}
}

func (this *Trie) insert(word string) {
	node := this
	for _, c := range word {
		if node.children[c] == nil {
			node.children[c] = newTrie()
		}
		node = node.children[c]
	}
	node.isEnd = true
}

func addBoldTag(s string, words []string) string {
	trie := newTrie()
	for _, w := range words {
		trie.insert(w)
	}
	n := len(s)
	var pairs [][]int
	for i := range s {
		node := trie
		for j := i; j < n; j++ {
			if node.children[s[j]] == nil {
				break
			}
			node = node.children[s[j]]
			if node.isEnd {
				pairs = append(pairs, []int{i, j})
			}
		}
	}
	if len(pairs) == 0 {
		return s
	}
	var t [][]int
	st, ed := pairs[0][0], pairs[0][1]
	for i := 1; i < len(pairs); i++ {
		a, b := pairs[i][0], pairs[i][1]
		if ed+1 < a {
			t = append(t, []int{st, ed})
			st, ed = a, b
		} else {
			ed = max(ed, b)
		}
	}
	t = append(t, []int{st, ed})
	var ans strings.Builder
	i, j := 0, 0
	for i < n {
		if j == len(t) {
			ans.WriteString(s[i:])
			break
		}
		st, ed = t[j][0], t[j][1]
		if i < st {
			ans.WriteString(s[i:st])
		}
		ans.WriteString("<b>")
		ans.WriteString(s[st : ed+1])
		ans.WriteString("</b>")
		i = ed + 1
		j++
	}
	return ans.String()
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
