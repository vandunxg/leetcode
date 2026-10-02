---
comments: true
difficulty: Hard
rating: 2133
source: Weekly Contest 187 Q4
tags:
    - Array
    - Binary Search
    - Matrix
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1439. Find the Kth Smallest Sum of a Matrix With Sorted Rows](https://leetcode.com/problems/find-the-kth-smallest-sum-of-a-matrix-with-sorted-rows)

[中文文档](/solution/1400-1499/1439.Find%20the%20Kth%20Smallest%20Sum%20of%20a%20Matrix%20With%20Sorted%20Rows/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <code>m x n</code> <code>mat</code> có các hàng được sắp xếp theo thứ tự không giảm và một số nguyên <code>k</code>.</p>

<p>Bạn được phép chọn <strong>chính xác một phần tử</strong> từ mỗi hàng để tạo thành một mảng.</p>

<p>Trả về <em>tổng của mảng nhỏ thứ </em><code>k<sup>th</sup></code><em> trong tất cả các mảng có thể tạo được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> mat = [[1,3,11],[2,4,6]], k = 5
<strong>Output:</strong> 7
<strong>Explanation:</strong> Chọn một phần tử từ mỗi hàng, k tổng nhỏ nhất đầu tiên là:
[1,2], [1,4], [3,2], [3,4], [1,6]. Trong đó tổng thứ 5 là 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> mat = [[1,3,11],[2,4,6]], k = 9
<strong>Output:</strong> 17
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> mat = [[1,10,10],[1,4,5],[2,3,6]], k = 7
<strong>Output:</strong> 9
<strong>Explanation:</strong> Chọn một phần tử từ mỗi hàng, k tổng nhỏ nhất đầu tiên là:
[1,1,2], [1,1,3], [1,4,2], [1,4,3], [1,1,6], [1,5,2], [1,5,3]. Trong đó tổng thứ 7 là 9.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat.length[i]</code></li>
	<li><code>1 &lt;= m, n &lt;= 40</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 5000</code></li>
	<li><code>1 &lt;= k &lt;= min(200, n<sup>m</sup>)</code></li>
	<li><code>mat[i]</code> là một mảng không giảm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chọn một giá trị từ mỗi hàng sẽ tạo ra $n^m$ tổng, nhưng $k\le 200$. Ta chỉ cần giữ lại $k$ tổng từng phần nhỏ nhất. Vì các hàng đã được sắp xếp, khi kết hợp với hàng tiếp theo, ta chỉ cần nhiều nhất $k$ phần tử đầu tiên của hàng đó.
>
> Gọi $pre$ là $k$ tổng nhỏ nhất tính đến thời điểm hiện tại. Cộng Descartes với hàng hiện tại, sắp xếp rồi giữ lại $k$ giá trị. Sau hàng cuối cùng, $pre[k-1]$ là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthSmallest(self, mat: List[List[int]], k: int) -> int:
        pre = [0]
        for cur in mat:
            pre = sorted(a + b for a in pre for b in cur[:k])[:k]
        return pre[-1]
```

#### Java

```java
class Solution {
    public int kthSmallest(int[][] mat, int k) {
        int m = mat.length, n = mat[0].length;
        List<Integer> pre = new ArrayList<>(k);
        List<Integer> cur = new ArrayList<>(n * k);
        pre.add(0);
        for (int[] row : mat) {
            cur.clear();
            for (int a : pre) {
                for (int b : row) {
                    cur.add(a + b);
                }
            }
            Collections.sort(cur);
            pre.clear();
            for (int i = 0; i < Math.min(k, cur.size()); ++i) {
                pre.add(cur.get(i));
            }
        }
        return pre.get(k - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kthSmallest(vector<vector<int>>& mat, int k) {
        int pre[k];
        int cur[mat[0].size() * k];
        memset(pre, 0, sizeof pre);
        int size = 1;
        for (auto& row : mat) {
            int i = 0;
            for (int j = 0; j < size; ++j) {
                for (int& v : row) {
                    cur[i++] = pre[j] + v;
                }
            }
            sort(cur, cur + i);
            size = min(i, k);
            for (int j = 0; j < size; ++j) {
                pre[j] = cur[j];
            }
        }
        return pre[k - 1];
    }
};
```

#### Go

```go
func kthSmallest(mat [][]int, k int) int {
	pre := []int{0}
	for _, row := range mat {
		cur := []int{}
		for _, a := range pre {
			for _, b := range row {
				cur = append(cur, a+b)
			}
		}
		sort.Ints(cur)
		pre = cur[:min(k, len(cur))]
	}
	return pre[k-1]
}
```

#### TypeScript

```ts
function kthSmallest(mat: number[][], k: number): number {
    let pre: number[] = [0];
    for (const cur of mat) {
        const next: number[] = [];
        for (const a of pre) {
            for (const b of cur) {
                next.push(a + b);
            }
        }
        pre = next.sort((a, b) => a - b).slice(0, k);
    }
    return pre[k - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
