---
comments: true
difficulty: Medium
rating: 1641
source: Weekly Contest 458 Q2
tags:
    - Union Find
    - Graph
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [3613. Minimize Maximum Component Cost](https://leetcode.com/problems/minimize-maximum-component-cost)

[中文文档](/solution/3600-3699/3613.Minimize%20Maximum%20Component%20Cost/README.md)

## Mô tả

<!-- description:start -->

<p data-end="331" data-start="85">Cho một đồ thị vô hướng liên thông gồm <code data-end="137" data-start="134">n</code> node được đánh số từ 0 đến <code data-end="171" data-start="164">n - 1</code> và một mảng số nguyên 2 chiều <code data-end="202" data-start="195">edges</code>, trong đó <code data-end="234" data-start="209">edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> biểu diễn một cạnh vô hướng giữa node <code data-end="279" data-start="275">u<sub>i</sub></code> và node <code data-end="293" data-start="289">v<sub>i</sub></code> có trọng số <code data-end="310" data-start="306">w<sub>i</sub></code>, cùng một số nguyên <code data-end="330" data-start="327">k</code>.</p>

<p data-end="461" data-start="333">Bạn được phép xóa bất kỳ số lượng cạnh nào khỏi đồ thị sao cho đồ thị thu được có <strong>nhiều nhất</strong> <code data-end="439" data-start="436">k</code> thành phần liên thông.</p>

<p data-end="589" data-start="463"><strong>Chi phí</strong> của một thành phần được định nghĩa là trọng số cạnh <strong>lớn nhất</strong> trong thành phần đó. Nếu một thành phần không có cạnh, chi phí của nó là 0.</p>

<p data-end="760" data-start="661">Trả về giá trị <strong>nhỏ nhất</strong> có thể của <strong>chi phí lớn nhất</strong> trong tất cả các thành phần <strong data-end="759" data-start="736">sau khi thực hiện các thao tác xóa</strong> đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[0,1,4],[1,2,3],[1,3,2],[3,4,6]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3613.Minimize%20Maximum%20Component%20Cost/images/minimizemaximumm.jpg" style="width: 535px; height: 225px;" /></p>

<ul>
    <li data-end="1070" data-start="1021">Xóa cạnh giữa node 3 và node 4 (trọng số 6).</li>
    <li data-end="1141" data-start="1073">Các thành phần thu được có chi phí lần lượt là 0 và 4, nên chi phí lớn nhất tổng thể là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1,5],[1,2,5],[2,3,5]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3613.Minimize%20Maximum%20Component%20Cost/images/minmax2.jpg" style="width: 315px; height: 55px;" /></p>

<ul>
    <li data-end="1315" data-start="1251">Không thể xóa cạnh nào, vì chỉ cho phép một thành phần (<code>k = 1</code>) nên đồ thị phải duy trì trạng thái liên thông hoàn toàn.</li>
    <li data-end="1389" data-start="1318">Chi phí của thành phần duy nhất đó bằng trọng số cạnh lớn nhất của nó, tức là 5.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>0 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
    <li><code>edges[i].length == 3</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li><code>1 &lt;= w<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
    <li><code>1 &lt;= k &lt;= n</code></li>
    <li>Đồ thị đầu vào liên thông.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí của một thành phần là trọng số cạnh lớn nhất của nó, và ta muốn tối thiểu hóa giá trị lớn nhất này trong khi vẫn giữ không quá $k$ thành phần. Nếu $k=n$, ta có thể xóa mọi cạnh và đáp án là $0$.
>
> Giá trị minimax có tính đơn điệu: nếu các cạnh có trọng số không vượt quá $w$ tạo ra không quá $k$ thành phần, thì mọi $w$ lớn hơn cũng có tính chất đó. Sắp xếp các cạnh rồi thêm chúng từ nhẹ nhất là quy trình của Kruskal.
>
> Bắt đầu với $n$ thành phần và thực hiện union. Khi số thành phần giảm xuống không quá $k$, trọng số hiện tại chính là chi phí minimax. Vì đồ thị đầu vào liên thông, quá trình duyệt sẽ thành công trừ khi ta đã trả về kết quả ở trường hợp $k=n$.

