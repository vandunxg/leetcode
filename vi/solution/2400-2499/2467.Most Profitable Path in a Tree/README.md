---
comments: true
difficulty: Medium
rating: 2053
source: Biweekly Contest 91 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Array
---

<!-- problem:start -->

# [2467. Most Profitable Path in a Tree](https://leetcode.com/problems/most-profitable-path-in-a-tree)

[中文文档](/solution/2400-2499/2467.Most%20Profitable%20Path%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code>, với nút gốc là nút <code>0</code>. Bạn được cho một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết giữa các nút <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> có một cạnh.</p>

<p>Tại mỗi nút <code>i</code> có một cánh cổng. Bạn cũng được cho một mảng các số nguyên chẵn <code>amount</code>, trong đó <code>amount[i]</code> biểu thị:</p>

<ul>
	<li>chi phí cần trả để mở cổng tại nút <code>i</code>, nếu <code>amount[i]</code> âm, hoặc</li>
	<li>phần thưởng nhận được khi mở cổng tại nút <code>i</code> nếu không.</li>
</ul>

<p>Trò chơi diễn ra như sau:</p>

<ul>
	<li>Ban đầu, Alice ở nút <code>0</code> còn Bob ở nút <code>bob</code>.</li>
	<li>Mỗi giây, Alice và Bob <b>mỗi người</b> di chuyển đến một nút kề. Alice di chuyển về phía một <strong>nút lá</strong>, còn Bob di chuyển về phía nút <code>0</code>.</li>
	<li>Với <strong>mọi</strong> nút trên đường đi của mình, Alice và Bob sẽ trả tiền để mở cổng tại nút đó hoặc nhận phần thưởng. Lưu ý rằng:
	<ul>
		<li>Nếu cổng đã <strong>được mở</strong>, không cần trả chi phí và cũng không nhận được phần thưởng.</li>
		<li>Nếu Alice và Bob đến một nút <strong>đồng thời</strong>, họ chia sẻ chi phí/phần thưởng khi mở cổng tại đó. Nói cách khác, nếu chi phí mở cổng là <code>c</code>, mỗi người sẽ trả <code>c / 2</code>. Tương tự, nếu phần thưởng tại cổng là <code>c</code>, mỗi người nhận <code>c / 2</code>.</li>
	</ul>
	</li>
	<li>Nếu Alice đến một nút lá, cô ấy dừng di chuyển. Tương tự, nếu Bob đến nút <code>0</code>, anh ấy dừng di chuyển. Lưu ý rằng hai sự kiện này <strong>độc lập</strong> với nhau.</li>
</ul>

<p>Hãy trả về <em>thu nhập ròng <strong>lớn nhất</strong> mà Alice có thể nhận được khi di chuyển về phía nút lá tối ưu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2467.Most%20Profitable%20Path%20in%20a%20Tree/images/eg1.png" style="width: 275px; height: 275px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[1,2],[1,3],[3,4]], bob = 3, amount = [-2,4,2,-4,6]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Hình trên minh họa cây đã cho. Trò chơi diễn ra như sau:
- Ban đầu Alice ở nút 0, Bob ở nút 3. Họ mở các cổng tại nút tương ứng.
  Thu nhập ròng của Alice lúc này là -2.
