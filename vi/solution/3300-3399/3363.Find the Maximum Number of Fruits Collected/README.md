---
comments: true
difficulty: Hard
rating: 2404
source: Biweekly Contest 144 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3363. Find the Maximum Number of Fruits Collected](https://leetcode.com/problems/find-the-maximum-number-of-fruits-collected)

[中文文档](/solution/3300-3399/3363.Find%20the%20Maximum%20Number%20of%20Fruits%20Collected/README.md)

## Mô tả

<!-- description:start -->

<p>Có một hầm ngục trong game gồm <code>n x n</code> phòng được sắp xếp trên một lưới.</p>

<p>Cho một mảng 2 chiều <code>fruits</code> có kích thước <code>n x n</code>, trong đó <code>fruits[i][j]</code> biểu thị số lượng trái cây trong phòng <code>(i, j)</code>. Ba đứa trẻ sẽ chơi trong hầm ngục, với vị trí <strong>ban đầu</strong> lần lượt là các phòng góc <code>(0, 0)</code>, <code>(0, n - 1)</code> và <code>(n - 1, 0)</code>.</p>

<p>Các đứa trẻ sẽ thực hiện <strong>chính xác</strong> <code>n - 1</code> bước theo các quy tắc sau để đi đến phòng <code>(n - 1, n - 1)</code>:</p>

<ul>
	<li>Đứa trẻ bắt đầu từ <code>(0, 0)</code> phải di chuyển từ phòng hiện tại <code>(i, j)</code> đến một trong các phòng <code>(i + 1, j + 1)</code>, <code>(i + 1, j)</code> và <code>(i, j + 1)</code> nếu phòng đích tồn tại.</li>
	<li>Đứa trẻ bắt đầu từ <code>(0, n - 1)</code> phải di chuyển từ phòng hiện tại <code>(i, j)</code> đến một trong các phòng <code>(i + 1, j - 1)</code>, <code>(i + 1, j)</code> và <code>(i + 1, j + 1)</code> nếu phòng đích tồn tại.</li>
	<li>Đứa trẻ bắt đầu từ <code>(n - 1, 0)</code> phải di chuyển từ phòng hiện tại <code>(i, j)</code> đến một trong các phòng <code>(i - 1, j + 1)</code>, <code>(i, j + 1)</code> và <code>(i + 1, j + 1)</code> nếu phòng đích tồn tại.</li>
</ul>

<p>Khi một đứa trẻ bước vào phòng, em sẽ thu thập toàn bộ trái cây ở đó. Nếu có từ hai đứa trẻ trở lên bước vào cùng một phòng, chỉ một đứa trẻ thu thập trái cây, và căn phòng sẽ trống sau khi các em rời đi.</p>

<p>Hãy trả về số lượng trái cây <strong>lớn nhất</strong> mà các đứa trẻ có thể thu thập từ hầm ngục.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">fruits = [[1,2,3,4],[5,6,8,7],[9,10,11,12],[13,14,15,16]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">100</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3363.Find%20the%20Maximum%20Number%20of%20Fruits%20Collected/images/clideo_editor_d0b446db9ba448e1a3fcdd0eecdb58d0-ezgifcom-crop.gif" style="width: 250px; height: 210px;" /></p>

<p>Trong ví dụ này:</p>

<ul>
	<li>Đứa trẻ 1<sup>st</sup> (màu xanh lá) đi theo đường <code>(0,0) -&gt; (1,1) -&gt; (2,2) -&gt; (3, 3)</code>.</li>
	<li>Đứa trẻ 2<sup>nd</sup> (màu đỏ) đi theo đường <code>(0,3) -&gt; (1,2) -&gt; (2,3) -&gt; (3, 3)</code>.</li>
	<li>Đứa trẻ 3<sup>rd</sup> (màu xanh dương) đi theo đường <code>(3,0) -&gt; (3,1) -&gt; (3,2) -&gt; (3, 3)</code>.</li>
</ul>

<p>Tổng cộng, các em thu thập được <code>1 + 6 + 11 + 16 + 4 + 8 + 12 + 13 + 14 + 15 = 100</code> trái cây.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">fruits = [[1,1],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong ví dụ này:</p>

<ul>
	<li>Đứa trẻ 1<sup>st</sup> đi theo đường <code>(0,0) -&gt; (1,1)</code>.</li>
	<li>Đứa trẻ 2<sup>nd</sup> đi theo đường <code>(0,1) -&gt; (1,1)</code>.</li>
	<li>Đứa trẻ 3<sup>rd</sup> đi theo đường <code>(1,0) -&gt; (1,1)</code>.</li>
</ul>

<p>Tổng cộng, các em thu thập được <code>1 + 1 + 1 + 1 = 4</code> trái cây.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == fruits.length == fruits[i].length &lt;= 1000</code></li>
	<li><code>0 &lt;= fruits[i][j] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ba đứa trẻ đi $n-1$ bước rồi gặp nhau. Đứa trẻ xuất phát từ $(0,0)$ bị buộc phải đi trên đường chéo chính; hai đứa trẻ còn lại luôn ở hẳn phía trên hoặc phía dưới đường chéo đó, nên các phòng không bị đi trùng.
>
> Ta cộng trực tiếp số trái cây trên đường chéo. Hai tam giác còn lại có DP riêng: $f[i][j]$ lấy giá trị tốt nhất trong ba ô đi vào rồi cộng thêm $\textit{fruits}[i][j]$.
>
> Các em dừng ở những ô ngay trước phòng góc, nên đáp án là tổng đường chéo cộng với $f[n-2][n-1]$ và $f[n-1][n-2]$.

<!-- thinking:end -->

Theo mô tả bài toán, đứa trẻ bắt đầu từ $(0, 0)$ muốn đến $(n - 1, n - 1)$ trong đúng $n - 1$ bước thì chỉ có thể đi qua các phòng trên đường chéo chính $(i, i)$, với $i = 0, 1, \ldots, n - 1$. Đứa trẻ bắt đầu từ $(0, n - 1)$ chỉ có thể đi qua các phòng phía trên đường chéo chính, còn đứa trẻ bắt đầu từ $(n - 1, 0)$ chỉ có thể đi qua các phòng phía dưới đường chéo chính. Điều này có nghĩa là ngoài phòng đích $(n - 1, n - 1)$, không có phòng nào khác được nhiều đứa trẻ đi qua.

Ta có thể dùng quy hoạch động để tính số trái cây mà các đứa trẻ bắt đầu từ $(0, n - 1)$ và $(n - 1, 0)$ có thể thu thập khi đến $(i, j)$. Đặt $f[i][j]$ là số trái cây một đứa trẻ có thể thu thập khi đến $(i, j)$.

Với đứa trẻ bắt đầu từ $(0, n - 1)$, công thức chuyển trạng thái là:

$$
f[i][j] = \max(f[i - 1][j], f[i - 1][j - 1], f[i - 1][j + 1]) + \text{fruits}[i][j]
$$

Lưu ý rằng $f[i - 1][j + 1]$ chỉ hợp lệ khi $j + 1 < n$.

Với đứa trẻ bắt đầu từ $(n - 1, 0)$, công thức chuyển trạng thái là:

$$
f[i][j] = \max(f[i][j - 1], f[i - 1][j - 1], f[i + 1][j - 1]) + \text{fruits}[i][j]
$$

Tương tự, $f[i + 1][j - 1]$ chỉ hợp lệ khi $i + 1 < n$.

Cuối cùng, đáp án là $\sum_{i=0}^{n-1} \text{fruits}[i][i] + f[n-2][n-1] + f[n-1][n-2]$, tức tổng số trái cây trên đường chéo chính cộng với số trái cây mà hai đứa trẻ có thể thu thập khi lần lượt đến $(n - 2, n - 1)$ và $(n - 1, n - 2)$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của lưới phòng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCollectedFruits(self, fruits: List[List[int]]) -> int:
        n = len(fruits)
        f = [[-inf] * n for _ in range(n)]
        f[0][n - 1] = fruits[0][n - 1]
        for i in range(1, n):
            for j in range(i + 1, n):
                f[i][j] = max(f[i - 1][j], f[i - 1][j - 1]) + fruits[i][j]
                if j + 1 < n:
                    f[i][j] = max(f[i][j], f[i - 1][j + 1] + fruits[i][j])
        f[n - 1][0] = fruits[n - 1][0]
        for j in range(1, n):
            for i in range(j + 1, n):
                f[i][j] = max(f[i][j - 1], f[i - 1][j - 1]) + fruits[i][j]
                if i + 1 < n:
                    f[i][j] = max(f[i][j], f[i + 1][j - 1] + fruits[i][j])
        return sum(fruits[i][i] for i in range(n)) + f[n - 2][n - 1] + f[n - 1][n - 2]
```

#### Java

```java
class Solution {
    public int maxCollectedFruits(int[][] fruits) {
        int n = fruits.length;
        final int inf = 1 << 29;
        int[][] f = new int[n][n];
        for (var row : f) {
            Arrays.fill(row, -inf);
        }
        f[0][n - 1] = fruits[0][n - 1];
        for (int i = 1; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                f[i][j] = Math.max(f[i - 1][j], f[i - 1][j - 1]) + fruits[i][j];
                if (j + 1 < n) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][j + 1] + fruits[i][j]);
                }
            }
        }
        f[n - 1][0] = fruits[n - 1][0];
        for (int j = 1; j < n; j++) {
            for (int i = j + 1; i < n; i++) {
                f[i][j] = Math.max(f[i][j - 1], f[i - 1][j - 1]) + fruits[i][j];
                if (i + 1 < n) {
                    f[i][j] = Math.max(f[i][j], f[i + 1][j - 1] + fruits[i][j]);
                }
            }
        }
        int ans = f[n - 2][n - 1] + f[n - 1][n - 2];
        for (int i = 0; i < n; i++) {
            ans += fruits[i][i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxCollectedFruits(vector<vector<int>>& fruits) {
        int n = fruits.size();
        const int inf = 1 << 29;
        vector<vector<int>> f(n, vector<int>(n, -inf));

        f[0][n - 1] = fruits[0][n - 1];
        for (int i = 1; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                f[i][j] = max(f[i - 1][j], f[i - 1][j - 1]) + fruits[i][j];
                if (j + 1 < n) {
                    f[i][j] = max(f[i][j], f[i - 1][j + 1] + fruits[i][j]);
                }
            }
        }

        f[n - 1][0] = fruits[n - 1][0];
        for (int j = 1; j < n; j++) {
            for (int i = j + 1; i < n; i++) {
                f[i][j] = max(f[i][j - 1], f[i - 1][j - 1]) + fruits[i][j];
                if (i + 1 < n) {
                    f[i][j] = max(f[i][j], f[i + 1][j - 1] + fruits[i][j]);
                }
            }
        }

        int ans = f[n - 2][n - 1] + f[n - 1][n - 2];
        for (int i = 0; i < n; i++) {
            ans += fruits[i][i];
        }

        return ans;
    }
};
```

#### Go

```go
func maxCollectedFruits(fruits [][]int) int {
	n := len(fruits)
	const inf = 1 << 29
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -inf
		}
	}

	f[0][n-1] = fruits[0][n-1]
	for i := 1; i < n; i++ {
		for j := i + 1; j < n; j++ {
			f[i][j] = max(f[i-1][j], f[i-1][j-1]) + fruits[i][j]
			if j+1 < n {
				f[i][j] = max(f[i][j], f[i-1][j+1]+fruits[i][j])
			}
		}
	}

	f[n-1][0] = fruits[n-1][0]
	for j := 1; j < n; j++ {
		for i := j + 1; i < n; i++ {
			f[i][j] = max(f[i][j-1], f[i-1][j-1]) + fruits[i][j]
			if i+1 < n {
				f[i][j] = max(f[i][j], f[i+1][j-1]+fruits[i][j])
			}
		}
	}

	ans := f[n-2][n-1] + f[n-1][n-2]
	for i := 0; i < n; i++ {
		ans += fruits[i][i]
	}

	return ans
}
```

#### TypeScript

```ts
function maxCollectedFruits(fruits: number[][]): number {
    const n = fruits.length;
    const inf = 1 << 29;
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(-inf));

    f[0][n - 1] = fruits[0][n - 1];
    for (let i = 1; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            f[i][j] = Math.max(f[i - 1][j], f[i - 1][j - 1]) + fruits[i][j];
            if (j + 1 < n) {
                f[i][j] = Math.max(f[i][j], f[i - 1][j + 1] + fruits[i][j]);
            }
        }
    }

    f[n - 1][0] = fruits[n - 1][0];
    for (let j = 1; j < n; j++) {
        for (let i = j + 1; i < n; i++) {
            f[i][j] = Math.max(f[i][j - 1], f[i - 1][j - 1]) + fruits[i][j];
            if (i + 1 < n) {
                f[i][j] = Math.max(f[i][j], f[i + 1][j - 1] + fruits[i][j]);
            }
        }
    }

    let ans = f[n - 2][n - 1] + f[n - 1][n - 2];
    for (let i = 0; i < n; i++) {
        ans += fruits[i][i];
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_collected_fruits(fruits: Vec<Vec<i32>>) -> i32 {
        let n = fruits.len();
        let inf = 1 << 29;
        let mut f = vec![vec![-inf; n]; n];

        f[0][n - 1] = fruits[0][n - 1];
        for i in 1..n {
            for j in i + 1..n {
                f[i][j] = std::cmp::max(f[i - 1][j], f[i - 1][j - 1]) + fruits[i][j];
                if j + 1 < n {
                    f[i][j] = std::cmp::max(f[i][j], f[i - 1][j + 1] + fruits[i][j]);
                }
            }
        }

        f[n - 1][0] = fruits[n - 1][0];
        for j in 1..n {
            for i in j + 1..n {
                f[i][j] = std::cmp::max(f[i][j - 1], f[i - 1][j - 1]) + fruits[i][j];
                if i + 1 < n {
                    f[i][j] = std::cmp::max(f[i][j], f[i + 1][j - 1] + fruits[i][j]);
                }
            }
        }

        let mut ans = f[n - 2][n - 1] + f[n - 1][n - 2];
        for i in 0..n {
            ans += fruits[i][i];
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
