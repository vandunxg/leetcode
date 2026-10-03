---
comments: true
difficulty: Medium
rating: 1648
source: Biweekly Contest 59 Q2
tags:
    - Greedy
    - Array
    - Matrix
---

<!-- problem:start -->

# [1975. Maximum Matrix Sum](https://leetcode.com/problems/maximum-matrix-sum)

[中文文档](/solution/1900-1999/1975.Maximum%20Matrix%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên kích thước <code>n x n</code>, được biểu diễn bởi <code>matrix</code>. Bạn có thể thực hiện thao tác sau <strong>bất kỳ số lần nào:</strong></p>

<ul>
	<li>Chọn bất kỳ hai phần tử <strong>kề nhau</strong> nào trong <code>matrix</code> và <strong>nhân</strong> mỗi phần tử với <code>-1</code>.</li>
</ul>

<p>Hai phần tử được xem là <strong>kề nhau</strong> khi và chỉ khi chúng có chung một <strong>cạnh</strong>.</p>

<p>Mục tiêu của bạn là <strong>tối đa hóa</strong> tổng các phần tử của ma trận. Trả về <em><strong>tổng lớn nhất</strong> của ma trận bằng thao tác được mô tả ở trên.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1975.Maximum%20Matrix%20Sum/images/pc79-q2ex1.png" style="width: 401px; height: 81px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,-1],[-1,1]]
<strong>Đầu ra:</strong> 4
<b>Giải thích:</b> Ta có thể thực hiện các bước sau để đạt tổng bằng 4:
- Nhân 2 phần tử trong hàng đầu tiên với -1.
- Nhân 2 phần tử trong cột đầu tiên với -1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1975.Maximum%20Matrix%20Sum/images/pc79-q2ex2.png" style="width: 321px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,2,3],[-1,-2,-3],[1,2,3]]
<strong>Đầu ra:</strong> 16
<b>Giải thích:</b> Ta có thể thực hiện bước sau để đạt tổng bằng 16:
- Nhân 2 phần tử cuối cùng trong hàng thứ hai với -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == matrix.length == matrix[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 250</code></li>
	<li><code>-10<sup>5</sup> &lt;= matrix[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Đổi dấu của hai phần tử kề nhau sẽ làm dấu âm di chuyển trong lưới. Có thể loại bỏ số 0 hoặc một số chẵn dấu âm; nếu số lượng dấu âm là lẻ thì cuối cùng còn lại đúng một dấu âm.
>
> Đáp án là tổng các giá trị tuyệt đối, trừ đi hai lần giá trị tuyệt đối nhỏ nhất khi số lượng dấu âm là lẻ.

<!-- thinking:end -->

Nếu ma trận có số 0 hoặc số lượng số âm trong ma trận là số chẵn, tổng lớn nhất là tổng các giá trị tuyệt đối của tất cả phần tử trong ma trận.

Ngược lại, nếu số lượng số âm trong ma trận là số lẻ, cuối cùng sẽ còn lại một số âm. Ta chọn số có giá trị tuyệt đối nhỏ nhất và để phần tử đó mang dấu âm để tổng cuối cùng đạt lớn nhất.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxMatrixSum(self, matrix: List[List[int]]) -> int:
        mi = inf
        s = cnt = 0
        for row in matrix:
            for x in row:
                cnt += x < 0
                y = abs(x)
                mi = min(mi, y)
                s += y
        return s if cnt % 2 == 0 else s - mi * 2
```

#### Java

```java
class Solution {
    public long maxMatrixSum(int[][] matrix) {
        long s = 0;
        int mi = 1 << 30, cnt = 0;
        for (var row : matrix) {
            for (int x : row) {
                cnt += x < 0 ? 1 : 0;
                int y = Math.abs(x);
                mi = Math.min(mi, y);
                s += y;
            }
        }
        return cnt % 2 == 0 ? s : s - mi * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxMatrixSum(vector<vector<int>>& matrix) {
        long long s = 0;
        int mi = 1 << 30, cnt = 0;
        for (const auto& row : matrix) {
            for (int x : row) {
                cnt += x < 0 ? 1 : 0;
                int y = abs(x);
                mi = min(mi, y);
                s += y;
            }
        }
        return cnt % 2 == 0 ? s : s - mi * 2;
    }
};
```

#### Go

```go
func maxMatrixSum(matrix [][]int) int64 {
	var s int64
	mi, cnt := 1<<30, 0
	for _, row := range matrix {
		for _, x := range row {
			if x < 0 {
				cnt++
				x = -x
			}
			mi = min(mi, x)
			s += int64(x)
		}
	}
	if cnt%2 == 0 {
		return s
	}
	return s - int64(mi*2)
}
```

#### TypeScript

```ts
function maxMatrixSum(matrix: number[][]): number {
    let [s, cnt, mi] = [0, 0, Infinity];
    for (const row of matrix) {
        for (const x of row) {
            if (x < 0) {
                ++cnt;
            }
            const y = Math.abs(x);
            s += y;
            mi = Math.min(mi, y);
        }
    }
    return cnt % 2 === 0 ? s : s - 2 * mi;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_matrix_sum(matrix: Vec<Vec<i32>>) -> i64 {
        let mut s = 0;
        let mut mi = i32::MAX;
        let mut cnt = 0;
        for row in matrix {
            for &x in row.iter() {
                cnt += if x < 0 { 1 } else { 0 };
                let y = x.abs();
                mi = mi.min(y);
                s += y as i64;
            }
        }
        if cnt % 2 == 0 {
            s
        } else {
            s - (mi as i64 * 2)
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 * @return {number}
 */
var maxMatrixSum = function (matrix) {
    let [s, cnt, mi] = [0, 0, Infinity];
    for (const row of matrix) {
        for (const x of row) {
            if (x < 0) {
                ++cnt;
            }
            const y = Math.abs(x);
            s += y;
            mi = Math.min(mi, y);
        }
    }
    return cnt % 2 === 0 ? s : s - 2 * mi;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
