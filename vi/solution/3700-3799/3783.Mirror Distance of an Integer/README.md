---
comments: true
difficulty: Easy
rating: 1170
source: Weekly Contest 481 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3783. Mirror Distance of an Integer](https://leetcode.com/problems/mirror-distance-of-an-integer)

[Tài liệu tiếng Trung](/solution/3700-3799/3783.Mirror%20Distance%20of%20an%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Định nghĩa <strong>khoảng cách đối xứng</strong> của nó là: <code>abs(n - reverse(n))</code>​​​​​​​ trong đó <code>reverse(n)</code> là số nguyên tạo thành bằng cách đảo ngược các chữ số của <code>n</code>.</p>

<p>Hãy trả về một số nguyên biểu thị khoảng cách đối xứng của <code>n</code>​​​​​​​.</p>

<p><code>abs(x)</code> biểu thị giá trị tuyệt đối của <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 25</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">27</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>reverse(25) = 52</code>.</li>
	<li>Do đó, đáp án là <code>abs(25 - 52) = 27</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>reverse(10) = 01</code>, tức là 1.</li>
	<li>Do đó, đáp án là <code>abs(10 - 1) = 9</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>reverse(7) = 7</code>.</li>
	<li>Do đó, đáp án là <code>abs(7 - 7) = 0</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách đối xứng là hiệu tuyệt đối giữa $n$ và số nguyên thu được bằng cách đảo ngược các chữ số của nó (các số 0 ở đầu sẽ biến mất). Có thể thực hiện thao tác đảo ngược này bằng cách đảo ngược chuỗi biểu diễn ở hệ thập phân hoặc lần lượt lấy chữ số cuối để tạo thành một số nguyên mới.

<!-- thinking:end -->

Ta định nghĩa một hàm $\text{reverse}(x)$ để đảo ngược các chữ số của số nguyên $x$. Cụ thể, ta khởi tạo biến $y$ bằng $0$, sau đó liên tục nối chữ số cuối của $x$ vào cuối $y$ và xóa chữ số cuối của $x$ cho đến khi $x$ trở thành $0$. Cuối cùng, $y$ chính là số nguyên đảo ngược.

Tiếp theo, ta tính khoảng cách đối xứng của số nguyên $n$, là $\text{abs}(n - \text{reverse}(n))$, rồi trả về kết quả.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là kích thước của số nguyên đầu vào.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mirrorDistance(self, n: int) -> int:
        return abs(n - int(str(n)[::-1]))
```

#### Java

```java
class Solution {
    public int mirrorDistance(int n) {
        return Math.abs(n - reverse(n));
    }

    private int reverse(int x) {
        int y = 0;
        for (; x > 0; x /= 10) {
            y = y * 10 + x % 10;
        }
        return y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mirrorDistance(int n) {
        auto reverse = [](int x) -> int {
            int y = 0;
            for (; x; x /= 10) {
                y = y * 10 + x % 10;
            }
            return y;
        };
        return abs(n - reverse(n));
    }
};
```

#### Go

```go
func mirrorDistance(n int) int {
	reverse := func(x int) int {
		y := 0
		for ; x > 0; x /= 10 {
			y = y*10 + x%10
		}
		return y
	}
	return abs(n - reverse(n))
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function mirrorDistance(n: number): number {
    const reverse = (x: number): number => {
        let y = 0;
        for (; x > 0; x = Math.floor(x / 10)) {
            y = y * 10 + (x % 10);
        }
        return y;
    };
    return Math.abs(n - reverse(n));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
