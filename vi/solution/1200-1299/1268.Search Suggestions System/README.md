---
comments: true
difficulty: Medium
rating: 1573
source: Weekly Contest 164 Q3
tags:
    - Trie
    - Array
    - String
    - Binary Search
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1268. Search Suggestions System](https://leetcode.com/problems/search-suggestions-system)

[中文文档](/solution/1200-1299/1268.Search%20Suggestions%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>products</code> và chuỗi <code>searchWord</code>.</p>

<p>Thiết kế một hệ thống đề xuất tối đa ba tên sản phẩm trong <code>products</code> sau mỗi ký tự của <code>searchWord</code> được nhập. Sản phẩm được đề xuất phải có tiền tố chung với <code>searchWord</code>. Nếu có hơn ba sản phẩm cùng tiền tố, trả về ba sản phẩm nhỏ nhất theo thứ tự từ điển.</p>

<p>Trả về <em>danh sách các danh sách chứa sản phẩm được đề xuất sau khi nhập từng ký tự của </em><code>searchWord</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> products = [&quot;mobile&quot;,&quot;mouse&quot;,&quot;moneypot&quot;,&quot;monitor&quot;,&quot;mousepad&quot;], searchWord = &quot;mouse&quot;
<strong>Đầu ra:</strong> [[&quot;mobile&quot;,&quot;moneypot&quot;,&quot;monitor&quot;],[&quot;mobile&quot;,&quot;moneypot&quot;,&quot;monitor&quot;],[&quot;mouse&quot;,&quot;mousepad&quot;],[&quot;mouse&quot;,&quot;mousepad&quot;],[&quot;mouse&quot;,&quot;mousepad&quot;]]
<strong>Giải thích:</strong> products sau khi sắp xếp theo thứ tự từ điển = [&quot;mobile&quot;,&quot;moneypot&quot;,&quot;monitor&quot;,&quot;mouse&quot;,&quot;mousepad&quot;].
Sau khi nhập m và mo, tất cả sản phẩm đều khớp, nên hệ thống hiển thị cho người dùng [&quot;mobile&quot;,&quot;moneypot&quot;,&quot;monitor&quot;].
Sau khi nhập mou, mous và mouse, hệ thống đề xuất [&quot;mouse&quot;,&quot;mousepad&quot;].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> products = [&quot;havana&quot;], searchWord = &quot;havana&quot;
<strong>Đầu ra:</strong> [[&quot;havana&quot;],[&quot;havana&quot;],[&quot;havana&quot;],[&quot;havana&quot;],[&quot;havana&quot;],[&quot;havana&quot;]]
<strong>Giải thích:</strong> Từ duy nhất &quot;havana&quot; luôn được đề xuất trong khi nhập searchWord.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= products.length &lt;= 1000</code></li>
	<li><code>1 &lt;= products[i].length &lt;= 3000</code></li>
	<li><code>1 &lt;= sum(products[i].length) &lt;= 2 * 10<sup>4</sup></code></li>
	<li>Tất cả chuỗi trong <code>products</code> đều <strong>khác nhau</strong>.</li>
	<li><code>products[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= searchWord.length &lt;= 1000</code></li>
	<li><code>searchWord</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting + Trie

<!-- thinking:start -->

> **Tư duy**
>
> Sau mỗi ký tự được nhập thêm, ta cần lấy ba sản phẩm nhỏ nhất theo thứ tự từ điển có cùng tiền tố đó. Có tối đa $1000$ sản phẩm: sắp xếp chúng rồi chèn vào trie; mỗi node lưu tối đa ba chỉ số theo thứ tự chèn, tương ứng với ba sản phẩm đầu tiên của tiền tố đó.
>
> Duyệt $searchWord$ để chuyển các danh sách chỉ số thành tên sản phẩm tương ứng. Việc sắp xếp đảm bảo thứ tự chèn cũng là thứ tự từ điển; trie giúp tìm các tiền tố.

<!-- thinking:end -->

Bài toán yêu cầu sau mỗi ký tự của `searchWord`, đề xuất tối đa ba sản phẩm trong mảng `products` có cùng tiền tố với `searchWord`. Nếu có hơn ba sản phẩm cùng tiền tố, trả về ba sản phẩm nhỏ nhất theo thứ tự từ điển.

Ta có thể dùng trie để tìm các sản phẩm có cùng tiền tố. Để lấy ba sản phẩm nhỏ nhất theo thứ tự từ điển, trước tiên sắp xếp mảng `products`, rồi lưu chỉ số của các sản phẩm đã sắp xếp vào trie.

Mỗi node của trie lưu các thông tin sau:

- `children`: Đây là mảng độ dài $26$ dùng để lưu các node con của node hiện tại. `children[i]` là node con có ký tự `i + 'a'`.
- `v`: Đây là mảng lưu chỉ số của các chuỗi trong `products` tương ứng với node con hiện tại; mảng chứa tối đa ba chỉ số.

Khi tìm kiếm, ta bắt đầu từ root của trie, lấy mảng chỉ số tương ứng với từng tiền tố và lưu vào mảng kết quả. Cuối cùng, chỉ cần dùng mỗi chỉ số để tra tên sản phẩm trong `products`.

Độ phức tạp thời gian là $O(L \times \log n + m)$ và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài các chuỗi trong mảng `products`, còn $n$ và $m$ lần lượt là số phần tử trong `products` và độ dài của `searchWord`.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children: List[Union[Trie, None]] = [None] * 26
        self.v: List[int] = []

    def insert(self, w, i):
        node = self
        for c in w:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
            if len(node.v) < 3:
                node.v.append(i)

    def search(self, w):
        node = self
        ans = [[] for _ in range(len(w))]
        for i, c in enumerate(w):
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                break
            node = node.children[idx]
            ans[i] = node.v
        return ans


class Solution:
    def suggestedProducts(
        self, products: List[str], searchWord: str
    ) -> List[List[str]]:
        products.sort()
        trie = Trie()
        for i, w in enumerate(products):
            trie.insert(w, i)
        return [[products[i] for i in v] for v in trie.search(searchWord)]
```

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];
    List<Integer> v = new ArrayList<>();

    public void insert(String w, int i) {
        Trie node = this;
        for (int j = 0; j < w.length(); ++j) {
            int idx = w.charAt(j) - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
            if (node.v.size() < 3) {
                node.v.add(i);
            }
        }
    }

    public List<Integer>[] search(String w) {
        Trie node = this;
        int n = w.length();
        List<Integer>[] ans = new List[n];
        Arrays.setAll(ans, k -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            int idx = w.charAt(i) - 'a';
            if (node.children[idx] == null) {
                break;
            }
            node = node.children[idx];
            ans[i] = node.v;
        }
        return ans;
    }
}

class Solution {
    public List<List<String>> suggestedProducts(String[] products, String searchWord) {
        Arrays.sort(products);
        Trie trie = new Trie();
        for (int i = 0; i < products.length; ++i) {
            trie.insert(products[i], i);
        }
        List<List<String>> ans = new ArrayList<>();
        for (var v : trie.search(searchWord)) {
            List<String> t = new ArrayList<>();
            for (int i : v) {
                t.add(products[i]);
            }
            ans.add(t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
public:
    void insert(string& w, int i) {
        Trie* node = this;
        for (int j = 0; j < w.size(); ++j) {
            int idx = w[j] - 'a';
            if (!node->children[idx]) {
                node->children[idx] = new Trie();
            }
            node = node->children[idx];
            if (node->v.size() < 3) {
                node->v.push_back(i);
            }
        }
    }

    vector<vector<int>> search(string& w) {
        Trie* node = this;
        int n = w.size();
        vector<vector<int>> ans(n);
        for (int i = 0; i < w.size(); ++i) {
            int idx = w[i] - 'a';
            if (!node->children[idx]) {
                break;
            }
            node = node->children[idx];
            ans[i] = move(node->v);
        }
        return ans;
    }

private:
    vector<Trie*> children = vector<Trie*>(26);
    vector<int> v;
};

class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
        sort(products.begin(), products.end());
        Trie* trie = new Trie();
        for (int i = 0; i < products.size(); ++i) {
            trie->insert(products[i], i);
        }
        vector<vector<string>> ans;
        for (auto& v : trie->search(searchWord)) {
            vector<string> t;
            for (int i : v) {
                t.push_back(products[i]);
            }
            ans.push_back(move(t));
        }
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children [26]*Trie
	v        []int
}

func newTrie() *Trie {
	return &Trie{}
}
func (this *Trie) insert(w string, i int) {
	node := this
	for _, c := range w {
		c -= 'a'
		if node.children[c] == nil {
			node.children[c] = newTrie()
		}
		node = node.children[c]
		if len(node.v) < 3 {
			node.v = append(node.v, i)
		}
	}
}

func (this *Trie) search(w string) [][]int {
	node := this
	n := len(w)
	ans := make([][]int, n)
	for i, c := range w {
		c -= 'a'
		if node.children[c] == nil {
			break
		}
		node = node.children[c]
		ans[i] = node.v
	}
	return ans
}

func suggestedProducts(products []string, searchWord string) (ans [][]string) {
	sort.Strings(products)
	trie := newTrie()
	for i, w := range products {
		trie.insert(w, i)
	}
	for _, v := range trie.search(searchWord) {
		t := []string{}
		for _, i := range v {
			t = append(t, products[i])
		}
		ans = append(ans, t)
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
