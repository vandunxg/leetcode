---
comments: true
difficulty: Medium
rating: 1699
source: Weekly Contest 457 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Array
    - Hash Table
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3607. Power Grid Maintenance](https://leetcode.com/problems/power-grid-maintenance)

[中文文档](/solution/3600-3699/3607.Power%20Grid%20Maintenance/README.md)

## Mô tả

<!-- description:start -->

<p data-end="401" data-start="120">Bạn được cho một số nguyên <code data-end="194" data-start="191">c</code> biểu thị <code data-end="211" data-start="208">c</code> trạm điện, mỗi trạm có một mã định danh duy nhất <code>id</code> từ 1 đến <code>c</code> (đánh số từ 1).</p>

<p data-end="401" data-start="120">Các trạm này được kết nối với nhau bằng <code data-end="295" data-start="292">n</code> cáp <strong>hai chiều</strong>, được biểu diễn bằng một mảng 2 chiều <code data-end="357" data-start="344">connections</code>, trong đó mỗi phần tử <code data-end="430" data-start="405">connections[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một kết nối giữa trạm <code>u<sub>i</sub></code> và trạm <code>v<sub>i</sub></code>. Các trạm được kết nối trực tiếp hoặc gián tiếp tạo thành một <strong>lưới điện</strong>.</p>

<p data-end="626" data-start="586">Ban đầu, <strong>tất cả</strong> các trạm đều đang hoạt động.</p>

<p data-end="720" data-start="628">Bạn cũng được cho một mảng 2 chiều <code data-end="667" data-start="658">queries</code>, trong đó mỗi truy vấn thuộc một trong <em>hai</em> loại sau:</p>

<ul data-end="995" data-start="722">
    <li data-end="921" data-start="722">
    <p data-end="921" data-start="724"><code data-end="732" data-start="724">[1, x]</code>: Yêu cầu kiểm tra bảo trì đối với trạm <code data-end="782" data-start="779">x</code>. Nếu trạm <code>x</code> đang hoạt động, chính trạm đó sẽ xử lý yêu cầu. Nếu trạm <code>x</code> đang ngoại tuyến, yêu cầu được xử lý bởi trạm đang hoạt động có <code>id</code> nhỏ nhất trong cùng <strong>lưới điện</strong> với <code>x</code>. Nếu trong lưới đó <strong>không</strong> có <strong>trạm đang hoạt động</strong> nào <em>tồn tại</em>, trả về -1.</p>
    </li>
    <li data-end="995" data-start="923">
    <p data-end="995" data-start="925"><code data-end="933" data-start="925">[2, x]</code>: Trạm <code data-end="946" data-start="943">x</code> chuyển sang ngoại tuyến (tức là không còn hoạt động).</p>
    </li>
</ul>

<p data-end="1106" data-start="997">Trả về một mảng số nguyên biểu diễn kết quả của mỗi truy vấn loại <code data-end="1080" data-start="1072">[1, x]</code> theo <strong>thứ tự</strong> xuất hiện.</p>

<p data-end="1106" data-start="997"><strong>Lưu ý:</strong> Cấu trúc của lưới điện được giữ nguyên; một nút ngoại tuyến (không hoạt động) vẫn thuộc lưới điện của nó và việc đưa nút đó về ngoại tuyến không làm thay đổi tính liên thông.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">c = 5, connections = [[1,2],[2,3],[3,4],[4,5]], queries = [[1,3],[2,1],[1,1],[2,2],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3607.Power%20Grid%20Maintenance/images/powergrid.jpg" style="width: 361px; height: 42px;" /></p>

<ul>
    <li data-end="223" data-start="143">Ban đầu, tất cả các trạm <code>{1, 2, 3, 4, 5}</code> đều đang hoạt động và tạo thành một lưới điện duy nhất.</li>
    <li data-end="322" data-start="226">Truy vấn <code>[1,3]</code>: Trạm 3 đang hoạt động, nên trạm 3 tự xử lý yêu cầu kiểm tra bảo trì.</li>
    <li data-end="402" data-start="325">Truy vấn <code>[2,1]</code>: Trạm 1 chuyển sang ngoại tuyến. Các trạm còn đang hoạt động là <code>{2, 3, 4, 5}</code>.</li>
    <li data-end="557" data-start="405">Truy vấn <code>[1,1]</code>: Trạm 1 đang ngoại tuyến, nên yêu cầu được xử lý bởi trạm đang hoạt động có <code>id</code> nhỏ nhất trong <code>{2, 3, 4, 5}</code>, tức là trạm 2.</li>
    <li data-end="641" data-start="560">Truy vấn <code>[2,2]</code>: Trạm 2 chuyển sang ngoại tuyến. Các trạm còn đang hoạt động là <code>{3, 4, 5}</code>.</li>
    <li data-end="800" data-start="644">Truy vấn <code>[1,2]</code>: Trạm 2 đang ngoại tuyến, nên yêu cầu được xử lý bởi trạm đang hoạt động có <code>id</code> nhỏ nhất trong <code>{3, 4, 5}</code>, tức là trạm 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">c = 3, connections = [], queries = [[1,1],[2,1],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li data-end="976" data-start="909">Không có kết nối nào, nên mỗi trạm là một lưới điện riêng biệt.</li>
    <li data-end="1096" data-start="979">Truy vấn <code>[1,1]</code>: Trạm 1 đang hoạt động trong lưới điện riêng, nên trạm 1 tự xử lý yêu cầu kiểm tra bảo trì.</li>
    <li data-end="1135" data-start="1099">Truy vấn <code>[2,1]</code>: Trạm 1 chuyển sang ngoại tuyến.</li>
    <li data-end="1237" data-start="1138">Truy vấn <code>[1,1]</code>: Trạm 1 đang ngoại tuyến và không còn trạm nào khác trong lưới điện của nó, nên kết quả là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="155" data-start="139"><code>1 &lt;= c &lt;= 10<sup>5</sup></code></li>
    <li data-end="213" data-start="158"><code>0 &lt;= n == connections.length &lt;= min(10<sup>5</sup>, c * (c - 1) / 2)</code></li>
    <li data-end="244" data-start="216"><code>connections[i].length == 2</code></li>
    <li data-end="295" data-start="247"><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= c</code></li>
    <li data-end="338" data-start="298"><code>u<sub>i</sub> != v<sub>i</sub></code></li>
    <li data-end="374" data-start="341"><code>1 &lt;= queries.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li data-end="401" data-start="377"><code>queries[i].length == 2</code></li>
    <li data-end="436" data-start="404"><code>queries[i][0]</code> là 1 hoặc 2.</li>
    <li data-end="462" data-start="439"><code>1 &lt;= queries[i][1] &lt;= c</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find + Sorted Set

<!-- thinking:start -->

> **Tư duy**
>
> Các kết nối được cố định trước khi xử lý truy vấn; những thao tác sau đó chỉ đưa các trạm về ngoại tuyến. Với $2\times 10^5$ truy vấn, việc duyệt tuyến tính để tìm id đang hoạt động nhỏ nhất trong một lưới điện là quá chậm.
>
> Union-Find gán mỗi trạm vào một thành phần liên thông. Các id đang hoạt động trong một thành phần cần hỗ trợ thao tác xóa và truy vấn phần tử nhỏ nhất, nên một ordered set là phù hợp.
>
> Đưa $1\ldots c$ vào ordered set tương ứng với root của chúng. Truy vấn $[1,x]$ trả về $x$ nếu nó vẫn còn trong set, nếu không thì trả về phần tử nhỏ nhất của set (hoặc $-1$). Truy vấn $[2,x]$ xóa $x$ khỏi set của root của nó.

<!-- thinking:end -->

Ta có thể dùng Union-Find để duy trì quan hệ kết nối giữa các trạm điện, từ đó xác định mỗi trạm thuộc lưới điện nào. Với mỗi lưới điện, ta dùng một sorted set (chẳng hạn như `SortedList` trong Python, `TreeSet` trong Java hoặc `std::set` trong C++) để lưu tất cả id của các trạm đang hoạt động trong lưới đó, giúp truy vấn và xóa trạm hiệu quả.

Các bước cụ thể như sau:

1. Khởi tạo cấu trúc Union-Find và xử lý tất cả các kết nối, gộp những trạm được kết nối vào cùng một tập hợp.
2. Tạo một sorted set cho mỗi lưới điện, ban đầu thêm tất cả id trạm vào set tương ứng với lưới điện của chúng.
3. Duyệt qua danh sách truy vấn:
    - Với truy vấn $[1, x]$, trước tiên tìm nút gốc của lưới điện mà trạm $x$ thuộc về, sau đó kiểm tra sorted set của lưới điện đó:
        - Nếu trạm $x$ đang hoạt động (tồn tại trong set), trả về $x$.
        - Nếu không, trả về trạm có id nhỏ nhất trong set (nếu set không rỗng), nếu không thì trả về -1.
    - Với truy vấn $[2, x]$, tìm nút gốc của lưới điện mà trạm $x$ thuộc về rồi xóa trạm $x$ khỏi sorted set của lưới điện đó, biểu thị rằng trạm đã ngoại tuyến.
4. Cuối cùng, trả về tất cả kết quả của các truy vấn loại $[1, x]$.

Độ phức tạp thời gian là $O((c + n + q) \log c)$ và độ phức tạp không gian là $O(c)$, trong đó $c$ là số trạm, còn $n$ và $q$ lần lượt là số kết nối và số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a, b):
        pa, pb = self.find(a), self.find(b)
        if pa == pb:
            return False
        if self.size[pa] > self.size[pb]:
            self.p[pb] = pa
            self.size[pa] += self.size[pb]
        else:
            self.p[pa] = pb
            self.size[pb] += self.size[pa]
        return True


class Solution:
    def processQueries(
        self, c: int, connections: List[List[int]], queries: List[List[int]]
    ) -> List[int]:
        uf = UnionFind(c + 1)
        for u, v in connections:
            uf.union(u, v)
        st = [SortedList() for _ in range(c + 1)]
        for i in range(1, c + 1):
            st[uf.find(i)].add(i)
        ans = []
        for a, x in queries:
            root = uf.find(x)
            if a == 1:
                if x in st[root]:
                    ans.append(x)
                elif len(st[root]):
                    ans.append(st[root][0])
                else:
                    ans.append(-1)
            else:
                st[root].discard(x)
        return ans
```

#### Java

```java
class UnionFind {
    private final int[] p;
    private final int[] size;

    public UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            size[i] = 1;
        }
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    public boolean union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }
}

