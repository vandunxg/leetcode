---
comments: true
difficulty: Medium
rating: 2081
source: Weekly Contest 415 Q3
tags:
    - Greedy
    - Trie
    - Segment Tree
    - Array
    - String
    - Binary Search
    - Dynamic Programming
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3291. Minimum Number of Valid Strings to Form Target I](https://leetcode.com/problems/minimum-number-of-valid-strings-to-form-target-i)

[中文文档](/solution/3200-3299/3291.Minimum%20Number%20of%20Valid%20Strings%20to%20Form%20Target%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng các chuỗi <code>words</code> và một chuỗi <code>target</code>.</p>

<p>Một chuỗi <code>x</code> được gọi là <strong>hợp lệ</strong> nếu <code>x</code> là <span data-keyword="string-prefix">prefix</span> của <strong>bất kỳ</strong> chuỗi nào trong <code>words</code>.</p>

<p>Trả về số lượng <strong>nhỏ nhất</strong> các chuỗi <strong>hợp lệ</strong> có thể được <em>nối</em> lại để tạo thành <code>target</code>. Nếu <strong>không thể</strong> tạo thành <code>target</code>, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abc&quot;,&quot;aaaaa&quot;,&quot;bcdef&quot;], target = &quot;aabcdabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi đích có thể được tạo thành bằng cách nối:</p>

<ul>
	<li>Tiền tố có độ dài 2 của <code>words[1]</code>, tức là <code>&quot;aa&quot;</code>.</li>
	<li>Tiền tố có độ dài 3 của <code>words[2]</code>, tức là <code>&quot;bcd&quot;</code>.</li>
	<li>Tiền tố có độ dài 3 của <code>words[0]</code>, tức là <code>&quot;abc&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abababab&quot;,&quot;ab&quot;], target = &quot;ababaababa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi đích có thể được tạo thành bằng cách nối:</p>

<ul>
	<li>Tiền tố có độ dài 5 của <code>words[0]</code>, tức là <code>&quot;ababa&quot;</code>.</li>
	<li>Tiền tố có độ dài 5 của <code>words[0]</code>, tức là <code>&quot;ababa&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abcdef&quot;], target = &quot;xyz&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 5 * 10<sup>3</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>sum(words[i].length) &lt;= 10<sup>5</sup></code>.</li>
	<li><code>words[i]</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= target.length &lt;= 5 * 10<sup>3</sup></code></li>
	<li><code>target</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie + Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Nối các tiền tố của $\textit{words}$ để tạo thành $\textit{target}$ với ít mảnh nhất. $|target|\le 5\times 10^3$ và tổng độ dài các từ là $10^5$; thử mọi từ tại mọi chỉ số sẽ lặp lại việc xử lý các tiền tố.
>
> Lưu mọi từ trong một trie. Từ $i$, duyệt $\textit{target}$ xuống trie; mỗi node tồn tại là một điểm cắt, đồng thời tính $\textit{dfs}(j+1)$. Sau khi ghi nhớ, mỗi vị trí bắt đầu chỉ cần duyệt nhiều nhất $O(n)$.

<!-- thinking:end -->

Ta có thể dùng một trie để lưu tất cả các chuỗi hợp lệ, sau đó dùng tìm kiếm có ghi nhớ để tính đáp án.

Ta định nghĩa hàm $\textit{dfs}(i)$, biểu diễn số lượng chuỗi nhỏ nhất cần nối bắt đầu từ ký tự thứ $i$ của chuỗi $\textit{target}$. Đáp án là $\textit{dfs}(0)$.

Hàm $\textit{dfs}(i)$ được tính như sau:

- Nếu $i \geq n$, điều đó có nghĩa là đã duyệt hết chuỗi $\textit{target}$, nên ta trả về $0$;
- Ngược lại, ta có thể tìm các chuỗi hợp lệ trong trie bắt đầu bằng $\textit{target}[i]$, rồi đệ quy tính $\textit{dfs}(i + \text{len}(w))$, trong đó $w$ là chuỗi hợp lệ được tìm thấy. Ta lấy giá trị nhỏ nhất trong các giá trị này rồi cộng thêm $1$ để làm giá trị trả về của $\textit{dfs}(i)$.

Để tránh tính toán lặp lại, ta sử dụng ghi nhớ.

Độ phức tạp thời gian là $O(n^2 + L)$, còn độ phức tạp không gian là $O(n + L)$. Trong đó, $n$ là độ dài chuỗi $\textit{target}$ và $L$ là tổng độ dài của tất cả các chuỗi hợp lệ.

<!-- tabs:start -->

#### Python3

