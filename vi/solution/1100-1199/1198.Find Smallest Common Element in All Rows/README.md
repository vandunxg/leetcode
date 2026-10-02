---
comments: true
difficulty: Medium
rating: 1401
source: Biweekly Contest 9 Q3
tags:
    - Array
    - Hash Table
    - Binary Search
    - Counting
    - Matrix
---

<!-- problem:start -->

# [1198. Find Smallest Common Element in All Rows 🔒](https://leetcode.com/problems/find-smallest-common-element-in-all-rows)

[中文文档](/solution/1100-1199/1198.Find%20Smallest%20Common%20Element%20in%20All%20Rows/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> <code>mat</code> mà mỗi hàng được sắp xếp theo thứ tự <strong>tăng dần</strong> <strong>nghiêm ngặt</strong>, hãy trả về <em><strong>phần tử chung nhỏ nhất</strong> trong tất cả các hàng</em>.</p>

<p>Nếu không có phần tử chung, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[1,2,3,4,5],[2,4,5,8,10],[3,5,7,9,11],[1,3,5,7,9]]
<strong>Đầu ra:</strong> 5
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[1,2,3],[2,3,4],[2,3,5]]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 10<sup>4</sup></code></li>
	<li><code>mat[i]</code> được sắp xếp theo thứ tự tăng dần nghiêm ngặt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm tần suất

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng tăng dần nghiêm ngặt nên mỗi giá trị xuất hiện nhiều nhất một lần trong mỗi hàng. Ta đếm khi duyệt; giá trị có số lần xuất hiện bằng số hàng sẽ có mặt trong mọi hàng. Vì duyệt các giá trị nhỏ trước, giá trị đầu tiên đạt số lần đếm đó là phần tử chung nhỏ nhất; nếu không có thì trả về $-1$.

<!-- thinking:end -->

Ta dùng mảng $cnt$ có độ dài $10001$ để đếm tần suất của từng số. Ta lần lượt duyệt các số trong ma trận và tăng tần suất tương ứng. Khi tần suất của một số bằng số hàng trong ma trận, nghĩa là số đó xuất hiện trong mọi hàng và do đó là phần tử chung nhỏ nhất. Ta trả về số này.

Nếu duyệt xong mà không tìm được phần tử chung nhỏ nhất, ta trả về $-1$.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(10^4)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestCommonElement(self, mat: List[List[int]]) -> int:
        cnt = Counter()
        for row in mat:
            for x in row:
                cnt[x] += 1
                if cnt[x] == len(mat):
                    return x
        return -1
```

#### Java

```java
class Solution {
    public int smallestCommonElement(int[][] mat) {
        int[] cnt = new int[10001];
        for (var row : mat) {
            for (int x : row) {
                if (++cnt[x] == mat.length) {
                    return x;
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestCommonElement(vector<vector<int>>& mat) {
        int cnt[10001]{};
        for (auto& row : mat) {
            for (int x : row) {
                if (++cnt[x] == mat.size()) {
                    return x;
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func smallestCommonElement(mat [][]int) int {
	cnt := [10001]int{}
	for _, row := range mat {
		for _, x := range row {
			cnt[x]++
			if cnt[x] == len(mat) {
				return x
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function smallestCommonElement(mat: number[][]): number {
    const cnt: number[] = new Array(10001).fill(0);
    for (const row of mat) {
        for (const x of row) {
            if (++cnt[x] == mat.length) {
                return x;
            }
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
