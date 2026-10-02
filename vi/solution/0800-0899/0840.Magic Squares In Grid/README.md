---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Math
    - Matrix
---

<!-- problem:start -->

# [840. Magic Squares In Grid](https://leetcode.com/problems/magic-squares-in-grid)

[中文文档](/solution/0800-0899/0840.Magic%20Squares%20In%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Ma phương</strong> <code>3 x 3</code> là ma trận <code>3 x 3</code> chứa các số khác nhau từ <strong>1 đến 9</strong>, sao cho tổng của mỗi hàng, mỗi cột và cả hai đường chéo đều bằng nhau.</p>

<p>Cho <code>grid</code> số nguyên kích thước <code>row x col</code>, có bao nhiêu ma phương con <code>3 x 3</code>?</p>

<p>Lưu ý: ma phương chỉ có thể chứa các số từ 1 đến 9, nhưng <code>grid</code> có thể chứa số lớn đến 15.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0840.Magic%20Squares%20In%20Grid/images/magic_main.jpg" style="width: 322px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[4,3,8,4],[9,5,1,9],[2,7,6,2]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích: </strong>
Ma trận con sau đây là ma phương 3 x 3:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0840.Magic%20Squares%20In%20Grid/images/magic_valid.jpg" style="width: 242px; height: 242px;" />
còn ma trận con này thì không:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0840.Magic%20Squares%20In%20Grid/images/magic_invalid.jpg" style="width: 242px; height: 242px;" />
Tổng cộng, trong grid đã cho chỉ có một ma phương.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[8]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>row == grid.length</code></li>
	<li><code>col == grid[i].length</code></li>
	<li><code>1 &lt;= row, col &lt;= 10</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 15</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ma phương sử dụng mỗi số từ $1\ldots 9$ đúng một lần và tổng các hàng, cột, đường chéo phải bằng nhau. Grid có kích thước tối đa $10\times 10$, nên có thể kiểm tra mọi vùng $3\times 3$.
>
> Thu thập các số cùng tổng của từng hàng, cột và đường chéo. Loại vùng nếu có số trùng, giá trị ngoài phạm vi hoặc các tổng không bằng nhau. Đếm số vùng hợp lệ.

<!-- thinking:end -->

Ta liệt kê trực tiếp tọa độ góc trên bên trái $(i, j)$ của từng ma trận con $3 \times 3$, rồi kiểm tra xem ma trận con đó có thỏa mãn tính chất “ma phương” hay không. Nếu có, tăng đáp án lên 1. Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numMagicSquaresInside(self, grid: List[List[int]]) -> int:
        def check(i: int, j: int) -> int:
            if i + 3 > m or j + 3 > n:
                return 0
            s = set()
            row = [0] * 3
            col = [0] * 3
            a = b = 0
            for x in range(i, i + 3):
                for y in range(j, j + 3):
                    v = grid[x][y]
                    if v < 1 or v > 9:
                        return 0
                    s.add(v)
                    row[x - i] += v
                    col[y - j] += v
                    if x - i == y - j:
                        a += v
                    if x - i == 2 - (y - j):
                        b += v
            if len(s) != 9 or a != b:
                return 0
            if any(x != a for x in row) or any(x != a for x in col):
                return 0
            return 1

        m, n = len(grid), len(grid[0])
        return sum(check(i, j) for i in range(m) for j in range(n))
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] grid;

    public int numMagicSquaresInside(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans += check(i, j);
            }
        }
        return ans;
    }

    private int check(int i, int j) {
        if (i + 3 > m || j + 3 > n) {
            return 0;
        }
        int[] cnt = new int[16];
        int[] row = new int[3];
        int[] col = new int[3];
        int a = 0, b = 0;
        for (int x = i; x < i + 3; ++x) {
            for (int y = j; y < j + 3; ++y) {
                int v = grid[x][y];
                if (v < 1 || v > 9 || ++cnt[v] > 1) {
                    return 0;
                }
                row[x - i] += v;
                col[y - j] += v;
                if (x - i == y - j) {
                    a += v;
                }
                if (x - i + y - j == 2) {
                    b += v;
                }
            }
        }
        if (a != b) {
            return 0;
        }
        for (int k = 0; k < 3; ++k) {
            if (row[k] != a || col[k] != a) {
                return 0;
            }
        }
        return 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numMagicSquaresInside(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int ans = 0;
        auto check = [&](int i, int j) {
            if (i + 3 > m || j + 3 > n) {
                return 0;
            }
            vector<int> cnt(16);
            vector<int> row(3);
            vector<int> col(3);
            int a = 0, b = 0;
            for (int x = i; x < i + 3; ++x) {
                for (int y = j; y < j + 3; ++y) {
                    int v = grid[x][y];
                    if (v < 1 || v > 9 || ++cnt[v] > 1) {
                        return 0;
                    }
                    row[x - i] += v;
                    col[y - j] += v;
                    if (x - i == y - j) {
                        a += v;
                    }
                    if (x - i + y - j == 2) {
                        b += v;
                    }
                }
            }
            if (a != b) {
                return 0;
            }
            for (int k = 0; k < 3; ++k) {
                if (row[k] != a || col[k] != a) {
                    return 0;
                }
            }
            return 1;
        };
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans += check(i, j);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numMagicSquaresInside(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	check := func(i, j int) int {
		if i+3 > m || j+3 > n {
			return 0
		}
		cnt := [16]int{}
		row := [3]int{}
		col := [3]int{}
		a, b := 0, 0
		for x := i; x < i+3; x++ {
			for y := j; y < j+3; y++ {
				v := grid[x][y]
				if v < 1 || v > 9 || cnt[v] > 0 {
					return 0
				}
				cnt[v]++
				row[x-i] += v
				col[y-j] += v
				if x-i == y-j {
					a += v
				}
				if x-i == 2-(y-j) {
					b += v
				}
			}
		}
		if a != b {
			return 0
		}
		for k := 0; k < 3; k++ {
			if row[k] != a || col[k] != a {
				return 0
			}
		}
		return 1
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			ans += check(i, j)
		}
	}
	return
}
```

#### TypeScript

```ts
function numMagicSquaresInside(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const check = (i: number, j: number): number => {
        if (i + 3 > m || j + 3 > n) {
            return 0;
        }
        const cnt: number[] = Array(16).fill(0);
        const row: number[] = Array(3).fill(0);
        const col: number[] = Array(3).fill(0);
        let [a, b] = [0, 0];
        for (let x = i; x < i + 3; ++x) {
            for (let y = j; y < j + 3; ++y) {
                const v = grid[x][y];
                if (v < 1 || v > 9 || ++cnt[v] > 1) {
                    return 0;
                }
                row[x - i] += v;
                col[y - j] += v;
                if (x - i === y - j) {
                    a += v;
                }
                if (x - i === 2 - (y - j)) {
                    b += v;
                }
            }
        }
        if (a !== b) {
            return 0;
        }
        for (let k = 0; k < 3; ++k) {
            if (row[k] !== a || col[k] !== a) {
                return 0;
            }
        }
        return 1;
    };
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            ans += check(i, j);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_magic_squares_inside(grid: Vec<Vec<i32>>) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut ans: i32 = 0;

        let check = |i: usize, j: usize, grid: &Vec<Vec<i32>>| -> i32 {
            if i + 3 > m || j + 3 > n {
                return 0;
            }

            let mut cnt = vec![0; 16];
            let mut row = vec![0; 3];
            let mut col = vec![0; 3];
            let mut a = 0;
            let mut b = 0;

            for x in i..i + 3 {
                for y in j..j + 3 {
                    let v = grid[x][y] as usize;
                    if v < 1 || v > 9 {
                        return 0;
                    }
                    cnt[v] += 1;
                    if cnt[v] > 1 {
                        return 0;
                    }

                    let vv = grid[x][y];
                    row[x - i] += vv;
                    col[y - j] += vv;

                    if x - i == y - j {
                        a += vv;
                    }
                    if x - i + y - j == 2 {
                        b += vv;
                    }
                }
            }

            if a != b {
                return 0;
            }

            for k in 0..3 {
                if row[k] != a || col[k] != a {
                    return 0;
                }
            }

            1
        };

        for i in 0..m {
            for j in 0..n {
                ans += check(i, j, &grid);
            }
        }

        ans
    }
}
```

#### JavaScript

```js
function numMagicSquaresInside(grid) {
    const m = grid.length;
    const n = grid[0].length;
    const check = (i, j) => {
        if (i + 3 > m || j + 3 > n) {
            return 0;
        }
        const cnt = Array(16).fill(0);
        const row = Array(3).fill(0);
        const col = Array(3).fill(0);
        let [a, b] = [0, 0];
        for (let x = i; x < i + 3; ++x) {
            for (let y = j; y < j + 3; ++y) {
                const v = grid[x][y];
                if (v < 1 || v > 9 || ++cnt[v] > 1) {
                    return 0;
                }
                row[x - i] += v;
                col[y - j] += v;
                if (x - i === y - j) {
                    a += v;
                }
                if (x - i === 2 - (y - j)) {
                    b += v;
                }
            }
        }
        if (a !== b) {
            return 0;
        }
        for (let k = 0; k < 3; ++k) {
            if (row[k] !== a || col[k] !== a) {
                return 0;
            }
        }
        return 1;
    };
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            ans += check(i, j);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
