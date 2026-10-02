---
comments: true
difficulty: Medium
rating: 1633
source: Weekly Contest 171 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [1319. Number of Operations to Make Network Connected](https://leetcode.com/problems/number-of-operations-to-make-network-connected)

[中文文档](/solution/1300-1399/1319.Number%20of%20Operations%20to%20Make%20Network%20Connected/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> máy tính được đánh số từ <code>0</code> đến <code>n - 1</code>, kết nối với nhau bằng các cáp Ethernet <code>connections</code> tạo thành một mạng. Trong đó, <code>connections[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị kết nối giữa hai máy tính <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>. Mọi máy tính đều có thể liên lạc với mọi máy tính khác trực tiếp hoặc gián tiếp qua mạng.</p>

<p>Bạn được cho mạng máy tính ban đầu <code>connections</code>. Bạn có thể tháo một số cáp đang nối trực tiếp hai máy tính rồi lắp chúng giữa một cặp máy tính chưa được kết nối để tạo kết nối trực tiếp.</p>

<p>Trả về <em>số lần tối thiểu cần thực hiện thao tác này để kết nối tất cả máy tính</em>. Nếu không thể, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1319.Number%20of%20Operations%20to%20Make%20Network%20Connected/images/sample_1_1677.png" style="width: 500px; height: 148px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, connections = [[0,1],[0,2],[1,2]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Tháo cáp nối máy tính 1 và 2 rồi lắp giữa máy tính 1 và 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1319.Number%20of%20Operations%20to%20Make%20Network%20Connected/images/sample_2_1677.png" style="width: 500px; height: 129px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có đủ cáp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= connections.length &lt;= min(n * (n - 1) / 2, 10<sup>5</sup>)</code></li>
	<li><code>connections[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Không có kết nối nào bị lặp.</li>
	<li>Không có hai máy tính nào được nối với nhau bằng nhiều hơn một cáp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể di chuyển các cáp hiện có để nối $n$ máy thành một mạng. Vì $n,m$ có thể lên đến $10^5$, xây dựng lại đồ thị không phải cách phù hợp. Để nối $k$ thành phần thành một cây, cần ít nhất $k-1$ cạnh dư.
>
> Dùng Union-Find trên các cạnh: nếu một cạnh nối hai máy trong cùng thành phần thì đó là cạnh dư; nếu không, ta gộp hai thành phần và giảm $k$. Nếu số cạnh dư ít hơn $k-1$ thì đáp án là $-1$; ngược lại, đáp án là $k-1$.

<!-- thinking:end -->

Ta có thể dùng cấu trúc dữ liệu Union-Find để theo dõi tính liên thông giữa các máy tính. Duyệt tất cả kết nối; với mỗi kết nối $(a, b)$, nếu $a$ và $b$ đã được kết nối thì đây là kết nối dư, ta tăng số lượng kết nối dư lên một. Ngược lại, ta nối $a$ với $b$ và giảm số thành phần liên thông đi một.

Cuối cùng, nếu số thành phần liên thông trừ một lớn hơn số kết nối dư, ta không thể kết nối tất cả máy tính nên trả về -1. Nếu không, trả về số thành phần liên thông trừ một.

Độ phức tạp thời gian là $O(m \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $m$ lần lượt là số máy tính và số kết nối.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeConnected(self, n: int, connections: List[List[int]]) -> int:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        cnt = 0
        p = list(range(n))
        for a, b in connections:
            pa, pb = find(a), find(b)
            if pa == pb:
                cnt += 1
            else:
                p[pa] = pb
                n -= 1
        return -1 if n - 1 > cnt else n - 1
```

#### Java

```java
class Solution {
    private int[] p;

    public int makeConnected(int n, int[][] connections) {
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        int cnt = 0;
        for (int[] e : connections) {
            int pa = find(e[0]), pb = find(e[1]);
            if (pa == pb) {
                ++cnt;
            } else {
                p[pa] = pb;
                --n;
            }
        }
        return n - 1 > cnt ? -1 : n - 1;
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
    int makeConnected(int n, vector<vector<int>>& connections) {
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        int cnt = 0;
        function<int(int)> find = [&](int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        for (const auto& c : connections) {
            int pa = find(c[0]), pb = find(c[1]);
            if (pa == pb) {
                ++cnt;
            } else {
                p[pa] = pb;
                --n;
            }
        }
        return cnt >= n - 1 ? n - 1 : -1;
    }
};
```

#### Go

```go
func makeConnected(n int, connections [][]int) int {
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	cnt := 0
	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for _, e := range connections {
		pa, pb := find(e[0]), find(e[1])
		if pa == pb {
			cnt++
		} else {
			p[pa] = pb
			n--
		}
	}
	if n-1 > cnt {
		return -1
	}
	return n - 1
}
```

#### TypeScript

```ts
function makeConnected(n: number, connections: number[][]): number {
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    let cnt = 0;
    for (const [a, b] of connections) {
        const [pa, pb] = [find(a), find(b)];
        if (pa === pb) {
            ++cnt;
        } else {
            p[pa] = pb;
            --n;
        }
    }
    return cnt >= n - 1 ? n - 1 : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
