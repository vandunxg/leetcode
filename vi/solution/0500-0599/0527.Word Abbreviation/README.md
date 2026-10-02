---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Trie
    - Array
    - String
    - Sorting
---

<!-- problem:start -->

# [527. Word Abbreviation 🔒](https://leetcode.com/problems/word-abbreviation)

[中文文档](/solution/0500-0599/0527.Word%20Abbreviation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các chuỗi <code>words</code> <strong>đôi một khác nhau</strong>, hãy trả về <em>các dạng <strong>viết tắt</strong> ngắn nhất có thể cho từng từ</em>.</p>

<p>Quy tắc viết tắt một chuỗi như sau:</p>

<ol>
	<li>Dạng viết tắt <strong>ban đầu</strong> của mỗi từ gồm ký tự đầu tiên, tiếp theo là số ký tự ở giữa, rồi đến ký tự cuối cùng.</li>
	<li>Nếu nhiều từ có cùng một dạng viết tắt, hãy thực hiện thao tác sau:
	<ul>
		<li><strong>Tăng</strong> độ dài prefix (số ký tự ở phần đầu) của dạng viết tắt mỗi từ thêm <code>1</code>.</li>
		<ul>
			<li>Ví dụ, ban đầu hai từ <code>[&quot;abcdef&quot;,&quot;abndef&quot;]</code> đều được viết tắt thành <code>&quot;a4f&quot;</code>. Sau đó, các bước lần lượt là <code>[&quot;a4f&quot;,&quot;a4f&quot;]</code> -&gt; <code>[&quot;ab3f&quot;,&quot;ab3f&quot;]</code> -&gt; <code>[&quot;abc2f&quot;,&quot;abn2f&quot;]</code>.</li>
		</ul>
		</li>
		<li>Lặp lại thao tác này cho đến khi mọi dạng viết tắt đều <strong>khác nhau</strong>.</li>
	</ul>
	</li>
	<li>Cuối cùng, nếu dạng viết tắt không làm từ ngắn hơn thì giữ nguyên từ ban đầu.</li>
</ol>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> words = ["like","god","internal","me","internet","interval","intension","face","intrusion"]
<strong>Đầu ra:</strong> ["l2e","god","internal","me","i6t","interval","inte4n","f2e","intr4n"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> words = ["aa","aaa"]
<strong>Đầu ra:</strong> ["aa","aaa"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 400</code></li>
	<li><code>2 &lt;= words[i].length &lt;= 400</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Mọi chuỗi trong <code>words</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie phân nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Dạng viết tắt gồm ký tự đầu, số ký tự ở giữa và ký tự cuối; nếu bị trùng thì cần prefix dài hơn. Có thể tìm LCP theo từng cặp với $n \le 400$, nhưng cách này lặp lại nhiều công việc xử lý prefix.
>
> Chỉ những từ có cùng độ dài và ký tự cuối mới có thể tạo ra dạng viết tắt trùng nhau, nên ta nhóm theo $(|w|, w[-1])$. Chèn từng nhóm vào một trie có lưu số từ đi qua mỗi node; node đầu tiên chỉ có một từ đi qua xác định prefix phân biệt. Nếu dạng viết tắt không ngắn hơn từ ban đầu thì dùng lại từ đó.

<!-- thinking:end -->

Ta nhận thấy nếu hai từ có cùng dạng viết tắt thì chúng phải có cùng ký tự đầu, ký tự cuối và độ dài. Vì vậy, ta có thể nhóm các từ theo độ dài và ký tự cuối, rồi dùng trie lưu thông tin từng nhóm.

Mỗi node trong trie có cấu trúc như sau:

- `children`: Mảng có độ dài $26$, lưu các node con của node này.
- `cnt`: Số từ đi qua node này.

Với mỗi từ, ta chèn từ đó vào trie và ghi nhận giá trị `cnt` của từng node.

Khi truy vấn, ta bắt đầu từ root. Với ký tự hiện tại, nếu giá trị `cnt` của node con tương ứng bằng $1$, ta đã tìm được prefix tạo ra dạng viết tắt duy nhất và trả về độ dài prefix hiện tại. Nếu chưa, ta tiếp tục đi xuống. Nếu duyệt hết mà vẫn chưa tìm được dạng viết tắt duy nhất, ta trả về độ dài từ ban đầu. Sau khi tìm được độ dài prefix cho mọi từ, ta kiểm tra dạng viết tắt có ngắn hơn từ gốc hay không: nếu có thì thêm dạng viết tắt vào kết quả, nếu không thì thêm từ ban đầu.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các từ.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    __slots__ = ["children", "cnt"]

    def __init__(self):
        self.children = [None] * 26
        self.cnt = 0

    def insert(self, w: str):
        node = self
        for c in w:
            idx = ord(c) - ord("a")
            if not node.children[idx]:
                node.children[idx] = Trie()
            node = node.children[idx]
            node.cnt += 1

    def search(self, w: str) -> int:
        node = self
        cnt = 0
        for c in w:
            cnt += 1
            idx = ord(c) - ord("a")
            node = node.children[idx]
            if node.cnt == 1:
                return cnt
        return len(w)


class Solution:
    def wordsAbbreviation(self, words: List[str]) -> List[str]:
        tries = {}
        for w in words:
            m = len(w)
            if (m, w[-1]) not in tries:
                tries[(m, w[-1])] = Trie()
            tries[(m, w[-1])].insert(w)
        ans = []
        for w in words:
            cnt = tries[(len(w), w[-1])].search(w)
            ans.append(
                w if cnt + 2 >= len(w) else w[:cnt] + str(len(w) - cnt - 1) + w[-1]
            )
        return ans
```

#### Java

```java
class Trie {
    private final Trie[] children = new Trie[26];
    private int cnt;

    public void insert(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
            ++node.cnt;
        }
    }

    public int search(String w) {
        Trie node = this;
        int ans = 0;
        for (char c : w.toCharArray()) {
            ++ans;
            int idx = c - 'a';
            node = node.children[idx];
            if (node.cnt == 1) {
                return ans;
            }
        }
        return w.length();
    }
}

class Solution {
    public List<String> wordsAbbreviation(List<String> words) {
        Map<List<Integer>, Trie> tries = new HashMap<>();
        for (var w : words) {
            var key = List.of(w.length(), w.charAt(w.length() - 1) - 'a');
            tries.putIfAbsent(key, new Trie());
            tries.get(key).insert(w);
        }
        List<String> ans = new ArrayList<>();
        for (var w : words) {
            int m = w.length();
            var key = List.of(m, w.charAt(m - 1) - 'a');
            int cnt = tries.get(key).search(w);
            ans.add(cnt + 2 >= m ? w : w.substring(0, cnt) + (m - cnt - 1) + w.substring(m - 1));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
public:
    Trie()
        : cnt(0) {
        fill(children.begin(), children.end(), nullptr);
    }

    void insert(const string& w) {
        Trie* node = this;
        for (char c : w) {
            int idx = c - 'a';
            if (node->children[idx] == nullptr) {
                node->children[idx] = new Trie();
            }
            node = node->children[idx];
            ++node->cnt;
        }
    }

    int search(const string& w) {
        Trie* node = this;
        int ans = 0;
        for (char c : w) {
            ++ans;
            int idx = c - 'a';
            node = node->children[idx];
            if (node->cnt == 1) {
                return ans;
            }
        }
        return w.size();
    }

private:
    array<Trie*, 26> children;
    int cnt;
};

class Solution {
public:
    vector<string> wordsAbbreviation(vector<string>& words) {
        map<pair<int, int>, Trie*> tries;
        for (const auto& w : words) {
            pair<int, int> key = {static_cast<int>(w.size()), w.back() - 'a'};
            if (tries.find(key) == tries.end()) {
                tries[key] = new Trie();
            }
            tries[key]->insert(w);
        }

        vector<string> ans;
        for (const auto& w : words) {
            int m = w.size();
            pair<int, int> key = {m, w.back() - 'a'};
            int cnt = tries[key]->search(w);
            ans.push_back((cnt + 2 >= m) ? w : w.substr(0, cnt) + to_string(m - cnt - 1) + w.back());
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

func (t *Trie) insert(w string) {
	node := t
	for _, c := range w {
		idx := c - 'a'
		if node.children[idx] == nil {
			node.children[idx] = &Trie{}
		}
		node = node.children[idx]
		node.cnt++
	}
}

func (t *Trie) search(w string) int {
	node := t
	ans := 0
	for _, c := range w {
		ans++
		idx := c - 'a'
		node = node.children[idx]
		if node.cnt == 1 {
			return ans
		}
	}
	return len(w)
}

func wordsAbbreviation(words []string) (ans []string) {
	tries := make(map[[2]int]*Trie)
	for _, w := range words {
		key := [2]int{len(w), int(w[len(w)-1] - 'a')}
		_, exists := tries[key]
		if !exists {
			tries[key] = &Trie{}
		}
		tries[key].insert(w)
	}

	for _, w := range words {
		m := len(w)
		key := [2]int{m, int(w[m-1] - 'a')}
		cnt := tries[key].search(w)
		if cnt+2 >= m {
			ans = append(ans, w)
		} else {
			abbr := w[:cnt] + fmt.Sprintf("%d", m-cnt-1) + w[m-1:]
			ans = append(ans, abbr)
		}
	}
	return
}
```

#### TypeScript

```ts
class Trie {
    private children: Trie[] = Array(26);
    private cnt: number = 0;

    insert(w: string): void {
        let node: Trie = this;
        for (const c of w) {
            const idx: number = c.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[idx]) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
            node.cnt++;
        }
    }

    search(w: string): number {
        let node: Trie = this;
        let ans: number = 0;
        for (const c of w) {
            ans++;
            const idx: number = c.charCodeAt(0) - 'a'.charCodeAt(0);
            node = node.children[idx];
            if (node.cnt === 1) {
                return ans;
            }
        }
        return w.length;
    }
}

function wordsAbbreviation(words: string[]): string[] {
    const tries: Map<string, Trie> = new Map();
    for (const w of words) {
        const key: string = `${w.length}-${w.charCodeAt(w.length - 1) - 'a'.charCodeAt(0)}`;
        if (!tries.get(key)) {
            tries.set(key, new Trie());
        }
        tries.get(key)!.insert(w);
    }

    const ans: string[] = [];
    for (const w of words) {
        const m: number = w.length;
        const key: string = `${m}-${w.charCodeAt(m - 1) - 'a'.charCodeAt(0)}`;
        const cnt: number = tries.get(key)!.search(w);
        ans.push(cnt + 2 >= m ? w : w.substring(0, cnt) + (m - cnt - 1) + w.substring(m - 1));
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