```python
def min(a: int, b: int) -> int:
    return a if a < b else b


class Trie:
    def __init__(self):
        self.children: List[Optional[Trie]] = [None] * 26

    def insert(self, w: str):
        node = self
        for i in map(lambda c: ord(c) - 97, w):
            if node.children[i] is None:
                node.children[i] = Trie()
            node = node.children[i]


class Solution:
    def minValidStrings(self, words: List[str], target: str) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= n:
                return 0
            node = trie
            ans = inf
            for j in range(i, n):
                k = ord(target[j]) - 97
                if node.children[k] is None:
                    break
                node = node.children[k]
                ans = min(ans, 1 + dfs(j + 1))
            return ans

        trie = Trie()
        for w in words:
            trie.insert(w)
        n = len(target)
        ans = dfs(0)
        return ans if ans < inf else -1
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];

    void insert(String w) {
        Trie node = this;
        for (int i = 0; i < w.length(); ++i) {
            int j = w.charAt(i) - 'a';
            if (node.children[j] == null) {
                node.children[j] = new Trie();
            }
            node = node.children[j];
        }
    }
}

class Solution {
    private Integer[] f;
    private char[] s;
    private Trie trie;
    private final int inf = 1 << 30;

    public int minValidStrings(String[] words, String target) {
        trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        s = target.toCharArray();
        f = new Integer[s.length];
        int ans = dfs(0);
        return ans < inf ? ans : -1;
    }

    private int dfs(int i) {
        if (i >= s.length) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        Trie node = trie;
        f[i] = inf;
        for (int j = i; j < s.length; ++j) {
            int k = s[j] - 'a';
            if (node.children[k] == null) {
                break;
            }
            f[i] = Math.min(f[i], 1 + dfs(j + 1));
            node = node.children[k];
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Trie {
public:
    Trie* children[26]{};

    void insert(string& word) {
        Trie* node = this;
        for (char& c : word) {
            int i = c - 'a';
            if (!node->children[i]) {
                node->children[i] = new Trie();
            }
            node = node->children[i];
        }
    }
};

class Solution {
public:
    int minValidStrings(vector<string>& words, string target) {
        int n = target.size();
        Trie* trie = new Trie();
        for (auto& w : words) {
            trie->insert(w);
        }
        const int inf = 1 << 30;
        int f[n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (i >= n) {
                return 0;
            }
            if (f[i] != -1) {
                return f[i];
            }
            f[i] = inf;
            Trie* node = trie;
            for (int j = i; j < n; ++j) {
                int k = target[j] - 'a';
                if (!node->children[k]) {
                    break;
                }
                node = node->children[k];
                f[i] = min(f[i], 1 + dfs(j + 1));
            }
            return f[i];
        };
        int ans = dfs(0);
        return ans < inf ? ans : -1;
    }
};
```

#### Go

```go
type Trie struct {
	children [26]*Trie
}

func (t *Trie) insert(word string) {
	node := t
	for _, c := range word {
		idx := c - 'a'
		if node.children[idx] == nil {
			node.children[idx] = &Trie{}
		}
		node = node.children[idx]
	}
}

func minValidStrings(words []string, target string) int {
	n := len(target)
	trie := &Trie{}
	for _, w := range words {
		trie.insert(w)
	}
	const inf int = 1 << 30
	f := make([]int, n)
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] != 0 {
			return f[i]
		}
		node := trie
		f[i] = inf
		for j := i; j < n; j++ {
			k := int(target[j] - 'a')
			if node.children[k] == nil {
				break
			}
			f[i] = min(f[i], 1+dfs(j+1))
			node = node.children[k]
		}
		return f[i]
	}
	if ans := dfs(0); ans < inf {
		return ans
	}
	return -1
}
```

#### TypeScript

```ts
class Trie {
    children: (Trie | null)[] = Array(26).fill(null);

    insert(word: string): void {
        let node: Trie = this;
        for (const c of word) {
            const i = c.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[i]) {
                node.children[i] = new Trie();
            }
            node = node.children[i];
        }
    }
}

function minValidStrings(words: string[], target: string): number {
    const n = target.length;
    const trie = new Trie();
    for (const w of words) {
        trie.insert(w);
    }
    const inf = 1 << 30;
    const f = Array(n).fill(0);

    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i]) {
            return f[i];
        }
        f[i] = inf;
        let node: Trie | null = trie;
        for (let j = i; j < n; ++j) {
            const k = target[j].charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node?.children[k]) {
                break;
            }
            node = node.children[k];
            f[i] = Math.min(f[i], 1 + dfs(j + 1));
        }
        return f[i];
    };

    const ans = dfs(0);
    return ans < inf ? ans : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
