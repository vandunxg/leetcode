---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Interactive
    - Matrix
---

<!-- problem:start -->

# [1428. Leftmost Column with at Least a One 🔒](https://leetcode.com/problems/leftmost-column-with-at-least-a-one)

[中文文档](/solution/1400-1499/1428.Leftmost%20Column%20with%20at%20Least%20a%20One/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Ma trận nhị phân được sắp xếp theo hàng</strong> là ma trận mà mọi phần tử đều là <code>0</code> hoặc <code>1</code>, và mỗi hàng được sắp xếp theo thứ tự không giảm.</p>

<p>Cho một <strong>ma trận nhị phân được sắp xếp theo hàng</strong> <code>binaryMatrix</code>, hãy trả về <em>chỉ số (đánh số từ <code>0</code>) của <strong>cột ngoài cùng bên trái</strong> chứa một giá trị 1</em>. Nếu không tồn tại chỉ số như vậy, trả về <code>-1</code>.</p>

<p><strong>Bạn không thể truy cập trực tiếp vào Binary Matrix.</strong> Bạn chỉ có thể truy cập ma trận thông qua interface <code>BinaryMatrix</code>:</p>

<ul>
	<li><code>BinaryMatrix.get(row, col)</code> trả về phần tử của ma trận tại chỉ số <code>(row, col)</code> (đánh số từ <code>0</code>).</li>
	<li><code>BinaryMatrix.dimensions()</code> trả về kích thước của ma trận dưới dạng danh sách gồm 2 phần tử <code>[rows, cols]</code>, nghĩa là ma trận có kích thước <code>rows x cols</code>.</li>
</ul>

<p>Các bài nộp thực hiện hơn <code>1000</code> lần gọi <code>BinaryMatrix.get</code> sẽ bị đánh giá là <em>Wrong Answer</em>. Ngoài ra, mọi lời giải cố gắng qua mặt trình chấm sẽ bị loại.</p>

<p>Để tùy chỉnh việc kiểm thử, đầu vào sẽ là toàn bộ ma trận nhị phân <code>mat</code>. Bạn sẽ không được truy cập trực tiếp vào ma trận nhị phân.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1428.Leftmost%20Column%20with%20at%20Least%20a%20One/images/untitled-diagram-5.jpg" style="width: 81px; height: 81px;" />
<pre>
<strong>Input:</strong> mat = [[0,0],[1,1]]
<strong>Output:</strong> 0
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1428.Leftmost%20Column%20with%20at%20Least%20a%20One/images/untitled-diagram-4.jpg" style="width: 81px; height: 81px;" />
<pre>
<strong>Input:</strong> mat = [[0,0],[0,1]]
<strong>Output:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1428.Leftmost%20Column%20with%20at%20Least%20a%20One/images/untitled-diagram-3.jpg" style="width: 81px; height: 81px;" />
<pre>
<strong>Input:</strong> mat = [[0,0],[0,0]]
<strong>Output:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>rows == mat.length</code></li>
	<li><code>cols == mat[i].length</code></li>
	<li><code>1 &lt;= rows, cols &lt;= 100</code></li>
	<li><code>mat[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>mat[i]</code> được sắp xếp theo thứ tự không giảm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Vì mỗi hàng được sắp xếp không giảm và số lần gọi `get` bị giới hạn, chúng ta không thể duyệt toàn bộ ma trận. Có thể tìm số 1 đầu tiên trong một hàng bằng tìm kiếm nhị phân với $O(\log n)$ lần gọi.
>
> Đáp án là cột nhỏ nhất trong các cột như vậy của mọi hàng, hoặc $-1$ nếu không có hàng nào chứa số 1.

<!-- thinking:end -->

Trước tiên, chúng ta gọi `BinaryMatrix.dimensions()` để lấy số hàng $m$ và số cột $n$ của ma trận. Sau đó, với mỗi hàng, chúng ta dùng tìm kiếm nhị phân để tìm chỉ số cột $j$ chứa số 1 ngoài cùng bên trái. Giá trị $j$ nhỏ nhất thỏa mãn trong tất cả các hàng là đáp án. Nếu không tồn tại cột như vậy, trả về $-1$.

Độ phức tạp thời gian là $O(m \times \log n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Chúng ta cần duyệt qua từng hàng và dùng tìm kiếm nhị phân trong mỗi hàng, với độ phức tạp thời gian là $O(\log n)$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# """
# This is BinaryMatrix's API interface.
# You should not implement it, or speculate about its implementation
# """
# class BinaryMatrix(object):
#    def get(self, row: int, col: int) -> int:
#    def dimensions(self) -> list[]:


class Solution:
    def leftMostColumnWithOne(self, binaryMatrix: "BinaryMatrix") -> int:
        m, n = binaryMatrix.dimensions()
        ans = n
        for i in range(m):
            j = bisect_left(range(n), 1, key=lambda k: binaryMatrix.get(i, k))
            ans = min(ans, j)
        return -1 if ans >= n else ans
```

#### Java

```java
/**
 * // This is the BinaryMatrix's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface BinaryMatrix {
 *     public int get(int row, int col) {}
 *     public List<Integer> dimensions {}
 * };
 */

class Solution {
    public int leftMostColumnWithOne(BinaryMatrix binaryMatrix) {
        List<Integer> e = binaryMatrix.dimensions();
        int m = e.get(0), n = e.get(1);
        int ans = n;
        for (int i = 0; i < m; ++i) {
            int l = 0, r = n;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (binaryMatrix.get(i, mid) == 1) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            ans = Math.min(ans, l);
        }
        return ans >= n ? -1 : ans;
    }
}
```

#### C++

```cpp
/**
 * // This is the BinaryMatrix's API interface.
 * // You should not implement it, or speculate about its implementation
 * class BinaryMatrix {
 *   public:
 *     int get(int row, int col);
 *     vector<int> dimensions();
 * };
 */

class Solution {
public:
    int leftMostColumnWithOne(BinaryMatrix& binaryMatrix) {
        auto e = binaryMatrix.dimensions();
        int m = e[0], n = e[1];
        int ans = n;
        for (int i = 0; i < m; ++i) {
            int l = 0, r = n;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (binaryMatrix.get(i, mid)) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            ans = min(ans, l);
        }
        return ans >= n ? -1 : ans;
    }
};
```

#### Go

```go
/**
 * // This is the BinaryMatrix's API interface.
 * // You should not implement it, or speculate about its implementation
 * type BinaryMatrix struct {
 *     Get func(int, int) int
 *     Dimensions func() []int
 * }
 */

func leftMostColumnWithOne(binaryMatrix BinaryMatrix) int {
	e := binaryMatrix.Dimensions()
	m, n := e[0], e[1]
	ans := n
	for i := 0; i < m; i++ {
		l, r := 0, n
		for l < r {
			mid := (l + r) >> 1
			if binaryMatrix.Get(i, mid) == 1 {
				r = mid
			} else {
				l = mid + 1
			}
		}
		ans = min(ans, l)
	}
	if ans >= n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
/**
 * // This is the BinaryMatrix's API interface.
 * // You should not implement it, or speculate about its implementation
 * class BinaryMatrix {
 *      get(row: number, col: number): number {}
 *
 *      dimensions(): number[] {}
 * }
 */

function leftMostColumnWithOne(binaryMatrix: BinaryMatrix) {
    const [m, n] = binaryMatrix.dimensions();
    let ans = n;
    for (let i = 0; i < m; ++i) {
        let [l, r] = [0, n];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (binaryMatrix.get(i, mid) === 1) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        ans = Math.min(ans, l);
    }
    return ans >= n ? -1 : ans;
}
```

#### Rust

```rust
/**
 * // This is the BinaryMatrix's API interface.
 * // You should not implement it, or speculate about its implementation
 *  struct BinaryMatrix;
 *  impl BinaryMatrix {
 *     fn get(row: i32, col: i32) -> i32;
 *     fn dimensions() -> Vec<i32>;
 *  };
 */

impl Solution {
    pub fn left_most_column_with_one(binaryMatrix: &BinaryMatrix) -> i32 {
        let e = binaryMatrix.dimensions();
        let m = e[0] as usize;
        let n = e[1] as usize;
        let mut ans = n;

        for i in 0..m {
            let (mut l, mut r) = (0, n);
            while l < r {
                let mid = (l + r) / 2;
                if binaryMatrix.get(i as i32, mid as i32) == 1 {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            ans = ans.min(l);
        }

        if ans >= n {
            -1
        } else {
            ans as i32
        }
    }
}
```

#### C#

```cs
/**
 * // This is BinaryMatrix's API interface.
 * // You should not implement it, or speculate about its implementation
 * class BinaryMatrix {
 *     public int Get(int row, int col) {}
 *     public IList<int> Dimensions() {}
 * }
 */

public class Solution {
    public int LeftMostColumnWithOne(BinaryMatrix binaryMatrix) {
        var e = binaryMatrix.Dimensions();
        int m = e[0], n = e[1];
        int ans = n;
        for (int i = 0; i < m; ++i) {
            int l = 0, r = n;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (binaryMatrix.Get(i, mid) == 1) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            ans = Math.Min(ans, l);
        }
        return ans >= n ? -1 : ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
