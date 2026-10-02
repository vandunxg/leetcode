---
comments: true
difficulty: Medium
rating: 1626
source: Biweekly Contest 8 Q3
tags:
    - Array
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [1182. Shortest Distance to Target Color 🔒](https://leetcode.com/problems/shortest-distance-to-target-color)

[中文文档](/solution/1100-1199/1182.Shortest%20Distance%20to%20Target%20Color/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>colors</code> gồm ba màu: <code>1</code>, <code>2</code> và <code>3</code>.</p>

<p>Bạn cũng được cho một số query. Mỗi query gồm hai số nguyên <code>i</code> và <code>c</code>; hãy trả về khoảng cách ngắn nhất từ chỉ số <code>i</code> đến màu đích <code>c</code>. Nếu không có vị trí phù hợp, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> colors = [1,1,2,1,3,2,2,3,3], queries = [[1,3],[2,2],[6,1]]
<strong>Output:</strong> [3,0,3]
<strong>Giải thích: </strong>
Vị trí gần nhất có màu 3 tính từ chỉ số 1 là chỉ số 4 (cách 3 bước).
Màu 2 gần nhất tính từ chỉ số 2 nằm ngay tại chỉ số 2 (cách 0 bước).
Vị trí gần nhất có màu 1 tính từ chỉ số 6 là chỉ số 3 (cách 3 bước).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> colors = [1,2], queries = [[0,3]]
<strong>Output:</strong> [-1]
<strong>Giải thích: </strong>Trong mảng không có màu 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= colors.length &lt;= 5*10^4</code></li>
	<li><code>1 &lt;= colors[i] &lt;= 3</code></li>
	<li><code>1&nbsp;&lt;= queries.length &lt;= 5*10^4</code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= queries[i][0] &lt;&nbsp;colors.length</code></li>
	<li><code>1 &lt;= queries[i][1] &lt;= 3</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi query, nếu đi lần lượt sang trái và phải từ $i$ thì thời gian tỉ lệ với số query nhân $n$. Chỉ có ba màu nên có thể tiền xử lý chỉ số gần nhất của mỗi màu ở cả hai phía. Kết quả của mỗi query là khoảng cách nhỏ hơn trong hai phía; dùng giá trị canh $\pm\infty$ để khoảng cách tới màu không tồn tại lớn hơn $n$.

<!-- thinking:end -->

Ta tiền xử lý khoảng cách từ mỗi vị trí đến màu $1$, $2$, $3$ gần nhất ở bên trái và bên phải, rồi lưu vào các mảng $left$ và $right$. Ban đầu, $left[0][0] = left[0][1] = left[0][2] = -\infty$, và $right[n][0] = right[n][1] = right[n][2] = \infty$, trong đó $n$ là độ dài mảng `colors`.

Với mỗi query $(i, c)$, khoảng cách nhỏ nhất là $d = \min(i - left[i + 1][c - 1], right[i][c - 1] - i)$. Nếu $d > n$ thì không có vị trí phù hợp và đáp án cho query này là $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài mảng `colors`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestDistanceColor(
        self, colors: List[int], queries: List[List[int]]
    ) -> List[int]:
        n = len(colors)
        right = [[inf] * 3 for _ in range(n + 1)]
        for i in range(n - 1, -1, -1):
            for j in range(3):
                right[i][j] = right[i + 1][j]
            right[i][colors[i] - 1] = i
        left = [[-inf] * 3 for _ in range(n + 1)]
        for i, c in enumerate(colors, 1):
            for j in range(3):
                left[i][j] = left[i - 1][j]
            left[i][c - 1] = i - 1
        ans = []
        for i, c in queries:
            d = min(i - left[i + 1][c - 1], right[i][c - 1] - i)
            ans.append(-1 if d > n else d)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> shortestDistanceColor(int[] colors, int[][] queries) {
        int n = colors.length;
        final int inf = 1 << 30;
        int[][] right = new int[n + 1][3];
        Arrays.fill(right[n], inf);
        for (int i = n - 1; i >= 0; --i) {
            for (int j = 0; j < 3; ++j) {
                right[i][j] = right[i + 1][j];
            }
            right[i][colors[i] - 1] = i;
        }
        int[][] left = new int[n + 1][3];
        Arrays.fill(left[0], -inf);
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 3; ++j) {
                left[i][j] = left[i - 1][j];
            }
            left[i][colors[i - 1] - 1] = i - 1;
        }
        List<Integer> ans = new ArrayList<>();
        for (int[] q : queries) {
            int i = q[0], c = q[1] - 1;
            int d = Math.min(i - left[i + 1][c], right[i][c] - i);
            ans.add(d > n ? -1 : d);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> shortestDistanceColor(vector<int>& colors, vector<vector<int>>& queries) {
        int n = colors.size();
        int right[n + 1][3];
        const int inf = 1 << 30;
        fill(right[n], right[n] + 3, inf);
        for (int i = n - 1; i >= 0; --i) {
            for (int j = 0; j < 3; ++j) {
                right[i][j] = right[i + 1][j];
            }
            right[i][colors[i] - 1] = i;
        }
        int left[n + 1][3];
        fill(left[0], left[0] + 3, -inf);
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 3; ++j) {
                left[i][j] = left[i - 1][j];
            }
            left[i][colors[i - 1] - 1] = i - 1;
        }
        vector<int> ans;
        for (auto& q : queries) {
            int i = q[0], c = q[1] - 1;
            int d = min(i - left[i + 1][c], right[i][c] - i);
            ans.push_back(d > n ? -1 : d);
        }
        return ans;
    }
};
```

#### Go

```go
func shortestDistanceColor(colors []int, queries [][]int) (ans []int) {
	n := len(colors)
	const inf = 1 << 30
	right := make([][3]int, n+1)
	left := make([][3]int, n+1)
	right[n] = [3]int{inf, inf, inf}
	left[0] = [3]int{-inf, -inf, -inf}
	for i := n - 1; i >= 0; i-- {
		for j := 0; j < 3; j++ {
			right[i][j] = right[i+1][j]
		}
		right[i][colors[i]-1] = i
	}
	for i := 1; i <= n; i++ {
		for j := 0; j < 3; j++ {
			left[i][j] = left[i-1][j]
		}
		left[i][colors[i-1]-1] = i - 1
	}
	for _, q := range queries {
		i, c := q[0], q[1]-1
		d := min(i-left[i+1][c], right[i][c]-i)
		if d > n {
			d = -1
		}
		ans = append(ans, d)
	}
	return
}
```

#### TypeScript

```ts
function shortestDistanceColor(colors: number[], queries: number[][]): number[] {
    const n = colors.length;
    const inf = 1 << 30;
    const right: number[][] = Array(n + 1)
        .fill(0)
        .map(() => Array(3).fill(inf));
    const left: number[][] = Array(n + 1)
        .fill(0)
        .map(() => Array(3).fill(-inf));
    for (let i = n - 1; i >= 0; --i) {
        for (let j = 0; j < 3; ++j) {
            right[i][j] = right[i + 1][j];
        }
        right[i][colors[i] - 1] = i;
    }
    for (let i = 1; i <= n; ++i) {
        for (let j = 0; j < 3; ++j) {
            left[i][j] = left[i - 1][j];
        }
        left[i][colors[i - 1] - 1] = i - 1;
    }
    const ans: number[] = [];
    for (const [i, c] of queries) {
        const d = Math.min(i - left[i + 1][c - 1], right[i][c - 1] - i);
        ans.push(d > n ? -1 : d);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