class Solution {
    public int[] processQueries(int c, int[][] connections, int[][] queries) {
        UnionFind uf = new UnionFind(c + 1);
        for (int[] e : connections) {
            uf.union(e[0], e[1]);
        }

        TreeSet<Integer>[] st = new TreeSet[c + 1];
        Arrays.setAll(st, k -> new TreeSet<>());
        for (int i = 1; i <= c; i++) {
            int root = uf.find(i);
            st[root].add(i);
        }

        List<Integer> ans = new ArrayList<>();
        for (int[] q : queries) {
            int a = q[0], x = q[1];
            int root = uf.find(x);

            if (a == 1) {
                if (st[root].contains(x)) {
                    ans.add(x);
                } else if (!st[root].isEmpty()) {
                    ans.add(st[root].first());
                } else {
                    ans.add(-1);
                }
            } else {
                st[root].remove(x);
            }
        }

        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    UnionFind(int n) {
        p = vector<int>(n);
        size = vector<int>(n, 1);
        iota(p.begin(), p.end(), 0);
    }

    bool unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

private:
    vector<int> p, size;
};

class Solution {
public:
    vector<int> processQueries(int c, vector<vector<int>>& connections, vector<vector<int>>& queries) {
        UnionFind uf(c + 1);
        for (auto& e : connections) {
            uf.unite(e[0], e[1]);
        }

        vector<set<int>> st(c + 1);
        for (int i = 1; i <= c; i++) {
            st[uf.find(i)].insert(i);
        }

        vector<int> ans;
        for (auto& q : queries) {
            int a = q[0], x = q[1];
            int root = uf.find(x);
            if (a == 1) {
                if (st[root].count(x)) {
                    ans.push_back(x);
                } else if (!st[root].empty()) {
                    ans.push_back(*st[root].begin());
                } else {
                    ans.push_back(-1);
                }
            } else {
                st[root].erase(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
