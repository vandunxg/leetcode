---
comments: true
difficulty: Medium
rating: 1444
source: Weekly Contest 191 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1465. Maximum Area of a Piece of Cake After Horizontal and Vertical Cuts](https://leetcode.com/problems/maximum-area-of-a-piece-of-cake-after-horizontal-and-vertical-cuts)

[中文文档](/solution/1400-1499/1465.Maximum%20Area%20of%20a%20Piece%20of%20Cake%20After%20Horizontal%20and%20Vertical%20Cuts/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chiếc bánh hình chữ nhật có kích thước <code>h x w</code> và hai mảng số nguyên <code>horizontalCuts</code> và <code>verticalCuts</code>, trong đó:</p>

<ul>
	<li><code>horizontalCuts[i]</code> là khoảng cách từ đỉnh của chiếc bánh hình chữ nhật đến đường cắt ngang thứ <code>i<sup>th</sup></code>, tương tự như vậy, và</li>
	<li><code>verticalCuts[j]</code> là khoảng cách từ cạnh trái của chiếc bánh hình chữ nhật đến đường cắt dọc thứ <code>j<sup>th</sup></code>.</li>
</ul>

<p>Hãy trả về <em>diện tích lớn nhất của một miếng bánh sau khi cắt tại mọi vị trí ngang và dọc được cung cấp trong hai mảng</em> <code>horizontalCuts</code> <em>và</em> <code>verticalCuts</code>. Vì đáp án có thể là một số lớn, hãy trả về <strong>phần dư</strong> của đáp án khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1465.Maximum%20Area%20of%20a%20Piece%20of%20Cake%20After%20Horizontal%20and%20Vertical%20Cuts/images/leetcode_max_area_2.png" style="width: 225px; height: 240px;" />
<pre>
<strong>Đầu vào:</strong> h = 5, w = 4, horizontalCuts = [1,2,4], verticalCuts = [1,3]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Hình phía trên biểu diễn chiếc bánh hình chữ nhật đã cho. Các đường màu đỏ là các đường cắt ngang và dọc. Sau khi cắt bánh, miếng bánh màu xanh lá có diện tích lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1465.Maximum%20Area%20of%20a%20Piece%20of%20Cake%20After%20Horizontal%20and%20Vertical%20Cuts/images/leetcode_max_area_3.png" style="width: 225px; height: 240px;" />
<pre>
<strong>Đầu vào:</strong> h = 5, w = 4, horizontalCuts = [3,1], verticalCuts = [1]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Hình phía trên biểu diễn chiếc bánh hình chữ nhật đã cho. Các đường màu đỏ là các đường cắt ngang và dọc. Sau khi cắt bánh, các miếng bánh màu xanh lá và vàng có diện tích lớn nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> h = 5, w = 4, horizontalCuts = [3], verticalCuts = [3]
<strong>Đầu ra:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= h, w &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= horizontalCuts.length &lt;= min(h - 1, 10<sup>5</sup>)</code></li>
	<li><code>1 &lt;= verticalCuts.length &lt;= min(w - 1, 10<sup>5</sup>)</code></li>
	<li><code>1 &lt;= horizontalCuts[i] &lt; h</code></li>
	<li><code>1 &lt;= verticalCuts[i] &lt; w</code></li>
	<li>Tất cả các phần tử trong <code>horizontalCuts</code> đều khác nhau.</li>
	<li>Tất cả các phần tử trong <code>verticalCuts</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Miếng lớn nhất có diện tích bằng tích của khoảng cách ngang lớn nhất và khoảng cách dọc lớn nhất. Sắp xếp các đường cắt, thêm các biên $0$ và $h$/$w$, lấy khoảng cách lớn nhất giữa các phần tử liền kề, rồi nhân chúng và lấy phần dư modulo $10^9+7$.

<!-- thinking:end -->

Trước tiên, ta lần lượt sắp xếp `horizontalCuts` và `verticalCuts`, sau đó duyệt qua hai mảng để tính hiệu lớn nhất giữa các phần tử liền kề. Gọi hai hiệu lớn nhất này lần lượt là $x$ và $y$. Cuối cùng, ta trả về $x \times y$.

Cần lưu ý rằng ta phải xét các trường hợp biên, tức là phần tử đầu tiên và phần tử cuối cùng của `horizontalCuts` và `verticalCuts`.

Độ phức tạp thời gian là $O(m\log m + n\log n)$, trong đó $m$ và $n$ lần lượt là độ dài của `horizontalCuts` và `verticalCuts`. Độ phức tạp không gian là $O(\log m + \log n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxArea(
        self, h: int, w: int, horizontalCuts: List[int], verticalCuts: List[int]
    ) -> int:
        horizontalCuts.extend([0, h])
        verticalCuts.extend([0, w])
        horizontalCuts.sort()
        verticalCuts.sort()
        x = max(b - a for a, b in pairwise(horizontalCuts))
        y = max(b - a for a, b in pairwise(verticalCuts))
        return (x * y) % (10**9 + 7)
```

#### Java

```java
class Solution {
    public int maxArea(int h, int w, int[] horizontalCuts, int[] verticalCuts) {
        final int mod = (int) 1e9 + 7;
        Arrays.sort(horizontalCuts);
        Arrays.sort(verticalCuts);
        int m = horizontalCuts.length;
        int n = verticalCuts.length;
        long x = Math.max(horizontalCuts[0], h - horizontalCuts[m - 1]);
        long y = Math.max(verticalCuts[0], w - verticalCuts[n - 1]);
        for (int i = 1; i < m; ++i) {
            x = Math.max(x, horizontalCuts[i] - horizontalCuts[i - 1]);
        }
        for (int i = 1; i < n; ++i) {
            y = Math.max(y, verticalCuts[i] - verticalCuts[i - 1]);
        }
        return (int) ((x * y) % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxArea(int h, int w, vector<int>& horizontalCuts, vector<int>& verticalCuts) {
        horizontalCuts.push_back(0);
        horizontalCuts.push_back(h);
        verticalCuts.push_back(0);
        verticalCuts.push_back(w);
        sort(horizontalCuts.begin(), horizontalCuts.end());
        sort(verticalCuts.begin(), verticalCuts.end());
        int x = 0, y = 0;
        for (int i = 1; i < horizontalCuts.size(); ++i) {
            x = max(x, horizontalCuts[i] - horizontalCuts[i - 1]);
        }
        for (int i = 1; i < verticalCuts.size(); ++i) {
            y = max(y, verticalCuts[i] - verticalCuts[i - 1]);
        }
        const int mod = 1e9 + 7;
        return (1ll * x * y) % mod;
    }
};
```

#### Go

```go
func maxArea(h int, w int, horizontalCuts []int, verticalCuts []int) int {
	horizontalCuts = append(horizontalCuts, []int{0, h}...)
	verticalCuts = append(verticalCuts, []int{0, w}...)
	sort.Ints(horizontalCuts)
	sort.Ints(verticalCuts)
	x, y := 0, 0
	const mod int = 1e9 + 7
	for i := 1; i < len(horizontalCuts); i++ {
		x = max(x, horizontalCuts[i]-horizontalCuts[i-1])
	}
	for i := 1; i < len(verticalCuts); i++ {
		y = max(y, verticalCuts[i]-verticalCuts[i-1])
	}
	return (x * y) % mod
}
```

#### TypeScript

```ts
function maxArea(h: number, w: number, horizontalCuts: number[], verticalCuts: number[]): number {
    const mod = 1e9 + 7;
    horizontalCuts.push(0, h);
    verticalCuts.push(0, w);
    horizontalCuts.sort((a, b) => a - b);
    verticalCuts.sort((a, b) => a - b);
    let [x, y] = [0, 0];
    for (let i = 1; i < horizontalCuts.length; i++) {
        x = Math.max(x, horizontalCuts[i] - horizontalCuts[i - 1]);
    }
    for (let i = 1; i < verticalCuts.length; i++) {
        y = Math.max(y, verticalCuts[i] - verticalCuts[i - 1]);
    }
    return Number((BigInt(x) * BigInt(y)) % BigInt(mod));
}
```

#### Rust

```rust
impl Solution {
    pub fn max_area(
        h: i32,
        w: i32,
        mut horizontal_cuts: Vec<i32>,
        mut vertical_cuts: Vec<i32>,
    ) -> i32 {
        const MOD: i64 = 1_000_000_007;

        horizontal_cuts.sort();
        vertical_cuts.sort();

        let m = horizontal_cuts.len();
        let n = vertical_cuts.len();

        let mut x = i64::max(
            horizontal_cuts[0] as i64,
            (h as i64) - (horizontal_cuts[m - 1] as i64),
        );
        let mut y = i64::max(
            vertical_cuts[0] as i64,
            (w as i64) - (vertical_cuts[n - 1] as i64),
        );

        for i in 1..m {
            x = i64::max(
                x,
                (horizontal_cuts[i] as i64) - (horizontal_cuts[i - 1] as i64),
            );
        }

        for i in 1..n {
            y = i64::max(y, (vertical_cuts[i] as i64) - (vertical_cuts[i - 1] as i64));
        }

        ((x * y) % MOD) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
