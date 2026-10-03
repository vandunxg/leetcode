---
comments: true
difficulty: Medium
rating: 1532
source: Weekly Contest 313 Q3
tags:
    - Greedy
    - Bit Manipulation
---

<!-- problem:start -->

# [2429. Minimize XOR](https://leetcode.com/problems/minimize-xor)

[中文文档](/solution/2400-2499/2429.Minimize%20XOR/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>num1</code> và <code>num2</code>, hãy tìm số nguyên dương <code>x</code> sao cho:</p>

<ul>
	<li><code>x</code> có cùng số bit 1 với <code>num2</code>, và</li>
	<li>Giá trị <code>x XOR num1</code> là <strong>nhỏ nhất</strong>.</li>
</ul>

<p>Lưu ý rằng <code>XOR</code> là phép XOR theo bit.</p>

<p>Trả về <em>số nguyên </em><code>x</code>. Các test case được tạo sao cho <code>x</code> được <strong>xác định duy nhất</strong>.</p>

<p>Số <strong>bit 1</strong> của một số nguyên là số lượng <code>1</code> trong biểu diễn nhị phân của số đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = 3, num2 = 5
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Biểu diễn nhị phân của num1 và num2 lần lượt là 0011 và 0101.
Số nguyên <strong>3</strong> có cùng số bit 1 với num2, và giá trị <code>3 XOR 3 = 0</code> là nhỏ nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = 1, num2 = 12
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Biểu diễn nhị phân của num1 và num2 lần lượt là 0001 và 1100.
Số nguyên <strong>3</strong> có cùng số bit 1 với num2, và giá trị <code>3 XOR 1 = 2</code> là nhỏ nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num1, num2 &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> $x$ phải có cùng số bit 1 với $num2$ và tối thiểu hóa $x\oplus num1$, vì vậy $x$ nên sử dụng lại các bit $1$ ở vị trí cao của $num1$. Do có nhiều nhất $31$ bit, ta có thể tham lam theo từng vị trí.
>
> Trước tiên chọn các bit $1$ của $num1$ từ cao xuống thấp; nếu còn vị trí, điền các bit $0$ của $num1$ từ thấp lên cao để XOR không tăng ở các bit cao.

<!-- thinking:end -->

Theo mô tả đề bài, trước tiên ta tính số bit 1 của $\textit{num2}$, ký hiệu là $\textit{cnt}$. Sau đó, ta duyệt các bit của $\textit{num1}$ từ cao xuống thấp; nếu bit hiện tại là $1$, ta đặt bit tương ứng trong $x$ thành $1$ và giảm $\textit{cnt}$, cho đến khi $\textit{cnt}$ bằng $0$. Nếu $\textit{cnt}$ vẫn khác $0$, ta duyệt từ bit thấp lên, đặt các vị trí mà $\textit{num1}$ có bit $0$ thành $1$ trong $x$, đồng thời giảm $\textit{cnt}$ cho đến khi bằng $0$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là giá trị lớn nhất của $\textit{num1}$ và $\textit{num2}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeXor(self, num1: int, num2: int) -> int:
        cnt = num2.bit_count()
        x = 0
        for i in range(30, -1, -1):
            if num1 >> i & 1 and cnt:
                x |= 1 << i
                cnt -= 1
        for i in range(30):
            if num1 >> i & 1 ^ 1 and cnt:
                x |= 1 << i
                cnt -= 1
        return x
```

#### Java

```java
class Solution {
    public int minimizeXor(int num1, int num2) {
        int cnt = Integer.bitCount(num2);
        int x = 0;
        for (int i = 30; i >= 0 && cnt > 0; --i) {
            if ((num1 >> i & 1) == 1) {
                x |= 1 << i;
                --cnt;
            }
        }
        for (int i = 0; cnt > 0; ++i) {
            if ((num1 >> i & 1) == 0) {
                x |= 1 << i;
                --cnt;
            }
        }
        return x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimizeXor(int num1, int num2) {
        int cnt = __builtin_popcount(num2);
        int x = 0;
        for (int i = 30; ~i && cnt; --i) {
            if (num1 >> i & 1) {
                x |= 1 << i;
                --cnt;
            }
        }
        for (int i = 0; cnt; ++i) {
            if (num1 >> i & 1 ^ 1) {
                x |= 1 << i;
                --cnt;
            }
        }
        return x;
    }
};
```

#### Go

```go
func minimizeXor(num1 int, num2 int) int {
	cnt := bits.OnesCount(uint(num2))
	x := 0
	for i := 30; i >= 0 && cnt > 0; i-- {
		if num1>>i&1 == 1 {
			x |= 1 << i
			cnt--
		}
	}
	for i := 0; cnt > 0; i++ {
		if num1>>i&1 == 0 {
			x |= 1 << i
			cnt--
		}
	}
	return x
}
```

#### TypeScript

```ts
function minimizeXor(num1: number, num2: number): number {
    let cnt = 0;
    while (num2) {
        num2 &= num2 - 1;
        ++cnt;
    }
    let x = 0;
    for (let i = 30; i >= 0 && cnt > 0; --i) {
        if ((num1 >> i) & 1) {
            x |= 1 << i;
            --cnt;
        }
    }
    for (let i = 0; cnt > 0; ++i) {
        if (!((num1 >> i) & 1)) {
            x |= 1 << i;
            --cnt;
        }
    }
    return x;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimize_xor(num1: i32, mut num2: i32) -> i32 {
        let mut cnt = 0;
        while num2 > 0 {
            num2 -= num2 & -num2;
            cnt += 1;
        }
        let mut x = 0;
        let mut c = cnt;
        for i in (0..=30).rev() {
            if c > 0 && (num1 >> i) & 1 == 1 {
                x |= 1 << i;
                c -= 1;
            }
        }
        for i in 0..=30 {
            if c == 0 {
                break;
            }
            if ((num1 >> i) & 1) == 0 {
                x |= 1 << i;
                c -= 1;
            }
        }
        x
    }
}
```

#### C#

```cs
public class Solution {
    public int MinimizeXor(int num1, int num2) {
        int cnt = BitOperations.PopCount((uint)num2);
        int x = 0;
        for (int i = 30; i >= 0 && cnt > 0; --i) {
            if (((num1 >> i) & 1) == 1) {
                x |= 1 << i;
                --cnt;
            }
        }
        for (int i = 0; cnt > 0; ++i) {
            if (((num1 >> i) & 1) == 0) {
                x |= 1 << i;
                --cnt;
            }
        }
        return x;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
