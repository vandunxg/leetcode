---
comments: true
difficulty: Hard
rating: 1725
source: Weekly Contest 311 Q4
tags:
    - Trie
    - Array
    - String
    - Counting
---

<!-- problem:start -->

# [2416. Sum of Prefix Scores of Strings](https://leetcode.com/problems/sum-of-prefix-scores-of-strings)

[中文文档](/solution/2400-2499/2416.Sum%20of%20Prefix%20Scores%20of%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>words</code> có kích thước <code>n</code>, gồm các chuỗi <strong>không rỗng</strong>.</p>

<p>Ta định nghĩa <strong>điểm số</strong> của một chuỗi <code>term</code> là <strong>số lượng</strong> chuỗi <code>words[i]</code> sao cho <code>term</code> là <strong>tiền tố</strong> của <code>words[i]</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>words = [&quot;a&quot;, &quot;ab&quot;, &quot;abc&quot;, &quot;cab&quot;]</code>, thì điểm số của <code>&quot;ab&quot;</code> là <code>2</code>, vì <code>&quot;ab&quot;</code> là tiền tố của cả <code>&quot;ab&quot;</code> và <code>&quot;abc&quot;</code>.</li>
</ul>

<p>Trả về <em>một mảng </em><code>answer</code><em> có kích thước </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là <strong>tổng</strong> điểm số của mọi tiền tố <strong>không rỗng</strong> của </em><code>words[i]</code>.</p>

<p><strong>Lưu ý</strong> rằng một chuỗi được xem là tiền tố của chính nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abc&quot;,&quot;ab&quot;,&quot;bc&quot;,&quot;b&quot;]
<strong>Đầu ra:</strong> [5,4,3,2]
<strong>Giải thích:</strong> Đáp án cho mỗi chuỗi như sau:
- &quot;abc&quot; có 3 tiền tố: &quot;a&quot;, &quot;ab&quot; và &quot;abc&quot;.
- Có 2 chuỗi có tiền tố &quot;a&quot;, 2 chuỗi có tiền tố &quot;ab&quot; và 1 chuỗi có tiền tố &quot;abc&quot;.
Tổng là answer[0] = 2 + 2 + 1 = 5.
- &quot;ab&quot; có 2 tiền tố: &quot;a&quot; và &quot;ab&quot;.
- Có 2 chuỗi có tiền tố &quot;a&quot; và 2 chuỗi có tiền tố &quot;ab&quot;.
Tổng là answer[1] = 2 + 2 = 4.
- &quot;bc&quot; có 2 tiền tố: &quot;b&quot; và &quot;bc&quot;.
- Có 2 chuỗi có tiền tố &quot;b&quot; và 1 chuỗi có tiền tố &quot;bc&quot;.
Tổng là answer[2] = 2 + 1 = 3.
- &quot;b&quot; có 1 tiền tố: &quot;b&quot;.
- Có 2 chuỗi có tiền tố &quot;b&quot;.
Tổng là answer[3] = 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abcd&quot;]
<strong>Đầu ra:</strong> [4]
<strong>Giải thích:</strong>
&quot;abcd&quot; có 4 tiền tố: &quot;a&quot;, &quot;ab&quot;, &quot;abc&quot; và &quot;abcd&quot;.
Mỗi tiền tố có điểm số bằng một, nên tổng là answer[0] = 1 + 1 + 1 + 1 = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 1000</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cây tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số của một từ là tổng số lần xuất hiện của tất cả tiền tố của từ đó. Việc duyệt từng từ cho mọi tiền tố sẽ tốn nhiều thời gian khi tổng độ dài đạt $10^6$. Các tiền tố chung nên được lưu trong một trie.
>
> Chèn từng từ vào trie và tăng $\textit{cnt}$ tại mỗi node. Sau đó, duyệt lại từng từ và cộng $\textit{cnt}$. Thời gian chạy tuyến tính theo tổng số ký tự.

<!-- thinking:end -->

Ta có thể sử dụng cây tiền tố để lưu tất cả tiền tố của các chuỗi và đếm số lần xuất hiện của mỗi tiền tố.

Ta định nghĩa cấu trúc node của cây tiền tố `Trie`, gồm hai thuộc tính:

- `children`: Một mảng có độ dài 26, dùng để lưu các node con của node hiện tại.
- `cnt`: Số lần xuất hiện của node hiện tại.

Ta định nghĩa hai phương thức cho cây tiền tố:

