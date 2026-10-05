---
comments: true
difficulty: Medium
rating: 1947
source: Weekly Contest 491 Q3
---

<!-- problem:start -->

# [3858. Minimum Bitwise OR From Grid](https://leetcode.com/problems/minimum-bitwise-or-from-grid)

[中文文档](/solution/3800-3899/3858.Minimum%20Bitwise%20OR%20From%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>grid</code> có kích thước <code>m x n</code>.</p>

<p>Bạn phải chọn <strong>chính xác một</strong> số nguyên từ mỗi hàng của grid.</p>

<p>Trả về một số nguyên biểu thị <strong>phép OR bitwise nhỏ nhất</strong> có thể có của các số nguyên được chọn từ mỗi hàng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,5],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn 1 từ hàng đầu tiên và 2 từ hàng thứ hai.</li>
	<li>Phép OR bitwise của <code>1 | 2 = 3</code>​​​​​​​, đây là giá trị nhỏ nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[3,5],[6,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn 5 từ hàng đầu tiên và 4 từ hàng thứ hai.</li>
	<li>Phép OR bitwise của <code>5 | 4 = 5</code>​​​​​​​, đây là giá trị nhỏ nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[7,9,8]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn 7 sẽ cho phép OR bitwise nhỏ nhất.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m == grid.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= n == grid[i].length &lt;= 10<sup>5</sup></code></li>
	<li><code>m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>5</sup>​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chọn một số ở mỗi hàng để tối thiểu hóa phép OR bitwise. Có nhiều nhất $10^5$ ô, nên không thể liệt kê các cách chọn.
>
> Ta muốn các bit cao của phép OR giữ bằng $0$. Thử các bit từ cao xuống thấp: khi các bit cao hơn đã được cố định, kiểm tra xem trong mỗi hàng còn giá trị nào tương thích với các bit đó hay không (các bit thấp hơn được tự do).
>
> Với mặt nạ thử $\textit{ans} \mid (2^i-1)$, nếu mỗi hàng có một $x$ được mặt nạ bao phủ, bit hiện tại có thể giữ bằng $0$; nếu không, bit đó bắt buộc phải được đặt thành 1.
>
> Điền các bit từ cao xuống thấp sẽ cho phép OR nhỏ nhất.

<!-- thinking:end -->
<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOR(self, grid: List[List[int]]) -> int:
        mx = max(map(max, grid))
        m = mx.bit_length()
        ans = 0
        for i in range(m - 1, -1, -1):
            mask = ans | ((1 << i) - 1)
            for row in grid:
                found = False
                for x in row:
                    if (x | mask) == mask:
                        found = True
                        break
                if not found:
                    ans |= 1 << i
                    break
        return ans
```

#### Java

```java
class Solution {
    public int minimumOR(int[][] grid) {
        int mx = 0;
        for (int[] row : grid) {
            for (int x : row) {
                mx = Math.max(mx, x);
            }
        }

        int m = 32 - Integer.numberOfLeadingZeros(mx);
        int ans = 0;

        for (int i = m - 1; i >= 0; i--) {
            int mask = ans | ((1 << i) - 1);
            for (int[] row : grid) {
                boolean found = false;
                for (int x : row) {
                    if ((x | mask) == mask) {
                        found = true;
                        break;
                    }
                }
                if (!found) {
                    ans |= 1 << i;
                    break;
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
    int minimumOR(vector<vector<int>>& grid) {
        int mx = 0;
        for (auto& row : grid) {
            mx = max(mx, ranges::max(row));
        }

        int m = 32 - __builtin_clz(mx);
        int ans = 0;

        for (int i = m - 1; i >= 0; i--) {
            int mask = ans | ((1 << i) - 1);
            for (auto& row : grid) {
                bool found = false;
                for (int x : row) {
                    if ((x | mask) == mask) {
                        found = true;
                        break;
                    }
                }
                if (!found) {
                    ans |= 1 << i;
                    break;
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func minimumOR(grid [][]int) int {
	mx := 0
	for _, row := range grid {
		mx = max(mx, slices.Max(row))
	}

	m := bits.Len(uint(mx))
	ans := 0

	for i := m - 1; i >= 0; i-- {
		mask := ans | ((1 << i) - 1)
		for _, row := range grid {
			found := false
			for _, x := range row {
				if (x | mask) == mask {
					found = true
					break
				}
			}
			if !found {
				ans |= 1 << i
				break
			}
		}
	}

	return ans
}
```

#### TypeScript

```ts
function minimumOR(grid: number[][]): number {
    let mx = 0;
    for (const row of grid) {
        mx = Math.max(mx, Math.max(...row));
    }

    const m = mx === 0 ? 0 : 32 - Math.clz32(mx);
    let ans = 0;

    for (let i = m - 1; i >= 0; i--) {
        const mask = ans | ((1 << i) - 1);
        for (const row of grid) {
            let found = false;
            for (const x of row) {
                if ((x | mask) === mask) {
                    found = true;
                    break;
                }
            }
            if (!found) {
                ans |= 1 << i;
                break;
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