- Alice và Bob cùng di chuyển đến nút 1.
&nbsp; Vì họ đến đây đồng thời, họ cùng mở cổng và chia sẻ phần thưởng.
&nbsp; Thu nhập ròng của Alice trở thành -2 + (4 / 2) = 0.
- Alice tiếp tục di chuyển đến nút 3. Vì Bob đã mở cổng tại đây, thu nhập của Alice không thay đổi.
&nbsp; Bob di chuyển đến nút 0 rồi dừng lại.
- Alice tiếp tục di chuyển đến nút 4 và mở cổng tại đây. Thu nhập ròng của cô ấy trở thành 0 + 6 = 6.
Bây giờ cả Alice và Bob đều không thể di chuyển thêm, nên trò chơi kết thúc.
Alice không thể đạt được thu nhập ròng cao hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2467.Most%20Profitable%20Path%20in%20a%20Tree/images/eg2.png" style="width: 250px; height: 78px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1]], bob = 1, amount = [-7280,2350]
<strong>Đầu ra:</strong> -7280
<strong>Giải thích:</strong>
Alice đi theo đường 0-&gt;1 còn Bob đi theo đường 1-&gt;0.
Vì vậy, Alice chỉ mở cổng tại nút 0. Do đó, thu nhập ròng của cô ấy là -7280.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>1 &lt;= bob &lt; n</code></li>
	<li><code>amount.length == n</code></li>
	<li><code>amount[i]</code> là một số nguyên <strong>chẵn</strong> trong khoảng <code>[-10<sup>4</sup>, 10<sup>4</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lần duyệt DFS

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi của Bob là tuyến duy nhất từ $bob\to 0$; Alice đi từ $0$ đến một nút lá. Với $n\le 10^5$, lần DFS đầu tiên lưu thời điểm Bob đến các nút trên đường đi vào $ts$.
>
> Lần DFS thứ hai bắt đầu từ $0$ và tính điểm: nếu thời điểm đến trùng với ts[i] thì chia đôi, Alice đến sớm hơn thì nhận toàn bộ, còn đến muộn hơn thì nhận 0. Cập nhật đáp án tại các nút lá.

<!-- thinking:end -->

Theo đề bài, đường đi của Bob là cố định, bắt đầu từ nút $bob$ và cuối cùng đến nút $0$. Vì vậy, trước tiên ta có thể chạy một lần DFS để tìm thời điểm Bob đến mỗi nút, rồi lưu các thời điểm đó vào mảng $ts$.

Sau đó, ta chạy một lần DFS khác để tìm điểm số lớn nhất trên mỗi đường đi của Alice. Gọi thời điểm Alice đến nút $i$ là $t$, và điểm tích lũy hiện tại là $v$. Sau khi Alice đi qua nút $i$, điểm tích lũy có ba trường hợp:

1. Thời điểm $t$ Alice đến nút $i$ bằng thời điểm $ts[i]$ Bob đến nút $i$. Khi đó Alice và Bob mở cổng tại nút $i$ cùng lúc, nên điểm Alice nhận được là $v + \frac{amount[i]}{2}$.
2. Thời điểm $t$ Alice đến nút $i$ nhỏ hơn thời điểm $ts[i]$ Bob đến nút $i$. Khi đó Alice mở cổng tại nút $i$, nên điểm Alice nhận được là $v + amount[i]$.
3. Thời điểm $t$ Alice đến nút $i$ lớn hơn thời điểm $ts[i]$ Bob đến nút $i$. Khi đó Alice không mở cổng tại nút $i$, nên điểm Alice nhận được là $v$, không thay đổi.

Khi Alice đến một nút lá, ta cập nhật điểm lớn nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số nút.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostProfitablePath(
        self, edges: List[List[int]], bob: int, amount: List[int]
    ) -> int:
        def dfs1(i, fa, t):
            if i == 0:
                ts[i] = min(ts[i], t)
                return True
            for j in g[i]:
                if j != fa and dfs1(j, i, t + 1):
                    ts[j] = min(ts[j], t + 1)
                    return True
            return False

        def dfs2(i, fa, t, v):
            if t == ts[i]:
                v += amount[i] // 2
            elif t < ts[i]:
                v += amount[i]
            nonlocal ans
            if len(g[i]) == 1 and g[i][0] == fa:
                ans = max(ans, v)
                return
            for j in g[i]:
                if j != fa:
                    dfs2(j, i, t + 1, v)

        n = len(edges) + 1
        g = defaultdict(list)
        ts = [n] * n
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        dfs1(bob, -1, 0)
        ts[bob] = 0
        ans = -inf
        dfs2(0, -1, 0, 0)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] amount;
    private int[] ts;
    private int ans = Integer.MIN_VALUE;

    public int mostProfitablePath(int[][] edges, int bob, int[] amount) {
        int n = edges.length + 1;
        g = new List[n];
        ts = new int[n];
        this.amount = amount;
        Arrays.setAll(g, k -> new ArrayList<>());
        Arrays.fill(ts, n);
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs1(bob, -1, 0);
        ts[bob] = 0;
        dfs2(0, -1, 0, 0);
        return ans;
    }

    private boolean dfs1(int i, int fa, int t) {
        if (i == 0) {
            ts[i] = Math.min(ts[i], t);
            return true;
        }
        for (int j : g[i]) {
            if (j != fa && dfs1(j, i, t + 1)) {
                ts[j] = Math.min(ts[j], t + 1);
                return true;
            }
        }
        return false;
    }

    private void dfs2(int i, int fa, int t, int v) {
        if (t == ts[i]) {
            v += amount[i] >> 1;
        } else if (t < ts[i]) {
            v += amount[i];
        }
        if (g[i].size() == 1 && g[i].get(0) == fa) {
            ans = Math.max(ans, v);
            return;
        }
        for (int j : g[i]) {
            if (j != fa) {
                dfs2(j, i, t + 1, v);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mostProfitablePath(vector<vector<int>>& edges, int bob, vector<int>& amount) {
        int n = edges.size() + 1;
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        vector<int> ts(n, n);
        function<bool(int i, int fa, int t)> dfs1 = [&](int i, int fa, int t) -> bool {
            if (i == 0) {
                ts[i] = t;
                return true;
            }
            for (int j : g[i]) {
                if (j != fa && dfs1(j, i, t + 1)) {
                    ts[j] = min(ts[j], t + 1);
                    return true;
                }
            }
            return false;
        };
        dfs1(bob, -1, 0);
        ts[bob] = 0;
        int ans = INT_MIN;
        function<void(int i, int fa, int t, int v)> dfs2 = [&](int i, int fa, int t, int v) {
            if (t == ts[i])
                v += amount[i] >> 1;
            else if (t < ts[i])
                v += amount[i];
            if (g[i].size() == 1 && g[i][0] == fa) {
                ans = max(ans, v);
                return;
            }
            for (int j : g[i])
                if (j != fa) dfs2(j, i, t + 1, v);
        };
        dfs2(0, -1, 0, 0);
        return ans;
    }
};
```

#### Go

```go
func mostProfitablePath(edges [][]int, bob int, amount []int) int {
	n := len(edges) + 1
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	ts := make([]int, n)
	for i := range ts {
		ts[i] = n
	}
	var dfs1 func(int, int, int) bool
	dfs1 = func(i, fa, t int) bool {
		if i == 0 {
			ts[i] = min(ts[i], t)
			return true
		}
		for _, j := range g[i] {
			if j != fa && dfs1(j, i, t+1) {
				ts[j] = min(ts[j], t+1)
				return true
			}
		}
		return false
	}
	dfs1(bob, -1, 0)
	ts[bob] = 0
	ans := -0x3f3f3f3f
	var dfs2 func(int, int, int, int)
	dfs2 = func(i, fa, t, v int) {
		if t == ts[i] {
			v += amount[i] >> 1
		} else if t < ts[i] {
			v += amount[i]
		}
		if len(g[i]) == 1 && g[i][0] == fa {
			ans = max(ans, v)
			return
		}
		for _, j := range g[i] {
			if j != fa {
				dfs2(j, i, t+1, v)
			}
		}
	}
	dfs2(0, -1, 0, 0)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
