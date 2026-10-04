---
comments: true
difficulty: Hard
rating: 2513
source: Biweekly Contest 131 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Ordered Set
---

<!-- problem:start -->

# [3161. Block Placement Queries](https://leetcode.com/problems/block-placement-queries)

[中文文档](/solution/3100-3199/3161.Block%20Placement%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Tồn tại một trục số vô hạn, có gốc tọa độ tại 0 và kéo dài theo chiều <strong>dương</strong> của trục x.</p>

<p>Bạn được cho một mảng 2D <code>queries</code>, gồm hai loại truy vấn:</p>

<ol>
    <li>Với truy vấn loại 1, <code>queries[i] = [1, x]</code>. Xây dựng một chướng ngại vật tại điểm cách gốc tọa độ một khoảng <code>x</code>. Đảm bảo rằng tại thời điểm truy vấn được thực hiện <strong>không</strong> có chướng ngại vật nào ở khoảng cách <code>x</code>.</li>
    <li>Với truy vấn loại 2, <code>queries[i] = [2, x, sz]</code>. Kiểm tra xem có thể đặt một khối có kích thước <code>sz</code> <em>ở bất kỳ vị trí nào</em> trong đoạn <code>[0, x]</code> trên trục số sao cho khối đó <strong>hoàn toàn</strong> nằm trong đoạn <code>[0, x]</code> hay không. <strong>Không thể </strong>đặt khối nếu khối giao với bất kỳ chướng ngại vật nào, nhưng khối có thể tiếp xúc với chướng ngại vật. Lưu ý rằng bạn <strong>không</strong> thực sự đặt khối. Các truy vấn độc lập với nhau.</li>
</ol>

<p>Trả về một mảng Boolean <code>results</code>, trong đó <code>results[i]</code> là <code>true</code> nếu có thể đặt khối được chỉ định trong truy vấn loại 2 thứ <code>i<sup>th</sup></code>, và là <code>false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">queries = [[1,2],[2,3,3],[2,3,1],[2,2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[false,true,true]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3161.Block%20Placement%20Queries/images/example0block.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 309px; height: 129px;" /></strong></p>

<p>Với truy vấn 0, đặt một chướng ngại vật tại <code>x = 2</code>. Có thể đặt một khối có kích thước nhiều nhất là 2 trước <code>x = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">queries = </span>[[1,7],[2,7,6],[1,2],[2,7,5],[2,7,6]]<!-- notionvc: 4a471445-5af1-4d72-b11b-94d351a2c8e9 --></p>

<p><strong>Đầu ra:</strong> [true,true,false]</p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3161.Block%20Placement%20Queries/images/example1block.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 310px; height: 130px;" /></strong></p>

<ul>
    <li>Đặt một chướng ngại vật tại <code>x = 7</code> trong truy vấn 0. Có thể đặt một khối có kích thước nhiều nhất là 7 trước <code>x = 7</code>.</li>
    <li>Đặt một chướng ngại vật tại <code>x = 2</code> trong truy vấn 2. Khi đó, có thể đặt một khối có kích thước nhiều nhất là 5 trước <code>x = 7</code>, và một khối có kích thước nhiều nhất là 2 trước <code>x = 2</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= queries.length &lt;= 15 * 10<sup>4</sup></code></li>
    <li><code>2 &lt;= queries[i].length &lt;= 3</code></li>
    <li><code>1 &lt;= queries[i][0] &lt;= 2</code></li>
    <li><code>1 &lt;= x, sz &lt;= min(5 * 10<sup>4</sup>, 3 * queries.length)</code></li>
    <li>Dữ liệu đầu vào được tạo sao cho với các truy vấn loại 1, không có chướng ngại vật nào ở khoảng cách <code>x</code> tại thời điểm truy vấn được thực hiện.</li>
    <li>Dữ liệu đầu vào được tạo sao cho có ít nhất một truy vấn loại 2.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Chướng ngại vật chỉ được thêm vào, còn truy vấn yêu cầu kiểm tra xem khối có kích thước $sz$ có vừa trong $[0,x]$ hay không. Nếu quét các khoảng trống trực tuyến thì độ phức tạp là $O(q^2)$.
>
> Đảo ngược thời gian sẽ biến thao tác thêm thành thao tác xóa, và các khoảng trống chỉ có thể mở rộng. Fenwick tree có thể lưu giá trị lớn nhất của độ dài khoảng trống, được đánh chỉ mục theo đầu mút bên phải.
>
> Trước tiên thêm tất cả chướng ngại vật cùng các sentinel, rồi ghi nhận các khoảng trống. Sau đó duyệt ngược: truy vấn kiểm tra giá trị lớn nhất trên đoạn tiền tố đến $pre$ và khoảng đuôi $x-pre$; khi xóa một chướng ngại vật, cập nhật khoảng trống của phần tử kế tiếp thành $nxt-pre$. Cuối cùng đảo ngược các đáp án đã thu thập.

<!-- thinking:end -->

Vì chướng ngại vật chỉ được thêm vào, ta có thể xử lý các truy vấn offline theo thứ tự ngược và biến thao tác "thêm một chướng ngại vật" thành "xóa một chướng ngại vật". Sau khi xóa, khoảng trống liền kề chỉ có thể lớn hơn, và Fenwick tree có thể duy trì độ dài lớn nhất của các khoảng trống.

Đưa tất cả chướng ngại vật vào một ordered set, đồng thời thêm hai sentinel $0$ và $m+1$, trong đó $m$ là tọa độ lớn nhất. Với mỗi cặp chướng ngại vật liền kề $x_1, x_2$, cập nhật chỉ số $x_2$ trong Fenwick tree bằng khoảng cách $x_2 - x_1$.

Sau đó duyệt các truy vấn từ cuối về đầu:

- Loại $2$: tìm chướng ngại vật cuối cùng $pre \le x$. Có thể đặt khối nếu khoảng trống lớn nhất trong $[0, pre]$ hoặc khoảng đuôi $(pre, x]$ có độ dài ít nhất là $sz$.
- Loại $1$: xóa chướng ngại vật $x$, rồi cập nhật khoảng trống tại chướng ngại vật kế tiếp $nxt$ thành $nxt - pre$.

Độ phức tạp thời gian là $O(q \times \log m)$, và độ phức tạp không gian là $O(m)$, trong đó $q$ là số truy vấn và $m$ là tọa độ lớn nhất.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] = max(self.c[x], v)
            x += x & -x

    def query(self, x: int) -> int:
        mx = 0
        while x:
            mx = max(mx, self.c[x])
            x -= x & -x
        return mx


