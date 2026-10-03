---
comments: true
difficulty: Medium
tags:
    - Design
    - Trie
    - Hash Table
    - String
---

<!-- problem:start -->

# [1804. Implement Trie II (Prefix Tree) 🔒](https://leetcode.com/problems/implement-trie-ii-prefix-tree)

[中文文档](/solution/1800-1899/1804.Implement%20Trie%20II%20%28Prefix%20Tree%29/README.md)

## Mô tả

<!-- description:start -->

<p><a href="https://en.wikipedia.org/wiki/Trie" target="_blank"><strong>Trie</strong></a> (đọc là "try"), hay <strong>cây tiền tố</strong>, là một cấu trúc dữ liệu dạng cây dùng để lưu trữ và truy xuất hiệu quả các key trong một tập dữ liệu chuỗi. Cấu trúc dữ liệu này có nhiều ứng dụng, chẳng hạn như tự động hoàn thành và kiểm tra chính tả.</p>

<p>Hãy triển khai class Trie:</p>

<ul>
	<li><code>Trie()</code> Khởi tạo đối tượng trie.</li>
	<li><code>void insert(String word)</code> Chèn chuỗi <code>word</code> vào trie.</li>
	<li><code>int countWordsEqualTo(String word)</code> Trả về số lần xuất hiện của chuỗi <code>word</code> trong trie.</li>
	<li><code>int countWordsStartingWith(String prefix)</code> Trả về số chuỗi trong trie có chuỗi <code>prefix</code> làm tiền tố.</li>
	<li><code>void erase(String word)</code> Xóa chuỗi <code>word</code> khỏi trie.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Trie&quot;, &quot;insert&quot;, &quot;insert&quot;, &quot;countWordsEqualTo&quot;, &quot;countWordsStartingWith&quot;, &quot;erase&quot;, &quot;countWordsEqualTo&quot;, &quot;countWordsStartingWith&quot;, &quot;erase&quot;, &quot;countWordsStartingWith&quot;]
[[], [&quot;apple&quot;], [&quot;apple&quot;], [&quot;apple&quot;], [&quot;app&quot;], [&quot;apple&quot;], [&quot;apple&quot;], [&quot;app&quot;], [&quot;apple&quot;], [&quot;app&quot;]]
<strong>Đầu ra</strong>
[null, null, null, 2, 2, null, 1, 1, null, 0]

<strong>Giải thích</strong>
Trie trie = new Trie();
trie.insert(&quot;apple&quot;);               // Inserts &quot;apple&quot;.
trie.insert(&quot;apple&quot;);               // Inserts another &quot;apple&quot;.
trie.countWordsEqualTo(&quot;apple&quot;);    // There are two instances of &quot;apple&quot; so return 2.
trie.countWordsStartingWith(&quot;app&quot;); // &quot;app&quot; is a prefix of &quot;apple&quot; so return 2.
trie.erase(&quot;apple&quot;);                // Erases one &quot;apple&quot;.
trie.countWordsEqualTo(&quot;apple&quot;);    // Now there is only one instance of &quot;apple&quot; so return 1.
trie.countWordsStartingWith(&quot;app&quot;); // return 1
trie.erase(&quot;apple&quot;);                // Erases &quot;apple&quot;. Now the trie is empty.
trie.countWordsStartingWith(&quot;app&quot;); // return 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length, prefix.length &lt;= 2000</code></li>
	<li><code>word</code> và <code>prefix</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tổng số lần gọi <strong>tất cả</strong> các hàm <code>insert</code>, <code>countWordsEqualTo</code>, <code>countWordsStartingWith</code> và <code>erase</code> không vượt quá <code>3 * 10<sup>4</sup></code>.</li>
	<li>Đảm bảo rằng với mọi lần gọi hàm <code>erase</code>, chuỗi <code>word</code> tồn tại trong trie.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Triển khai Trie bằng mảng

<!-- thinking:start -->

> **Tư duy**
>
> Trie thông thường chỉ trả lời được câu hỏi một chuỗi có tồn tại hay không. Ở đây ta còn cần đếm số lần xuất hiện của một từ, đếm số từ có tiền tố cho trước và xóa từ. Việc duyệt mọi chuỗi đã chèn cho mỗi truy vấn sẽ quá chậm với giới hạn số thao tác như đề bài.
>
> Lưu hai bộ đếm ở mỗi node: $v$ cho số từ kết thúc tại đây và $pv$ cho số từ đi qua node. Khi chèn, tăng $pv$ trên đường đi và tăng $v$ ở node cuối; khi truy vấn, đi đến node rồi đọc bộ đếm tương ứng; khi xóa, giảm các bộ đếm trên cùng đường đi. Mỗi thao tác đều có độ phức tạp tuyến tính theo độ dài từ.

<!-- thinking:end -->

Mỗi node trong Trie gồm ba phần:

1. Một mảng con trỏ `children` trỏ đến các node con. Trong bài này, độ dài mảng là 26, tương ứng với 26 chữ cái tiếng Anh viết thường. `children[0]` tương ứng với chữ cái viết thường a, ..., `children[25]` tương ứng với chữ cái viết thường z.
1. Một biến int `v`, biểu diễn số chuỗi kết thúc tại node này.
1. Một biến int `pv`, biểu diễn số chuỗi có node này là node tiền tố.

### 1. Chèn chuỗi

Ta bắt đầu từ gốc của Trie và chèn chuỗi. Với node con tương ứng với ký tự hiện tại, có hai trường hợp:

