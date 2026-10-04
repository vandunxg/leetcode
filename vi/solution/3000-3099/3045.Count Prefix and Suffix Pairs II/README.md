---
comments: true
difficulty: Hard
rating: 2327
source: Weekly Contest 385 Q4
tags:
    - Trie
    - Array
    - String
    - String Matching
    - Hash Function
    - Rolling Hash
    - Extended KMP
---

<!-- problem:start -->

# [3045. Count Prefix and Suffix Pairs II](https://leetcode.com/problems/count-prefix-and-suffix-pairs-ii)

[中文文档](/solution/3000-3099/3045.Count%20Prefix%20and%20Suffix%20Pairs%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng chuỗi <code>words</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Ta định nghĩa một hàm <strong>boolean</strong> <code>isPrefixAndSuffix</code> nhận hai chuỗi <code>str1</code> và <code>str2</code>:</p>

<ul>
	<li><code>isPrefixAndSuffix(str1, str2)</code> trả về <code>true</code> nếu <code>str1</code> <strong>vừa là</strong> <span data-keyword="string-prefix">prefix</span> vừa là <span data-keyword="string-suffix">suffix</span> của <code>str2</code>, và trả về <code>false</code> trong trường hợp ngược lại.</li>
</ul>

<p>Ví dụ, <code>isPrefixAndSuffix(&quot;aba&quot;, &quot;ababa&quot;)</code> là <code>true</code> vì <code>&quot;aba&quot;</code> là prefix và cũng là suffix của <code>&quot;ababa&quot;</code>, nhưng <code>isPrefixAndSuffix(&quot;abc&quot;, &quot;abcd&quot;)</code> là <code>false</code>.</p>

<p>Trả về <em>một số nguyên biểu thị <strong>số lượng</strong> cặp chỉ số </em><code>(i<em>, </em>j)</code><em> sao cho </em><code>i &lt; j</code><em> và </em><code>isPrefixAndSuffix(words[i], words[j])</code><em> là </em><code>true</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;aba&quot;,&quot;ababa&quot;,&quot;aa&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Trong ví dụ này, các cặp chỉ số được đếm là:
i = 0 và j = 1 vì isPrefixAndSuffix(&quot;a&quot;, &quot;aba&quot;) là true.
i = 0 và j = 2 vì isPrefixAndSuffix(&quot;a&quot;, &quot;ababa&quot;) là true.
i = 0 và j = 3 vì isPrefixAndSuffix(&quot;a&quot;, &quot;aa&quot;) là true.
i = 1 và j = 2 vì isPrefixAndSuffix(&quot;aba&quot;, &quot;ababa&quot;) là true.
Do đó, đáp án là 4.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;pa&quot;,&quot;papa&quot;,&quot;ma&quot;,&quot;mama&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, các cặp chỉ số được đếm là:
i = 0 và j = 1 vì isPrefixAndSuffix(&quot;pa&quot;, &quot;papa&quot;) là true.
i = 2 và j = 3 vì isPrefixAndSuffix(&quot;ma&quot;, &quot;mama&quot;) là true.
Do đó, đáp án là 2.  </pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abab&quot;,&quot;ab&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Trong ví dụ này, cặp chỉ số hợp lệ duy nhất là i = 0 và j = 1, và isPrefixAndSuffix(&quot;abab&quot;, &quot;ab&quot;) là false.
Do đó, đáp án là 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10<sup>5</sup></code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tổng độ dài của tất cả <code>words[i]</code> không vượt quá <code>5 * 10<sup>5</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie

<!-- thinking:start -->

> **Tư duy**
>
> $n$ và tổng độ dài đạt tới $10^5$, vì vậy vòng lặp kép của phần I không còn phù hợp và cần dùng trie từ phương pháp thứ hai của phần I.
>
> Các cặp $(s[i], s[m-1-i])$ là các cạnh. Khi chèn theo thứ tự đầu vào, các bộ đếm đã có trên đường đi chính là các chuỗi trước đó mà chuỗi hiện tại vừa là prefix vừa là suffix.
>
> Ta tăng bộ đếm của nút kết thúc sau đó để chỉ đếm các cặp có $i<j$.

<!-- thinking:end -->

Ta có thể xem mỗi chuỗi $s$ trong mảng chuỗi là một danh sách các cặp ký tự, trong đó mỗi cặp ký tự $(s[i], s[m - i - 1])$ biểu diễn cặp ký tự thứ $i$ của prefix và suffix của chuỗi $s$.

Ta có thể dùng một trie để lưu tất cả các cặp ký tự, sau đó với mỗi chuỗi $s$, tìm tất cả các cặp ký tự $(s[i], s[m - i - 1])$ trong trie và cộng số lượng của chúng vào đáp án.

Độ phức tạp thời gian là $O(n \times m)$, còn độ phức tạp không gian là $O(n \times m)$. Ở đây, $n$ và $m$ lần lượt là độ dài của `words` và độ dài lớn nhất của các chuỗi.

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
    public long countPrefixSuffixPairs(String[] words) {
        long ans = 0;
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
    long long countPrefixSuffixPairs(vector<string>& words) {
        long long ans = 0;
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

func countPrefixSuffixPairs(words []string) (ans int64) {
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
			ans += int64(node.cnt)
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
