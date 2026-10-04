---
comments: true
difficulty: Medium
rating: 1917
source: Weekly Contest 465 Q2
tags:
    - Math
    - Backtracking
    - Number Theory
---

<!-- problem:start -->

# [3669. Balanced K-Factor Decomposition](https://leetcode.com/problems/balanced-k-factor-decomposition)

[中文文档](/solution/3600-3699/3669.Balanced%20K-Factor%20Decomposition/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>k</code>.</p>

<p>Chia số <code>n</code> thành <strong>đúng</strong> <code>k</code> số nguyên dương sao cho <strong>tích</strong> của các số này <strong>bằng</strong> <code>n</code>.</p>

<p>Trả về <strong>một</strong> cách chia bất kỳ sao cho độ chênh lệch <strong>lớn nhất</strong> giữa hai số bất kỳ là <strong>nhỏ nhất</strong>. Có thể trả về kết quả theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 100, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[10,10]</span></p>

<p><strong>Giải thích:</strong></p>

<p data-end="157" data-start="0">Cách chia <code>[10, 10]</code> cho <code>10 * 10 = 100</code> và độ chênh lệch lớn nhất - nhỏ nhất là 0, đây là giá trị nhỏ nhất.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 44, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2,11]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li data-end="46" data-start="2">Cách chia <code>[1, 1, 44]</code> cho độ chênh lệch là 43</li>
	<li data-end="93" data-start="49">Cách chia <code>[1, 2, 22]</code> cho độ chênh lệch là 21</li>
	<li data-end="140" data-start="96">Cách chia <code>[1, 4, 11]</code> cho độ chênh lệch là 10</li>
	<li data-end="186" data-start="143">Cách chia <code>[2, 2, 11]</code> cho độ chênh lệch là 9</li>
</ul>

<p data-end="264" data-is-last-node="" data-is-only-node="" data-start="188">Do đó, <code>[2, 2, 11]</code> là cách chia tối ưu với độ chênh lệch nhỏ nhất là 9.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li data-end="54" data-start="37"><code data-end="52" data-start="37">4 &lt;= n &lt;= 10<sup><span style="font-size: 10.8333px;">5</span></sup></code></li>
	<li data-end="71" data-start="57"><code data-end="69" data-start="57">2 &lt;= k &lt;= 5</code></li>
	<li data-end="145" data-is-last-node="" data-start="74"><code data-end="77" data-start="74">k</code> nhỏ hơn nghiêm ngặt số lượng ước dương của <code data-end="144" data-is-only-node="" data-start="141">n</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phân tích $n$ thành $k$ số nguyên dương sao cho khoảng cách giữa số lớn nhất và số nhỏ nhất là nhỏ nhất. $k\le 5$ và $n\le 10^5$ cho phép ta dùng bảng ước và tìm kiếm.
>
> $\textit{dfs}(i,x,\textit{mi},\textit{mx})$ vẫn cần chọn $i$ thừa số và tích còn lại là $x$. Thử từng ước $y$ của $x$ rồi đệ quy với $x/y$.
>
> Khi $i=0$, $x$ cuối cùng sẽ cập nhật khoảng cách. Lưu lại đường đi có khoảng cách nhỏ nhất. Sieve khiến mỗi giá trị còn lại phân nhánh theo $O(\sigma(x))$ ước.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
mx = 10**5 + 1
g = [[] for _ in range(mx)]
for i in range(1, mx):
    for j in range(i, mx, i):
        g[j].append(i)


