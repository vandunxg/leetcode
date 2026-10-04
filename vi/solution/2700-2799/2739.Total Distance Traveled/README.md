---
comments: true
difficulty: Easy
rating: 1262
source: Weekly Contest 350 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [2739. Total Distance Traveled](https://leetcode.com/problems/total-distance-traveled)

[中文文档](/solution/2700-2799/2739.Total%20Distance%20Traveled/README.md)

## Mô tả

<!-- description:start -->

<p>Một chiếc xe tải có hai bình nhiên liệu. Cho hai số nguyên <code>mainTank</code> biểu diễn lượng nhiên liệu tính bằng lít trong bình chính và <code>additionalTank</code> biểu diễn lượng nhiên liệu tính bằng lít trong bình phụ.</p>

<p>Xe tải có mức tiêu hao nhiên liệu là <code>10</code> km cho mỗi lít. Mỗi khi bình chính đã dùng hết <code>5</code> lít nhiên liệu, nếu bình phụ còn ít nhất <code>1</code> lít nhiên liệu thì <code>1</code> lít nhiên liệu sẽ được chuyển từ bình phụ sang bình chính.</p>

<p>Trả về <em>quãng đường lớn nhất có thể di chuyển được</em>.</p>

<p><strong>Lưu ý: </strong>Việc tiếp nhiên liệu từ bình phụ không diễn ra liên tục. Nó xảy ra đột ngột và ngay lập tức sau mỗi 5 lít được tiêu thụ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> mainTank = 5, additionalTank = 10
<strong>Đầu ra:</strong> 60
<strong>Giải thích:</strong>
Sau khi dùng hết 5 lít nhiên liệu, lượng nhiên liệu còn lại là (5 - 5 + 1) = 1 lít và quãng đường đã đi được là 50km.
Sau khi dùng thêm 1 lít nhiên liệu, không có nhiên liệu nào được chuyển vào bình chính và bình chính trở nên trống rỗng.
Tổng quãng đường đã đi được là 60km.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mainTank = 1, additionalTank = 2
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
Sau khi dùng hết 1 lít nhiên liệu, bình chính trở nên trống rỗng.
Tổng quãng đường đã đi được là 10km.

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= mainTank, additionalTank &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Cứ mỗi $5$ lít từ bình chính được sử dụng, một lít sẽ được chuyển từ bình phụ nếu bình phụ còn nhiên liệu; mỗi lít giúp xe đi được $10$ km. Hai bình có lượng nhiên liệu đủ nhỏ để ta có thể mô phỏng theo từng lít.
>
> Ta dùng một vòng lặp để tiêu thụ nhiên liệu trong bình chính: cộng thêm $10$ km và cứ sau mỗi năm lít thì chuyển một lít từ bình phụ, cho đến khi bình chính cạn nhiên liệu.

<!-- thinking:end -->

Ta có thể mô phỏng quá trình xe di chuyển. Mỗi lần, xe tiêu thụ 1 lít nhiên liệu từ bình chính và đi được 10 km. Mỗi khi bình chính đã tiêu thụ 5 lít, nếu bình phụ còn nhiên liệu thì 1 lít nhiên liệu sẽ được chuyển sang bình chính. Quá trình mô phỏng tiếp tục cho đến khi bình chính cạn nhiên liệu.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là lượng nhiên liệu trong bình chính và bình phụ. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distanceTraveled(self, mainTank: int, additionalTank: int) -> int:
        ans = cur = 0
        while mainTank:
            cur += 1
            ans += 10
            mainTank -= 1
            if cur % 5 == 0 and additionalTank:
                additionalTank -= 1
                mainTank += 1
        return ans
```

#### Java

```java
class Solution {
    public int distanceTraveled(int mainTank, int additionalTank) {
        int ans = 0, cur = 0;
        while (mainTank > 0) {
            cur++;
            ans += 10;
            mainTank--;
            if (cur % 5 == 0 && additionalTank > 0) {
                additionalTank--;
                mainTank++;
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
    int distanceTraveled(int mainTank, int additionalTank) {
        int ans = 0, cur = 0;
        while (mainTank > 0) {
            cur++;
            ans += 10;
            mainTank--;
            if (cur % 5 == 0 && additionalTank > 0) {
                additionalTank--;
                mainTank++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func distanceTraveled(mainTank int, additionalTank int) (ans int) {
	cur := 0
	for mainTank > 0 {
		cur++
		ans += 10
		mainTank--
		if cur%5 == 0 && additionalTank > 0 {
			additionalTank--
			mainTank++
		}
	}
	return
}
```

#### Rust

```rust
impl Solution {
    pub fn distance_traveled(mut main_tank: i32, mut additional_tank: i32) -> i32 {
        let mut cur = 0;
        let mut ans = 0;

        while main_tank > 0 {
            cur += 1;
            main_tank -= 1;
            ans += 10;

            if cur % 5 == 0 && additional_tank > 0 {
                additional_tank -= 1;
                main_tank += 1;
            }
        }

        ans
    }
}
```

#### JavaScript

```js
var distanceTraveled = function (mainTank, additionalTank) {
    let ans = 0,
        cur = 0;
    while (mainTank) {
        cur++;
        ans += 10;
        mainTank--;
        if (cur % 5 === 0 && additionalTank) {
            additionalTank--;
            mainTank++;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
