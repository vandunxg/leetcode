---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Math
    - Matrix
---

<!-- problem:start -->

# [2128. Remove All Ones With Row and Column Flips 🔒](https://leetcode.com/problems/remove-all-ones-with-row-and-column-flips)

[中文文档](/solution/2100-2199/2128.Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận nhị phân <code>m x n</code> là <code>grid</code>.</p>

<p>Trong một thao tác, bạn có thể chọn <strong>bất kỳ</strong> hàng hoặc cột nào rồi lật tất cả giá trị trong hàng hoặc cột đó (tức là đổi tất cả <code>0</code> thành <code>1</code> và tất cả <code>1</code> thành <code>0</code>).</p>

<p>Hãy trả về <code>true</code><em> nếu có thể xóa tất cả </em><code>1</code><em> khỏi </em><code>grid</code> bằng <strong>bất kỳ</strong> số thao tác nào hoặc <code>false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2128.Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips/images/image-20220103191300-1.png" style="width: 756px; height: 225px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,0],[1,0,1],[0,1,0]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Một cách có thể xóa tất cả các số 1 khỏi grid là:
- Lật hàng ở giữa
- Lật cột ở giữa
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2128.Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips/images/image-20220103181204-7.png" style="width: 237px; height: 225px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0],[0,0,0],[0,0,0]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể xóa tất cả các số 1 khỏi grid.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2128.Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips/images/image-20220103181224-8.png" style="width: 114px; height: 100px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> grid không có số 1 nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>grid[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng và cột về hiệu ứng chỉ cần lật nhiều nhất một lần, còn mục tiêu là đưa ma trận về toàn số 0. Hai hàng có cùng mẫu lật cột phải bằng nhau hoặc đối nhau. Việc liệt kê toàn bộ $2^{m+n}$ tập hợp các phép lật là quá lớn.
>
> Chuẩn hóa mọi hàng để bắt đầu bằng $0$ (lật hàng nếu phần tử đầu tiên khác phần tử đầu tiên của hàng $0$) sẽ gom các hàng tương đương về cùng một mẫu. Nếu tập hợp chứa mẫu thứ hai thì không thể xóa mẫu đó bằng cùng các phép lật cột.
>
> Vì vậy, ta XOR một hàng khi cần, thêm tuple tương ứng vào tập hợp và trả về đúng khi kích thước tập hợp bằng $1$.

<!-- thinking:end -->

Ta nhận thấy nếu hai hàng trong ma trận thỏa mãn một trong các điều kiện sau, chúng có thể được đưa về giống nhau bằng cách lật một số cột:

1. Các phần tử tương ứng của hai hàng bằng nhau, tức là nếu một hàng là $1,0,0,1$ thì hàng còn lại cũng là $1,0,0,1$;
1. Các phần tử tương ứng của hai hàng đối nhau, tức là nếu một hàng là $1,0,0,1$ thì hàng còn lại là $0,1,1,0$.

Ta gọi hai hàng thỏa mãn một trong các điều kiện trên là "hàng tương đương". Đáp án của bài toán là số lượng hàng tương đương lớn nhất trong ma trận.

Do đó, ta có thể duyệt qua từng hàng của ma trận và chuyển mỗi hàng thành một "hàng tương đương" bắt đầu bằng $0$. Cụ thể:

- Nếu phần tử đầu tiên của hàng hiện tại là $0$, giữ nguyên hàng;
- Nếu phần tử đầu tiên của hàng hiện tại là $1$, lật mọi phần tử trong hàng, tức là đổi $0$ thành $1$ và $1$ thành $0$. Nói cách khác, ta lật các hàng bắt đầu bằng $1$ thành các "hàng tương đương" bắt đầu bằng $0$.

Theo cách này, ta chỉ cần dùng một hash table để đếm mỗi hàng sau khi chuyển đổi. Nếu hash table chỉ chứa một phần tử ở cuối, điều đó có nghĩa là ta có thể xóa tất cả các số $1$ khỏi ma trận bằng cách lật các hàng hoặc cột.

Độ phức tạp thời gian là $O(mn)$ và độ phức tạp không gian là $O(m)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

Bài toán liên quan:

- [1072. Flip Columns For Maximum Number of Equal Rows](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1072.Flip%20Columns%20For%20Maximum%20Number%20of%20Equal%20Rows/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeOnes(self, grid: List[List[int]]) -> bool:
        s = set()
        for row in grid:
            t = tuple(row) if row[0] == grid[0][0] else tuple(x ^ 1 for x in row)
            s.add(t)
        return len(s) == 1
```

#### Java

```java
class Solution {
    public boolean removeOnes(int[][] grid) {
        Set<String> s = new HashSet<>();
        int n = grid[0].length;
        for (var row : grid) {
            var cs = new char[n];
            for (int i = 0; i < n; ++i) {
                cs[i] = (char) (row[0] ^ row[i]);
            }
            s.add(String.valueOf(cs));
        }
        return s.size() == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool removeOnes(vector<vector<int>>& grid) {
        unordered_set<string> s;
        for (auto& row : grid) {
            string t;
            for (int x : row) {
                t.push_back('0' + (row[0] == 0 ? x : x ^ 1));
            }
            s.insert(t);
        }
        return s.size() == 1;
    }
};
```

#### Go

```go
func removeOnes(grid [][]int) bool {
	s := map[string]bool{}
	for _, row := range grid {
		t := []byte{}
		for _, x := range row {
			if row[0] == 1 {
				x ^= 1
			}
			t = append(t, byte(x)+'0')
		}
		s[string(t)] = true
	}
	return len(s) == 1
}
```

#### TypeScript

```ts
function removeOnes(grid: number[][]): boolean {
    const s = new Set<string>();
    for (const row of grid) {
        if (row[0] === 1) {
            for (let i = 0; i < row.length; i++) {
                row[i] ^= 1;
            }
        }
        const t = row.join('');
        s.add(t);
    }
    return s.size === 1;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn remove_ones(grid: Vec<Vec<i32>>) -> bool {
        let n = grid[0].len();
        let mut set = HashSet::new();

        for row in grid.iter() {
            let mut pattern = String::with_capacity(n);
            for &x in row.iter() {
                pattern.push(((row[0] ^ x) as u8 + b'0') as char);
            }
            set.insert(pattern);
        }

        set.len() == 1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
