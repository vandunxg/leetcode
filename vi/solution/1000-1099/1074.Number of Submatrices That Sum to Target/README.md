---
comments: true
difficulty: Hard
rating: 2189
source: Weekly Contest 139 Q4
tags:
    - Array
    - Hash Table
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [1074. Number of Submatrices That Sum to Target](https://leetcode.com/problems/number-of-submatrices-that-sum-to-target)

[中文文档](/solution/1000-1099/1074.Number%20of%20Submatrices%20That%20Sum%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>matrix</code> và <code>target</code>, hãy trả về số ma trận con không rỗng có tổng bằng <font face="monospace">target</font>.</p>

<p>Ma trận con <code>x1, y1, x2, y2</code> gồm tất cả ô <code>matrix[x][y]</code> thỏa mãn <code>x1 &lt;= x &lt;= x2</code> và <code>y1 &lt;= y &lt;= y2</code>.</p>

<p>Hai ma trận con <code>(x1, y1, x2, y2)</code> và <code>(x1&#39;, y1&#39;, x2&#39;, y2&#39;)</code> được xem là khác nhau nếu có ít nhất một tọa độ khác nhau, ví dụ <code>x1 != x1&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1074.Number%20of%20Submatrices%20That%20Sum%20to%20Target/images/mate1.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[0,1,0],[1,1,1],[0,1,0]], target = 0
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có bốn ma trận con 1x1 chỉ chứa giá trị 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[1,-1],[-1,1]], target = 0
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Gồm hai ma trận con 1x2, hai ma trận con 2x1 và một ma trận con 2x2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[904]], target = 0
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= matrix.length &lt;= 100</code></li>
	<li><code>1 &lt;= matrix[0].length &lt;= 100</code></li>
	<li><code>-1000 &lt;= matrix[i][j] &lt;= 1000</code></li>
	<li><code>-10^8 &lt;= target &lt;= 10^8</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Dùng bốn vòng lặp lồng nhau trên ma trận $100\times 100$ sẽ có độ phức tạp $O(n^4)$. Nếu cố định hàng trên và hàng dưới, ta có thể gộp mỗi cột thành một tổng, từ đó bài toán còn lại là tìm “các mảng con có tổng bằng $\textit{target}$” trên mảng một chiều.
>
> Bài toán một chiều này được giải bằng prefix sum kết hợp với map: sau khi cộng thêm $x$, tra cứu $s-\textit{target}$. Có $O(n^2)$ cặp hàng và mỗi cặp cần duyệt tuyến tính.
>
> Cộng $f(\textit{col})$ cho mọi cặp hàng làm biên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSubmatrixSumTarget(self, matrix: List[List[int]], target: int) -> int:
        def f(nums: List[int]) -> int:
            d = defaultdict(int)
            d[0] = 1
            cnt = s = 0
            for x in nums:
                s += x
                cnt += d[s - target]
                d[s] += 1
            return cnt

        m, n = len(matrix), len(matrix[0])
        ans = 0
        for i in range(m):
            col = [0] * n
            for j in range(i, m):
                for k in range(n):
                    col[k] += matrix[j][k]
                ans += f(col)
        return ans
```

#### Java

```java
class Solution {
    public int numSubmatrixSumTarget(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            int[] col = new int[n];
            for (int j = i; j < m; ++j) {
                for (int k = 0; k < n; ++k) {
                    col[k] += matrix[j][k];
                }
                ans += f(col, target);
            }
        }
        return ans;
    }

    private int f(int[] nums, int target) {
        Map<Integer, Integer> d = new HashMap<>();
        d.put(0, 1);
        int s = 0, cnt = 0;
        for (int x : nums) {
            s += x;
            cnt += d.getOrDefault(s - target, 0);
            d.merge(s, 1, Integer::sum);
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numSubmatrixSumTarget(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            vector<int> col(n);
            for (int j = i; j < m; ++j) {
                for (int k = 0; k < n; ++k) {
                    col[k] += matrix[j][k];
                }
                ans += f(col, target);
            }
        }
        return ans;
    }

    int f(vector<int>& nums, int target) {
        unordered_map<int, int> d{{0, 1}};
        int cnt = 0, s = 0;
        for (int& x : nums) {
            s += x;
            if (d.count(s - target)) {
                cnt += d[s - target];
            }
            ++d[s];
        }
        return cnt;
    }
};
```

#### Go

```go
func numSubmatrixSumTarget(matrix [][]int, target int) (ans int) {
	m, n := len(matrix), len(matrix[0])
	for i := 0; i < m; i++ {
		col := make([]int, n)
		for j := i; j < m; j++ {
			for k := 0; k < n; k++ {
				col[k] += matrix[j][k]
			}
			ans += f(col, target)
		}
	}
	return
}

func f(nums []int, target int) (cnt int) {
	d := map[int]int{0: 1}
	s := 0
	for _, x := range nums {
		s += x
		if v, ok := d[s-target]; ok {
			cnt += v
		}
		d[s]++
	}
	return
}
```

#### TypeScript

```ts
function numSubmatrixSumTarget(matrix: number[][], target: number): number {
    const m = matrix.length;
    const n = matrix[0].length;
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        const col: number[] = new Array(n).fill(0);
        for (let j = i; j < m; ++j) {
            for (let k = 0; k < n; ++k) {
                col[k] += matrix[j][k];
            }
            ans += f(col, target);
        }
    }
    return ans;
}

function f(nums: number[], target: number): number {
    const d: Map<number, number> = new Map();
    d.set(0, 1);
    let cnt = 0;
    let s = 0;
    for (const x of nums) {
        s += x;
        if (d.has(s - target)) {
            cnt += d.get(s - target)!;
        }
        d.set(s, (d.get(s) || 0) + 1);
    }
    return cnt;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
