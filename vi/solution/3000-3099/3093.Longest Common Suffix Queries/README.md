---
comments: true
difficulty: Hard
rating: 2118
source: Weekly Contest 390 Q4
tags:
    - Trie
    - Array
    - String
---

<!-- problem:start -->

# [3093. Longest Common Suffix Queries](https://leetcode.com/problems/longest-common-suffix-queries)

[中文文档](/solution/3000-3099/3093.Longest%20Common%20Suffix%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng chuỗi <code>wordsContainer</code> và <code>wordsQuery</code>.</p>

<p>Với mỗi <code>wordsQuery[i]</code>, cần tìm một chuỗi trong <code>wordsContainer</code> có <strong>hậu tố chung dài nhất</strong> với <code>wordsQuery[i]</code>. Nếu có từ hai chuỗi trở lên trong <code>wordsContainer</code> có cùng hậu tố chung dài nhất, hãy tìm chuỗi có độ dài <strong>nhỏ nhất</strong>. Nếu có từ hai chuỗi trở lên có <strong>cùng</strong> độ dài nhỏ nhất, hãy tìm chuỗi xuất hiện <strong>sớm hơn</strong> trong <code>wordsContainer</code>.</p>

<p>Trả về <em>một mảng số nguyên </em><code>ans</code><em>, trong đó </em><code>ans[i]</code><em> là chỉ số của chuỗi trong </em><code>wordsContainer</code><em> có <strong>hậu tố chung dài nhất</strong> với </em><code>wordsQuery[i]</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">wordsContainer = [&quot;abcd&quot;,&quot;bcd&quot;,&quot;xbcd&quot;], wordsQuery = [&quot;cd&quot;,&quot;bcd&quot;,&quot;xyz&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hãy xét riêng từng <code>wordsQuery[i]</code>:</p>

<ul>
	<li>Với <code>wordsQuery[0] = &quot;cd&quot;</code>, các chuỗi trong <code>wordsContainer</code> có hậu tố chung dài nhất <code>&quot;cd&quot;</code> nằm ở các chỉ số 0, 1 và 2. Trong số đó, đáp án là chuỗi ở chỉ số 1 vì chuỗi này có độ dài nhỏ nhất là 3.</li>
	<li>Với <code>wordsQuery[1] = &quot;bcd&quot;</code>, các chuỗi trong <code>wordsContainer</code> có hậu tố chung dài nhất <code>&quot;bcd&quot;</code> nằm ở các chỉ số 0, 1 và 2. Trong số đó, đáp án là chuỗi ở chỉ số 1 vì chuỗi này có độ dài nhỏ nhất là 3.</li>
	<li>Với <code>wordsQuery[2] = &quot;xyz&quot;</code>, không có chuỗi nào trong <code>wordsContainer</code> có hậu tố chung. Do đó, hậu tố chung dài nhất là <code>&quot;&quot;</code>, và hậu tố này được chia sẻ bởi các chuỗi ở chỉ số 0, 1 và 2. Trong số đó, đáp án là chuỗi ở chỉ số 1 vì chuỗi này có độ dài nhỏ nhất là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">wordsContainer = [&quot;abcdefgh&quot;,&quot;poiuygh&quot;,&quot;ghghgh&quot;], wordsQuery = [&quot;gh&quot;,&quot;acbfgh&quot;,&quot;acbfegh&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,0,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hãy xét riêng từng <code>wordsQuery[i]</code>:</p>

<ul>
	<li>Với <code>wordsQuery[0] = &quot;gh&quot;</code>, các chuỗi trong <code>wordsContainer</code> có hậu tố chung dài nhất <code>&quot;gh&quot;</code> nằm ở các chỉ số 0, 1 và 2. Trong số đó, đáp án là chuỗi ở chỉ số 2 vì chuỗi này có độ dài nhỏ nhất là 6.</li>
	<li>Với <code>wordsQuery[1] = &quot;acbfgh&quot;</code>, chỉ chuỗi ở chỉ số 0 có hậu tố chung dài nhất <code>&quot;fgh&quot;</code>. Do đó, đây là đáp án, dù chuỗi ở chỉ số 2 ngắn hơn.</li>
	<li>Với <code>wordsQuery[2] = &quot;acbfegh&quot;</code>, các chuỗi trong <code>wordsContainer</code> có hậu tố chung dài nhất <code>&quot;gh&quot;</code> nằm ở các chỉ số 0, 1 và 2. Trong số đó, đáp án là chuỗi ở chỉ số 2 vì chuỗi này có độ dài nhỏ nhất là 6.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= wordsContainer.length, wordsQuery.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= wordsContainer[i].length &lt;= 5 * 10<sup>3</sup></code></li>
	<li><code>1 &lt;= wordsQuery[i].length &lt;= 5 * 10<sup>3</sup></code></li>
	<li><code>wordsContainer[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li><code>wordsQuery[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li>Tổng <code>wordsContainer[i].length</code> không vượt quá <code>5 * 10<sup>5</sup></code>.</li>
	<li>Tổng <code>wordsQuery[i].length</code> không vượt quá <code>5 * 10<sup>5</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn cần tìm chuỗi có hậu tố chung dài nhất, sau đó là chuỗi ngắn nhất và cuối cùng là chỉ số nhỏ nhất. Tổng độ dài quá lớn nên không thể kiểm tra từng cặp hậu tố.
>
> Một hậu tố chung chính là tiền tố chung của các chuỗi đảo ngược, và Trie có thể lưu trữ tiền tố này. Mỗi node lưu cặp tốt nhất (độ dài nhỏ nhất, chỉ số nhỏ nhất) tương ứng với hậu tố đó.
>
> Ta chèn các từ trong mảng theo thứ tự đảo ngược, đồng thời cập nhật cặp tốt nhất trên đường đi, rồi duyệt query theo thứ tự đảo ngược cho đến khi không còn cạnh tiếp theo.

<!-- thinking:end -->

Bài toán yêu cầu tìm hậu tố chung dài nhất, vì vậy ta có thể sử dụng Trie.

Ta định nghĩa cấu trúc của node trong Trie như sau:

- `children`: Một mảng có độ dài 26, dùng để lưu các node con.
- `length`: Độ dài của chuỗi ngắn nhất tại node hiện tại.
- `idx`: Chỉ số của chuỗi tại node hiện tại.

Ta duyệt mảng chuỗi `wordsContainer` và chèn từng chuỗi vào Trie theo thứ tự ngược. Trong quá trình chèn, ta cập nhật `length` và `idx` của mỗi node.

Tiếp theo, ta duyệt mảng chuỗi `wordsQuery`. Với mỗi chuỗi, ta tìm trong Trie chỉ số của chuỗi có hậu tố chung dài nhất. Trong quá trình tìm kiếm, nếu gặp một node null, điều đó có nghĩa là không còn hậu tố chung nào nữa, và ta có thể trả về trực tiếp `idx` của node hiện tại.

Độ phức tạp thời gian là $(L_1 \times |\Sigma| + L_2)$, còn độ phức tạp không gian là $O(L_1 \times |\Sigma|)$. Ở đây, $L_1$ và $L_2$ lần lượt là tổng độ dài các chuỗi trong `wordsContainer` và `wordsQuery`; còn $\Sigma$ là kích thước của tập ký tự, trong bài toán này $\Sigma = 26$.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    __slots__ = ("children", "length", "idx")

    def __init__(self):
        self.children = [None] * 26
        self.length = inf
        self.idx = inf

    def insert(self, w: str, i: int):
        node = self
        if node.length > len(w):
            node.length = len(w)
            node.idx = i
        for c in w[::-1]:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
            if node.length > len(w):
                node.length = len(w)
                node.idx = i

    def query(self, w: str) -> int:
        node = self
        for c in w[::-1]:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                break
            node = node.children[idx]
        return node.idx


class Solution:
    def stringIndices(
        self, wordsContainer: List[str], wordsQuery: List[str]
    ) -> List[int]:
        trie = Trie()
        for i, w in enumerate(wordsContainer):
            trie.insert(w, i)
        return [trie.query(w) for w in wordsQuery]
```

#### Java

```java
class Trie {
    private final int inf = 1 << 30;
    private Trie[] children = new Trie[26];
    private int length = inf;
    private int idx = inf;

    public void insert(String w, int i) {
        Trie node = this;
        if (node.length > w.length()) {
            node.length = w.length();
            node.idx = i;
        }
        for (int k = w.length() - 1; k >= 0; --k) {
            int idx = w.charAt(k) - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
            if (node.length > w.length()) {
                node.length = w.length();
                node.idx = i;
            }
        }
    }

    public int query(String w) {
        Trie node = this;
        for (int k = w.length() - 1; k >= 0; --k) {
            int idx = w.charAt(k) - 'a';
            if (node.children[idx] == null) {
                break;
            }
            node = node.children[idx];
        }
        return node.idx;
    }
}

class Solution {
    public int[] stringIndices(String[] wordsContainer, String[] wordsQuery) {
        Trie trie = new Trie();
        for (int i = 0; i < wordsContainer.length; ++i) {
            trie.insert(wordsContainer[i], i);
        }
        int n = wordsQuery.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = trie.query(wordsQuery[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
private:
    const int inf = 1 << 30;
    Trie* children[26];
    int length = inf;
    int idx = inf;

public:
    Trie() {
        for (int i = 0; i < 26; ++i) {
            children[i] = nullptr;
        }
    }

    void insert(string w, int i) {
        Trie* node = this;
        if (node->length > w.length()) {
            node->length = w.length();
            node->idx = i;
        }
        for (int k = w.length() - 1; k >= 0; --k) {
            int idx = w[k] - 'a';
            if (node->children[idx] == nullptr) {
                node->children[idx] = new Trie();
            }
            node = node->children[idx];
            if (node->length > w.length()) {
                node->length = w.length();
                node->idx = i;
            }
        }
    }

    int query(string w) {
        Trie* node = this;
        for (int k = w.length() - 1; k >= 0; --k) {
            int idx = w[k] - 'a';
            if (node->children[idx] == nullptr) {
                break;
            }
            node = node->children[idx];
        }
        return node->idx;
    }
};

class Solution {
public:
    vector<int> stringIndices(vector<string>& wordsContainer, vector<string>& wordsQuery) {
        Trie* trie = new Trie();
        for (int i = 0; i < wordsContainer.size(); ++i) {
            trie->insert(wordsContainer[i], i);
        }
        int n = wordsQuery.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            ans[i] = trie->query(wordsQuery[i]);
        }
        return ans;
    }
};
```

#### Go

```go
const inf = 1 << 30

type Trie struct {
	children [26]*Trie
	length   int
	idx      int
}

func newTrie() *Trie {
	return &Trie{length: inf, idx: inf}
}

func (t *Trie) insert(w string, i int) {
	node := t
	if node.length > len(w) {
		node.length = len(w)
		node.idx = i
	}
	for k := len(w) - 1; k >= 0; k-- {
		idx := int(w[k] - 'a')
		if node.children[idx] == nil {
			node.children[idx] = newTrie()
		}
		node = node.children[idx]
		if node.length > len(w) {
			node.length = len(w)
			node.idx = i
		}
	}
}

func (t *Trie) query(w string) int {
	node := t
	for k := len(w) - 1; k >= 0; k-- {
		idx := int(w[k] - 'a')
		if node.children[idx] == nil {
			break
		}
		node = node.children[idx]
	}
	return node.idx
}

func stringIndices(wordsContainer []string, wordsQuery []string) (ans []int) {
	trie := newTrie()
	for i, w := range wordsContainer {
		trie.insert(w, i)
	}
	for _, w := range wordsQuery {
		ans = append(ans, trie.query(w))
	}
	return
}
```

#### TypeScript

```ts
class Trie {
    private children: Trie[] = new Array<Trie>(26);
    private length: number = Infinity;
    private idx: number = Infinity;

    public insert(w: string, i: number): void {
        let node: Trie = this;
        if (node.length > w.length) {
            node.length = w.length;
            node.idx = i;
        }
        for (let k: number = w.length - 1; k >= 0; --k) {
            let idx: number = w.charCodeAt(k) - 'a'.charCodeAt(0);
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
            if (node.length > w.length) {
                node.length = w.length;
                node.idx = i;
            }
        }
    }

    public query(w: string): number {
        let node: Trie = this;
        for (let k: number = w.length - 1; k >= 0; --k) {
            let idx: number = w.charCodeAt(k) - 'a'.charCodeAt(0);
            if (node.children[idx] == null) {
                break;
            }
            node = node.children[idx];
        }
        return node.idx;
    }
}

function stringIndices(wordsContainer: string[], wordsQuery: string[]): number[] {
    const trie: Trie = new Trie();
    for (let i: number = 0; i < wordsContainer.length; ++i) {
        trie.insert(wordsContainer[i], i);
    }
    const n: number = wordsQuery.length;
    const ans: number[] = new Array<number>(n);
    for (let i: number = 0; i < n; ++i) {
        ans[i] = trie.query(wordsQuery[i]);
    }
    return ans;
}
```

#### Rust

```rust
const INF: i32 = i32::MAX;

struct TrieNode {
    children: [Option<Box<TrieNode>>; 26],
    length: i32,
    idx: i32,
}

impl TrieNode {
    fn new() -> Self {
        Self {
            children: Default::default(),
            length: INF,
            idx: INF,
        }
    }

    fn insert(&mut self, w: &[u8], i: i32) {
        let len = w.len() as i32;
        if self.length > len {
            self.length = len;
            self.idx = i;
        }
        let mut node = self;
        for c in w.iter().rev() {
            let idx = (*c - b'a') as usize;
            if node.children[idx].is_none() {
                node.children[idx] = Some(Box::new(TrieNode::new()));
            }
            node = node.children[idx].as_mut().unwrap();
            if node.length > len {
                node.length = len;
                node.idx = i;
            }
        }
    }

    fn query(&self, w: &[u8]) -> i32 {
        let mut node = self;
        for c in w.iter().rev() {
            let idx = (*c - b'a') as usize;
            match &node.children[idx] {
                Some(child) => node = child,
                None => break,
            }
        }
        node.idx
    }
}

impl Solution {
    pub fn string_indices(words_container: Vec<String>, words_query: Vec<String>) -> Vec<i32> {
        let mut trie = TrieNode::new();
        for (i, w) in words_container.iter().enumerate() {
            trie.insert(w.as_bytes(), i as i32);
        }
        words_query
            .iter()
            .map(|w| trie.query(w.as_bytes()))
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
