---
comments: true
difficulty: Medium
rating: 1718
source: Biweekly Contest 86 Q3
tags:
    - Bit Manipulation
    - Array
    - Backtracking
    - Enumeration
    - Matrix
---

<!-- problem:start -->

# [2397. Maximum Rows Covered by Columns](https://leetcode.com/problems/maximum-rows-covered-by-columns)

[中文文档](/solution/2300-2399/2397.Maximum%20Rows%20Covered%20by%20Columns/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận nhị phân <code>matrix</code> kích thước <code>m x n</code> và một số nguyên <code>numSelect</code>.</p>

<p>Mục tiêu của bạn là chọn chính xác <code>numSelect</code> cột <strong>khác nhau</strong> từ <code>matrix</code> sao cho số hàng được bao phủ là nhiều nhất có thể.</p>

<p>Một hàng được xem là <strong>được bao phủ</strong> nếu tất cả các <code>1</code> trong hàng đó đều nằm trong các cột mà bạn đã chọn. Nếu một hàng không có bất kỳ <code>1</code> nào, hàng đó cũng được xem là được bao phủ.</p>

<p>Cụ thể hơn, gọi <code>selected = {c<sub>1</sub>, c<sub>2</sub>, ...., c<sub>numSelect</sub>}</code> là tập các cột được bạn chọn. Một hàng <code>i</code> được <strong>bao phủ</strong> bởi <code>selected</code> nếu:</p>

<ul>
	<li>Với mọi ô mà <code>matrix[i][j] == 1</code>, cột <code>j</code> thuộc <code>selected</code>.</li>
	<li>Hoặc hàng <code>i</code> không có ô nào có giá trị <code>1</code>.</li>
</ul>

<p>Trả về số hàng <strong>được bao phủ</strong> <strong>lớn nhất</strong> bởi một tập gồm <code>numSelect</code> cột.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2397.Maximum%20Rows%20Covered%20by%20Columns/images/rowscovered.png" style="width: 240px; height: 400px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[0,0,0],[1,0,1],[0,1,1],[0,0,1]], numSelect = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách để bao phủ 3 hàng được minh họa trong sơ đồ phía trên.<br />
Ta chọn s = {0, 2}.<br />
- Hàng 0 được bao phủ vì hàng này không có số 1 nào.<br />
- Hàng 1 được bao phủ vì các cột có giá trị 1, tức là 0 và 2, đều có trong s.<br />
- Hàng 2 không được bao phủ vì matrix[2][1] == 1 nhưng 1 không có trong s.<br />
- Hàng 3 được bao phủ vì matrix[2][2] == 1 và 2 có trong s.<br />
Do đó, ta có thể bao phủ ba hàng.<br />
Lưu ý rằng s = {1, 2} cũng bao phủ 3 hàng, nhưng có thể chứng minh rằng không thể bao phủ nhiều hơn ba hàng.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2397.Maximum%20Rows%20Covered%20by%20Columns/images/rowscovered2.png" style="height: 250px; width: 84px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[1],[0]], numSelect = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn cột duy nhất sẽ bao phủ cả hai hàng vì toàn bộ ma trận được chọn.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 12</code></li>
	<li><code>matrix[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>1 &lt;= numSelect&nbsp;&lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Chọn chính xác $numSelect$ cột để bao phủ nhiều hàng nhất có thể (mọi số $1$ trong hàng đều nằm ở một cột được chọn). Vì $n \le 12$, ta có thể biểu diễn các tập con của các cột bằng một bit mask.
>
> Biểu diễn mỗi hàng bằng một mask. Liệt kê các mask cột có popcount phù hợp và đếm những hàng thỏa mãn $row \land mask = row$.

<!-- thinking:end -->

Trước hết, ta chuyển mỗi hàng của ma trận thành một số nhị phân và lưu vào mảng $rows$. Ở đây, $rows[i]$ biểu diễn số nhị phân tương ứng với hàng thứ $i$, còn bit thứ $j$ của số nhị phân $rows[i]$ biểu diễn giá trị tại hàng thứ $i$ và cột thứ $j$.

Tiếp theo, ta liệt kê tất cả $2^n$ phương án chọn cột, trong đó $n$ là số cột của ma trận. Với mỗi phương án chọn cột, ta kiểm tra xem đã chọn `numSelect` cột hay chưa. Nếu chưa, ta bỏ qua phương án đó. Ngược lại, ta đếm số hàng trong ma trận được các cột đã chọn bao phủ, tức là số các số nhị phân $rows[i]$ bằng phép AND bit giữa $rows[i]$ và phương án chọn cột $mask$. Sau đó, ta cập nhật số hàng lớn nhất.

Độ phức tạp thời gian là $O(2^n \times m)$, và độ phức tạp không gian là $O(m)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumRows(self, matrix: List[List[int]], numSelect: int) -> int:
        rows = []
        for row in matrix:
            mask = reduce(or_, (1 << j for j, x in enumerate(row) if x), 0)
            rows.append(mask)

        ans = 0
        for mask in range(1 << len(matrix[0])):
            if mask.bit_count() != numSelect:
                continue
            t = sum((x & mask) == x for x in rows)
            ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public int maximumRows(int[][] matrix, int numSelect) {
        int m = matrix.length, n = matrix[0].length;
        int[] rows = new int[m];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] == 1) {
                    rows[i] |= 1 << j;
                }
            }
        }
        int ans = 0;
        for (int mask = 1; mask < 1 << n; ++mask) {
            if (Integer.bitCount(mask) != numSelect) {
                continue;
            }
            int t = 0;
            for (int x : rows) {
                if ((x & mask) == x) {
                    ++t;
                }
            }
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumRows(vector<vector<int>>& matrix, int numSelect) {
        int m = matrix.size(), n = matrix[0].size();
        int rows[m];
        memset(rows, 0, sizeof(rows));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j]) {
                    rows[i] |= 1 << j;
                }
            }
        }
        int ans = 0;
        for (int mask = 1; mask < 1 << n; ++mask) {
            if (__builtin_popcount(mask) != numSelect) {
                continue;
            }
            int t = 0;
            for (int x : rows) {
                t += (x & mask) == x;
            }
            ans = max(ans, t);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumRows(matrix [][]int, numSelect int) (ans int) {
	m, n := len(matrix), len(matrix[0])
	rows := make([]int, m)
	for i, row := range matrix {
		for j, x := range row {
			if x == 1 {
				rows[i] |= 1 << j
			}
		}
	}
	for mask := 1; mask < 1<<n; mask++ {
		if bits.OnesCount(uint(mask)) != numSelect {
			continue
		}
		t := 0
		for _, x := range rows {
			if (x & mask) == x {
				t++
			}
		}
		if ans < t {
			ans = t
		}
	}
	return
}
```

#### TypeScript

```ts
function maximumRows(matrix: number[][], numSelect: number): number {
    const [m, n] = [matrix.length, matrix[0].length];
    const rows: number[] = Array(m).fill(0);
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (matrix[i][j]) {
                rows[i] |= 1 << j;
            }
        }
    }
    let ans = 0;
    for (let mask = 1; mask < 1 << n; ++mask) {
        if (bitCount(mask) !== numSelect) {
            continue;
        }
        let t = 0;
        for (const x of rows) {
            if ((x & mask) === x) {
                ++t;
            }
        }
        ans = Math.max(ans, t);
    }
    return ans;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
