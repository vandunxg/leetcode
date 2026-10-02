---
comments: true
difficulty: Easy
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [661. Image Smoother](https://leetcode.com/problems/image-smoother)

[中文文档](/solution/0600-0699/0661.Image%20Smoother/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Bộ lọc làm mượt ảnh</strong> có kích thước <code>3 x 3</code> và được áp dụng cho từng ô ảnh bằng cách lấy trung bình của ô đó cùng tám ô xung quanh rồi làm tròn xuống (tức trung bình của chín ô trong vùng màu xanh). Nếu một hoặc nhiều ô xung quanh nằm ngoài ảnh, ta không tính chúng vào giá trị trung bình (tức chỉ lấy trung bình các ô nằm trong vùng màu đỏ).</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0661.Image%20Smoother/images/smoother-grid.jpg" style="width: 493px; height: 493px;" />
<p>Cho ma trận số nguyên <code>m x n</code> <code>img</code> biểu diễn mức xám của ảnh, hãy trả về <em>ảnh sau khi áp dụng bộ lọc làm mượt lên từng ô</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0661.Image%20Smoother/images/smooth-grid.jpg" style="width: 613px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> img = [[1,1,1],[1,0,1],[1,1,1]]
<strong>Đầu ra:</strong> [[0,0,0],[0,0,0],[0,0,0]]
<strong>Giải thích:</strong>
Với các ô (0,0), (0,2), (2,0), (2,2): floor(3/4) = floor(0.75) = 0
Với các ô (0,1), (1,0), (1,2), (2,1): floor(5/6) = floor(0.83333333) = 0
Với ô (1,1): floor(8/9) = floor(0.88888889) = 0
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0661.Image%20Smoother/images/smooth2-grid.jpg" style="width: 613px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> img = [[100,200,100],[200,50,200],[100,200,100]]
<strong>Đầu ra:</strong> [[137,141,137],[141,138,141],[137,141,137]]
<strong>Giải thích:</strong>
Với các ô (0,0), (0,2), (2,0), (2,2): floor((100+200+200+50)/4) = floor(137.5) = 137
Với các ô (0,1), (1,0), (1,2), (2,1): floor((200+200+50+200+100+100)/6) = floor(141.666667) = 141
Với ô (1,1): floor((50+200+200+200+200+100+100+100+100)/9) = floor(138.888889) = 138
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == img.length</code></li>
	<li><code>n == img[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>0 &lt;= img[i][j] &lt;= 255</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ô được thay bằng trung bình làm tròn xuống của chính nó và tối đa tám ô lân cận. Có thể duyệt trực tiếp ma trận $200\times 200$.
>
> Với $(i,j)$, tính tổng các ô hợp lệ trong vùng $[i-1,i+1]\times[j-1,j+1]$ rồi ghi vào ma trận mới để các giá trị đã cập nhật không ảnh hưởng đến ô lân cận.

<!-- thinking:end -->

Ta tạo mảng 2 chiều $\textit{ans}$ kích thước $m \times n$, trong đó $\textit{ans}[i][j]$ là giá trị sau khi làm mượt của ô ở hàng thứ $i$ và cột thứ $j$ trong ảnh.

Để tính $\textit{ans}[i][j]$, ta duyệt ô ở hàng thứ $i$, cột thứ $j$ của $\textit{img}$ cùng 8 ô xung quanh, tính tổng $s$ và số lượng $cnt$, sau đó lấy trung bình $s / cnt$ và lưu vào $\textit{ans}[i][j]$.

Sau khi duyệt xong, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của $\textit{img}$. Nếu không tính phần bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def imageSmoother(self, img: List[List[int]]) -> List[List[int]]:
        m, n = len(img), len(img[0])
        ans = [[0] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                s = cnt = 0
                for x in range(i - 1, i + 2):
                    for y in range(j - 1, j + 2):
                        if 0 <= x < m and 0 <= y < n:
                            cnt += 1
                            s += img[x][y]
                ans[i][j] = s // cnt
        return ans
```

#### Java

```java
class Solution {
    public int[][] imageSmoother(int[][] img) {
        int m = img.length;
        int n = img[0].length;
        int[][] ans = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int s = 0;
                int cnt = 0;
                for (int x = i - 1; x <= i + 1; ++x) {
                    for (int y = j - 1; y <= j + 1; ++y) {
                        if (x >= 0 && x < m && y >= 0 && y < n) {
                            ++cnt;
                            s += img[x][y];
                        }
                    }
                }
                ans[i][j] = s / cnt;
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
    vector<vector<int>> imageSmoother(vector<vector<int>>& img) {
        int m = img.size(), n = img[0].size();
        vector<vector<int>> ans(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int s = 0, cnt = 0;
                for (int x = i - 1; x <= i + 1; ++x) {
                    for (int y = j - 1; y <= j + 1; ++y) {
                        if (x < 0 || x >= m || y < 0 || y >= n) {
                            continue;
                        }
                        ++cnt;
                        s += img[x][y];
                    }
                }
                ans[i][j] = s / cnt;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func imageSmoother(img [][]int) [][]int {
	m, n := len(img), len(img[0])
	ans := make([][]int, m)
	for i, row := range img {
		ans[i] = make([]int, n)
		for j := range row {
			s, cnt := 0, 0
			for x := i - 1; x <= i+1; x++ {
				for y := j - 1; y <= j+1; y++ {
					if x >= 0 && x < m && y >= 0 && y < n {
						cnt++
						s += img[x][y]
					}
				}
			}
			ans[i][j] = s / cnt
		}
	}
	return ans
}
```

#### TypeScript

```ts
function imageSmoother(img: number[][]): number[][] {
    const m = img.length;
    const n = img[0].length;
    const ans: number[][] = Array.from({ length: m }, () => Array(n).fill(0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            let s = 0;
            let cnt = 0;
            for (let x = i - 1; x <= i + 1; ++x) {
                for (let y = j - 1; y <= j + 1; ++y) {
                    if (x >= 0 && x < m && y >= 0 && y < n) {
                        ++cnt;
                        s += img[x][y];
                    }
                }
            }
            ans[i][j] = Math.floor(s / cnt);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn image_smoother(img: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let m = img.len();
        let n = img[0].len();
        let mut ans = vec![vec![0; n]; m];
        for i in 0..m {
            for j in 0..n {
                let mut s = 0;
                let mut cnt = 0;
                for x in i.saturating_sub(1)..=(i + 1).min(m - 1) {
                    for y in j.saturating_sub(1)..=(j + 1).min(n - 1) {
                        s += img[x][y];
                        cnt += 1;
                    }
                }
                ans[i][j] = s / cnt;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