- Node con tồn tại. Di chuyển đến node con theo con trỏ và tiếp tục xử lý ký tự tiếp theo.
- Node con không tồn tại. Tạo node con mới, lưu nó vào vị trí tương ứng trong mảng `children`, sau đó di chuyển đến node con theo con trỏ và tăng giá trị `pv` của node con thêm 1. Tiếp tục tìm ký tự tiếp theo.

Lặp lại các bước trên cho đến khi xử lý ký tự cuối cùng của chuỗi, sau đó tăng giá trị `v` của node hiện tại thêm 1.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

### 2. Tìm tiền tố

Ta bắt đầu từ gốc của Trie và tìm tiền tố. Với node con tương ứng với ký tự hiện tại, có hai trường hợp:

- Node con tồn tại. Di chuyển đến node con theo con trỏ và tiếp tục tìm ký tự tiếp theo.
- Node con không tồn tại. Điều này có nghĩa Trie không chứa tiền tố này, nên trả về con trỏ null.

Lặp lại các bước trên cho đến khi nhận được con trỏ null hoặc tìm thấy ký tự cuối cùng của tiền tố.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

### 3. Xóa chuỗi

Ta bắt đầu từ node gốc của Trie, lần lượt giảm giá trị `pv` của node con tương ứng đi 1 cho đến khi tìm thấy ký tự cuối cùng của chuỗi. Sau đó giảm giá trị `v` của node hiện tại đi 1.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26
        self.v = self.pv = 0

    def insert(self, word: str) -> None:
        node = self
        for c in word:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
            node.pv += 1
        node.v += 1

    def countWordsEqualTo(self, word: str) -> int:
        node = self.search(word)
        return 0 if node is None else node.v

    def countWordsStartingWith(self, prefix: str) -> int:
        node = self.search(prefix)
        return 0 if node is None else node.pv

    def erase(self, word: str) -> None:
        node = self
        for c in word:
            idx = ord(c) - ord('a')
            node = node.children[idx]
            node.pv -= 1
        node.v -= 1

    def search(self, word):
        node = self
        for c in word:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                return None
            node = node.children[idx]
        return node


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.countWordsEqualTo(word)
# param_3 = obj.countWordsStartingWith(prefix)
# obj.erase(word)
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private int v;
    private int pv;

    public Trie() {
    }

    public void insert(String word) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
            ++node.pv;
        }
        ++node.v;
    }

    public int countWordsEqualTo(String word) {
        Trie node = search(word);
        return node == null ? 0 : node.v;
    }

    public int countWordsStartingWith(String prefix) {
        Trie node = search(prefix);
        return node == null ? 0 : node.pv;
    }

    public void erase(String word) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            c -= 'a';
            node = node.children[c];
            --node.pv;
        }
        --node.v;
    }

    private Trie search(String word) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                return null;
            }
            node = node.children[c];
        }
        return node;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * int param_2 = obj.countWordsEqualTo(word);
 * int param_3 = obj.countWordsStartingWith(prefix);
 * obj.erase(word);
 */
```

#### C++

```cpp
class Trie {
public:
    Trie()
        : children(26)
        , v(0)
        , pv(0) {
    }

    void insert(string word) {
        Trie* node = this;
        for (char c : word) {
            c -= 'a';
            if (!node->children[c]) {
                node->children[c] = new Trie();
            }
            node = node->children[c];
            ++node->pv;
        }
        ++node->v;
    }

    int countWordsEqualTo(string word) {
        Trie* node = search(word);
        return node ? node->v : 0;
    }

    int countWordsStartingWith(string prefix) {
        Trie* node = search(prefix);
        return node ? node->pv : 0;
    }

    void erase(string word) {
        Trie* node = this;
        for (char c : word) {
            c -= 'a';
            node = node->children[c];
            --node->pv;
        }
        --node->v;
    }

private:
    vector<Trie*> children;
    int v, pv;

    Trie* search(string& word) {
        Trie* node = this;
        for (char c : word) {
            c -= 'a';
            if (!node->children[c]) {
                return nullptr;
            }
            node = node->children[c];
        }
        return node;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * int param_2 = obj->countWordsEqualTo(word);
 * int param_3 = obj->countWordsStartingWith(prefix);
 * obj->erase(word);
 */
```

#### Go

```go
type Trie struct {
	children [26]*Trie
	v        int
	pv       int
}

func Constructor() (_ Trie) { return }

func (this *Trie) Insert(word string) {
	node := this
	for _, c := range word {
		c -= 'a'
		if node.children[c] == nil {
			node.children[c] = &Trie{}
		}
		node = node.children[c]
		node.pv++
	}
	node.v++
}

func (this *Trie) CountWordsEqualTo(word string) int {
	node := this.search(word)
	if node == nil {
		return 0
	}
	return node.v
}

func (this *Trie) CountWordsStartingWith(prefix string) int {
	node := this.search(prefix)
	if node == nil {
		return 0
	}
	return node.pv
}

func (this *Trie) Erase(word string) {
	node := this
	for _, c := range word {
		c -= 'a'
		node = node.children[c]
		node.pv--
	}
	node.v--
}

func (this *Trie) search(word string) *Trie {
	node := this
	for _, c := range word {
		c -= 'a'
		if node.children[c] == nil {
			return nil
		}
		node = node.children[c]
	}
	return node
}

/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.CountWordsEqualTo(word);
 * param_3 := obj.CountWordsStartingWith(prefix);
 * obj.Erase(word);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
