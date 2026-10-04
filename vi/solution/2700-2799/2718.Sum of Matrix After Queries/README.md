---
comments: true
difficulty: Medium
rating: 1768
source: Weekly Contest 348 Q3
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2718. Sum of Matrix After Queries](https://leetcode.com/problems/sum-of-matrix-after-queries)

[中文文档](/solution/2700-2799/2718.Sum%20of%20Matrix%20After%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và <strong>mảng 2 chiều</strong> được <strong>đánh chỉ số từ 0</strong> <code>queries</code>, trong đó <code>queries[i] = [type<sub>i</sub>, index<sub>i</sub>, val<sub>i</sub>]</code>.</p>

<p>Ban đầu, có một ma trận <strong>0-indexed</strong> kích thước <code>n x n</code> được điền toàn bộ bằng <code>0</code>. Với mỗi truy vấn, bạn phải thực hiện một trong các thay đổi sau:</p>

<ul>
	<li>nếu <code>type<sub>i</sub> == 0</code>, đặt các giá trị trong hàng có <code>index<sub>i</sub></code> bằng <code>val<sub>i</sub></code>, ghi đè mọi giá trị trước đó.</li>
	<li>nếu <code>type<sub>i</sub> == 1</code>, đặt các giá trị trong cột có <code>index<sub>i</sub></code> bằng <code>val<sub>i</sub></code>, ghi đè mọi giá trị trước đó.</li>
</ul>

<p>Trả về <em>tổng các số nguyên trong ma trận sau khi áp dụng tất cả truy vấn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2718.Sum%20of%20Matrix%20After%20Queries/images/exm1.png" style="width: 681px; height: 161px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, queries = [[0,0,1],[1,2,2],[0,2,3],[1,0,4]]
<strong>Đầu ra:</strong> 23
<strong>Giải thích:</strong> Hình ảnh trên mô tả ma trận sau mỗi truy vấn. Tổng của ma trận sau khi áp dụng tất cả truy vấn là 23.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2718.Sum%20of%20Matrix%20After%20Queries/images/exm2.png" style="width: 681px; height: 331px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, queries = [[0,0,4],[0,1,2],[1,0,1],[0,2,3],[1,2,1]]
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong> Hình ảnh trên mô tả ma trận sau mỗi truy vấn. Tổng của ma trận sau khi áp dụng tất cả truy vấn là 17.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>queries[i].length == 3</code></li>
	<li><code>0 &lt;= type<sub>i</sub> &lt;= 1</code></li>
	<li><code>0 &lt;= index<sub>i</sub>&nbsp;&lt; n</code></li>
	<li><code>0 &lt;= val<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta thực hiện các phép gán hàng và cột trên ma trận $n\times n$ và cần tính tổng cuối cùng. Vì $n$ có thể lên tới $10^4$, không thể tạo toàn bộ ma trận; chỉ phép ghi cuối cùng lên một hàng hoặc cột còn hiệu lực.
>
> Hãy duyệt các truy vấn từ cuối lên đầu. Lần đầu tiên một hàng (hoặc cột) xuất hiện chính là phép ghi còn hiệu lực, và nó bao phủ các ô chưa bị một cột (hoặc hàng) ở phía sau chiếm giữ. Hai set lưu các hàng và cột đã được xử lý; cộng $v$ nhân với số cột (hoặc hàng) còn lại.

<!-- thinking:end -->

Vì giá trị của mỗi hàng và cột phụ thuộc vào lần sửa đổi cuối cùng, ta có thể duyệt tất cả truy vấn theo thứ tự ngược lại và dùng các hash table $row$ và $col$ để ghi nhận những hàng và cột đã được sửa đổi.

Với mỗi truy vấn $(t, i, v)$:

- Nếu $t = 0$, ta kiểm tra xem hàng thứ $i$ đã được sửa đổi hay chưa. Nếu chưa, ta cộng $v \times (n - |col|)$ vào đáp án, trong đó $|col|$ là kích thước của $col$, sau đó thêm $i$ vào $row$.
- Nếu $t = 1$, ta kiểm tra xem cột thứ $i$ đã được sửa đổi hay chưa. Nếu chưa, ta cộng $v \times (n - |row|)$ vào đáp án, trong đó $|row|$ là kích thước của $row$, sau đó thêm $i$ vào $col$.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(n)$. Trong đó, $m$ là số lượng truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def matrixSumQueries(self, n: int, queries: List[List[int]]) -> int:
        row = set()
        col = set()
        ans = 0
        for t, i, v in queries[::-1]:
            if t == 0:
                if i not in row:
                    ans += v * (n - len(col))
                    row.add(i)
            else:
                if i not in col:
                    ans += v * (n - len(row))
                    col.add(i)
        return ans
```

#### Java

```java
class Solution {
    public long matrixSumQueries(int n, int[][] queries) {
        Set<Integer> row = new HashSet<>();
        Set<Integer> col = new HashSet<>();
        int m = queries.length;
        long ans = 0;
        for (int k = m - 1; k >= 0; --k) {
            var q = queries[k];
            int t = q[0], i = q[1], v = q[2];
            if (t == 0) {
                if (row.add(i)) {
                    ans += 1L * (n - col.size()) * v;
                }
            } else {
                if (col.add(i)) {
                    ans += 1L * (n - row.size()) * v;
                }
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
    long long matrixSumQueries(int n, vector<vector<int>>& queries) {
        unordered_set<int> row, col;
        reverse(queries.begin(), queries.end());
        long long ans = 0;
        for (auto& q : queries) {
            int t = q[0], i = q[1], v = q[2];
            if (t == 0) {
                if (!row.count(i)) {
                    ans += 1LL * (n - col.size()) * v;
                    row.insert(i);
                }
            } else {
                if (!col.count(i)) {
                    ans += 1LL * (n - row.size()) * v;
                    col.insert(i);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func matrixSumQueries(n int, queries [][]int) (ans int64) {
	row, col := map[int]bool{}, map[int]bool{}
	m := len(queries)
	for k := m - 1; k >= 0; k-- {
		t, i, v := queries[k][0], queries[k][1], queries[k][2]
		if t == 0 {
			if !row[i] {
				ans += int64(v * (n - len(col)))
				row[i] = true
			}
		} else {
			if !col[i] {
				ans += int64(v * (n - len(row)))
				col[i] = true
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function matrixSumQueries(n: number, queries: number[][]): number {
    const row: Set<number> = new Set();
    const col: Set<number> = new Set();
    let ans = 0;
    queries.reverse();
    for (const [t, i, v] of queries) {
        if (t === 0) {
            if (!row.has(i)) {
                ans += v * (n - col.size);
                row.add(i);
            }
        } else {
            if (!col.has(i)) {
                ans += v * (n - row.size);
                col.add(i);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
