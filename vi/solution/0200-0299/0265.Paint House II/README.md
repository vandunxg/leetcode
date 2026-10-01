---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [265. Paint House II 🔒](https://leetcode.com/problems/paint-house-ii)

[中文文档](/solution/0200-0299/0265.Paint%20House%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có một hàng gồm <code>n</code> ngôi nhà, mỗi nhà có thể được sơn bằng một trong <code>k</code> màu. Chi phí sơn mỗi nhà sẽ khác nhau tùy theo màu. Hãy sơn tất cả các nhà sao cho hai nhà liền kề không cùng màu.</p>

<p>Chi phí sơn mỗi nhà bằng một màu cụ thể được biểu diễn trong ma trận chi phí <code>n x k</code> costs.</p>

<ul>
	<li>Ví dụ, <code>costs[0][0]</code> là chi phí sơn nhà <code>0</code> bằng màu <code>0</code>; <code>costs[1][2]</code> là chi phí sơn nhà <code>1</code> bằng màu <code>2</code>, v.v.</li>
</ul>

<p>Hãy trả về <em>chi phí nhỏ nhất để sơn tất cả các nhà</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [[1,5,3],[2,9,4]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Sơn nhà 0 màu 0 và nhà 1 màu 2. Chi phí nhỏ nhất: 1 + 4 = 5; 
hoặc sơn nhà 0 màu 2 và nhà 1 màu 0. Chi phí nhỏ nhất: 3 + 2 = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [[1,3],[2,4]]
<strong>Đầu ra:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>costs.length == n</code></li>
	<li><code>costs[i].length == k</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>2 &lt;= k &lt;= 20</code></li>
	<li><code>1 &lt;= costs[i][j] &lt;= 20</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán với độ phức tạp thời gian <code>O(nk)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai nhà liền kề không thể cùng màu, và với $k$ màu thì không thể liệt kê mọi cách sơn. Chi phí tốt nhất khi sơn nhà $i$ màu $j$ bằng $costs[i][j]$ cộng với chi phí nhỏ nhất trong các màu khác để sơn nhà $i-1$.
>
> Mảng $f$ lưu chi phí của hàng trước; với mỗi nhà và mỗi màu, ta duyệt các màu khác ở hàng trước để tìm chi phí nhỏ nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCostII(self, costs: List[List[int]]) -> int:
        n, k = len(costs), len(costs[0])
        f = costs[0][:]
        for i in range(1, n):
            g = costs[i][:]
            for j in range(k):
                t = min(f[h] for h in range(k) if h != j)
                g[j] += t
            f = g
        return min(f)
```

#### Java

```java
class Solution {
    public int minCostII(int[][] costs) {
        int n = costs.length, k = costs[0].length;
        int[] f = costs[0].clone();
        for (int i = 1; i < n; ++i) {
            int[] g = costs[i].clone();
            for (int j = 0; j < k; ++j) {
                int t = Integer.MAX_VALUE;
                for (int h = 0; h < k; ++h) {
                    if (h != j) {
                        t = Math.min(t, f[h]);
                    }
                }
                g[j] += t;
            }
            f = g;
        }
        return Arrays.stream(f).min().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCostII(vector<vector<int>>& costs) {
        int n = costs.size(), k = costs[0].size();
        vector<int> f = costs[0];
        for (int i = 1; i < n; ++i) {
            vector<int> g = costs[i];
            for (int j = 0; j < k; ++j) {
                int t = INT_MAX;
                for (int h = 0; h < k; ++h) {
                    if (h != j) {
                        t = min(t, f[h]);
                    }
                }
                g[j] += t;
            }
            f = move(g);
        }
        return *min_element(f.begin(), f.end());
    }
};
```

#### Go

```go
func minCostII(costs [][]int) int {
	n, k := len(costs), len(costs[0])
	f := cp(costs[0])
	for i := 1; i < n; i++ {
		g := cp(costs[i])
		for j := 0; j < k; j++ {
			t := math.MaxInt32
			for h := 0; h < k; h++ {
				if h != j && t > f[h] {
					t = f[h]
				}
			}
			g[j] += t
		}
		f = g
	}
	return slices.Min(f)
}

func cp(arr []int) []int {
	t := make([]int, len(arr))
	copy(t, arr)
	return t
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
