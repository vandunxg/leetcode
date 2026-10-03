---
comments: true
difficulty: Easy
rating: 1240
source: Weekly Contest 268 Q1
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2078. Two Furthest Houses With Different Colors](https://leetcode.com/problems/two-furthest-houses-with-different-colors)

[中文文档](/solution/2000-2099/2078.Two%20Furthest%20Houses%20With%20Different%20Colors/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> ngôi nhà được xếp thành một hàng đều nhau trên phố, và mỗi ngôi nhà đều được sơn đẹp mắt. Bạn được cho một mảng số nguyên <code>colors</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>, trong đó <code>colors[i]</code> biểu thị màu của ngôi nhà thứ <code>i<sup>th</sup></code>.</p>

<p>Trả về <em><strong>khoảng cách lớn nhất</strong> giữa <strong>hai</strong> ngôi nhà có <strong>màu khác nhau</strong></em>.</p>

<p>Khoảng cách giữa ngôi nhà thứ <code>i<sup>th</sup></code> và <code>j<sup>th</sup></code> là <code>abs(i - j)</code>, trong đó <code>abs(x)</code> là <strong>giá trị tuyệt đối</strong> của <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2078.Two%20Furthest%20Houses%20With%20Different%20Colors/images/eg1.png" style="width: 610px; height: 84px;" />
<pre>
<strong>Đầu vào:</strong> colors = [<u><strong>1</strong></u>,1,1,<strong><u>6</u></strong>,1,1,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong hình trên, màu 1 là xanh dương và màu 6 là đỏ.
Hai ngôi nhà xa nhau nhất có màu khác nhau là ngôi nhà 0 và ngôi nhà 3.
Ngôi nhà 0 có màu 1, còn ngôi nhà 3 có màu 6. Khoảng cách giữa chúng là abs(0 - 3) = 3.
Lưu ý rằng ngôi nhà 3 và ngôi nhà 6 cũng cho đáp án tối ưu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2078.Two%20Furthest%20Houses%20With%20Different%20Colors/images/eg2.png" style="width: 426px; height: 84px;" />
<pre>
<strong>Đầu vào:</strong> colors = [<u><strong>1</strong></u>,8,3,8,<u><strong>3</strong></u>]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Trong hình trên, màu 1 là xanh dương, màu 8 là vàng và màu 3 là xanh lá.
Hai ngôi nhà xa nhau nhất có màu khác nhau là ngôi nhà 0 và ngôi nhà 4.
Ngôi nhà 0 có màu 1, còn ngôi nhà 4 có màu 3. Khoảng cách giữa chúng là abs(0 - 4) = 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> colors = [<u><strong>0</strong></u>,<strong><u>1</u></strong>]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Hai ngôi nhà xa nhau nhất có màu khác nhau là ngôi nhà 0 và ngôi nhà 1.
Ngôi nhà 0 có màu 0, còn ngôi nhà 1 có màu 1. Khoảng cách giữa chúng là abs(0 - 1) = 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n ==&nbsp;colors.length</code></li>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= colors[i] &lt;= 100</code></li>
	<li>Dữ liệu kiểm thử được tạo sao cho <strong>ít nhất</strong> hai ngôi nhà có màu khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 100$ cho phép xét mọi cặp, nhưng nghiệm tối ưu luôn sử dụng một đầu mút: nếu hai đầu mút có cùng màu, hai ngôi nhà khác màu xa nhau nhất sẽ ghép với đầu mút còn lại.
>
> So sánh $colors[0]$ và $colors[-1]$; nếu chúng bằng nhau, ta đi từ hai phía vào trong đến ngôi nhà đầu tiên có màu khác và lấy khoảng cách lớn hơn.

<!-- thinking:end -->

Ta có thể nhận thấy rằng nếu ngôi nhà đầu tiên và ngôi nhà cuối cùng có màu khác nhau, khoảng cách lớn nhất là $n - 1$.

Nếu ngôi nhà đầu tiên và ngôi nhà cuối cùng có cùng màu, ta có thể duyệt từ trái sang để tìm ngôi nhà đầu tiên có màu khác (gọi chỉ số của nó là $i$), đồng thời duyệt từ phải sang để tìm ngôi nhà đầu tiên có màu khác (gọi chỉ số của nó là $j$). Khi đó, khoảng cách lớn nhất là $\max(n - i - 1, j)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số ngôi nhà. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, colors: List[int]) -> int:
        n = len(colors)
        if colors[0] != colors[-1]:
            return n - 1
        i, j = 1, n - 2
        while colors[i] == colors[0]:
            i += 1
        while colors[j] == colors[0]:
            j -= 1
        return max(n - i - 1, j)
```

#### Java

```java
class Solution {
    public int maxDistance(int[] colors) {
        int n = colors.length;
        if (colors[0] != colors[n - 1]) {
            return n - 1;
        }
        int i = 1, j = n - 2;
        while (colors[i] == colors[0]) {
            ++i;
        }
        while (colors[j] == colors[0]) {
            --j;
        }
        return Math.max(n - i - 1, j);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(vector<int>& colors) {
        int n = colors.size();
        if (colors[0] != colors[n - 1]) {
            return n - 1;
        }
        int i = 1, j = n - 2;
        while (colors[i] == colors[0]) {
            ++i;
        }
        while (colors[j] == colors[0]) {
            --j;
        }
        return max(n - i - 1, j);
    }
};
```

#### Go

```go
func maxDistance(colors []int) int {
	n := len(colors)
	if colors[0] != colors[n-1] {
		return n - 1
	}
	i, j := 1, n-2
	for colors[i] == colors[0] {
		i++
	}
	for colors[j] == colors[0] {
		j--
	}
	return max(n-i-1, j)
}
```

#### TypeScript

```ts
function maxDistance(colors: number[]): number {
    const n = colors.length;
    if (colors[0] !== colors[n - 1]) {
        return n - 1;
    }
    let [i, j] = [1, n - 2];
    while (colors[i] === colors[0]) {
        i++;
    }
    while (colors[j] === colors[0]) {
        j--;
    }
    return Math.max(n - i - 1, j);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_distance(colors: Vec<i32>) -> i32 {
        let n = colors.len();
        if colors[0] != colors[n - 1] {
            return (n - 1) as i32;
        }
        let mut i = 1;
        while colors[i] == colors[0] {
            i += 1;
        }
        let mut j = n - 2;
        while colors[j] == colors[0] {
            j -= 1;
        }
        std::cmp::max((n - i - 1) as i32, j as i32)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