<!-- thinking:end -->

Nếu $k = n$, điều đó có nghĩa là có thể xóa tất cả các cạnh. Khi đó, mọi thành phần liên thông đều là một node riêng lẻ, và chi phí lớn nhất là 0.

Nếu không, ta có thể sắp xếp tất cả các cạnh theo trọng số tăng dần, sau đó sử dụng cấu trúc dữ liệu union-find để duy trì các thành phần liên thông.

Ban đầu, ta coi tất cả các node chưa được kết nối, mỗi node là một thành phần liên thông độc lập. Bắt đầu từ cạnh có trọng số nhỏ nhất, ta thử thêm cạnh đó vào các thành phần liên thông hiện tại. Nếu số thành phần liên thông sau khi thêm cạnh nhỏ hơn hoặc bằng $k$, điều đó có nghĩa là có thể xóa tất cả các cạnh còn lại, và trọng số của cạnh hiện tại chính là chi phí lớn nhất cần tìm. Ta trả về trọng số này. Nếu không, ta tiếp tục xử lý cạnh tiếp theo.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, n: int, edges: List[List[int]], k: int) -> int:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        if k == n:
            return 0
        edges.sort(key=lambda x: x[2])
        cnt = n
        p = list(range(n))
        for u, v, w in edges:
            pu, pv = find(u), find(v)
            if pu != pv:
                p[pu] = pv
                cnt -= 1
                if cnt <= k:
                    return w
        return 0
```

#### Java

```java
class Solution {
    private int[] p;

    public int minCost(int n, int[][] edges, int k) {
        if (k == n) {
            return 0;
        }
        p = new int[n];
        Arrays.setAll(p, i -> i);
        Arrays.sort(edges, Comparator.comparingInt(a -> a[2]));
        int cnt = n;
        for (var e : edges) {
            int u = e[0], v = e[1], w = e[2];
            int pu = find(u), pv = find(v);
            if (pu != pv) {
                p[pu] = pv;
                if (--cnt <= k) {
                    return w;
                }
            }
        }
        return 0;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(int n, vector<vector<int>>& edges, int k) {
        if (k == n) {
            return 0;
        }
        vector<int> p(n);
        ranges::iota(p, 0);
        ranges::sort(edges, {}, [](const auto& e) { return e[2]; });
        auto find = [&](this auto&& find, int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        int cnt = n;
        for (const auto& e : edges) {
            int u = e[0], v = e[1], w = e[2];
            int pu = find(u), pv = find(v);
            if (pu != pv) {
                p[pu] = pv;
                if (--cnt <= k) {
                    return w;
                }
            }
        }
        return 0;
    }
};
```

#### Go

```go
func minCost(n int, edges [][]int, k int) int {
    p := make([]int, n)
    for i := range p {
        p[i] = i
    }

    var find func(int) int
    find = func(x int) int {
        if p[x] != x {
            p[x] = find(p[x])
        }
        return p[x]
    }

    if k == n {
        return 0
    }

    slices.SortFunc(edges, func(a, b []int) int {
        return a[2] - b[2]
    })

    cnt := n
    for _, e := range edges {
        u, v, w := e[0], e[1], e[2]
        pu, pv := find(u), find(v)
        if pu != pv {
            p[pu] = pv
            if cnt--; cnt <= k {
                return w
            }
        }
    }

    return 0
}
```

#### TypeScript

```ts
function minCost(n: number, edges: number[][], k: number): number {
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };

    if (k === n) {
        return 0;
    }

    edges.sort((a, b) => a[2] - b[2]);
    let cnt = n;
    for (const [u, v, w] of edges) {
        const pu = find(u),
            pv = find(v);
        if (pu !== pv) {
            p[pu] = pv;
            if (--cnt <= k) {
                return w;
            }
        }
    }
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
