---
comments: true
difficulty: Medium
tags:
    - Math
    - Rejection Sampling
    - Probability and Statistics
    - Randomized
---

<!-- problem:start -->

# [470. Implement Rand10() Using Rand7()](https://leetcode.com/problems/implement-rand10-using-rand7)

[中文文档](/solution/0400-0499/0470.Implement%20Rand10%28%29%20Using%20Rand7%28%29/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <strong>API</strong> <code>rand7()</code> tạo số nguyên ngẫu nhiên đồng đều trong khoảng <code>[1, 7]</code>, hãy viết hàm <code>rand10()</code> tạo số nguyên ngẫu nhiên đồng đều trong khoảng <code>[1, 10]</code>. Bạn chỉ được gọi API <code>rand7()</code>, không được gọi API nào khác. Vui lòng <strong>không</strong> sử dụng API random có sẵn của ngôn ngữ lập trình.</p>

<p>Mỗi test case có một đối số <strong>nội bộ</strong> <code>n</code>, cho biết hàm <code>rand10()</code> bạn cài đặt sẽ được gọi bao nhiêu lần trong quá trình kiểm thử. Lưu ý, đây <strong>không phải đối số</strong> được truyền vào <code>rand10()</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> [2]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> [2,8]
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> [3,8,10]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Giá trị <a href="https://en.wikipedia.org/wiki/Expected_value" target="_blank">kỳ vọng</a> của số lần gọi hàm <code>rand7()</code> là bao nhiêu?</li>
	<li>Bạn có thể giảm thiểu số lần gọi <code>rand7()</code> không?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần chuyển $rand7$ đồng đều thành $rand10$ đồng đều. Phép tính $\textit{rand7}\bmod 10$ bị lệch xác suất. Hai lần gọi cho ta một số nguyên phân bố đồng đều trong $[1,49]$.
>
> Rejection sampling giữ lại các giá trị trong $[1,40]$ rồi trả về $x\bmod 10+1$; các giá trị từ $41$ đến $49$ sẽ được lấy lại. Vì $40$ chia hết cho $10$, mỗi phần dư xuất hiện đúng bốn lần.
>
> Số lần gọi kỳ vọng là hằng số. Việc loại bỏ phần đuôi giúp mọi kết quả có xác suất như nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# The rand7() API is already defined for you.
# def rand7():
# @return a random integer in the range 1 to 7


class Solution:
    def rand10(self):
        """
        :rtype: int
        """
        while 1:
            i = rand7() - 1
            j = rand7()
            x = i * 7 + j
            if x <= 40:
                return x % 10 + 1
```

#### Java

```java
/**
 * The rand7() API is already defined in the parent class SolBase.
 * public int rand7();
 * @return a random integer in the range 1 to 7
 */
class Solution extends SolBase {
    public int rand10() {
        while (true) {
            int i = rand7() - 1;
            int j = rand7();
            int x = i * 7 + j;
            if (x <= 40) {
                return x % 10 + 1;
            }
        }
    }
}
```

#### C++

```cpp
// The rand7() API is already defined for you.
// int rand7();
// @return a random integer in the range 1 to 7

class Solution {
public:
    int rand10() {
        while (1) {
            int i = rand7() - 1;
            int j = rand7();
            int x = i * 7 + j;
            if (x <= 40) {
                return x % 10 + 1;
            }
        }
    }
};
```

#### Go

```go
func rand10() int {
	for {
		i := rand7() - 1
		j := rand7()
		x := i*7 + j
		if x <= 40 {
			return x%10 + 1
		}
	}
}
```

#### TypeScript

```ts
/**
 * The rand7() API is already defined for you.
 * function rand7(): number {}
 * @return a random integer in the range 1 to 7
 */

function rand10(): number {
    while (true) {
        const i = rand7() - 1;
        const j = rand7();
        const x = i * 7 + j;
        if (x <= 40) {
            return (x % 10) + 1;
        }
    }
}
```

#### Rust

```rust
/**
 * The rand7() API is already defined for you.
 * @return a random integer in the range 1 to 7
 * fn rand7() -> i32;
 */

impl Solution {
    pub fn rand10() -> i32 {
        loop {
            let i = rand7() - 1;
            let j = rand7();
            let x = i * 7 + j;
            if x <= 40 {
                return (x % 10) + 1;
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
