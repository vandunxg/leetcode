---
comments: true
difficulty: Medium
rating: 1601
source: Weekly Contest 386 Q2
tags:
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [3047. Find the Largest Area of Square Inside Two Rectangles](https://leetcode.com/problems/find-the-largest-area-of-square-inside-two-rectangles)

[中文文档](/solution/3000-3099/3047.Find%20the%20Largest%20Area%20of%20Square%20Inside%20Two%20Rectangles/README.md)

## Mô tả

<!-- description:start -->

<p>Trên mặt phẳng 2D có <code>n</code> hình chữ nhật với các cạnh song song với trục x và y. Bạn được cho hai mảng số nguyên 2D&nbsp;<code>bottomLeft</code> và <code>topRight</code>, trong đó <code>bottomLeft[i] = [a_i, b_i]</code> và <code>topRight[i] = [c_i, d_i]</code> lần lượt biểu diễn tọa độ <strong>góc dưới bên trái</strong> và <strong>góc trên bên phải</strong> của hình chữ nhật thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn cần tìm diện tích <strong>lớn nhất</strong> của một <strong>hình vuông</strong> có thể nằm trong vùng giao của ít nhất hai hình chữ nhật. Trả về <code>0</code> nếu không tồn tại hình vuông như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3047.Find%20the%20Largest%20Area%20of%20Square%20Inside%20Two%20Rectangles/images/example12.png" style="width: 443px; height: 364px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem;" />
<p><strong>Đầu vào:</strong> bottomLeft = [[1,1],[2,2],[3,1]], topRight = [[3,3],[4,4],[6,6]]</p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Một hình vuông có độ dài cạnh 1 có thể nằm trong vùng giao của hình chữ nhật 0 và 1 hoặc vùng giao của hình chữ nhật 1 và 2. Vì vậy, diện tích lớn nhất là 1. Có thể chứng minh rằng không hình vuông nào có độ dài cạnh lớn hơn có thể nằm trong vùng giao của bất kỳ cặp hình chữ nhật nào.</p>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3047.Find%20the%20Largest%20Area%20of%20Square%20Inside%20Two%20Rectangles/images/diag.png" style="width: 451px; height: 470px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem;" />
<p><strong>Đầu vào:</strong> bottomLeft = [[1,1],[1,3],[1,5]], topRight = [[5,5],[5,7],[5,9]]</p>

<p><strong>Đầu ra:</strong> 4</p>

<p><strong>Giải thích:</strong></p>

<p>Một hình vuông có độ dài cạnh 2 có thể nằm trong vùng giao của hình chữ nhật 0 và 1 hoặc vùng giao của hình chữ nhật 1 và 2. Vì vậy, diện tích lớn nhất là <code>2 * 2 = 4</code>. Có thể chứng minh rằng không hình vuông nào có độ dài cạnh lớn hơn có thể nằm trong vùng giao của bất kỳ cặp hình chữ nhật nào.</p>

<p><strong class="example">Ví dụ 3:</strong></p>
<code> <img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3047.Find%20the%20Largest%20Area%20of%20Square%20Inside%20Two%20Rectangles/images/rectanglesexample2.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 445px; height: 365px;" /> </code>

<p><strong>Đầu vào:</strong> bottomLeft = [[1,1],[2,2],[1,2]], topRight = [[3,3],[4,4],[3,4]]</p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Một hình vuông có độ dài cạnh 1 có thể nằm trong vùng giao của bất kỳ cặp hình chữ nhật nào. Ngoài ra, không hình vuông nào lớn hơn có thể nằm trong vùng giao, nên diện tích lớn nhất là 1. Lưu ý rằng vùng này có thể được tạo bởi giao của nhiều hơn 2 hình chữ nhật.</p>

<p><strong class="example">Ví dụ 4:</strong></p>
<code> <img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3047.Find%20the%20Largest%20Area%20of%20Square%20Inside%20Two%20Rectangles/images/rectanglesexample3.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 444px; height: 364px;" /> </code>

<p><strong>Đầu vào:&nbsp;</strong>bottomLeft = [[1,1],[3,3],[3,1]], topRight = [[2,2],[4,4],[4,2]]</p>

<p><strong>Đầu ra:</strong> 0</p>

<p><strong>Giải thích:</strong></p>

<p>Không có cặp hình chữ nhật nào giao nhau, nên đáp án là 0.</p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == bottomLeft.length == topRight.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>3</sup></code></li>
	<li><code>bottomLeft[i].length == topRight[i].length == 2</code></li>
	<li><code>1 &lt;= bottomLeft[i][0], bottomLeft[i][1] &lt;= 10<sup>7</sup></code></li>
	<li><code>1 &lt;= topRight[i][0], topRight[i][1] &lt;= 10<sup>7</sup></code></li>
	<li><code>bottomLeft[i][0] &lt; topRight[i][0]</code></li>
	<li><code>bottomLeft[i][1] &lt; topRight[i][1]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Có $n \le 10^3$ hình chữ nhật, và hình vuông phải nằm trong vùng giao của một cặp hình chữ nhật. Độ dài cạnh của nó bị giới hạn bởi giá trị nhỏ hơn giữa chiều rộng và chiều cao của vùng giao đó.
>
> Chiều rộng và chiều cao của vùng giao có thể tính trong $O(1)$, nên ta duyệt qua các cặp hình chữ nhật và lấy giá trị lớn nhất của $\min(w,h)^2$.
>
> Một vùng giao không có kích thước dương đóng góp $0$.

<!-- thinking:end -->

Ta có thể duyệt qua hai hình chữ nhật, trong đó tọa độ góc dưới bên trái và góc trên bên phải của hình chữ nhật 1 lần lượt là $(x_1, y_1)$ và $(x_2, y_2)$, còn tọa độ góc dưới bên trái và góc trên bên phải của hình chữ nhật 2 lần lượt là $(x_3, y_3)$ và $(x_4, y_4)$.

Nếu hình chữ nhật 1 và hình chữ nhật 2 giao nhau, tọa độ của vùng giao là:

- Tọa độ x của góc dưới bên trái là giá trị lớn hơn giữa các tọa độ x của góc dưới bên trái của hai hình chữ nhật, tức là $\max(x_1, x_3)$;
- Tọa độ y của góc dưới bên trái là giá trị lớn hơn giữa các tọa độ y của góc dưới bên trái của hai hình chữ nhật, tức là $\max(y_1, y_3)$;
- Tọa độ x của góc trên bên phải là giá trị nhỏ hơn giữa các tọa độ x của góc trên bên phải của hai hình chữ nhật, tức là $\min(x_2, x_4)$;
- Tọa độ y của góc trên bên phải là giá trị nhỏ hơn giữa các tọa độ y của góc trên bên phải của hai hình chữ nhật, tức là $\min(y_2, y_4)$.

Khi đó, chiều rộng và chiều cao của vùng giao lần lượt là $w = \min(x_2, x_4) - \max(x_1, x_3)$ và $h = \min(y_2, y_4) - \max(y_1, y_3)$. Ta lấy giá trị nhỏ hơn trong hai giá trị này làm độ dài cạnh, tức là $e = \min(w, h)$. Nếu $e > 0$, ta có thể tạo được một hình vuông có diện tích $e^2$. Ta lấy diện tích lớn nhất trong tất cả các hình vuông.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là số hình chữ nhật. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestSquareArea(
        self, bottomLeft: List[List[int]], topRight: List[List[int]]
    ) -> int:
        ans = 0
        for ((x1, y1), (x2, y2)), ((x3, y3), (x4, y4)) in combinations(
            zip(bottomLeft, topRight), 2
        ):
            w = min(x2, x4) - max(x1, x3)
            h = min(y2, y4) - max(y1, y3)
            e = min(w, h)
            if e > 0:
                ans = max(ans, e * e)
        return ans
```

#### Java

```java
class Solution {
    public long largestSquareArea(int[][] bottomLeft, int[][] topRight) {
        long ans = 0;
        for (int i = 0; i < bottomLeft.length; ++i) {
            int x1 = bottomLeft[i][0], y1 = bottomLeft[i][1];
            int x2 = topRight[i][0], y2 = topRight[i][1];
            for (int j = i + 1; j < bottomLeft.length; ++j) {
                int x3 = bottomLeft[j][0], y3 = bottomLeft[j][1];
                int x4 = topRight[j][0], y4 = topRight[j][1];
                int w = Math.min(x2, x4) - Math.max(x1, x3);
                int h = Math.min(y2, y4) - Math.max(y1, y3);
                int e = Math.min(w, h);
                if (e > 0) {
                    ans = Math.max(ans, 1L * e * e);
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
    long long largestSquareArea(vector<vector<int>>& bottomLeft, vector<vector<int>>& topRight) {
        long long ans = 0;
        for (int i = 0; i < bottomLeft.size(); ++i) {
            int x1 = bottomLeft[i][0], y1 = bottomLeft[i][1];
            int x2 = topRight[i][0], y2 = topRight[i][1];
            for (int j = i + 1; j < bottomLeft.size(); ++j) {
                int x3 = bottomLeft[j][0], y3 = bottomLeft[j][1];
                int x4 = topRight[j][0], y4 = topRight[j][1];
                int w = min(x2, x4) - max(x1, x3);
                int h = min(y2, y4) - max(y1, y3);
                int e = min(w, h);
                if (e > 0) {
                    ans = max(ans, 1LL * e * e);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestSquareArea(bottomLeft [][]int, topRight [][]int) (ans int64) {
	for i, b1 := range bottomLeft {
		t1 := topRight[i]
		x1, y1 := b1[0], b1[1]
		x2, y2 := t1[0], t1[1]
		for j := i + 1; j < len(bottomLeft); j++ {
			x3, y3 := bottomLeft[j][0], bottomLeft[j][1]
			x4, y4 := topRight[j][0], topRight[j][1]
			w := min(x2, x4) - max(x1, x3)
			h := min(y2, y4) - max(y1, y3)
			e := min(w, h)
			if e > 0 {
				ans = max(ans, int64(e)*int64(e))
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function largestSquareArea(bottomLeft: number[][], topRight: number[][]): number {
    let ans = 0;
    for (let i = 0; i < bottomLeft.length; ++i) {
        const [x1, y1] = bottomLeft[i];
        const [x2, y2] = topRight[i];
        for (let j = i + 1; j < bottomLeft.length; ++j) {
            const [x3, y3] = bottomLeft[j];
            const [x4, y4] = topRight[j];
            const w = Math.min(x2, x4) - Math.max(x1, x3);
            const h = Math.min(y2, y4) - Math.max(y1, y3);
            const e = Math.min(w, h);
            if (e > 0) {
                ans = Math.max(ans, e * e);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn largest_square_area(bottom_left: Vec<Vec<i32>>, top_right: Vec<Vec<i32>>) -> i64 {
        let mut ans: i64 = 0;
        let n = bottom_left.len();

        for i in 0..n {
            let x1 = bottom_left[i][0];
            let y1 = bottom_left[i][1];
            let x2 = top_right[i][0];
            let y2 = top_right[i][1];

            for j in (i + 1)..n {
                let x3 = bottom_left[j][0];
                let y3 = bottom_left[j][1];
                let x4 = top_right[j][0];
                let y4 = top_right[j][1];

                let w = (x2.min(x4) - x1.max(x3)) as i64;
                let h = (y2.min(y4) - y1.max(y3)) as i64;
                let e = w.min(h);

                if e > 0 {
                    ans = ans.max(e * e);
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
