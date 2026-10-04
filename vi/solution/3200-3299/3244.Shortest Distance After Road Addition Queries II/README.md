---
comments: true
difficulty: Hard
rating: 2270
source: Weekly Contest 409 Q3
tags:
    - Greedy
    - Graph
    - Array
    - Ordered Set
---

<!-- problem:start -->

# [3244. Shortest Distance After Road Addition Queries II](https://leetcode.com/problems/shortest-distance-after-road-addition-queries-ii)

[中文文档](/solution/3200-3299/3244.Shortest%20Distance%20After%20Road%20Addition%20Queries%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> và một mảng số nguyên 2 chiều <code>queries</code>.</p>

<p>Có <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n - 1</code>. Ban đầu, có một con đường <strong>một chiều</strong> từ thành phố <code>i</code> đến thành phố <code>i + 1</code> với mọi <code>0 &lt;= i &lt; n - 1</code>.</p>

<p><code>queries[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu diễn việc thêm một con đường <strong>một chiều</strong> mới từ thành phố <code>u<sub>i</sub></code> đến thành phố <code>v<sub>i</sub></code>. Sau mỗi truy vấn, bạn cần tìm <strong>độ dài</strong> của <strong>đường đi ngắn nhất</strong> từ thành phố <code>0</code> đến thành phố <code>n - 1</code>.</p>

<p>Không tồn tại hai truy vấn sao cho <code>queries[i][0] &lt; queries[j][0] &lt; queries[i][1] &lt; queries[j][1]</code>.</p>

<p>Trả về một mảng <code>answer</code>, trong đó với mỗi <code>i</code> thuộc khoảng <code>[0, queries.length - 1]</code>, <code>answer[i]</code> là <em>độ dài đường đi ngắn nhất</em> từ thành phố <code>0</code> đến thành phố <code>n - 1</code> sau khi xử lý <strong>tổng cộng </strong><code>i + 1</code> truy vấn đầu tiên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, queries = [[2,4],[0,2],[0,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,2,1]</span></p>

<p><strong>Giải thích: </strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3244.Shortest%20Distance%20After%20Road%20Addition%20Queries%20II/images/image8.jpg" style="width: 350px; height: 60px;" /></p>

<p>Sau khi thêm con đường từ 2 đến 4, độ dài đường đi ngắn nhất từ 0 đến 4 là 3.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3244.Shortest%20Distance%20After%20Road%20Addition%20Queries%20II/images/image9.jpg" style="width: 350px; height: 60px;" /></p>

<p>Sau khi thêm con đường từ 0 đến 2, độ dài đường đi ngắn nhất từ 0 đến 4 là 2.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3244.Shortest%20Distance%20After%20Road%20Addition%20Queries%20II/images/image10.jpg" style="width: 350px; height: 96px;" /></p>

<p>Sau khi thêm con đường từ 0 đến 4, độ dài đường đi ngắn nhất từ 0 đến 4 là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, queries = [[0,3],[0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3244.Shortest%20Distance%20After%20Road%20Addition%20Queries%20II/images/image11.jpg" style="width: 300px; height: 70px;" /></p>

<p>Sau khi thêm con đường từ 0 đến 3, độ dài đường đi ngắn nhất từ 0 đến 3 là 1.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3244.Shortest%20Distance%20After%20Road%20Addition%20Queries%20II/images/image12.jpg" style="width: 300px; height: 70px;" /></p>

<p>Sau khi thêm con đường từ 0 đến 2, độ dài đường đi ngắn nhất vẫn là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i].length == 2</code></li>
    <li><code>0 &lt;= queries[i][0] &lt; queries[i][1] &lt; n</code></li>
    <li><code>1 &lt; queries[i][1] - queries[i][0]</code></li>
    <li>Không có con đường nào bị lặp lại trong các truy vấn.</li>
    <li>Không tồn tại hai truy vấn sao cho <code>i != j</code> và <code>queries[i][0] &lt; queries[j][0] &lt; queries[i][1] &lt; queries[j][1]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Ghi lại vị trí nhảy

<!-- thinking:start -->

> **Tư duy**
>
> Bối cảnh giống bài trước, nhưng các cạnh mới không lồng lên nhau và giới hạn khiến việc chạy BFS sau mỗi truy vấn là không thể. Một cạnh $u\to v$ loại bỏ các thành phố vốn phải đi qua ở giữa, nên khoảng cách giảm đi bằng số nút được bao phủ.
>
> $\textit{nxt}[i]$ là thành phố hiện được đi tới từ $i$. Nếu cạnh mới thực sự rút ngắn đường đi, ta đi theo $\textit{nxt}$ trong $[u,v)$, xóa các bước nhảy đó và giảm $\textit{cnt}$. Mỗi chỉ số chỉ bị xóa nhiều nhất một lần, nên tổng thời gian là tuyến tính.

<!-- thinking:end -->

Ta định nghĩa một mảng $\textit{nxt}$ có độ dài $n - 1$, trong đó $\textit{nxt}[i]$ biểu diễn thành phố tiếp theo có thể đi tới từ thành phố $i$. Ban đầu, $\textit{nxt}[i] = i + 1$.

Với mỗi truy vấn $[u, v]$, nếu $u'$ và $v'$ đã được nối từ trước, đồng thời $u' \leq u < v \leq v'$, thì ta có thể bỏ qua truy vấn này. Ngược lại, ta cần đặt số thành phố tiếp theo của các thành phố từ $\textit{nxt}[u]$ đến $\textit{nxt}[v - 1]$ thành $0$, đồng thời đặt $\textit{nxt}[u]$ thành $v$.

Trong quá trình này, ta duy trì một biến $\textit{cnt}$, biểu diễn độ dài đường đi ngắn nhất từ thành phố $0$ đến thành phố $n - 1$. Ban đầu, $\textit{cnt} = n - 1$. Mỗi khi đặt số thành phố tiếp theo của các thành phố trong $[\textit{nxt}[u], \textit{v})$ thành $0$, $\textit{cnt}$ giảm đi $1$.

Độ phức tạp thời gian là $O(n + q)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $q$ lần lượt là số thành phố và số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestDistanceAfterQueries(
        self, n: int, queries: List[List[int]]
    ) -> List[int]:
        nxt = list(range(1, n))
        ans = []
        cnt = n - 1
        for u, v in queries:
            if 0 < nxt[u] < v:
                i = nxt[u]
                while i < v:
                    cnt -= 1
                    nxt[i], i = 0, nxt[i]
                nxt[u] = v
            ans.append(cnt)
        return ans
```

#### Java

```java
class Solution {
    public int[] shortestDistanceAfterQueries(int n, int[][] queries) {
        int[] nxt = new int[n - 1];
        for (int i = 1; i < n; ++i) {
            nxt[i - 1] = i;
        }
        int m = queries.length;
        int cnt = n - 1;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int u = queries[i][0], v = queries[i][1];
            if (nxt[u] > 0 && nxt[u] < v) {
                int j = nxt[u];
                while (j < v) {
                    --cnt;
                    int t = nxt[j];
                    nxt[j] = 0;
                    j = t;
                }
                nxt[u] = v;
            }
            ans[i] = cnt;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> shortestDistanceAfterQueries(int n, vector<vector<int>>& queries) {
        vector<int> nxt(n - 1);
        iota(nxt.begin(), nxt.end(), 1);
        int cnt = n - 1;
        vector<int> ans;
        for (const auto& q : queries) {
            int u = q[0], v = q[1];
            if (nxt[u] && nxt[u] < v) {
                int i = nxt[u];
                while (i < v) {
                    --cnt;
                    int t = nxt[i];
                    nxt[i] = 0;
                    i = t;
                }
                nxt[u] = v;
            }
            ans.push_back(cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func shortestDistanceAfterQueries(n int, queries [][]int) (ans []int) {
    nxt := make([]int, n-1)
    for i := range nxt {
        nxt[i] = i + 1
    }
    cnt := n - 1
    for _, q := range queries {
        u, v := q[0], q[1]
        if nxt[u] > 0 && nxt[u] < v {
            i := nxt[u]
            for i < v {
                cnt--
                nxt[i], i = 0, nxt[i]
            }
            nxt[u] = v
        }
        ans = append(ans, cnt)
    }
    return
}
```

#### TypeScript

```ts
function shortestDistanceAfterQueries(n: number, queries: number[][]): number[] {
    const nxt: number[] = Array.from({ length: n - 1 }, (_, i) => i + 1);
    const ans: number[] = [];
    let cnt = n - 1;
    for (const [u, v] of queries) {
        if (nxt[u] && nxt[u] < v) {
            let i = nxt[u];
            while (i < v) {
                --cnt;
                [nxt[i], i] = [0, nxt[i]];
            }
            nxt[u] = v;
        }
        ans.push(cnt);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
