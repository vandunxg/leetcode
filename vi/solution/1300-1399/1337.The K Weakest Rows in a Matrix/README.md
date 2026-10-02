---
comments: true
difficulty: Easy
rating: 1224
source: Weekly Contest 174 Q1
tags:
    - Array
    - Binary Search
    - Matrix
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1337. The K Weakest Rows in a Matrix](https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix)

[中文文档](/solution/1300-1399/1337.The%20K%20Weakest%20Rows%20in%20a%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>mat</code>, trong đó các giá trị <code>1</code> biểu thị binh lính và <code>0</code> biểu thị dân thường. Binh lính được xếp <strong>phía trước</strong> dân thường, tức là trong mỗi hàng, mọi <code>1</code> đều nằm bên <strong>trái</strong> mọi <code>0</code>.</p>

<p>Hàng <code>i</code> được xem là <strong>yếu hơn</strong> hàng <code>j</code> nếu thỏa một trong các điều kiện sau:</p>

<ul>
	<li>Số binh lính ở hàng <code>i</code> ít hơn số binh lính ở hàng <code>j</code>.</li>
	<li>Hai hàng có cùng số binh lính và <code>i &lt; j</code>.</li>
</ul>

<p>Trả về <em>chỉ số của <code>k</code> hàng <strong>yếu nhất</strong> trong ma trận, sắp xếp từ yếu nhất đến mạnh nhất</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> mat = 
[[1,1,0,0,0],
 [1,1,1,1,0],
 [1,0,0,0,0],
 [1,1,0,0,0],
 [1,1,1,1,1]], 
k = 3
<strong>Output:</strong> [2,0,3]
<strong>Giải thích:</strong> 
Số binh lính ở mỗi hàng là: 
- Hàng 0: 2 
- Hàng 1: 4 
- Hàng 2: 1 
- Hàng 3: 2 
- Hàng 4: 5 
Thứ tự các hàng từ yếu nhất đến mạnh nhất là [2,0,3,1,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> mat = 
[[1,0,0,0],
 [1,1,1,1],
 [1,0,0,0],
 [1,0,0,0]], 
k = 2
<strong>Output:</strong> [0,2]
<strong>Giải thích:</strong> 
Số binh lính ở mỗi hàng là: 
- Hàng 0: 1 
- Hàng 1: 4 
- Hàng 2: 1 
- Hàng 3: 1 
Thứ tự các hàng từ yếu nhất đến mạnh nhất là [0,2,3,1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>2 &lt;= n, m &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= m</code></li>
	<li><code>matrix[i][j]</code> chỉ có thể là 0 hoặc 1.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng được sắp xếp theo số binh lính, rồi theo chỉ số; ta lấy $k$ hàng đầu tiên. Vì các số 1 nằm bên trái các số 0, số binh lính chính là vị trí của số 0 đầu tiên. Tìm kiếm nhị phân số 0 trong hàng đảo ngược sẽ cho số lượng đó; sắp xếp chỉ số hàng theo số lượng rồi lấy $k$ phần tử đầu là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kWeakestRows(self, mat: List[List[int]], k: int) -> List[int]:
        m, n = len(mat), len(mat[0])
        ans = [n - bisect_right(row[::-1], 0) for row in mat]
        idx = list(range(m))
        idx.sort(key=lambda i: ans[i])
        return idx[:k]
```

#### Java

```java
class Solution {
    public int[] kWeakestRows(int[][] mat, int k) {
        int m = mat.length, n = mat[0].length;
        int[] res = new int[m];
        List<Integer> idx = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            idx.add(i);
            int[] row = mat[i];
            int left = 0, right = n;
            while (left < right) {
                int mid = (left + right) >> 1;
                if (row[mid] == 0) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            res[i] = left;
        }
        idx.sort(Comparator.comparingInt(a -> res[a]));
        int[] ans = new int[k];
        for (int i = 0; i < k; ++i) {
            ans[i] = idx.get(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int search(vector<int>& m) {
        int l = 0;
        int h = m.size() - 1;
        while (l <= h) {
            int mid = l + (h - l) / 2;
            if (m[mid] == 0)
                h = mid - 1;
            else
                l = mid + 1;
        }
        return l;
    }

    vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
        vector<pair<int, int>> p;
        vector<int> res;
        for (int i = 0; i < mat.size(); i++) {
            int count = search(mat[i]);
            p.push_back({count, i});
        }
        sort(p.begin(), p.end());
        for (int i = 0; i < k; i++) {
            res.push_back(p[i].second);
        }
        return res;
    }
};
```

#### Go

```go
func kWeakestRows(mat [][]int, k int) []int {
	m, n := len(mat), len(mat[0])
	res := make([]int, m)
	var idx []int
	for i, row := range mat {
		idx = append(idx, i)
		left, right := 0, n
		for left < right {
			mid := (left + right) >> 1
			if row[mid] == 0 {
				right = mid
			} else {
				left = mid + 1
			}
		}
		res[i] = left
	}
	sort.Slice(idx, func(i, j int) bool {
		return res[idx[i]] < res[idx[j]] || (res[idx[i]] == res[idx[j]] && idx[i] < idx[j])
	})
	return idx[:k]
}
```

#### TypeScript

```ts
function kWeakestRows(mat: number[][], k: number): number[] {
    let n = mat.length;
    let sumMap = mat.map((d, i) => [d.reduce((a, c) => a + c, 0), i]);
    let ans = [];
    // 冒泡排序
    for (let i = 0; i < k; i++) {
        for (let j = i; j < n; j++) {
            if (
                sumMap[j][0] < sumMap[i][0] ||
                (sumMap[j][0] == sumMap[i][0] && sumMap[i][1] > sumMap[j][1])
            ) {
                [sumMap[i], sumMap[j]] = [sumMap[j], sumMap[i]];
            }
        }
        ans.push(sumMap[i][1]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
