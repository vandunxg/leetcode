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

# [677. Map Sum Pairs](https://leetcode.com/problems/map-sum-pairs)

[中文文档](/solution/0600-0699/0677.Map%20Sum%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một map hỗ trợ các thao tác sau:</p>

<ul>
	<li>Ánh xạ một key dạng chuỗi tới một value cho trước.</li>
	<li>Trả về tổng các value có key bắt đầu bằng một chuỗi cho trước.</li>
</ul>

<p>Hãy triển khai class <code>MapSum</code>:</p>

<ul>
	<li><code>MapSum()</code> Khởi tạo object <code>MapSum</code>.</li>
	<li><code>void insert(String key, int val)</code> Chèn cặp <code>key-val</code> vào map. Nếu <code>key</code> đã tồn tại, cặp <code>key-value</code> cũ sẽ được thay bằng cặp mới.</li>
	<li><code>int sum(string prefix)</code> Trả về tổng value của tất cả các cặp có <code>key</code> bắt đầu bằng <code>prefix</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MapSum&quot;, &quot;insert&quot;, &quot;sum&quot;, &quot;insert&quot;, &quot;sum&quot;]
[[], [&quot;apple&quot;, 3], [&quot;ap&quot;], [&quot;app&quot;, 2], [&quot;ap&quot;]]
<strong>Đầu ra</strong>
[null, null, 3, null, 5]

<strong>Giải thích</strong>
MapSum mapSum = new MapSum();
mapSum.insert(&quot;apple&quot;, 3);  
mapSum.sum(&quot;ap&quot;);           // return 3 (<u>ap</u>ple = 3)
mapSum.insert(&quot;app&quot;, 2);    
mapSum.sum(&quot;ap&quot;);           // return 5 (<u>ap</u>ple + <u>ap</u>p = 3 + 2 = 5)
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= key.length, prefix.length &lt;= 50</code></li>
	<li><code>key</code> và <code>prefix</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= val &lt;= 1000</code></li>
	<li>Sẽ có tối đa <code>50</code> lần gọi <code>insert</code> và <code>sum</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Trie

<!-- thinking:start -->

> **Tư duy**
>
> `sum(prefix)` cần tính tổng các key có prefix đó, còn `insert` có thể ghi đè giá trị. Duyệt mọi key sẽ tốn thời gian tuyến tính theo kích thước map.
>
> Các node trong trie lưu tổng của subtree. Khi insert, cộng $\Delta=val-old$ dọc theo đường đi; `sum` trả về tổng tại node ứng với prefix. Hash map lưu giá trị trước đó.

<!-- thinking:end -->

Ta dùng hash table $d$ để lưu các cặp key-value và trie $t$ để lưu tổng prefix của các cặp đó. Mỗi node trong trie chứa hai thông tin:

- `val`: tổng value của các cặp key-value có prefix tương ứng với node này
- `children`: mảng có độ dài $26$ lưu các node con của node này

Khi chèn một cặp key-value $(key, val)$, trước tiên ta kiểm tra key đã có trong hash table chưa. Nếu đã có, cần trừ value cũ rồi cộng value mới vào `val` của mỗi node trên đường đi trong trie. Nếu chưa có, chỉ cần cộng value mới vào `val` của các node đó.

Khi truy vấn tổng prefix, ta bắt đầu từ root của trie và duyệt chuỗi prefix. Nếu node hiện tại không có node con ứng với ký tự đang xét, nghĩa là prefix không tồn tại trong trie và ta trả về $0$. Nếu có, tiếp tục duyệt ký tự tiếp theo. Sau khi duyệt hết prefix, trả về `val` của node hiện tại.

Về độ phức tạp thời gian, thao tác chèn một cặp key-value tốn $O(n)$, trong đó $n$ là độ dài key. Truy vấn tổng prefix tốn $O(m)$, trong đó $m$ là độ dài prefix.

Độ phức tạp không gian là $O(n \times m \times C)$, trong đó $n$ là số key, $m$ là độ dài tối đa của các key, còn $C$ là kích thước bảng ký tự, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children: List[Trie | None] = [None] * 26
        self.val: int = 0

    def insert(self, w: str, x: int):
        node = self
        for c in w:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                node.children[idx] = Trie()
            node = node.children[idx]
            node.val += x

    def search(self, w: str) -> int:
        node = self
        for c in w:
            idx = ord(c) - ord('a')
            if node.children[idx] is None:
                return 0
            node = node.children[idx]
        return node.val


