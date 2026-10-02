---
comments: true
difficulty: Hard
rating: 1873
source: Weekly Contest 125 Q4
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1001. Grid Illumination](https://leetcode.com/problems/grid-illumination)

[中文文档](/solution/1000-1099/1001.Grid%20Illumination/README.md)

## Mô tả

<!-- description:start -->

<p>Có một lưới 2D <code>grid</code> kích thước <code>n x n</code>, mỗi ô ban đầu có một chiếc đèn <strong>tắt</strong>.</p>

<p>Cho mảng 2D các vị trí đèn <code>lamps</code>, trong đó <code>lamps[i] = [row<sub>i</sub>, col<sub>i</sub>]</code> cho biết đèn tại <code>grid[row<sub>i</sub>][col<sub>i</sub>]</code> đang <strong>bật</strong>. Nếu cùng một đèn xuất hiện nhiều lần trong danh sách, đèn đó vẫn chỉ được bật.</p>

<p>Khi được bật, một chiếc đèn <strong>chiếu sáng ô của nó</strong> và <strong>mọi ô khác</strong> cùng <strong>hàng, cột hoặc đường chéo</strong>.</p>

<p>Ta còn được cho mảng 2D <code>queries</code>, trong đó <code>queries[j] = [row<sub>j</sub>, col<sub>j</sub>]</code>. Với query thứ <code>j<sup>th</sup></code>, hãy xác định ô <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code> có được chiếu sáng hay không. Sau khi trả lời query thứ <code>j<sup>th</sup></code>, hãy <strong>tắt</strong> đèn tại <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code> và <strong>8 đèn kề</strong> nếu có. Hai đèn được xem là kề nhau nếu ô của chúng chung một cạnh hoặc góc với <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code>.</p>

<p>Trả về <em>mảng số nguyên </em><code>ans</code><em>, trong đó </em><code>ans[j]</code><em> bằng </em><code>1</code><em> nếu ô trong query thứ </em><code>j<sup>th</sup></code><em> được chiếu sáng, hoặc bằng </em><code>0</code><em> nếu không được chiếu sáng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1001.Grid%20Illumination/images/illu_1.jpg" style="width: 750px; height: 209px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, lamps = [[0,0],[4,4]], queries = [[1,1],[1,0]]
<strong>Đầu ra:</strong> [1,0]
<strong>Giải thích:</strong> Ban đầu, tất cả đèn trong lưới đều tắt. Hình trên minh họa lưới sau khi bật đèn tại grid[0][0], rồi bật đèn tại grid[4][4].
Với query thứ 0, ta kiểm tra đèn tại grid[1][1] có được chiếu sáng hay không (ô màu xanh). Ô này được chiếu sáng nên đặt ans[0] = 1. Sau đó, ta tắt tất cả đèn trong vùng hình vuông màu đỏ.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1001.Grid%20Illumination/images/illu_step1.jpg" style="width: 500px; height: 218px;" />
Với query thứ 1, ta kiểm tra đèn tại grid[1][0] có được chiếu sáng hay không (ô màu xanh). Ô này không được chiếu sáng nên đặt ans[1] = 0. Sau đó, ta tắt tất cả đèn trong vùng hình chữ nhật màu đỏ.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1001.Grid%20Illumination/images/illu_step2.jpg" style="width: 500px; height: 219px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, lamps = [[0,0],[4,4]], queries = [[1,1],[1,1]]
<strong>Đầu ra:</strong> [1,1]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, lamps = [[0,0],[0,4]], queries = [[0,4],[0,1],[1,4]]
<strong>Đầu ra:</strong> [1,1,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= lamps.length &lt;= 20000</code></li>
	<li><code>0 &lt;= queries.length &lt;= 20000</code></li>
	<li><code>lamps[i].length == 2</code></li>
	<li><code>0 &lt;= row<sub>i</sub>, col<sub>i</sub> &lt; n</code></li>
	<li><code>queries[j].length == 2</code></li>
	<li><code>0 &lt;= row<sub>j</sub>, col<sub>j</sub> &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Cạnh lưới $n$ có thể bằng $10^9$, nên không thể mô phỏng việc chiếu sáng từng ô. Vì số đèn và query tối đa chỉ là $2\times 10^4$, ta chỉ cần theo dõi các hàng, cột và đường chéo có đèn.
>
> Đèn tại $(x,y)$ chiếu sáng hàng $x$, cột $y$ và hai đường chéo $x-y$, $x+y$. Ô được query sáng khi một trong bốn đường này vẫn còn đèn; sau đó ta tắt đèn tại ô đó và tám ô lân cận, đồng thời giảm số đèn trên các đường tương ứng.
>
> Dùng một set để lưu tọa độ đèn không trùng lặp và bốn hash map để đếm số đèn trên từng đường. Mỗi query kiểm tra bốn bộ đếm và cập nhật một vùng lân cận có kích thước cố định, nên thời gian phụ thuộc vào số đèn và query chứ không phụ thuộc vào $n$.

<!-- thinking:end -->

Giả sử tọa độ của một đèn là $(x, y)$. Khi đó, chỉ số hàng là $x$, chỉ số cột là $y$, giá trị đường chéo chính là $x-y$, còn giá trị đường chéo phụ là $x+y$. Sau khi xác định được giá trị đại diện duy nhất cho mỗi đường, ta dùng hash table để ghi lại số đèn trên đường đó.

Ta duyệt mảng $\textit{lamps}$; với mỗi đèn, tăng số đèn trên hàng, cột, đường chéo chính và đường chéo phụ của nó lên $1$.

Lưu ý, khi xử lý $\textit{lamps}$, ta cần loại bỏ các phần tử trùng vì những đèn được liệt kê nhiều lần vẫn chỉ là một đèn.

Tiếp theo, ta duyệt các query và kiểm tra hàng, cột, đường chéo chính hoặc đường chéo phụ đi qua điểm được query hiện tại có đèn hay không. Nếu có, đặt giá trị tương ứng thành $1$ để đánh dấu ô được chiếu sáng. Sau đó, ta tắt đèn bằng cách kiểm tra tám ô lân cận và chính ô được query xem có đèn hay không. Nếu có, giảm số đèn trên hàng, cột, đường chéo chính và đường chéo phụ tương ứng đi $1$, rồi xóa đèn khỏi lưới.

Cuối cùng, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(m + q)$, trong đó $m$ và $q$ lần lượt là độ dài của các mảng $\textit{lamps}$ và $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gridIllumination(
        self, n: int, lamps: List[List[int]], queries: List[List[int]]
    ) -> List[int]:
        s = {(i, j) for i, j in lamps}
        row, col, diag1, diag2 = Counter(), Counter(), Counter(), Counter()
        for i, j in s:
            row[i] += 1
            col[j] += 1
            diag1[i - j] += 1
            diag2[i + j] += 1
        ans = [0] * len(queries)
        for k, (i, j) in enumerate(queries):
            if row[i] or col[j] or diag1[i - j] or diag2[i + j]:
                ans[k] = 1
            for x in range(i - 1, i + 2):
                for y in range(j - 1, j + 2):
                    if (x, y) in s:
                        s.remove((x, y))
                        row[x] -= 1
                        col[y] -= 1
                        diag1[x - y] -= 1
                        diag2[x + y] -= 1
        return ans
```

#### Java

```java
class Solution {
    private int n;
    public int[] gridIllumination(int n, int[][] lamps, int[][] queries) {
        this.n = n;
        Set<Long> s = new HashSet<>();
        Map<Integer, Integer> row = new HashMap<>();
        Map<Integer, Integer> col = new HashMap<>();
        Map<Integer, Integer> diag1 = new HashMap<>();
        Map<Integer, Integer> diag2 = new HashMap<>();
        for (var lamp : lamps) {
            int i = lamp[0], j = lamp[1];
            if (s.add(f(i, j))) {
                merge(row, i, 1);
                merge(col, j, 1);
                merge(diag1, i - j, 1);
                merge(diag2, i + j, 1);
            }
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int k = 0; k < m; ++k) {
            int i = queries[k][0], j = queries[k][1];
            if (exist(row, i) || exist(col, j) || exist(diag1, i - j) || exist(diag2, i + j)) {
                ans[k] = 1;
            }
            for (int x = i - 1; x <= i + 1; ++x) {
                for (int y = j - 1; y <= j + 1; ++y) {
                    if (x < 0 || x >= n || y < 0 || y >= n || !s.contains(f(x, y))) {
                        continue;
                    }
                    s.remove(f(x, y));
                    merge(row, x, -1);
                    merge(col, y, -1);
                    merge(diag1, x - y, -1);
                    merge(diag2, x + y, -1);
                }
            }
        }
        return ans;
    }

    private void merge(Map<Integer, Integer> cnt, int x, int d) {
        if (cnt.merge(x, d, Integer::sum) == 0) {
            cnt.remove(x);
        }
    }

    private boolean exist(Map<Integer, Integer> cnt, int x) {
        return cnt.getOrDefault(x, 0) > 0;
    }

    private long f(long i, long j) {
        return i * n + j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> gridIllumination(int n, vector<vector<int>>& lamps, vector<vector<int>>& queries) {
        auto f = [&](int i, int j) -> long long {
            return (long long) i * n + j;
        };
        unordered_set<long long> s;
        unordered_map<int, int> row, col, diag1, diag2;
        for (auto& lamp : lamps) {
            int i = lamp[0], j = lamp[1];
            if (!s.count(f(i, j))) {
                s.insert(f(i, j));
                row[i]++;
                col[j]++;
                diag1[i - j]++;
                diag2[i + j]++;
            }
        }
        int m = queries.size();
        vector<int> ans(m);
        for (int k = 0; k < m; ++k) {
            int i = queries[k][0], j = queries[k][1];
            if (row[i] > 0 || col[j] > 0 || diag1[i - j] > 0 || diag2[i + j] > 0) {
                ans[k] = 1;
            }
            for (int x = i - 1; x <= i + 1; ++x) {
                for (int y = j - 1; y <= j + 1; ++y) {
                    if (x < 0 || x >= n || y < 0 || y >= n || !s.count(f(x, y))) {
                        continue;
                    }
                    s.erase(f(x, y));
                    row[x]--;
                    col[y]--;
                    diag1[x - y]--;
                    diag2[x + y]--;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func gridIllumination(n int, lamps [][]int, queries [][]int) []int {
	row, col, diag1, diag2 := map[int]int{}, map[int]int{}, map[int]int{}, map[int]int{}
	type pair struct{ x, y int }
	s := map[pair]bool{}
	for _, lamp := range lamps {
		i, j := lamp[0], lamp[1]
		p := pair{i, j}
		if !s[p] {
			s[p] = true
			row[i]++
			col[j]++
			diag1[i-j]++
			diag2[i+j]++
		}
	}
	m := len(queries)
	ans := make([]int, m)
	for k, q := range queries {
		i, j := q[0], q[1]
		if row[i] > 0 || col[j] > 0 || diag1[i-j] > 0 || diag2[i+j] > 0 {
			ans[k] = 1
		}
		for x := i - 1; x <= i+1; x++ {
			for y := j - 1; y <= j+1; y++ {
				p := pair{x, y}
				if s[p] {
					s[p] = false
					row[x]--
					col[y]--
					diag1[x-y]--
					diag2[x+y]--
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function gridIllumination(n: number, lamps: number[][], queries: number[][]): number[] {
    const row = new Map<number, number>();
    const col = new Map<number, number>();
    const diag1 = new Map<number, number>();
    const diag2 = new Map<number, number>();
    const s = new Set<number>();
    for (const [i, j] of lamps) {
        if (s.has(i * n + j)) {
            continue;
        }
        s.add(i * n + j);
        row.set(i, (row.get(i) || 0) + 1);
        col.set(j, (col.get(j) || 0) + 1);
        diag1.set(i - j, (diag1.get(i - j) || 0) + 1);
        diag2.set(i + j, (diag2.get(i + j) || 0) + 1);
    }
    const ans: number[] = [];
    for (const [i, j] of queries) {
        if (row.get(i)! > 0 || col.get(j)! > 0 || diag1.get(i - j)! > 0 || diag2.get(i + j)! > 0) {
            ans.push(1);
        } else {
            ans.push(0);
        }
        for (let x = i - 1; x <= i + 1; ++x) {
            for (let y = j - 1; y <= j + 1; ++y) {
                if (x < 0 || x >= n || y < 0 || y >= n || !s.has(x * n + y)) {
                    continue;
                }
                s.delete(x * n + y);
                row.set(x, row.get(x)! - 1);
                col.set(y, col.get(y)! - 1);
                diag1.set(x - y, diag1.get(x - y)! - 1);
                diag2.set(x + y, diag2.get(x + y)! - 1);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