- `insert`: Chèn một chuỗi, thêm các tiền tố của chuỗi đó vào cây tiền tố.
- `search`: Tìm kiếm một chuỗi và trả về tổng số lần xuất hiện của các tiền tố của chuỗi đó.

Ta duyệt qua tất cả các chuỗi và chèn từng chuỗi vào cây tiền tố. Sau đó, ta duyệt lại tất cả các chuỗi, gọi phương thức `search` cho từng chuỗi và cộng số lần xuất hiện của mỗi tiền tố.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    __slots__ = "children", "cnt"

    def __init__(self):
        self.children = [None] * 26
        self.cnt = 0

    def insert(self, w):
        node = self
        for c in w:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
            node.cnt += 1

    def search(self, w):
        node = self
        ans = 0
        for c in w:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                return ans
            node = node.children[idx]
            ans += node.cnt
        return ans


class Solution:
    def sumPrefixScores(self, words: List[str]) -> List[int]:
        trie = Trie()
        for w in words:
            trie.insert(w)
        return [trie.search(w) for w in words]
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private int cnt;

    public void insert(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
            ++node.cnt;
        }
    }

    public int search(String w) {
        Trie node = this;
        int ans = 0;
        for (char c : w.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                return ans;
            }
            node = node.children[c];
            ans += node.cnt;
        }
        return ans;
    }
}

class Solution {
    public int[] sumPrefixScores(String[] words) {
        Trie trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        int[] ans = new int[words.length];
        for (int i = 0; i < words.length; ++i) {
            ans[i] = trie.search(words[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
private:
    Trie* children[26]{};
    int cnt = 0;

public:
    void insert(string& w) {
        Trie* node = this;
        for (char c : w) {
            int idx = c - 'a';
            if (!node->children[idx]) {
                node->children[idx] = new Trie();
            }
            node = node->children[idx];
            ++node->cnt;
        }
    }

    int search(string& w) {
        Trie* node = this;
        int ans = 0;
        for (char c : w) {
            int idx = c - 'a';
            if (!node->children[idx]) {
                return ans;
            }
            node = node->children[idx];
            ans += node->cnt;
        }
        return ans;
    }
};

class Solution {
public:
    vector<int> sumPrefixScores(vector<string>& words) {
        Trie* trie = new Trie();
        for (auto& w : words) {
            trie->insert(w);
        }
        vector<int> ans;
        for (auto& w : words) {
            ans.push_back(trie->search(w));
        }
        return ans;
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

func (this *Trie) search(word string) int {
	node := this
	ans := 0
	for _, c := range word {
		c -= 'a'
		if node.children[c] == nil {
			return ans
		}
		node = node.children[c]
		ans += node.cnt
	}
	return ans
}

func sumPrefixScores(words []string) []int {
	trie := newTrie()
	for _, w := range words {
		trie.insert(w)
	}
	ans := make([]int, len(words))
	for i, w := range words {
		ans[i] = trie.search(w)
	}
	return ans
}
```

#### TypeScript

```ts
class Trie {
    children: Array<any>;
    cnt: number;

    constructor() {
        this.children = Array(26);
        this.cnt = 0;
    }

    insert(w: string): void {
        let node = this;
        for (const c of w) {
            const idx = c.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[idx]) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
            node.cnt++;
        }
    }

    search(w: string): number {
        let node = this;
        let ans = 0;
        for (const c of w) {
            const idx = c.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[idx]) {
                return ans;
            }
            node = node.children[idx];
            ans += node.cnt;
        }
        return ans;
    }
}

function sumPrefixScores(words: string[]): number[] {
    const trie = new Trie();
    for (const w of words) {
        trie.insert(w);
    }
    return words.map(w => trie.search(w));
}
```

#### JavaScript

```js
class Trie {
    constructor() {
        this.children = {};
        this.cnt = 0;
    }

    insert(w) {
        let node = this;
        for (const c of w) {
            if (!node.children[c]) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
            node.cnt++;
        }
    }

    search(w) {
        let node = this;
        let ans = 0;
        for (const c of w) {
            if (!node.children[c]) {
                return ans;
            }
            node = node.children[c];
            ans += node.cnt;
        }
        return ans;
    }
}

/**
 * @param {string[]} words
 * @return {number[]}
 */
var sumPrefixScores = function (words) {
    const trie = new Trie();
    for (const w of words) {
        trie.insert(w);
    }
    return words.map(w => trie.search(w));
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