class MapSum:
    def __init__(self):
        self.d = defaultdict(int)
        self.tree = Trie()

    def insert(self, key: str, val: int) -> None:
        x = val - self.d[key]
        self.d[key] = val
        self.tree.insert(key, x)

    def sum(self, prefix: str) -> int:
        return self.tree.search(prefix)


# Your MapSum object will be instantiated and called as such:
# obj = MapSum()
# obj.insert(key,val)
# param_2 = obj.sum(prefix)
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private int val;

    public void insert(String w, int x) {
        Trie node = this;
        for (int i = 0; i < w.length(); ++i) {
            int idx = w.charAt(i) - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
            node.val += x;
        }
    }

    public int search(String w) {
        Trie node = this;
        for (int i = 0; i < w.length(); ++i) {
            int idx = w.charAt(i) - 'a';
            if (node.children[idx] == null) {
                return 0;
            }
            node = node.children[idx];
        }
        return node.val;
    }
}

class MapSum {
    private Map<String, Integer> d = new HashMap<>();
    private Trie trie = new Trie();

    public MapSum() {
    }

    public void insert(String key, int val) {
        int x = val - d.getOrDefault(key, 0);
        d.put(key, val);
        trie.insert(key, x);
    }

    public int sum(String prefix) {
        return trie.search(prefix);
    }
}

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum obj = new MapSum();
 * obj.insert(key,val);
 * int param_2 = obj.sum(prefix);
 */
```

#### C++

```cpp
class Trie {
public:
    Trie()
        : children(26, nullptr) {
    }

    void insert(string& w, int x) {
        Trie* node = this;
        for (char c : w) {
            c -= 'a';
            if (!node->children[c]) {
                node->children[c] = new Trie();
            }
            node = node->children[c];
            node->val += x;
        }
    }

    int search(string& w) {
        Trie* node = this;
        for (char c : w) {
            c -= 'a';
            if (!node->children[c]) {
                return 0;
            }
            node = node->children[c];
        }
        return node->val;
    }

private:
    vector<Trie*> children;
    int val = 0;
};

class MapSum {
public:
    MapSum() {
    }

    void insert(string key, int val) {
        int x = val - d[key];
        d[key] = val;
        trie->insert(key, x);
    }

    int sum(string prefix) {
        return trie->search(prefix);
    }

private:
    unordered_map<string, int> d;
    Trie* trie = new Trie();
};

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum* obj = new MapSum();
 * obj->insert(key,val);
 * int param_2 = obj->sum(prefix);
 */
```

#### Go

```go
type trie struct {
	children [26]*trie
	val      int
}

func (t *trie) insert(w string, x int) {
	for _, c := range w {
		c -= 'a'
		if t.children[c] == nil {
			t.children[c] = &trie{}
		}
		t = t.children[c]
		t.val += x
	}
}

func (t *trie) search(w string) int {
	for _, c := range w {
		c -= 'a'
		if t.children[c] == nil {
			return 0
		}
		t = t.children[c]
	}
	return t.val
}

type MapSum struct {
	d map[string]int
	t *trie
}

func Constructor() MapSum {
	return MapSum{make(map[string]int), &trie{}}
}

func (this *MapSum) Insert(key string, val int) {
	x := val - this.d[key]
	this.d[key] = val
	this.t.insert(key, x)
}

func (this *MapSum) Sum(prefix string) int {
	return this.t.search(prefix)
}

/**
 * Your MapSum object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(key,val);
 * param_2 := obj.Sum(prefix);
 */
```

#### TypeScript

```ts
class Trie {
    children: Trie[];
    val: number;

    constructor() {
        this.children = new Array(26);
        this.val = 0;
    }

    insert(w: string, x: number) {
        let node: Trie = this;
        for (const c of w) {
            const i = c.charCodeAt(0) - 97;
            if (!node.children[i]) {
                node.children[i] = new Trie();
            }
            node = node.children[i];
            node.val += x;
        }
    }

    search(w: string): number {
        let node: Trie = this;
        for (const c of w) {
            const i = c.charCodeAt(0) - 97;
            if (!node.children[i]) {
                return 0;
            }
            node = node.children[i];
        }
        return node.val;
    }
}

