---
comments: true
difficulty: Medium
rating: 2045
source: Biweekly Contest 142 Q2
tags:
    - Tree
    - Depth-First Search
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [3331. Find Subtree Sizes After Changes](https://leetcode.com/problems/find-subtree-sizes-after-changes)

[中文文档](/solution/3300-3399/3331.Find%20Subtree%20Sizes%20After%20Changes/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một cây có gốc là node 0, gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng <code>parent</code> có kích thước <code>n</code>, trong đó <code>parent[i]</code> là node cha của node <code>i</code>. Vì node 0 là gốc nên <code>parent[0] == -1</code>.</p>

<p>Bạn cũng được cho một chuỗi <code>s</code> có độ dài <code>n</code>, trong đó <code>s[i]</code> là ký tự được gán cho node <code>i</code>.</p>

<p>Ta thực hiện các thay đổi sau trên cây <strong>một</strong> lần và <strong>đồng thời</strong> cho tất cả node <code>x</code> từ <code>1</code> đến <code>n - 1</code>:</p>

<ul>
    <li>Tìm node <code>y</code> <strong>gần nhất</strong> với node <code>x</code> sao cho <code>y</code> là tổ tiên của <code>x</code> và <code>s[x] == s[y]</code>.</li>
    <li>Nếu node <code>y</code> không tồn tại thì không làm gì.</li>
    <li>Nếu không, <strong>xóa</strong> cạnh giữa <code>x</code> và node cha hiện tại của nó, rồi đặt node <code>y</code> làm cha mới của <code>x</code> bằng cách thêm một cạnh giữa chúng.</li>
</ul>

<p>Trả về một mảng <code>answer</code> có kích thước <code>n</code>, trong đó <code>answer[i]</code> là <strong>kích thước</strong> của <span data-keyword="subtree">cây con</span> có gốc tại node <code>i</code> trong cây <strong>cuối cùng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">parent = [-1,0,0,1,1,1], s = &quot;abaabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,3,1,1,1,1]</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3331.Find%20Subtree%20Sizes%20After%20Changes/images/graphex1drawio.png" style="width: 230px; height: 277px;" />
<p>Node cha của node 3 sẽ thay đổi từ node 1 thành node 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">parent = [-1,0,4,0,1], s = &quot;abbba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,2,1,1,1]</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3331.Find%20Subtree%20Sizes%20After%20Changes/images/exgraph2drawio.png" style="width: 160px; height: 308px;" />
<p>Các thay đổi sau sẽ diễn ra đồng thời:</p>

<ul>
    <li>Node cha của node 4 sẽ thay đổi từ node 1 thành node 0.</li>
    <li>Node cha của node 2 sẽ thay đổi từ node 4 thành node 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>n == parent.length == s.length</code></li>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= parent[i] &lt;= n - 1</code> với mọi <code>i &gt;= 1</code>.</li>
    <li><code>parent[0] == -1</code></li>
    <li><code>parent</code> biểu diễn một cây hợp lệ.</li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một node có thể được nối lại dưới tổ tiên gần nhất có cùng ký tự. Với $n \le 10^5$, ta không nên xây dựng lại danh sách cạnh rồi tính lại các cây con.
>
> Chỉ cần một lần DFS: các stack $\textit{d}[c]$ lưu các tổ tiên của ký tự $c$. Trước khi quay lui, ta cộng kích thước cây con hiện tại vào tổ tiên gần nhất trước đó có cùng ký tự nếu tồn tại, nếu không thì cộng vào node cha.
>
> Duyệt post-order đảm bảo $\textit{ans}[i]$ đã bao gồm mọi hậu duệ; thao tác lấy phần tử ra khỏi stack khôi phục lại chuỗi tổ tiên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSubtreeSizes(self, parent: List[int], s: str) -> List[int]:
        def dfs(i: int, fa: int):
            ans[i] = 1
            d[s[i]].append(i)
            for j in g[i]:
                dfs(j, i)
            k = fa
            if len(d[s[i]]) > 1:
                k = d[s[i]][-2]
            if k != -1:
                ans[k] += ans[i]
            d[s[i]].pop()

        n = len(s)
        g = [[] for _ in range(n)]
        for i in range(1, n):
            g[parent[i]].append(i)
        d = defaultdict(list)
        ans = [0] * n
        dfs(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private List<Integer>[] d;
    private char[] s;
    private int[] ans;

    public int[] findSubtreeSizes(int[] parent, String s) {
        int n = s.length();
        g = new List[n];
        d = new List[26];
        this.s = s.toCharArray();
        Arrays.setAll(g, k -> new ArrayList<>());
        Arrays.setAll(d, k -> new ArrayList<>());
        for (int i = 1; i < n; ++i) {
            g[parent[i]].add(i);
        }
        ans = new int[n];
        dfs(0, -1);
        return ans;
    }

    private void dfs(int i, int fa) {
        ans[i] = 1;
        int idx = s[i] - 'a';
        d[idx].add(i);
        for (int j : g[i]) {
            dfs(j, i);
        }
        int k = d[idx].size() > 1 ? d[idx].get(d[idx].size() - 2) : fa;
        if (k >= 0) {
            ans[k] += ans[i];
        }
        d[idx].remove(d[idx].size() - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findSubtreeSizes(vector<int>& parent, string s) {
        int n = s.size();
        vector<int> g[n];
        vector<int> d[26];
        for (int i = 1; i < n; ++i) {
            g[parent[i]].push_back(i);
        }
        vector<int> ans(n);
        auto dfs = [&](this auto&& dfs, int i, int fa) -> void {
            ans[i] = 1;
            int idx = s[i] - 'a';
            d[idx].push_back(i);
            for (int j : g[i]) {
                dfs(j, i);
            }
            int k = d[idx].size() > 1 ? d[idx][d[idx].size() - 2] : fa;
            if (k >= 0) {
                ans[k] += ans[i];
            }
            d[idx].pop_back();
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func findSubtreeSizes(parent []int, s string) []int {
    n := len(s)
    g := make([][]int, n)
    for i := 1; i < n; i++ {
        g[parent[i]] = append(g[parent[i]], i)
    }
    d := [26][]int{}
    ans := make([]int, n)
    var dfs func(int, int)
    dfs = func(i, fa int) {
        ans[i] = 1
        idx := int(s[i] - 'a')
        d[idx] = append(d[idx], i)
        for _, j := range g[i] {
            dfs(j, i)
        }
        k := fa
        if len(d[idx]) > 1 {
            k = d[idx][len(d[idx])-2]
        }
        if k != -1 {
            ans[k] += ans[i]
        }
        d[idx] = d[idx][:len(d[idx])-1]
    }
    dfs(0, -1)
    return ans
}
```

#### TypeScript

```ts
function findSubtreeSizes(parent: number[], s: string): number[] {
    const n = parent.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    const d: number[][] = Array.from({ length: 26 }, () => []);
    for (let i = 1; i < n; ++i) {
        g[parent[i]].push(i);
    }
    const ans: number[] = Array(n).fill(1);
    const dfs = (i: number, fa: number): void => {
        const idx = s.charCodeAt(i) - 97;
        d[idx].push(i);
        for (const j of g[i]) {
            dfs(j, i);
        }
        const k = d[idx].length > 1 ? d[idx].at(-2)! : fa;
        if (k >= 0) {
            ans[k] += ans[i];
        }
        d[idx].pop();
    };
    dfs(0, -1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