class Solution:
    def minDifference(self, n: int, k: int) -> List[int]:
        def dfs(i: int, x: int, mi: int, mx: int):
            if i == 0:
                nonlocal cur, ans
                d = max(mx, x) - min(mi, x)
                if d < cur:
                    cur = d
                    path[i] = x
                    ans = path[:]
                return
            for y in g[x]:
                path[i] = y
                dfs(i - 1, x // y, min(mi, y), max(mx, y))

        ans = None
        path = [0] * k
        cur = inf
        dfs(k - 1, n, inf, 0)
        return ans
```

#### Java

```java
class Solution {
    static final int MX = 100_001;
    static List<Integer>[] g = new ArrayList[MX];

    static {
        for (int i = 0; i < MX; i++) {
            g[i] = new ArrayList<>();
        }
        for (int i = 1; i < MX; i++) {
            for (int j = i; j < MX; j += i) {
                g[j].add(i);
            }
        }
    }

    private int cur;
    private int[] ans;
    private int[] path;

    public int[] minDifference(int n, int k) {
        cur = Integer.MAX_VALUE;
        ans = null;
        path = new int[k];
        dfs(k - 1, n, Integer.MAX_VALUE, 0);
        return ans;
    }

    private void dfs(int i, int x, int mi, int mx) {
        if (i == 0) {
            int d = Math.max(mx, x) - Math.min(mi, x);
            if (d < cur) {
                cur = d;
                path[i] = x;
                ans = path.clone();
            }
            return;
        }
        for (int y : g[x]) {
            path[i] = y;
            dfs(i - 1, x / y, Math.min(mi, y), Math.max(mx, y));
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    static const int MX = 100001;
    static vector<vector<int>> g;

    vector<int> ans;
    vector<int> path;
    int cur;

    vector<int> minDifference(int n, int k) {
        if (g.empty()) {
            g.resize(MX);
            for (int i = 1; i < MX; i++) {
                for (int j = i; j < MX; j += i) {
                    g[j].push_back(i);
                }
            }
        }

        cur = INT_MAX;
        ans.clear();
        path.assign(k, 0);

        dfs(k - 1, n, INT_MAX, 0);
        return ans;
    }

private:
    void dfs(int i, int x, int mi, int mx) {
        if (i == 0) {
            int d = max(mx, x) - min(mi, x);
            if (d < cur) {
                cur = d;
                path[i] = x;
                ans = path;
            }
            return;
        }
        for (int y : g[x]) {
            path[i] = y;
            dfs(i - 1, x / y, min(mi, y), max(mx, y));
        }
    }
};

vector<vector<int>> Solution::g;
```

#### Go

```go
const MX = 100001

var g [][]int

func init() {
	g = make([][]int, MX)
	for i := 1; i < MX; i++ {
		for j := i; j < MX; j += i {
			g[j] = append(g[j], i)
		}
	}
}

var (
	cur  int
	ans  []int
	path []int
)

func minDifference(n int, k int) []int {
	cur = math.MaxInt32
	ans = nil
	path = make([]int, k)
	dfs(k-1, n, math.MaxInt32, 0)
	return ans
}

func dfs(i, x, mi, mx int) {
	if i == 0 {
		d := max(mx, x) - min(mi, x)
		if d < cur {
			cur = d
			path[i] = x
			ans = slices.Clone(path)
		}
		return
	}
	for _, y := range g[x] {
		path[i] = y
		dfs(i-1, x/y, min(mi, y), max(mx, y))
	}
}
```

#### TypeScript

```ts
const MX = 100001;
const g: number[][] = Array.from({ length: MX }, () => []);
for (let i = 1; i < MX; i++) {
    for (let j = i; j < MX; j += i) {
        g[j].push(i);
    }
}

function minDifference(n: number, k: number): number[] {
    let cur = Number.MAX_SAFE_INTEGER;
    let ans: number[] | null = null;
    const path: number[] = Array(k).fill(0);

    function dfs(i: number, x: number, mi: number, mx: number): void {
        if (i === 0) {
            const d = Math.max(mx, x) - Math.min(mi, x);
            if (d < cur) {
                cur = d;
                path[i] = x;
                ans = [...path];
            }
            return;
        }
        for (const y of g[x]) {
            path[i] = y;
            dfs(i - 1, Math.floor(x / y), Math.min(mi, y), Math.max(mx, y));
        }
    }

    dfs(k - 1, n, Number.MAX_SAFE_INTEGER, 0);
    return ans ?? [];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
