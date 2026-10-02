---
comments: true
difficulty: Hard
rating: 1927
source: Biweekly Contest 26 Q4
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - Unbounded Knapsack
---

<!-- problem:start -->

# [1449. Form Largest Integer With Digits That Add up to Target](https://leetcode.com/problems/form-largest-integer-with-digits-that-add-up-to-target)

[中文文档](/solution/1400-1499/1449.Form%20Largest%20Integer%20With%20Digits%20That%20Add%20up%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>cost</code> và một số nguyên <code>target</code>, hãy trả về <em>số nguyên <strong>lớn nhất</strong> mà bạn có thể tạo theo các quy tắc sau</em>:</p>

<ul>
	<li>Chi phí để tô chữ số <code>(i + 1)</code> được cho bởi <code>cost[i]</code> (<strong>đánh chỉ số từ 0</strong>).</li>
	<li>Tổng chi phí sử dụng phải bằng <code>target</code>.</li>
	<li>Số nguyên không được chứa chữ số <code>0</code>.</li>
</ul>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án dưới dạng chuỗi. Nếu không thể tạo được số nguyên nào thỏa mãn điều kiện, hãy trả về <code>&quot;0&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [4,3,2,5,6,7,2,5,5], target = 9
<strong>Đầu ra:</strong> &quot;7772&quot;
<strong>Giải thích:</strong> Chi phí để tô chữ số &#39;7&#39; là 2, còn chữ số &#39;2&#39; là 3. Khi đó cost(&quot;7772&quot;) = 2*3+ 3*1 = 9. Bạn cũng có thể tô &quot;977&quot;, nhưng &quot;7772&quot; là số lớn hơn.
<strong>Chữ số    chi phí</strong>
  1  -&gt;   4
  2  -&gt;   3
  3  -&gt;   2
  4  -&gt;   5
  5  -&gt;   6
  6  -&gt;   7
  7  -&gt;   2
  8  -&gt;   5
  9  -&gt;   5
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [7,6,5,5,5,6,8,7,8], target = 12
<strong>Đầu ra:</strong> &quot;85&quot;
<strong>Giải thích:</strong> Chi phí để tô chữ số &#39;8&#39; là 7, còn chữ số &#39;5&#39; là 5. Khi đó cost(&quot;85&quot;) = 7 + 5 = 12.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [2,4,6,2,4,6,4,4,4], target = 5
<strong>Đầu ra:</strong> &quot;0&quot;
<strong>Giải thích:</strong> Không thể tạo số nguyên nào có tổng chi phí bằng target.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>cost.length == 9</code></li>
	<li><code>1 &lt;= cost[i], target &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ số $1$– $9$ có chi phí và có thể được dùng lại để đạt `target`. Số nguyên lớn nhất theo thứ tự từ điển trước hết phải có độ dài lớn nhất, sau đó ưu tiên các chữ số lớn hơn.
>
> $f[i][j]$ là độ dài lớn nhất khi dùng $i$ chữ số đầu tiên với chi phí chính xác là $j$ (bài toán ba lô không giới hạn). $g[i][j]$ ghi lại việc chữ số $i$ có được chọn hay không để ta có thể truy vết từ $9$ xuống. Trường hợp không thể tạo đáp án cho giá trị `0`.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestNumber(self, cost: List[int], target: int) -> str:
        f = [[-inf] * (target + 1) for _ in range(10)]
        f[0][0] = 0
        g = [[0] * (target + 1) for _ in range(10)]
        for i, c in enumerate(cost, 1):
            for j in range(target + 1):
                if j < c or f[i][j - c] + 1 < f[i - 1][j]:
                    f[i][j] = f[i - 1][j]
                    g[i][j] = j
                else:
                    f[i][j] = f[i][j - c] + 1
                    g[i][j] = j - c
        if f[9][target] < 0:
            return "0"
        ans = []
        i, j = 9, target
        while i:
            if j == g[i][j]:
                i -= 1
            else:
                ans.append(str(i))
                j = g[i][j]
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String largestNumber(int[] cost, int target) {
        final int inf = 1 << 30;
        int[][] f = new int[10][target + 1];
        int[][] g = new int[10][target + 1];
        for (var e : f) {
            Arrays.fill(e, -inf);
        }
        f[0][0] = 0;
        for (int i = 1; i <= 9; ++i) {
            int c = cost[i - 1];
            for (int j = 0; j <= target; ++j) {
                if (j < c || f[i][j - c] + 1 < f[i - 1][j]) {
                    f[i][j] = f[i - 1][j];
                    g[i][j] = j;
                } else {
                    f[i][j] = f[i][j - c] + 1;
                    g[i][j] = j - c;
                }
            }
        }
        if (f[9][target] < 0) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 9, j = target; i > 0;) {
            if (j == g[i][j]) {
                --i;
            } else {
                sb.append(i);
                j = g[i][j];
            }
        }
        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string largestNumber(vector<int>& cost, int target) {
        const int inf = 1 << 30;
        vector<vector<int>> f(10, vector<int>(target + 1, -inf));
        vector<vector<int>> g(10, vector<int>(target + 1));
        f[0][0] = 0;
        for (int i = 1; i <= 9; ++i) {
            int c = cost[i - 1];
            for (int j = 0; j <= target; ++j) {
                if (j < c || f[i][j - c] + 1 < f[i - 1][j]) {
                    f[i][j] = f[i - 1][j];
                    g[i][j] = j;
                } else {
                    f[i][j] = f[i][j - c] + 1;
                    g[i][j] = j - c;
                }
            }
        }
        if (f[9][target] < 0) {
            return "0";
        }
        string ans;
        for (int i = 9, j = target; i;) {
            if (g[i][j] == j) {
                --i;
            } else {
                ans += '0' + i;
                j = g[i][j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestNumber(cost []int, target int) string {
	const inf = 1 << 30
	f := make([][]int, 10)
	g := make([][]int, 10)
	for i := range f {
		f[i] = make([]int, target+1)
		g[i] = make([]int, target+1)
		for j := range f[i] {
			f[i][j] = -inf
		}
	}
	f[0][0] = 0
	for i := 1; i <= 9; i++ {
		c := cost[i-1]
		for j := 0; j <= target; j++ {
			if j < c || f[i][j-c]+1 < f[i-1][j] {
				f[i][j] = f[i-1][j]
				g[i][j] = j
			} else {
				f[i][j] = f[i][j-c] + 1
				g[i][j] = j - c
			}
		}
	}
	if f[9][target] < 0 {
		return "0"
	}
	ans := []byte{}
	for i, j := 9, target; i > 0; {
		if g[i][j] == j {
			i--
		} else {
			ans = append(ans, '0'+byte(i))
			j = g[i][j]
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function largestNumber(cost: number[], target: number): string {
    const inf = 1 << 30;
    const f: number[][] = Array(10)
        .fill(0)
        .map(() => Array(target + 1).fill(-inf));
    const g: number[][] = Array(10)
        .fill(0)
        .map(() => Array(target + 1).fill(0));
    f[0][0] = 0;
    for (let i = 1; i <= 9; ++i) {
        const c = cost[i - 1];
        for (let j = 0; j <= target; ++j) {
            if (j < c || f[i][j - c] + 1 < f[i - 1][j]) {
                f[i][j] = f[i - 1][j];
                g[i][j] = j;
            } else {
                f[i][j] = f[i][j - c] + 1;
                g[i][j] = j - c;
            }
        }
    }
    if (f[9][target] < 0) {
        return '0';
    }
    const ans: number[] = [];
    for (let i = 9, j = target; i;) {
        if (g[i][j] === j) {
            --i;
        } else {
            ans.push(i);
            j = g[i][j];
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