class MapSum {
    d: Map<string, number>;
    t: Trie;
    constructor() {
        this.d = new Map();
        this.t = new Trie();
    }

    insert(key: string, val: number): void {
        const x = val - (this.d.get(key) ?? 0);
        this.d.set(key, val);
        this.t.insert(key, x);
    }

    sum(prefix: string): number {
        return this.t.search(prefix);
    }
}

/**
 * Your MapSum object will be instantiated and called as such:
 * var obj = new MapSum()
 * obj.insert(key,val)
 * var param_2 = obj.sum(prefix)
 */
```

#### Rust

```rust
struct Trie {
    children: Vec<Option<Box<Trie>>>,
    val: i32,
}

impl Trie {
    fn new() -> Self {
        Trie {
            children: (0..26).map(|_| None).collect(),
            val: 0,
        }
    }

    fn insert(&mut self, w: &str, x: i32) {
        let mut node = self;
        for c in w.chars() {
            let idx = (c as usize) - ('a' as usize);
            if node.children[idx].is_none() {
                node.children[idx] = Some(Box::new(Trie::new()));
            }
            node = node.children[idx].as_mut().unwrap();
            node.val += x;
        }
    }

    fn search(&self, w: &str) -> i32 {
        let mut node = self;
        for c in w.chars() {
            let idx = (c as usize) - ('a' as usize);
            if node.children[idx].is_none() {
                return 0;
            }
            node = node.children[idx].as_ref().unwrap();
        }
        node.val
    }
}

struct MapSum {
    d: std::collections::HashMap<String, i32>,
    trie: Trie,
}

impl MapSum {
    fn new() -> Self {
        MapSum {
            d: std::collections::HashMap::new(),
            trie: Trie::new(),
        }
    }

    fn insert(&mut self, key: String, val: i32) {
        let x = val - self.d.get(&key).unwrap_or(&0);
        self.d.insert(key.clone(), val);
        self.trie.insert(&key, x);
    }

    fn sum(&self, prefix: String) -> i32 {
        self.trie.search(&prefix)
    }
}
```

#### JavaScript

```js
class Trie {
    constructor() {
        this.children = new Array(26);
        this.val = 0;
    }

    insert(w, x) {
        let node = this;
        for (const c of w) {
            const i = c.charCodeAt(0) - 97;
            if (!node.children[i]) {
                node.children[i] = new Trie();
            }
            node = node.children[i];
            node.val += x;
        }
    }

    search(w) {
        let node = this;
        for (const c of w) {
            const i = c.charCodeAt(0) - 97;
            if (!node.children[i]) {
                return 0;
            }
            node = node.children[i];
        }
        return node.val;
    }
}

var MapSum = function () {
    this.d = new Map();
    this.t = new Trie();
};

/**
 * @param {string} key
 * @param {number} val
 * @return {void}
 */
MapSum.prototype.insert = function (key, val) {
    const x = val - (this.d.get(key) ?? 0);
    this.d.set(key, val);
    this.t.insert(key, x);
};

/**
 * @param {string} prefix
 * @return {number}
 */
MapSum.prototype.sum = function (prefix) {
    return this.t.search(prefix);
};

/**
 * Your MapSum object will be instantiated and called as such:
 * var obj = new MapSum()
 * obj.insert(key,val)
 * var param_2 = obj.sum(prefix)
 */
```

#### C#

```cs
public class Trie {
    private Trie[] children = new Trie[26];
    private int val;

    public void Insert(string w, int x) {
        Trie node = this;
        for (int i = 0; i < w.Length; ++i) {
            int idx = w[i] - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
            node.val += x;
        }
    }

    public int Search(string w) {
        Trie node = this;
        for (int i = 0; i < w.Length; ++i) {
            int idx = w[i] - 'a';
            if (node.children[idx] == null) {
                return 0;
            }
            node = node.children[idx];
        }
        return node.val;
    }
}

public class MapSum {
    private Dictionary<string, int> d = new Dictionary<string, int>();
    private Trie trie = new Trie();

    public MapSum() {
    }

    public void Insert(string key, int val) {
        int x = val - (d.ContainsKey(key) ? d[key] : 0);
        d[key] = val;
        trie.Insert(key, x);
    }

    public int Sum(string prefix) {
        return trie.Search(prefix);
    }
}

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum obj = new MapSum();
 * obj.Insert(key,val);
 * int param_2 = obj.Sum(prefix);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
