---
comments: true
difficulty: Hard
rating: 2486
source: Weekly Contest 247 Q4
tags:
    - Tree
    - Depth-First Search
    - Graph
    - Topological Sort
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
    - Tree DP
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [1916. Count Ways to Build Rooms in an Ant Colony](https://leetcode.com/problems/count-ways-to-build-rooms-in-an-ant-colony)

[中文文档](/solution/1900-1999/1916.Count%20Ways%20to%20Build%20Rooms%20in%20an%20Ant%20Colony/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn là một con kiến có nhiệm vụ xây thêm <code>n</code> căn phòng được đánh số từ <code>0</code> đến <code>n-1</code> cho tổ của mình. Bạn được cung cấp kế hoạch mở rộng dưới dạng một mảng số nguyên <strong>đánh chỉ số từ 0</strong> có độ dài <code>n</code>, <code>prevRoom</code>, trong đó <code>prevRoom[i]</code> cho biết bạn phải xây phòng <code>prevRoom[i]</code> trước khi xây phòng <code>i</code>, và hai phòng này phải được kết nối <strong>trực tiếp</strong>. Phòng <code>0</code> đã được xây sẵn, nên <code>prevRoom[0] = -1</code>. Kế hoạch mở rộng được cho sao cho sau khi xây xong tất cả các phòng, mọi phòng đều có thể đi tới từ phòng <code>0</code>.</p>

<p>Bạn chỉ có thể xây <strong>một phòng</strong> tại một thời điểm, và chỉ có thể tự do di chuyển giữa các phòng <strong>đã xây</strong> nếu chúng được <strong>kết nối</strong>.&nbsp;Bạn có thể chọn xây <strong>bất kỳ phòng nào</strong> miễn là <strong>phòng trước đó</strong>&nbsp;đã được xây.</p>

<p>Hãy trả về <em><strong>số lượng thứ tự khác nhau</strong> mà bạn có thể dùng để xây tất cả các phòng</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1916.Count%20Ways%20to%20Build%20Rooms%20in%20an%20Ant%20Colony/images/d1.jpg" style="width: 200px; height: 212px;" />
<pre>
<strong>Đầu vào:</strong> prevRoom = [-1,0,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>&nbsp;Chỉ có một cách để xây các phòng còn lại: 0 &rarr; 1 &rarr; 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1916.Count%20Ways%20to%20Build%20Rooms%20in%20an%20Ant%20Colony/images/d2.jpg" style="width: 200px; height: 239px;" /></strong>

<pre>
<strong>Đầu vào:</strong> prevRoom = [-1,0,0,1,2]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:
</strong>Có 6 cách:
0 &rarr; 1 &rarr; 3 &rarr; 2 &rarr; 4
0 &rarr; 2 &rarr; 4 &rarr; 1 &rarr; 3
0 &rarr; 1 &rarr; 2 &rarr; 3 &rarr; 4
0 &rarr; 1 &rarr; 2 &rarr; 4 &rarr; 3
0 &rarr; 2 &rarr; 1 &rarr; 3 &rarr; 4
0 &rarr; 2 &rarr; 1 &rarr; 4 &rarr; 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == prevRoom.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>prevRoom[0] == -1</code></li>
	<li><code>0 &lt;= prevRoom[i] &lt; n</code> với mọi <code>1 &lt;= i &lt; n</code></li>
	<li>Sau khi xây xong tất cả các phòng, mọi phòng đều có thể đi tới từ phòng <code>0</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các thứ tự xây hợp lệ chính là các thứ tự topo của cây đã cho. Việc thử mọi hoán vị là không thể với $n\le 10^5$.
>
> Thứ tự bên trong các cây con rời nhau là độc lập; việc gộp hai cây con tương ứng với việc chọn các vị trí cho một cây con bằng hệ số nhị thức. Ta lần lượt gộp các cây con từ trái sang phải.
>
> DFS trả về kích thước cây con và nhân với $\binom{s+t}{t}$ khi gộp một cây con có kích thước $t$ vào phần đã gộp có kích thước $s$, tất cả đều được tính modulo $10^9+7$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToBuildRooms(self, prevRoom: List[int]) -> int:
        modulo = 10**9 + 7
        ingoing = defaultdict(set)
        outgoing = defaultdict(set)

        for i in range(1, len(prevRoom)):
            ingoing[i].add(prevRoom[i])
            outgoing[prevRoom[i]].add(i)
        ans = [1]

        def recurse(i):
            if len(outgoing[i]) == 0:
                return 1

            nodes_in_tree = 0
            for v in outgoing[i]:
                cn = recurse(v)
                if nodes_in_tree != 0:
                    ans[0] *= comb(nodes_in_tree + cn, cn)
                    ans[0] %= modulo
                nodes_in_tree += cn
            return nodes_in_tree + 1

        recurse(0)
        return ans[0] % modulo
```

#### Java

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    private List<Integer>[] g;
    private long[] fact;
    private long[] invFact;
    private long ans = 1;

    public int waysToBuildRooms(int[] prevRoom) {
        int n = prevRoom.length;
        g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int i = 1; i < n; ++i) {
            g[prevRoom[i]].add(i);
        }
        fact = new long[n + 1];
        invFact = new long[n + 1];
        fact[0] = 1;
        for (int i = 1; i <= n; ++i) {
            fact[i] = fact[i - 1] * i % MOD;
        }
        invFact[n] = qpow(fact[n], MOD - 2);
        for (int i = n; i > 0; --i) {
            invFact[i - 1] = invFact[i] * i % MOD;
        }
        dfs(0);
        return (int) ans;
    }

    private int dfs(int u) {
        int merged = 0;
        for (int v : g[u]) {
            int cn = dfs(v);
            if (merged != 0) {
                ans = ans * comb(merged + cn, cn) % MOD;
            }
            merged += cn;
        }
        return merged + 1;
    }

    private long comb(int n, int k) {
        return fact[n] * invFact[k] % MOD * invFact[n - k] % MOD;
    }

    private long qpow(long a, long n) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % MOD;
            }
            a = a * a % MOD;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToBuildRooms(vector<int>& prevRoom) {
        int n = prevRoom.size();
        g.assign(n, {});
        for (int i = 1; i < n; ++i) {
            g[prevRoom[i]].push_back(i);
        }
        fact.assign(n + 1, 1);
        invFact.assign(n + 1, 1);
        for (int i = 1; i <= n; ++i) {
            fact[i] = fact[i - 1] * i % MOD;
        }
        invFact[n] = qpow(fact[n], MOD - 2);
        for (int i = n; i > 0; --i) {
            invFact[i - 1] = invFact[i] * i % MOD;
        }
        ans = 1;
        dfs(0);
        return ans;
    }

private:
    static constexpr int MOD = 1e9 + 7;
    vector<vector<int>> g;
    vector<long long> fact, invFact;
    int ans;

    int dfs(int u) {
        int merged = 0;
        for (int v : g[u]) {
            int cn = dfs(v);
            if (merged) {
                ans = 1LL * ans * comb(merged + cn, cn) % MOD;
            }
            merged += cn;
        }
        return merged + 1;
    }

    long long comb(int n, int k) {
        return fact[n] * invFact[k] % MOD * invFact[n - k] % MOD;
    }

    long long qpow(long long a, long long n) {
        long long res = 1;
        for (; n; n >>= 1) {
            if (n & 1) {
                res = res * a % MOD;
            }
            a = a * a % MOD;
        }
        return res;
    }
};
```

#### Go

```go
func waysToBuildRooms(prevRoom []int) int {
	const mod = 1_000_000_007
	n := len(prevRoom)
	g := make([][]int, n)
	for i := 1; i < n; i++ {
		g[prevRoom[i]] = append(g[prevRoom[i]], i)
	}
	fact := make([]int, n+1)
	invFact := make([]int, n+1)
	fact[0] = 1
	for i := 1; i <= n; i++ {
		fact[i] = fact[i-1] * i % mod
	}
	qpow := func(a, n int) int {
		res := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				res = res * a % mod
			}
			a = a * a % mod
		}
		return res
	}
	invFact[n] = qpow(fact[n], mod-2)
	for i := n; i > 0; i-- {
		invFact[i-1] = invFact[i] * i % mod
	}
	comb := func(n, k int) int {
		return fact[n] * invFact[k] % mod * invFact[n-k] % mod
	}
	ans := 1
	var dfs func(int) int
	dfs = func(u int) int {
		merged := 0
		for _, v := range g[u] {
			cn := dfs(v)
			if merged != 0 {
				ans = ans * comb(merged+cn, cn) % mod
			}
			merged += cn
		}
		return merged + 1
	}
	dfs(0)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
