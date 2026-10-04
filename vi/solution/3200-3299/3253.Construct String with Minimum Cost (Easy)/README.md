---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [3253. Construct String with Minimum Cost (Easy) 🔒](https://leetcode.com/problems/construct-string-with-minimum-cost-easy)

[中文文档](/solution/3200-3299/3253.Construct%20String%20with%20Minimum%20Cost%20%28Easy%29/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>target</code>, một mảng chuỗi <code>words</code> và một mảng số nguyên <code>costs</code>, hai mảng có cùng độ dài.</p>

<p>Ban đầu, hãy coi như có một chuỗi rỗng <code>s</code>.</p>

<p>Bạn có thể thực hiện thao tác sau một số lần bất kỳ (bao gồm cả <strong>0</strong> lần):</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> trong khoảng <code>[0, words.length - 1]</code>.</li>
	<li>Nối <code>words[i]</code> vào <code>s</code>.</li>
	<li>Chi phí của thao tác là <code>costs[i]</code>.</li>
</ul>

<p>Trả về chi phí <strong>nhỏ nhất</strong> để biến <code>s</code> thành <code>target</code>. Nếu không thể thực hiện, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = &quot;abcdef&quot;, words = [&quot;abdef&quot;,&quot;abc&quot;,&quot;d&quot;,&quot;def&quot;,&quot;ef&quot;], costs = [100,1,1,10,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chi phí nhỏ nhất có thể đạt được bằng cách thực hiện các thao tác sau:</p>

<ul>
	<li>Chọn chỉ số 1 và nối <code>&quot;abc&quot;</code> vào <code>s</code> với chi phí 1, thu được <code>s = &quot;abc&quot;</code>.</li>
	<li>Chọn chỉ số 2 và nối <code>&quot;d&quot;</code> vào <code>s</code> với chi phí 1, thu được <code>s = &quot;abcd&quot;</code>.</li>
	<li>Chọn chỉ số 4 và nối <code>&quot;ef&quot;</code> vào <code>s</code> với chi phí 5, thu được <code>s = &quot;abcdef&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = &quot;aaaa&quot;, words = [&quot;z&quot;,&quot;zz&quot;,&quot;zzz&quot;], costs = [1,10,100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể biến <code>s</code> thành <code>target</code>, nên ta trả về -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target.length &lt;= 2000</code></li>
	<li><code>1 &lt;= words.length == costs.length &lt;= 50</code></li>
	<li><code>1 &lt;= words[i].length &lt;= target.length</code></li>
	<li><code>target</code> và <code>words[i]</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= costs[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie + Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{target}$ có độ dài $2000$ và có nhiều nhất $50$ từ, vì vậy việc so khớp mọi từ tại mọi chỉ số vẫn chạy được, nhưng các tiền tố chung sẽ bị so sánh lặp lại. Trie cho phép ta duyệt từ $i$ để tìm ra mọi vị trí kết thúc phù hợp.
>
> $\textit{dfs}(i)$ là chi phí nhỏ nhất để tạo $\textit{target}[i:]$. Khi duyệt trie đến một node lưu chi phí, ta cộng $\textit{dfs}(j+1)$. Sau khi ghi nhớ, có $n$ trạng thái, mỗi trạng thái duyệt $O(n)$.

<!-- thinking:end -->

Trước tiên, ta tạo một Trie $\textit{trie}$, trong đó mỗi node của Trie chứa một mảng $\textit{children}$ có độ dài $26$, và mỗi phần tử trong mảng là một con trỏ đến node tiếp theo. Mỗi node trong Trie cũng chứa một biến $\textit{cost}$, biểu diễn chi phí nhỏ nhất từ node gốc đến node hiện tại.

Ta duyệt qua mảng $\textit{words}$, chèn từng từ vào Trie đồng thời cập nhật biến $\textit{cost}$ cho mỗi node.

Tiếp theo, ta định nghĩa hàm tìm kiếm có ghi nhớ $\textit{dfs}(i)$, biểu diễn chi phí nhỏ nhất để tạo chuỗi bắt đầu từ $\textit{target}[i]$. Đáp án là $\textit{dfs}(0)$.

Quá trình tính toán của hàm $\textit{dfs}(i)$ như sau:

- Nếu $i \geq \textit{len}(\textit{target})$, điều đó có nghĩa là toàn bộ chuỗi đã được tạo, nên trả về $0$.
- Ngược lại, ta bắt đầu từ node gốc của $\textit{trie}$ và duyệt tất cả các hậu tố bắt đầu từ $\textit{target}[i]$, tìm chi phí nhỏ nhất, bằng biến $\textit{cost}$ trong $\textit{trie}$ cộng với kết quả của $\textit{dfs}(j+1)$, trong đó $j$ là vị trí kết thúc của hậu tố bắt đầu từ $\textit{target}[i]$.

Cuối cùng, nếu $\textit{dfs}(0) < \textit{inf}$, trả về $\textit{dfs}(0)$; nếu không, trả về $-1$.

Độ phức tạp thời gian là $O(n^2 + L)$, và độ phức tạp không gian là $O(n + L)$. Trong đó, $n$ là độ dài của $\textit{target}$, còn $L$ là tổng độ dài của tất cả các từ trong mảng $\textit{words}$.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children: List[Optional[Trie]] = [None] * 26
        self.cost = inf

    def insert(self, word: str, cost: int):
        node = self
        for c in word:
            idx = ord(c) - ord("a")
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.cost = min(node.cost, cost)


class Solution:
    def minimumCost(self, target: str, words: List[str], costs: List[int]) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= len(target):
                return 0
            ans = inf
            node = trie
            for j in range(i, len(target)):
                idx = ord(target[j]) - ord("a")
                if node.children[idx] is None:
                    return ans
                node = node.children[idx]
                ans = min(ans, node.cost + dfs(j + 1))
            return ans

        trie = Trie()
        for word, cost in zip(words, costs):
            trie.insert(word, cost)
        ans = dfs(0)
        return ans if ans < inf else -1
```

#### Java

```java
class Trie {
    public final int inf = 1 << 29;
    public Trie[] children = new Trie[26];
    public int cost = inf;

    public void insert(String word, int cost) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        node.cost = Math.min(node.cost, cost);
    }
}

class Solution {
    private Trie trie = new Trie();
    private char[] target;
    private Integer[] f;

    public int minimumCost(String target, String[] words, int[] costs) {
        for (int i = 0; i < words.length; ++i) {
            trie.insert(words[i], costs[i]);
        }
        this.target = target.toCharArray();
        f = new Integer[target.length()];
        int ans = dfs(0);
        return ans < trie.inf ? ans : -1;
    }

    private int dfs(int i) {
        if (i >= target.length) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        f[i] = trie.inf;
        Trie node = trie;
        for (int j = i; j < target.length; ++j) {
            int idx = target[j] - 'a';
            if (node.children[idx] == null) {
                return f[i];
            }
            node = node.children[idx];
            f[i] = Math.min(f[i], node.cost + dfs(j + 1));
        }
        return f[i];
    }
}
```

#### C++

```cpp
const int inf = 1 << 29;

class Trie {
public:
    Trie* children[26]{};
    int cost = inf;

    void insert(string& word, int cost) {
        Trie* node = this;
        for (char c : word) {
            int idx = c - 'a';
            if (!node->children[idx]) {
                node->children[idx] = new Trie();
            }
            node = node->children[idx];
        }
        node->cost = min(node->cost, cost);
    }
};

class Solution {
public:
    int minimumCost(string target, vector<string>& words, vector<int>& costs) {
        Trie* trie = new Trie();
        for (int i = 0; i < words.size(); ++i) {
            trie->insert(words[i], costs[i]);
        }
        int n = target.length();
        int f[n];
        memset(f, 0, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (i >= n) {
                return 0;
            }
            if (f[i]) {
                return f[i];
            }
            f[i] = inf;
            Trie* node = trie;
            for (int j = i; j < n; ++j) {
                int idx = target[j] - 'a';
                if (!node->children[idx]) {
                    return f[i];
                }
                node = node->children[idx];
                f[i] = min(f[i], node->cost + dfs(j + 1));
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
const inf = 1 << 29

type Trie struct {
	children [26]*Trie
	cost     int
}

func NewTrie() *Trie {
	return &Trie{cost: inf}
}

func (t *Trie) insert(word string, cost int) {
	node := t
	for _, c := range word {
		idx := c - 'a'
		if node.children[idx] == nil {
			node.children[idx] = NewTrie()
		}
		node = node.children[idx]
	}
	node.cost = min(node.cost, cost)
}

func minimumCost(target string, words []string, costs []int) int {
	trie := NewTrie()
	for i, word := range words {
		trie.insert(word, costs[i])
	}

	n := len(target)
	f := make([]int, n)
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] != 0 {
			return f[i]
		}
		f[i] = inf
		node := trie
		for j := i; j < n; j++ {
			idx := target[j] - 'a'
			if node.children[idx] == nil {
				return f[i]
			}
			node = node.children[idx]
			f[i] = min(f[i], node.cost+dfs(j+1))
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
const inf = 1 << 29;

class Trie {
    children: (Trie | null)[];
    cost: number;

    constructor() {
        this.children = Array(26).fill(null);
        this.cost = inf;
    }

    insert(word: string, cost: number): void {
        let node: Trie = this;
        for (const c of word) {
            const idx = c.charCodeAt(0) - 97;
            if (!node.children[idx]) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx]!;
        }
        node.cost = Math.min(node.cost, cost);
    }
}

function minimumCost(target: string, words: string[], costs: number[]): number {
    const trie = new Trie();
    for (let i = 0; i < words.length; ++i) {
        trie.insert(words[i], costs[i]);
    }

    const n = target.length;
    const f: number[] = Array(n).fill(0);
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
            const idx = target.charCodeAt(j) - 97;
            if (!node?.children[idx]) {
                return f[i];
            }
            node = node.children[idx];
            f[i] = Math.min(f[i], node!.cost + dfs(j + 1));
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
