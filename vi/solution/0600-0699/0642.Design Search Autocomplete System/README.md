---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Design
    - Trie
    - String
    - Data Stream
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [642. Design Search Autocomplete System 🔒](https://leetcode.com/problems/design-search-autocomplete-system)

[中文文档](/solution/0600-0699/0642.Design%20Search%20Autocomplete%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế hệ thống autocomplete tìm kiếm cho một search engine. Người dùng có thể nhập một câu (gồm ít nhất một từ và kết thúc bằng ký tự đặc biệt <code>&#39;#&#39;</code>).</p>

<p>Cho mảng chuỗi <code>sentences</code> và mảng số nguyên <code>times</code>, cả hai đều có độ dài <code>n</code>. Trong đó, <code>sentences[i]</code> là câu đã được nhập trước đây và <code>times[i]</code> là số lần câu đó được nhập. Với mỗi ký tự đầu vào ngoại trừ <code>&#39;#&#39;</code>, hãy trả về tối đa <code>3</code> câu phổ biến nhất trong lịch sử có cùng prefix với phần câu đã nhập.</p>

<p>Các quy tắc cụ thể như sau:</p>

<ul>
	<li>Độ phổ biến của một câu được xác định bằng số lần người dùng đã nhập chính xác câu đó trước đây.</li>
	<li>Ba câu phổ biến nhất được trả về phải được sắp xếp theo độ phổ biến (câu phổ biến nhất đứng đầu). Nếu nhiều câu có cùng độ phổ biến, dùng thứ tự mã ASCII (ký tự có mã nhỏ hơn đứng trước).</li>
	<li>Nếu có ít hơn <code>3</code> câu phổ biến, hãy trả về tất cả các câu đó.</li>
	<li>Khi ký tự đầu vào là ký tự đặc biệt, nghĩa là câu đã kết thúc; trong trường hợp này, hãy trả về danh sách rỗng.</li>
</ul>

<p>Hãy triển khai class <code>AutocompleteSystem</code>:</p>

<ul>
	<li><code>AutocompleteSystem(String[] sentences, int[] times)</code> Khởi tạo object bằng các mảng <code>sentences</code> và <code>times</code>.</li>
	<li><code>List&lt;String&gt; input(char c)</code> Cho biết người dùng đã nhập ký tự <code>c</code>.
	<ul>
		<li>Trả về mảng rỗng <code>[]</code> nếu <code>c == &#39;#&#39;</code> và lưu câu vừa nhập vào system.</li>
		<li>Trả về tối đa <code>3</code> câu phổ biến nhất trong lịch sử có cùng prefix với phần câu đã nhập. Nếu có ít hơn <code>3</code> câu khớp, hãy trả về tất cả.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;AutocompleteSystem&quot;, &quot;input&quot;, &quot;input&quot;, &quot;input&quot;, &quot;input&quot;]
[[[&quot;i love you&quot;, &quot;island&quot;, &quot;iroman&quot;, &quot;i love leetcode&quot;], [5, 3, 2, 2]], [&quot;i&quot;], [&quot; &quot;], [&quot;a&quot;], [&quot;#&quot;]]
<strong>Đầu ra</strong>
[null, [&quot;i love you&quot;, &quot;island&quot;, &quot;i love leetcode&quot;], [&quot;i love you&quot;, &quot;i love leetcode&quot;], [], []]

<strong>Giải thích</strong>
AutocompleteSystem obj = new AutocompleteSystem([&quot;i love you&quot;, &quot;island&quot;, &quot;iroman&quot;, &quot;i love leetcode&quot;], [5, 3, 2, 2]);
obj.input(&quot;i&quot;); // return [&quot;i love you&quot;, &quot;island&quot;, &quot;i love leetcode&quot;]. There are four sentences that have prefix &quot;i&quot;. Among them, &quot;ironman&quot; and &quot;i love leetcode&quot; have same hot degree. Since &#39; &#39; has ASCII code 32 and &#39;r&#39; has ASCII code 114, &quot;i love leetcode&quot; should be in front of &quot;ironman&quot;. Also we only need to output top 3 hot sentences, so &quot;ironman&quot; will be ignored.
obj.input(&quot; &quot;); // return [&quot;i love you&quot;, &quot;i love leetcode&quot;]. There are only two sentences that have prefix &quot;i &quot;.
obj.input(&quot;a&quot;); // return []. There are no sentences that have prefix &quot;i a&quot;.
obj.input(&quot;#&quot;); // return []. The user finished the input, the sentence &quot;i a&quot; should be saved as a historical sentence in system. And the following input will be counted as a new search.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == sentences.length</code></li>
	<li><code>n == times.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= sentences[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= times[i] &lt;= 50</code></li>
	<li><code>c</code> là chữ cái tiếng Anh viết thường, dấu thăng <code>&#39;#&#39;</code> hoặc dấu cách <code>&#39; &#39;</code>.</li>
	<li>Mỗi câu được kiểm thử là một dãy ký tự <code>c</code> kết thúc bằng ký tự <code>&#39;#&#39;</code>.</li>
	<li>Độ dài mỗi câu được kiểm thử nằm trong khoảng <code>[1, 200]</code>.</li>
	<li>Các từ trong mỗi câu đầu vào được phân tách bằng một dấu cách.</li>
	<li>Sẽ có tối đa <code>5000</code> lần gọi <code>input</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ký tự được nhập cần trả về ba câu phổ biến nhất có prefix tương ứng. Quét toàn bộ câu sau mỗi lần gõ sẽ chậm.
>
> Trie lưu các câu và độ phổ biến của chúng. Sau khi đi theo prefix hiện tại, DFS cây con, sắp xếp theo $(-heat, lex)$ rồi giữ lại ba câu đầu. Khi gặp `#`, thêm câu vừa hoàn tất trở lại trie.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 27
        self.v = 0
        self.w = ''

    def insert(self, w, t):
        node = self
        for c in w:
            idx = 26 if c == ' ' else ord(c) - ord('a')
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
        node.v += t
        node.w = w

    def search(self, pref):
        node = self
        for c in pref:
            idx = 26 if c == ' ' else ord(c) - ord('a')
            if node.children[idx] is None:
                return None
            node = node.children[idx]
        return node


class AutocompleteSystem:
    def __init__(self, sentences: List[str], times: List[int]):
        self.trie = Trie()
        for a, b in zip(sentences, times):
            self.trie.insert(a, b)
        self.t = []

    def input(self, c: str) -> List[str]:
        def dfs(node):
            if node is None:
                return
            if node.v:
                res.append((node.v, node.w))
            for nxt in node.children:
                dfs(nxt)

        if c == '#':
            s = ''.join(self.t)
            self.trie.insert(s, 1)
            self.t = []
            return []

        res = []
        self.t.append(c)
        node = self.trie.search(''.join(self.t))
        if node is None:
            return res
        dfs(node)
        res.sort(key=lambda x: (-x[0], x[1]))
        return [v[1] for v in res[:3]]


# Your AutocompleteSystem object will be instantiated and called as such:
# obj = AutocompleteSystem(sentences, times)
# param_1 = obj.input(c)
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[27];
    int v;
    String w = "";

    void insert(String w, int t) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            int idx = c == ' ' ? 26 : c - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        node.v += t;
        node.w = w;
    }

    Trie search(String pref) {
        Trie node = this;
        for (char c : pref.toCharArray()) {
            int idx = c == ' ' ? 26 : c - 'a';
            if (node.children[idx] == null) {
                return null;
            }
            node = node.children[idx];
        }
        return node;
    }
}

class AutocompleteSystem {
    private Trie trie = new Trie();
    private StringBuilder t = new StringBuilder();

    public AutocompleteSystem(String[] sentences, int[] times) {
        int i = 0;
        for (String s : sentences) {
            trie.insert(s, times[i++]);
        }
    }

    public List<String> input(char c) {
        List<String> res = new ArrayList<>();
        if (c == '#') {
            trie.insert(t.toString(), 1);
            t = new StringBuilder();
            return res;
        }
        t.append(c);
        Trie node = trie.search(t.toString());
        if (node == null) {
            return res;
        }
        PriorityQueue<Trie> q
            = new PriorityQueue<>((a, b) -> a.v == b.v ? b.w.compareTo(a.w) : a.v - b.v);
        dfs(node, q);
        while (!q.isEmpty()) {
            res.add(0, q.poll().w);
        }
        return res;
    }

    private void dfs(Trie node, PriorityQueue q) {
        if (node == null) {
            return;
        }
        if (node.v > 0) {
            q.offer(node);
            if (q.size() > 3) {
                q.poll();
            }
        }
        for (Trie nxt : node.children) {
            dfs(nxt, q);
        }
    }
}

/**
 * Your AutocompleteSystem object will be instantiated and called as such:
 * AutocompleteSystem obj = new AutocompleteSystem(sentences, times);
 * List<String> param_1 = obj.input(c);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
