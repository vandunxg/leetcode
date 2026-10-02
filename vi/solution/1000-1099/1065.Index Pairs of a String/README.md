---
comments: true
difficulty: Easy
rating: 1389
source: Biweekly Contest 1 Q2
tags:
    - Trie
    - Array
    - String
    - Sorting
    - Aho-Corasick
---

<!-- problem:start -->

# [1065. Index Pairs of a String 🔒](https://leetcode.com/problems/index-pairs-of-a-string)

[中文文档](/solution/1000-1099/1065.Index%20Pairs%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>text</code> và mảng chuỗi <code>words</code>. Trả về mảng chứa mọi cặp chỉ số <code>[i, j]</code> sao cho chuỗi con <code>text[i...j]</code> có trong <code>words</code>.</p>

<p>Trả về các cặp <code>[i, j]</code> theo thứ tự tăng dần: sắp xếp theo tọa độ thứ nhất, nếu bằng nhau thì sắp xếp theo tọa độ thứ hai.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;thestoryofleetcodeandme&quot;, words = [&quot;story&quot;,&quot;fleet&quot;,&quot;leetcode&quot;]
<strong>Đầu ra:</strong> [[3,7],[9,13],[10,17]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;ababa&quot;, words = [&quot;aba&quot;,&quot;ab&quot;]
<strong>Đầu ra:</strong> [[0,1],[0,2],[2,3],[2,4]]
<strong>Giải thích:</strong> Lưu ý rằng các kết quả khớp có thể chồng lấn: &quot;aba&quot; xuất hiện tại [0,2] và [2,4].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 100</code></li>
	<li><code>1 &lt;= words.length &lt;= 20</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 50</code></li>
	<li><code>text</code> và <code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Mọi chuỗi trong <code>words</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có thể kiểm tra từng chuỗi con của $text$ với danh sách từ. Cả hai dữ liệu đều nhỏ, nên chỉ cần đưa các từ vào set rồi xét mọi đoạn $[i,j]$.
>
> Set hỗ trợ tra cứu trong thời gian hằng số kỳ vọng; đồng thời, duyệt các chỉ số theo thứ tự vốn đã tạo ra các cặp được sắp xếp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def indexPairs(self, text: str, words: List[str]) -> List[List[int]]:
        words = set(words)
        n = len(text)
        return [
            [i, j] for i in range(n) for j in range(i, n) if text[i : j + 1] in words
        ]
```

#### Java

```java
class Solution {
    public int[][] indexPairs(String text, String[] words) {
        Set<String> s = new HashSet<>(Arrays.asList(words));
        int n = text.length();
        List<int[]> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            for (int j = i; j < n; ++j) {
                if (s.contains(text.substring(i, j + 1))) {
                    ans.add(new int[] {i, j});
                }
            }
        }
        return ans.toArray(new int[ans.size()][2]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> indexPairs(string text, vector<string>& words) {
        unordered_set<string> s(words.begin(), words.end());
        int n = text.size();
        vector<vector<int>> ans;
        for (int i = 0; i < n; ++i) {
            for (int j = i; j < n; ++j) {
                if (s.count(text.substr(i, j - i + 1))) {
                    ans.push_back({i, j});
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func indexPairs(text string, words []string) (ans [][]int) {
	s := map[string]bool{}
	for _, w := range words {
		s[w] = true
	}
	n := len(text)
	for i := 0; i < n; i++ {
		for j := i; j < n; j++ {
			if s[text[i:j+1]] {
				ans = append(ans, []int{i, j})
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Thử mọi lát cắt gây ra nhiều lần tra cứu thất bại, nhất là với chuỗi dài. Các tiền tố chung cho phép duyệt trie từ mỗi vị trí bắt đầu và dừng ngay khi ký tự không khớp.
>
> Chèn các từ vào trie, rồi với mỗi $i$, tiếp tục duyệt node sang phải và ghi nhận $[i,j]$ khi gặp node đánh dấu kết thúc từ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26
        self.is_end = False

    def insert(self, word):
        node = self
        for c in word:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True


class Solution:
    def indexPairs(self, text: str, words: List[str]) -> List[List[int]]:
        trie = Trie()
        for w in words:
            trie.insert(w)
        n = len(text)
        ans = []
        for i in range(n):
            node = trie
            for j in range(i, n):
                idx = ord(text[j]) - ord('a')
                if node.children[idx] is None:
                    break
                node = node.children[idx]
                if node.is_end:
                    ans.append([i, j])
        return ans
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];
    boolean isEnd = false;

    void insert(String word) {
        Trie node = this;
        for (char c : word.toCharArray()) {
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
    public int[][] indexPairs(String text, String[] words) {
        Trie trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        int n = text.length();
        List<int[]> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            Trie node = trie;
            for (int j = i; j < n; ++j) {
                int idx = text.charAt(j) - 'a';
                if (node.children[idx] == null) {
                    break;
                }
                node = node.children[idx];
                if (node.isEnd) {
                    ans.add(new int[] {i, j});
                }
            }
        }
        return ans.toArray(new int[ans.size()][2]);
    }
}
```

#### C++

```cpp
class Trie {
public:
    vector<Trie*> children;
    bool isEnd = false;

    Trie() {
        children.resize(26);
    }

    void insert(string word) {
        Trie* node = this;
        for (char c : word) {
            c -= 'a';
            if (!node->children[c]) node->children[c] = new Trie();
            node = node->children[c];
        }
        node->isEnd = true;
    }
};

class Solution {
public:
    vector<vector<int>> indexPairs(string text, vector<string>& words) {
        Trie* trie = new Trie();
        for (auto w : words) trie->insert(w);
        int n = text.size();
        vector<vector<int>> ans;
        for (int i = 0; i < n; ++i) {
            Trie* node = trie;
            for (int j = i; j < n; ++j) {
                int idx = text[j] - 'a';
                if (!node->children[idx]) break;
                node = node->children[idx];
                if (node->isEnd) ans.push_back({i, j});
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

func (this *Trie) insert(word string) {
	node := this
	for _, c := range word {
		idx := int(c - 'a')
		if node.children[idx] == nil {
			node.children[idx] = newTrie()
		}
		node = node.children[idx]
	}
	node.isEnd = true
}

func indexPairs(text string, words []string) [][]int {
	trie := newTrie()
	for _, w := range words {
		trie.insert(w)
	}
	n := len(text)
	var ans [][]int
	for i := range text {
		node := trie
		for j := i; j < n; j++ {
			idx := int(text[j] - 'a')
			if node.children[idx] == nil {
				break
			}
			node = node.children[idx]
			if node.isEnd {
				ans = append(ans, []int{i, j})
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
