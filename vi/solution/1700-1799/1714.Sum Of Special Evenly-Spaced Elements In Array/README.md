---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
    - Sqrt Decomposition
---

<!-- problem:start -->

# [1714. Sum Of Special Evenly-Spaced Elements In Array 🔒](https://leetcode.com/problems/sum-of-special-evenly-spaced-elements-in-array)

[中文文档](/solution/1700-1799/1714.Sum%20Of%20Special%20Evenly-Spaced%20Elements%20In%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, gồm <code>n</code> số nguyên không âm.</p>

<p>Bạn cũng được cho mảng <code>queries</code>, trong đó <code>queries[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>. Đáp án của truy vấn thứ <code>i<sup>th</sup></code> là tổng mọi <code>nums[j]</code> sao cho <code>x<sub>i</sub> &lt;= j &lt; n</code> và <code>(j - x<sub>i</sub>)</code> chia hết cho <code>y<sub>i</sub></code>.</p>

<p>Trả về <em>một mảng </em><code>answer</code><em>, trong đó </em><code>answer.length == queries.length</code><em> và </em><code>answer[i]</code><em> là đáp án của </em><code>i<sup>th</sup></code><em> truy vấn, lấy <b>modulo</b> </em><code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [0,1,2,3,4,5,6,7], queries = [[0,3],[5,1],[4,2]]
<strong>Output:</strong> [9,18,10]
<strong>Explanation:</strong> Đáp án của các truy vấn như sau:
1) Các chỉ số j thỏa truy vấn này là 0, 3 và 6. nums[0] + nums[3] + nums[6] = 9
2) Các chỉ số j thỏa truy vấn này là 5, 6 và 7. nums[5] + nums[6] + nums[7] = 18
3) Các chỉ số j thỏa truy vấn này là 4 và 6. nums[4] + nums[6] = 10
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [100,200,101,201,102,202,103,203], queries = [[0,7]]
<strong>Output:</strong> [303]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 1.5 * 10<sup>5</sup></code></li>
	<li><code>0 &lt;= x<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= y<sub>i</sub> &lt;= 5 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Block Decomposition

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn cộng các phần tử cách nhau $y$ vị trí, bắt đầu từ $x$. Duyệt theo bước cho từng truy vấn quá chậm khi $q\le 1.5\times 10^5$ và $n\le 5\times 10^4$, đặc biệt với $y$ nhỏ.
>
> Bước lớn tạo ra dãy ngắn nên có thể cộng trực tiếp; bước nhỏ tạo ra dãy dài nên cần tiền xử lý. Ta chia tại $\sqrt{n}$.
>
> $\textit{suf}[i][j]$ là tổng hậu tố bắt đầu từ $j$ với bước $i$. Tra cứu khi $y\le\sqrt{n}$; ngược lại thì duyệt trực tiếp. Tổng độ phức tạp là $O((n+q)\sqrt{n})$.

<!-- thinking:end -->

Đây là bài toán phân rã block điển hình. Với truy vấn có bước lớn, ta có thể brute force trực tiếp; với truy vấn có bước nhỏ, ta tiền xử lý tổng hậu tố của từng vị trí rồi truy vấn trực tiếp.

Trong bài này, ta lấy ngưỡng bước lớn là $\sqrt{n}$, nhờ đó đảm bảo độ phức tạp của mỗi truy vấn là $O(\sqrt{n})$.

Ta định nghĩa mảng hai chiều $suf$, trong đó $suf[i][j]$ là tổng hậu tố bắt đầu từ vị trí $j$ với bước $i$. Với mỗi truy vấn $[x, y]$, ta chia thành hai trường hợp:

- Nếu $y \le \sqrt{n}$, ta truy vấn trực tiếp $suf[y][x]$;
- Nếu $y > \sqrt{n}$, ta brute force trực tiếp.

Độ phức tạp thời gian là $O((n + m) \times \sqrt{n})$, còn độ phức tạp không gian là $O(n \times \sqrt{n})$. Trong đó, $n$ là độ dài mảng và $m$ là số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def solve(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        mod = 10**9 + 7
        n = len(nums)
        m = int(sqrt(n))
        suf = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(n - 1, -1, -1):
                suf[i][j] = suf[i][min(n, j + i)] + nums[j]
        ans = []
        for x, y in queries:
            if y <= m:
                ans.append(suf[y][x] % mod)
            else:
                ans.append(sum(nums[x::y]) % mod)
        return ans
```

#### Java

```java
class Solution {
    public int[] solve(int[] nums, int[][] queries) {
        int n = nums.length;
        int m = (int) Math.sqrt(n);
        final int mod = (int) 1e9 + 7;
        int[][] suf = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = n - 1; j >= 0; --j) {
                suf[i][j] = (suf[i][Math.min(n, j + i)] + nums[j]) % mod;
            }
        }
        int k = queries.length;
        int[] ans = new int[k];
        for (int i = 0; i < k; ++i) {
            int x = queries[i][0];
            int y = queries[i][1];
            if (y <= m) {
                ans[i] = suf[y][x];
            } else {
                int s = 0;
                for (int j = x; j < n; j += y) {
                    s = (s + nums[j]) % mod;
                }
                ans[i] = s;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> solve(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        int m = (int) sqrt(n);
        const int mod = 1e9 + 7;
        int suf[m + 1][n + 1];
        memset(suf, 0, sizeof(suf));
        for (int i = 1; i <= m; ++i) {
            for (int j = n - 1; ~j; --j) {
                suf[i][j] = (suf[i][min(n, j + i)] + nums[j]) % mod;
            }
        }
        vector<int> ans;
        for (auto& q : queries) {
            int x = q[0], y = q[1];
            if (y <= m) {
                ans.push_back(suf[y][x]);
            } else {
                int s = 0;
                for (int i = x; i < n; i += y) {
                    s = (s + nums[i]) % mod;
                }
                ans.push_back(s);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func solve(nums []int, queries [][]int) (ans []int) {
	n := len(nums)
	m := int(math.Sqrt(float64(n)))
	const mod int = 1e9 + 7
	suf := make([][]int, m+1)
	for i := range suf {
		suf[i] = make([]int, n+1)
		for j := n - 1; j >= 0; j-- {
			suf[i][j] = (suf[i][min(n, j+i)] + nums[j]) % mod
		}
	}
	for _, q := range queries {
		x, y := q[0], q[1]
		if y <= m {
			ans = append(ans, suf[y][x])
		} else {
			s := 0
			for i := x; i < n; i += y {
				s = (s + nums[i]) % mod
			}
			ans = append(ans, s)
		}
	}
	return
}
```

#### TypeScript

```ts
function solve(nums: number[], queries: number[][]): number[] {
    const n = nums.length;
    const m = Math.floor(Math.sqrt(n));
    const mod = 10 ** 9 + 7;
    const suf: number[][] = Array(m + 1)
        .fill(0)
        .map(() => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = n - 1; j >= 0; --j) {
            suf[i][j] = (suf[i][Math.min(n, j + i)] + nums[j]) % mod;
        }
    }
    const ans: number[] = [];
    for (const [x, y] of queries) {
        if (y <= m) {
            ans.push(suf[y][x]);
        } else {
            let s = 0;
            for (let i = x; i < n; i += y) {
                s = (s + nums[i]) % mod;
            }
            ans.push(s);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
