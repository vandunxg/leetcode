---
comments: true
difficulty: Easy
rating: 1214
source: Weekly Contest 385 Q1
tags:
    - Trie
    - Array
    - String
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3042. Count Prefix and Suffix Pairs I](https://leetcode.com/problems/count-prefix-and-suffix-pairs-i)

[中文文档](/solution/3000-3099/3042.Count%20Prefix%20and%20Suffix%20Pairs%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Định nghĩa hàm <strong>boolean</strong> <code>isPrefixAndSuffix</code> nhận vào hai chuỗi <code>str1</code> và <code>str2</code>:</p>

<ul>
	<li><code>isPrefixAndSuffix(str1, str2)</code> trả về <code>true</code> nếu <code>str1</code> <strong>vừa là</strong> <span data-keyword="string-prefix">tiền tố</span> và cũng là <span data-keyword="string-suffix">hậu tố</span> của <code>str2</code>, và trả về <code>false</code> trong các trường hợp còn lại.</li>
</ul>

<p>Ví dụ, <code>isPrefixAndSuffix(&quot;aba&quot;, &quot;ababa&quot;)</code> là <code>true</code> vì <code>&quot;aba&quot;</code> là tiền tố và cũng là hậu tố của <code>&quot;ababa&quot;</code>, nhưng <code>isPrefixAndSuffix(&quot;abc&quot;, &quot;abcd&quot;)</code> là <code>false</code>.</p>

<p>Trả về <em>một số nguyên biểu thị <strong>số lượng</strong> cặp chỉ số </em><code>(i, j)</code><em> sao cho </em><code>i &lt; j</code><em> và </em><code>isPrefixAndSuffix(words[i], words[j])</code><em> là </em><code>true</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;aba&quot;,&quot;ababa&quot;,&quot;aa&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các cặp chỉ số được đếm trong ví dụ này là:
i = 0 và j = 1 vì isPrefixAndSuffix(&quot;a&quot;, &quot;aba&quot;) là true.
i = 0 và j = 2 vì isPrefixAndSuffix(&quot;a&quot;, &quot;ababa&quot;) là true.
i = 0 và j = 3 vì isPrefixAndSuffix(&quot;a&quot;, &quot;aa&quot;) là true.
i = 1 và j = 2 vì isPrefixAndSuffix(&quot;aba&quot;, &quot;ababa&quot;) là true.
Vì vậy, đáp án là 4.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;pa&quot;,&quot;papa&quot;,&quot;ma&quot;,&quot;mama&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các cặp chỉ số được đếm trong ví dụ này là:
i = 0 và j = 1 vì isPrefixAndSuffix(&quot;pa&quot;, &quot;papa&quot;) là true.
i = 2 và j = 3 vì isPrefixAndSuffix(&quot;ma&quot;, &quot;mama&quot;) là true.
Vì vậy, đáp án là 2.  </pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abab&quot;,&quot;ab&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Trong ví dụ này, cặp chỉ số hợp lệ duy nhất là i = 0 và j = 1, và isPrefixAndSuffix(&quot;abab&quot;, &quot;ab&quot;) là false.
Vì vậy, đáp án là 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 50</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n \le 50$ và mỗi chuỗi có độ dài không quá $10$, ta có thể kiểm tra mọi cặp $i<j$ xem words[i] có vừa là tiền tố vừa là hậu tố của $\textit{words}[j]$ hay không.
>
> Thư viện đã có sẵn $\textit{startswith}/\textit{endswith}$ để duyệt hai đầu chuỗi, và chi phí này là chấp nhận được.

<!-- thinking:end -->

Ta có thể liệt kê tất cả các cặp chỉ số $(i, j)$ với $i < j$, sau đó xác định xem `words[i]` có là tiền tố hoặc hậu tố của `words[j]` hay không. Nếu đúng, ta tăng biến đếm.

Độ phức tạp thời gian là $O(n^2 \times m)$, trong đó $n$ và $m$ lần lượt là độ dài của `words` và độ dài lớn nhất của các chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPrefixSuffixPairs(self, words: List[str]) -> int:
        ans = 0
        for i, s in enumerate(words):
            for t in words[i + 1 :]:
                ans += t.endswith(s) and t.startswith(s)
        return ans
```

#### Java

```java
class Solution {
    public int countPrefixSuffixPairs(String[] words) {
        int ans = 0;
        int n = words.length;
        for (int i = 0; i < n; ++i) {
            String s = words[i];
            for (int j = i + 1; j < n; ++j) {
                String t = words[j];
                if (t.startsWith(s) && t.endsWith(s)) {
                    ++ans;
                }
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
    int countPrefixSuffixPairs(vector<string>& words) {
        int ans = 0;
        int n = words.size();
        for (int i = 0; i < n; ++i) {
            string s = words[i];
            for (int j = i + 1; j < n; ++j) {
                string t = words[j];
                if (t.find(s) == 0 && t.rfind(s) == t.length() - s.length()) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPrefixSuffixPairs(words []string) (ans int) {
	for i, s := range words {
		for _, t := range words[i+1:] {
			if strings.HasPrefix(t, s) && strings.HasSuffix(t, s) {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countPrefixSuffixPairs(words: string[]): number {
    let ans = 0;
    for (let i = 0; i < words.length; ++i) {
        const s = words[i];
        for (const t of words.slice(i + 1)) {
            if (t.startsWith(s) && t.endsWith(s)) {
                ++ans;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Trie

<!-- thinking:start -->

> **Tư duy**
>
> Khi $n$ và $m$ tăng, việc liệt kê từng cặp sẽ trở nên chậm hơn. Một chuỗi ngắn hơn là tiền tố-hậu tố khi và chỉ khi mọi cặp $(s[i], s[m-1-i])$ đều khớp với chuỗi dài hơn.
>
> Các cặp đó trở thành các cạnh của trie. Chèn các chuỗi theo thứ tự trong mảng và cộng số đếm tại các node trên đường đi sẽ đếm được các cặp với thời gian tuyến tính theo tổng độ dài.

<!-- thinking:end -->

Ta có thể xem mỗi chuỗi $s$ trong mảng chuỗi là một danh sách các cặp ký tự, trong đó mỗi cặp ký tự $(s[i], s[m - i - 1])$ biểu diễn cặp ký tự thứ $i$ của tiền tố và hậu tố của chuỗi $s$.

Ta có thể dùng một trie để lưu tất cả các cặp ký tự, sau đó với mỗi chuỗi $s$, tìm tất cả các cặp ký tự $(s[i], s[m - i - 1])$ trong trie và cộng số đếm của chúng vào đáp án.

Độ phức tạp thời gian là $O(n \times m)$, và độ phức tạp không gian là $O(n \times m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của `words` và độ dài lớn nhất của các chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = ["children", "cnt"]

    def __init__(self):
        self.children = {}
        self.cnt = 0


class Solution:
    def countPrefixSuffixPairs(self, words: List[str]) -> int:
        ans = 0
        trie = Node()
        for s in words:
            node = trie
            for p in zip(s, reversed(s)):
                if p not in node.children:
                    node.children[p] = Node()
                node = node.children[p]
                ans += node.cnt
            node.cnt += 1
        return ans
```

#### Java

```java
class Node {
    Map<Integer, Node> children = new HashMap<>();
    int cnt;
}

class Solution {
    public int countPrefixSuffixPairs(String[] words) {
        int ans = 0;
        Node trie = new Node();
        for (String s : words) {
            Node node = trie;
            int m = s.length();
            for (int i = 0; i < m; ++i) {
                int p = s.charAt(i) * 32 + s.charAt(m - i - 1);
                node.children.putIfAbsent(p, new Node());
                node = node.children.get(p);
                ans += node.cnt;
            }
            ++node.cnt;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Node {
public:
    unordered_map<int, Node*> children;
    int cnt;

    Node()
        : cnt(0) {}
};

class Solution {
public:
    int countPrefixSuffixPairs(vector<string>& words) {
        int ans = 0;
        Node* trie = new Node();
        for (const string& s : words) {
            Node* node = trie;
            int m = s.length();
            for (int i = 0; i < m; ++i) {
                int p = s[i] * 32 + s[m - i - 1];
                if (node->children.find(p) == node->children.end()) {
                    node->children[p] = new Node();
                }
                node = node->children[p];
                ans += node->cnt;
            }
            ++node->cnt;
        }
        return ans;
    }
};
```

#### Go

```go
type Node struct {
	children map[int]*Node
	cnt      int
}

func countPrefixSuffixPairs(words []string) (ans int) {
	trie := &Node{children: make(map[int]*Node)}
	for _, s := range words {
		node := trie
		m := len(s)
		for i := 0; i < m; i++ {
			p := int(s[i])*32 + int(s[m-i-1])
			if _, ok := node.children[p]; !ok {
				node.children[p] = &Node{children: make(map[int]*Node)}
			}
			node = node.children[p]
			ans += node.cnt
		}
		node.cnt++
	}
	return
}
```

#### TypeScript

```ts
class Node {
    children: Map<number, Node> = new Map<number, Node>();
    cnt: number = 0;
}

function countPrefixSuffixPairs(words: string[]): number {
    let ans: number = 0;
    const trie: Node = new Node();
    for (const s of words) {
        let node: Node = trie;
        const m: number = s.length;
        for (let i: number = 0; i < m; ++i) {
            const p: number = s.charCodeAt(i) * 32 + s.charCodeAt(m - i - 1);
            if (!node.children.has(p)) {
                node.children.set(p, new Node());
            }
            node = node.children.get(p)!;
            ans += node.cnt;
        }
        ++node.cnt;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
