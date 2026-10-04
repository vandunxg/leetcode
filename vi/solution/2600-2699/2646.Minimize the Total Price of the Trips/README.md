---
comments: true
difficulty: Hard
rating: 2238
source: Weekly Contest 341 Q4
tags:
    - Tree
    - Depth-First Search
    - Graph
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2646. Minimize the Total Price of the Trips](https://leetcode.com/problems/minimize-the-total-price-of-the-trips)

[中文文档](/solution/2600-2699/2646.Minimize%20the%20Total%20Price%20of%20the%20Trips/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng, không gốc gồm <code>n</code> nút được đánh chỉ số từ <code>0</code> đến <code>n - 1</code>. Cho số nguyên <code>n</code> và mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối hai nút <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây.</p>

<p>Mỗi nút có một mức giá tương ứng. Cho mảng số nguyên <code>price</code>, trong đó <code>price[i]</code> là mức giá của nút <code>i<sup>th</sup></code>.</p>

<p><strong>Tổng giá</strong> của một đường đi là tổng mức giá của tất cả các nút nằm trên đường đi đó.</p>

<p>Ngoài ra, cho mảng số nguyên 2 chiều <code>trips</code>, trong đó <code>trips[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> cho biết chuyến đi thứ <code>i<sup>th</sup></code> bắt đầu từ nút <code>start<sub>i</sub></code> và di chuyển đến nút <code>end<sub>i</sub></code> theo bất kỳ đường đi nào.</p>

<p>Trước khi thực hiện chuyến đi đầu tiên, bạn có thể chọn một số nút <strong>không kề nhau</strong> và giảm một nửa mức giá của chúng.</p>

<p>Trả về <em>tổng giá nhỏ nhất để thực hiện tất cả các chuyến đi đã cho</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2646.Minimize%20the%20Total%20Price%20of%20the%20Trips/images/diagram2.png" style="width: 541px; height: 181px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,1],[1,2],[1,3]], price = [2,2,10,6], trips = [[0,3],[2,1],[2,3]]
<strong>Đầu ra:</strong> 23
<strong>Giải thích:</strong> Hình trên mô tả cây sau khi gốc hóa tại nút 2. Phần đầu là cây ban đầu và phần thứ hai là cây sau khi chọn các nút 0, 2 và 3, rồi giảm một nửa mức giá của chúng.
Trong chuyến đi thứ <sup>nhất</sup>, ta chọn đường đi [0,1,3]. Tổng giá của đường đi đó là 1 + 2 + 3 = 6.
Trong chuyến đi thứ <sup>hai</sup>, ta chọn đường đi [2,1]. Tổng giá của đường đi đó là 2 + 5 = 7.
Trong chuyến đi thứ <sup>ba</sup>, ta chọn đường đi [2,1,3]. Tổng giá của đường đi đó là 5 + 2 + 3 = 10.
Tổng giá của tất cả các chuyến đi là 6 + 7 + 10 = 23.
Có thể chứng minh rằng 23 là đáp án nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2646.Minimize%20the%20Total%20Price%20of%20the%20Trips/images/diagram3.png" style="width: 456px; height: 111px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, edges = [[0,1]], price = [2,2], trips = [[0,0]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Hình trên mô tả cây sau khi gốc hóa tại nút 0. Phần đầu là cây ban đầu và phần thứ hai là cây sau khi chọn nút 0, rồi giảm một nửa mức giá của nó.
Trong chuyến đi thứ <sup>nhất</sup>, ta chọn đường đi [0]. Tổng giá của đường đi đó là 1.
Tổng giá của tất cả các chuyến đi là 1. Có thể chứng minh rằng 1 là đáp án nhỏ nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>price.length == n</code></li>
	<li><code>price[i]</code> là một số nguyên chẵn.</li>
	<li><code>1 &lt;= price[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= trips.length &lt;= 100</code></li>
	<li><code>0 &lt;= start<sub>i</sub>, end<sub>i</sub>&nbsp;&lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt toàn bộ

<!-- thinking:start -->

> **Tư duy**
>
> Các nút trên chuyến đi có mức giá, và hai nút kề nhau không thể cùng được giảm một nửa. Việc tìm tập các nút được giảm một nửa sau khi liệt kê các đường đi có độ phức tạp hàm mũ; với $n \le 50$, ta có thể đếm số lần đi qua mỗi nút, sau đó dùng tree DP để quyết định các nút được giảm một nửa.
>
> Dùng DFS cho mỗi chuyến đi để cập nhật vào $cnt$. DP trả về (mức giá đầy đủ, mức giá giảm một nửa) tại mỗi nút: khi nút hiện tại giữ nguyên mức giá, các nút con có thể chọn giá trị nhỏ hơn trong hai trạng thái; khi nút hiện tại được giảm một nửa, các nút con buộc phải giữ nguyên mức giá. Ở nút gốc, chọn giá trị nhỏ hơn trong hai trạng thái.

<!-- thinking:end -->

Ta có thể duyệt qua từng phần tử $div$ trong $divisors$ và tính xem có bao nhiêu phần tử trong $nums$ có thể chia hết cho $div$, ký hiệu là $cnt$.

- Nếu $cnt$ lớn hơn điểm chia hết lớn nhất hiện tại $mx$, cập nhật $mx = cnt$ và cập nhật $ans = div$.
- Nếu $cnt$ bằng $mx$ và $div$ nhỏ hơn $ans$, cập nhật $ans = div$.

Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là độ dài của $nums$ và $divisors$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTotalPrice(
        self, n: int, edges: List[List[int]], price: List[int], trips: List[List[int]]
    ) -> int:
        def dfs(i: int, fa: int, k: int) -> bool:
            cnt[i] += 1
            if i == k:
                return True
            ok = any(j != fa and dfs(j, i, k) for j in g[i])
            if not ok:
                cnt[i] -= 1
            return ok

        def dfs2(i: int, fa: int) -> (int, int):
            a = cnt[i] * price[i]
            b = a // 2
            for j in g[i]:
                if j != fa:
                    x, y = dfs2(j, i)
                    a += min(x, y)
                    b += x
            return a, b

        g = [[] for _ in range(n)]
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        cnt = Counter()
        for start, end in trips:
            dfs(start, -1, end)
        return min(dfs2(0, -1))
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] price;
    private int[] cnt;

    public int minimumTotalPrice(int n, int[][] edges, int[] price, int[][] trips) {
        this.price = price;
        cnt = new int[n];
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        for (var t : trips) {
            int start = t[0], end = t[1];
            dfs(start, -1, end);
        }
        int[] ans = dfs2(0, -1);
        return Math.min(ans[0], ans[1]);
    }

    private boolean dfs(int i, int fa, int k) {
        ++cnt[i];
        if (i == k) {
            return true;
        }
        boolean ok = false;
        for (int j : g[i]) {
            if (j != fa) {
                ok = dfs(j, i, k);
                if (ok) {
                    break;
                }
            }
        }
        if (!ok) {
            --cnt[i];
        }
        return ok;
    }

    private int[] dfs2(int i, int fa) {
        int a = cnt[i] * price[i];
        int b = a >> 1;
        for (int j : g[i]) {
            if (j != fa) {
                var t = dfs2(j, i);
                a += Math.min(t[0], t[1]);
                b += t[0];
            }
        }
        return new int[] {a, b};
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTotalPrice(int n, vector<vector<int>>& edges, vector<int>& price, vector<vector<int>>& trips) {
        vector<vector<int>> g(n);
        vector<int> cnt(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        function<bool(int, int, int)> dfs = [&](int i, int fa, int k) -> bool {
            ++cnt[i];
            if (i == k) {
                return true;
            }
            bool ok = false;
            for (int j : g[i]) {
                if (j != fa) {
                    ok = dfs(j, i, k);
                    if (ok) {
                        break;
                    }
                }
            }
            if (!ok) {
                --cnt[i];
            }
            return ok;
        };
        function<pair<int, int>(int, int)> dfs2 = [&](int i, int fa) -> pair<int, int> {
            int a = cnt[i] * price[i];
            int b = a >> 1;
            for (int j : g[i]) {
                if (j != fa) {
                    auto [x, y] = dfs2(j, i);
                    a += min(x, y);
                    b += x;
                }
            }
            return {a, b};
        };
        for (auto& t : trips) {
            int start = t[0], end = t[1];
            dfs(start, -1, end);
        }
        auto [a, b] = dfs2(0, -1);
        return min(a, b);
    }
};
```

#### Go

```go
func minimumTotalPrice(n int, edges [][]int, price []int, trips [][]int) int {
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	cnt := make([]int, n)
	var dfs func(int, int, int) bool
	dfs = func(i, fa, k int) bool {
		cnt[i]++
		if i == k {
			return true
		}
		ok := false
		for _, j := range g[i] {
			if j != fa {
				ok = dfs(j, i, k)
				if ok {
					break
				}
			}
		}
		if !ok {
			cnt[i]--
		}
		return ok
	}
	for _, t := range trips {
		start, end := t[0], t[1]
		dfs(start, -1, end)
	}
	var dfs2 func(int, int) (int, int)
	dfs2 = func(i, fa int) (int, int) {
		a := price[i] * cnt[i]
		b := a >> 1
		for _, j := range g[i] {
			if j != fa {
				x, y := dfs2(j, i)
				a += min(x, y)
				b += x
			}
		}
		return a, b
	}
	a, b := dfs2(0, -1)
	return min(a, b)
}
```

#### TypeScript

```ts
function minimumTotalPrice(
    n: number,
    edges: number[][],
    price: number[],
    trips: number[][],
): number {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    const cnt: number[] = new Array(n).fill(0);
    const dfs = (i: number, fa: number, k: number): boolean => {
        ++cnt[i];
        if (i === k) {
            return true;
        }
        let ok = false;
        for (const j of g[i]) {
            if (j !== fa) {
                ok = dfs(j, i, k);
                if (ok) {
                    break;
                }
            }
        }
        if (!ok) {
            --cnt[i];
        }
        return ok;
    };
    for (const [start, end] of trips) {
        dfs(start, -1, end);
    }
    const dfs2 = (i: number, fa: number): number[] => {
        let a: number = price[i] * cnt[i];
        let b: number = a >> 1;
        for (const j of g[i]) {
            if (j !== fa) {
                const [x, y] = dfs2(j, i);
                a += Math.min(x, y);
                b += x;
            }
        }
        return [a, b];
    };
    const [a, b] = dfs2(0, -1);
    return Math.min(a, b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
