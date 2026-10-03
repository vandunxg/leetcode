---
comments: true
difficulty: Hard
rating: 2231
source: Biweekly Contest 46 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [1766. Tree of Coprimes](https://leetcode.com/problems/tree-of-coprimes)

[中文文档](/solution/1700-1799/1766.Tree%20of%20Coprimes/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây (tức đồ thị vô hướng liên thông không có chu trình) gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code> và đúng <code>n - 1</code> cạnh. Mỗi node có một giá trị, và <strong>gốc</strong> của cây là node <code>0</code>.</p>

<p>Để biểu diễn cây này, bạn được cho mảng số nguyên <code>nums</code> và mảng 2D <code>edges</code>. Mỗi <code>nums[i]</code> biểu thị giá trị của node thứ <code>i<sup>th</sup></code>, còn mỗi <code>edges[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> biểu thị một cạnh giữa hai node <code>u<sub>j</sub></code> và <code>v<sub>j</sub></code> trong cây.</p>

<p>Hai giá trị <code>x</code> và <code>y</code> <strong>nguyên tố cùng nhau</strong> nếu <code>gcd(x, y) == 1</code>, trong đó <code>gcd(x, y)</code> là <strong>ước chung lớn nhất</strong> của <code>x</code> và <code>y</code>.</p>

<p>Tổ tiên của node <code>i</code> là một node khác nằm trên đường đi ngắn nhất từ node <code>i</code> đến <strong>gốc</strong>. Một node <strong>không</strong> được xem là tổ tiên của chính nó.</p>

<p>Trả về <em>mảng </em><code>ans</code><em> kích thước </em><code>n</code>, <em>trong đó </em><code>ans[i]</code><em> là tổ tiên gần nhất của node </em><code>i</code><em> sao cho </em><code>nums[i]</code> <em>và </em><code>nums[ans[i]]</code> là <strong>nguyên tố cùng nhau</strong>, hoặc <code>-1</code><em> nếu không có tổ tiên như vậy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1766.Tree%20of%20Coprimes/images/untitled-diagram.png" style="width: 191px; height: 281px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,3,2], edges = [[0,1],[1,2],[1,3]]
<strong>Đầu ra:</strong> [-1,0,0,1]
<strong>Giải thích:</strong> Trong hình trên, giá trị của mỗi node được đặt trong ngoặc đơn.
- Node 0 không có tổ tiên nguyên tố cùng nhau.
- Node 1 chỉ có một tổ tiên là node 0. Giá trị của chúng nguyên tố cùng nhau (gcd(2,3) == 1).
- Node 2 có hai tổ tiên là node 1 và node 0. Giá trị của node 1 không nguyên tố cùng nhau (gcd(3,3) == 3), nhưng
  giá trị của node 0 thì có (gcd(2,3) == 1), nên node 0 là tổ tiên hợp lệ gần nhất.
- Node 3 có hai tổ tiên là node 1 và node 0. Node 3 nguyên tố cùng nhau với node 1 (gcd(3,2) == 1), nên node 1 là
  tổ tiên hợp lệ gần nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1766.Tree%20of%20Coprimes/images/untitled-diagram1.png" style="width: 441px; height: 291px;" /></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,6,10,2,3,6,15], edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]]
<strong>Đầu ra:</strong> [-1,0,-1,0,0,0,-1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == n</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[j].length == 2</code></li>
	<li><code>0 &lt;= u<sub>j</sub>, v<sub>j</sub> &lt; n</code></li>
	<li><code>u<sub>j</sub> != v<sub>j</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Liệt kê + Stack + Quay lui

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi node, ta cần tổ tiên gần nhất có giá trị nguyên tố cùng nhau với nó. $n\le 10^5$ nhưng các giá trị nằm trong $[1,50]$, nên chỉ cần một stack cho mỗi giá trị.
>
> Tiền xử lý các cặp nguyên tố cùng nhau trong $1..50$. Khi DFS, xét đỉnh stack của mọi giá trị nguyên tố cùng nhau với $nums[i]$ và chọn node sâu nhất.
>
> Đẩy $(i,\textit{depth})$ vào stack của $nums[i]$ trước khi đệ quy và lấy ra sau đó, để đỉnh stack luôn là tổ tiên gần nhất.

<!-- thinking:end -->

Vì miền giá trị của $nums[i]$ trong bài toán là $[1, 50]$, ta có thể tiền xử lý các số nguyên tố cùng nhau với từng số và lưu chúng vào mảng $f$, trong đó $f[i]$ biểu thị tất cả các số nguyên tố cùng nhau với $i$.

Tiếp theo, ta dùng phương pháp quay lui để duyệt toàn bộ cây từ node gốc. Với mỗi node $i$, ta lấy tất cả các số nguyên tố cùng nhau với $nums[i]$ thông qua mảng $f$. Sau đó, ta liệt kê tất cả các số nguyên tố cùng nhau với $nums[i]$ và tìm node tổ tiên $t$ đã xuất hiện có độ sâu lớn nhất; đó là tổ tiên nguyên tố cùng nhau gần nhất của $i$. Ta có thể dùng mảng stack $stks$ độ dài $51$ để lưu mỗi giá trị $v$ đã xuất hiện cùng độ sâu của nó. Phần tử trên cùng của mỗi stack $stks[v]$ là node tổ tiên gần nhất có độ sâu lớn nhất.

Độ phức tạp thời gian là $O(n \times M)$, còn độ phức tạp không gian là $O(M^2 + n)$. Trong đó $n$ là số node và $M$ là giá trị lớn nhất của $nums[i]$; trong bài này $M = 50$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getCoprimes(self, nums: List[int], edges: List[List[int]]) -> List[int]:
        def dfs(i, fa, depth):
            t = k = -1
            for v in f[nums[i]]:
                stk = stks[v]
                if stk and stk[-1][1] > k:
                    t, k = stk[-1]
            ans[i] = t
            for j in g[i]:
                if j != fa:
                    stks[nums[i]].append((i, depth))
                    dfs(j, i, depth + 1)
                    stks[nums[i]].pop()

        g = defaultdict(list)
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        f = defaultdict(list)
        for i in range(1, 51):
            for j in range(1, 51):
                if gcd(i, j) == 1:
                    f[i].append(j)
        stks = defaultdict(list)
        ans = [-1] * len(nums)
        dfs(0, -1, 0)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private List<Integer>[] f;
    private Deque<int[]>[] stks;
    private int[] nums;
    private int[] ans;

    public int[] getCoprimes(int[] nums, int[][] edges) {
        int n = nums.length;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        f = new List[51];
        stks = new Deque[51];
        Arrays.setAll(f, k -> new ArrayList<>());
        Arrays.setAll(stks, k -> new ArrayDeque<>());
        for (int i = 1; i < 51; ++i) {
            for (int j = 1; j < 51; ++j) {
                if (gcd(i, j) == 1) {
                    f[i].add(j);
                }
            }
        }
        this.nums = nums;
        ans = new int[n];
        dfs(0, -1, 0);
        return ans;
    }

    private void dfs(int i, int fa, int depth) {
        int t = -1, k = -1;
        for (int v : f[nums[i]]) {
            var stk = stks[v];
            if (!stk.isEmpty() && stk.peek()[1] > k) {
                t = stk.peek()[0];
                k = stk.peek()[1];
            }
        }
        ans[i] = t;
        for (int j : g[i]) {
            if (j != fa) {
                stks[nums[i]].push(new int[] {i, depth});
                dfs(j, i, depth + 1);
                stks[nums[i]].pop();
            }
        }
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getCoprimes(vector<int>& nums, vector<vector<int>>& edges) {
        int n = nums.size();
        vector<vector<int>> g(n);
        vector<vector<int>> f(51);
        vector<stack<pair<int, int>>> stks(51);
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].emplace_back(v);
            g[v].emplace_back(u);
        }
        for (int i = 1; i < 51; ++i) {
            for (int j = 1; j < 51; ++j) {
                if (__gcd(i, j) == 1) {
                    f[i].emplace_back(j);
                }
            }
        }
        vector<int> ans(n);
        function<void(int, int, int)> dfs = [&](int i, int fa, int depth) {
            int t = -1, k = -1;
            for (int v : f[nums[i]]) {
                auto& stk = stks[v];
                if (!stk.empty() && stk.top().second > k) {
                    t = stk.top().first;
                    k = stk.top().second;
                }
            }
            ans[i] = t;
            for (int j : g[i]) {
                if (j != fa) {
                    stks[nums[i]].push({i, depth});
                    dfs(j, i, depth + 1);
                    stks[nums[i]].pop();
                }
            }
        };
        dfs(0, -1, 0);
        return ans;
    }
};
```

#### Go

```go
func getCoprimes(nums []int, edges [][]int) []int {
	n := len(nums)
	g := make([][]int, n)
	f := [51][]int{}
	type pair struct{ first, second int }
	stks := [51][]pair{}
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	for i := 1; i < 51; i++ {
		for j := 1; j < 51; j++ {
			if gcd(i, j) == 1 {
				f[i] = append(f[i], j)
			}
		}
	}
	ans := make([]int, n)
	var dfs func(i, fa, depth int)
	dfs = func(i, fa, depth int) {
		t, k := -1, -1
		for _, v := range f[nums[i]] {
			stk := stks[v]
			if len(stk) > 0 && stk[len(stk)-1].second > k {
				t, k = stk[len(stk)-1].first, stk[len(stk)-1].second
			}
		}
		ans[i] = t
		for _, j := range g[i] {
			if j != fa {
				stks[nums[i]] = append(stks[nums[i]], pair{i, depth})
				dfs(j, i, depth+1)
				stks[nums[i]] = stks[nums[i]][:len(stks[nums[i]])-1]
			}
		}
	}
	dfs(0, -1, 0)
	return ans
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