class Solution:
    def getResults(self, queries: List[List[int]]) -> List[bool]:
        m = max(q[1] for q in queries)
        sl = SortedList([0, m + 1])
        for q in queries:
            if q[0] == 1:
                sl.add(q[1])
        tree = BinaryIndexedTree(m + 1)
        for x1, x2 in pairwise(sl):
            tree.update(x2, x2 - x1)
        ans = []
        for q in reversed(queries):
            x = q[1]
            if q[0] == 1:
                i = sl.index(x)
                tree.update(sl[i + 1], sl[i + 1] - sl[i - 1])
                sl.remove(x)
            else:
                i = sl.bisect_right(x)
                pre = sl[i - 1]
                ans.append(tree.query(pre) >= q[2] or x - pre >= q[2])
        return ans[::-1]
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] = Math.max(c[x], v);
            x += x & -x;
        }
    }

    public int query(int x) {
        int mx = 0;
        while (x > 0) {
            mx = Math.max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

class Solution {
    public List<Boolean> getResults(int[][] queries) {
        int m = 0;
        for (int[] q : queries) {
            m = Math.max(m, q[1]);
        }
        TreeSet<Integer> ts = new TreeSet<>();
        ts.add(0);
        ts.add(m + 1);
        for (int[] q : queries) {
            if (q[0] == 1) {
                ts.add(q[1]);
            }
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(m + 1);
        int pre = 0;
        for (int x : ts) {
            if (x > 0) {
                tree.update(x, x - pre);
            }
            pre = x;
        }
        List<Boolean> ans = new ArrayList<>();
        for (int i = queries.length - 1; i >= 0; --i) {
            int[] q = queries[i];
            int x = q[1];
            if (q[0] == 1) {
                int nxt = ts.higher(x);
                tree.update(nxt, nxt - ts.lower(x));
                ts.remove(x);
            } else {
                int p = ts.floor(x);
                ans.add(tree.query(p) >= q[2] || x - p >= q[2]);
            }
        }
        Collections.reverse(ans);
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
private:
    int n;
    vector<int> c;

public:
    BinaryIndexedTree(int n) {
        this->n = n;
        c.resize(n + 1);
    }

    void update(int x, int v) {
        while (x <= n) {
            c[x] = max(c[x], v);
            x += x & -x;
        }
    }

    int query(int x) {
        int mx = 0;
        while (x > 0) {
            mx = max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
};

class Solution {
public:
    vector<bool> getResults(vector<vector<int>>& queries) {
        int m = 0;
        for (auto& q : queries) {
            m = max(m, q[1]);
        }
        set<int> ts{0, m + 1};
        for (auto& q : queries) {
            if (q[0] == 1) {
                ts.insert(q[1]);
            }
        }
        BinaryIndexedTree tree(m + 1);
        int pre = 0;
        for (int x : ts) {
            if (x) {
                tree.update(x, x - pre);
            }
            pre = x;
        }
        vector<bool> ans;
        for (int i = queries.size() - 1; i >= 0; --i) {
            int x = queries[i][1];
            if (queries[i][0] == 1) {
                auto it = ts.find(x);
                tree.update(*next(it), *next(it) - *prev(it));
                ts.erase(it);
            } else {
                auto it = prev(ts.upper_bound(x));
                ans.push_back(tree.query(*it) >= queries[i][2] || x - *it >= queries[i][2]);
            }
        }
        ranges::reverse(ans);
        return ans;
    }
};
```

#### Go

```go
func getResults(queries [][]int) []bool {
    m := 0
    for _, q := range queries {
        m = max(m, q[1])
    }
    st := redblacktree.New[int, struct{}]()
    st.Put(0, struct{}{})
    st.Put(m+1, struct{}{})
    for _, q := range queries {
        if q[0] == 1 {
            st.Put(q[1], struct{}{})
        }
    }
    tree := newBinaryIndexedTree(m + 1)
    it := st.Iterator()
    it.Next()
    pre := it.Key()
    for it.Next() {
        x := it.Key()
        tree.update(x, x-pre)
        pre = x
    }
    ans := []bool{}
    for i := len(queries) - 1; i >= 0; i-- {
        q := queries[i]
        x := q[1]
        if q[0] == 1 {
            nxt, _ := st.Ceiling(x + 1)
            p, _ := st.Floor(x - 1)
            st.Remove(x)
            tree.update(nxt.Key, nxt.Key-p.Key)
        } else {
            node, _ := st.Floor(x)
            p := node.Key
            ans = append(ans, tree.query(p) >= q[2] || x-p >= q[2])
        }
    }
    slices.Reverse(ans)
    return ans
}

type binaryIndexedTree struct {
    n int
    c []int
}

func newBinaryIndexedTree(n int) *binaryIndexedTree {
    return &binaryIndexedTree{n: n, c: make([]int, n+1)}
}

func (t *binaryIndexedTree) update(x, v int) {
    for x <= t.n {
        t.c[x] = max(t.c[x], v)
        x += x & -x
    }
}

func (t *binaryIndexedTree) query(x int) int {
    mx := 0
    for x > 0 {
        mx = max(mx, t.c[x])
        x -= x & -x
    }
    return mx
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
