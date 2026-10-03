---
comments: true
difficulty: Hard
rating: 2126
source: Weekly Contest 289 Q4
tags:
    - Tree
    - Depth-First Search
    - Graph
    - Topological Sort
    - Array
    - String
---

<!-- problem:start -->

# [2246. Longest Path With Different Adjacent Characters](https://leetcode.com/problems/longest-path-with-different-adjacent-characters)

[中文文档](/solution/2200-2299/2246.Longest%20Path%20With%20Different%20Adjacent%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <strong>cây</strong> (tức là một đồ thị liên thông, vô hướng và không có chu trình) <strong>có gốc</strong> tại node <code>0</code>, gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Cây được biểu diễn bằng mảng <strong>đánh chỉ số từ 0</strong> <code>parent</code> có kích thước <code>n</code>, trong đó <code>parent[i]</code> là node cha của node <code>i</code>. Vì node <code>0</code> là gốc nên <code>parent[0] == -1</code>.</p>

<p>Bạn cũng được cho một chuỗi <code>s</code> có độ dài <code>n</code>, trong đó <code>s[i]</code> là ký tự được gán cho node <code>i</code>.</p>

<p>Hãy trả về <em>độ dài của <strong>đường đi dài nhất</strong> trong cây sao cho không có cặp node <strong>liền kề</strong> nào trên đường đi có cùng ký tự được gán cho chúng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2246.Longest%20Path%20With%20Different%20Adjacent%20Characters/images/testingdrawio.png" style="width: 201px; height: 241px;" />
<pre>
<strong>Đầu vào:</strong> parent = [-1,0,0,1,1,2], s = &quot;abacbe&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đường đi dài nhất mà mỗi hai node liền kề có ký tự khác nhau trong cây là: 0 -&gt; 1 -&gt; 3. Độ dài của đường đi này là 3, nên kết quả trả về là 3.
Có thể chứng minh rằng không tồn tại đường đi dài hơn thỏa mãn các điều kiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2246.Longest%20Path%20With%20Different%20Adjacent%20Characters/images/graph2drawio.png" style="width: 201px; height: 221px;" />
<pre>
<strong>Đầu vào:</strong> parent = [-1,0,0,0], s = &quot;aabc&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đường đi dài nhất mà mỗi hai node liền kề có ký tự khác nhau trong cây là: 2 -&gt; 0 -&gt; 3. Độ dài của đường đi này là 3, nên kết quả trả về là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == parent.length == s.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= parent[i] &lt;= n - 1</code> với mọi <code>i &gt;= 1</code></li>
	<li><code>parent[0] == -1</code></li>
	<li><code>parent</code> biểu diễn một cây hợp lệ.</li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DP trên cây

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm đường đi dài nhất trong cây sao cho các nhãn liền kề khác nhau. Với $n \le 10^5$, việc thử từng cặp đầu mút là không khả thi. Một đường đi hoặc nằm hoàn toàn trong một cây con, hoặc là sự ghép của hai nhánh đi xuống tại một đỉnh.
>
> DFS trả về độ dài nhánh đi xuống dài nhất mà bước đầu tiên có ký tự khác. Với mỗi node con, ta ghép chain tốt nhất hiện tại với chain mới nếu các nhãn khác nhau, đồng thời giữ lại độ dài nhánh đi xuống lớn nhất. Cộng thêm một ở cuối để tính node hiện tại.

<!-- thinking:end -->

Đầu tiên, ta xây dựng danh sách kề $g$ dựa trên mảng $parent$, trong đó $g[i]$ biểu diễn tất cả node con của node $i$.

Sau đó, ta bắt đầu DFS từ node gốc. Với mỗi node $i$, ta duyệt qua từng node con $j$ trong $g[i]$. Nếu $s[i] \neq s[j]$, ta có thể bắt đầu từ node $i$, đi qua node $j$ và đến một node lá. Độ dài của đường đi này là $x = 1 + \textit{dfs}(j)$. Ta dùng $mx$ để ghi nhận độ dài đường đi lớn nhất bắt đầu từ node $i$. Đồng thời, trong quá trình duyệt, ta cập nhật đáp án $ans = \max(ans, mx + x)$.

Cuối cùng, ta trả về $ans + 1$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPath(self, parent: List[int], s: str) -> int:
        def dfs(i: int) -> int:
            mx = 0
            nonlocal ans
            for j in g[i]:
                x = dfs(j) + 1
                if s[i] != s[j]:
                    ans = max(ans, mx + x)
                    mx = max(mx, x)
            return mx

        g = defaultdict(list)
        for i in range(1, len(parent)):
            g[parent[i]].append(i)
        ans = 0
        dfs(0)
        return ans + 1
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private String s;
    private int ans;

    public int longestPath(int[] parent, String s) {
        int n = parent.length;
        g = new List[n];
        this.s = s;
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 1; i < n; ++i) {
            g[parent[i]].add(i);
        }
        dfs(0);
        return ans + 1;
    }

    private int dfs(int i) {
        int mx = 0;
        for (int j : g[i]) {
            int x = dfs(j) + 1;
            if (s.charAt(i) != s.charAt(j)) {
                ans = Math.max(ans, mx + x);
                mx = Math.max(mx, x);
            }
        }
        return mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestPath(vector<int>& parent, string s) {
        int n = parent.size();
        vector<int> g[n];
        for (int i = 1; i < n; ++i) {
            g[parent[i]].push_back(i);
        }
        int ans = 0;
        function<int(int)> dfs = [&](int i) -> int {
            int mx = 0;
            for (int j : g[i]) {
                int x = dfs(j) + 1;
                if (s[i] != s[j]) {
                    ans = max(ans, mx + x);
                    mx = max(mx, x);
                }
            }
            return mx;
        };
        dfs(0);
        return ans + 1;
    }
};
```

#### Go

```go
func longestPath(parent []int, s string) int {
	n := len(parent)
	g := make([][]int, n)
	for i := 1; i < n; i++ {
		g[parent[i]] = append(g[parent[i]], i)
	}
	ans := 0
	var dfs func(int) int
	dfs = func(i int) int {
		mx := 0
		for _, j := range g[i] {
			x := dfs(j) + 1
			if s[i] != s[j] {
				ans = max(ans, x+mx)
				mx = max(mx, x)
			}
		}
		return mx
	}
	dfs(0)
	return ans + 1
}
```

#### TypeScript

```ts
function longestPath(parent: number[], s: string): number {
    const n = parent.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (let i = 1; i < n; ++i) {
        g[parent[i]].push(i);
    }
    let ans = 0;
    const dfs = (i: number): number => {
        let mx = 0;
        for (const j of g[i]) {
            const x = dfs(j) + 1;
            if (s[i] !== s[j]) {
                ans = Math.max(ans, mx + x);
                mx = Math.max(mx, x);
            }
        }
        return mx;
    };
    dfs(0);
    return ans + 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
