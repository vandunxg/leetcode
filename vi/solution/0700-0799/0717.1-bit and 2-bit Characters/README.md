---
comments: true
difficulty: Easy
tags:
    - Array
---

<!-- problem:start -->

# [717. 1-bit and 2-bit Characters](https://leetcode.com/problems/1-bit-and-2-bit-characters)

[中文文档](/solution/0700-0799/0717.1-bit%20and%202-bit%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Ta có hai ký tự đặc biệt:</p>

<ul>
	<li>Ký tự thứ nhất được biểu diễn bằng một bit <code>0</code>.</li>
	<li>Ký tự thứ hai được biểu diễn bằng hai bit (<code>10</code> hoặc <code>11</code>).</li>
</ul>

<p>Cho mảng nhị phân <code>bits</code> có phần tử cuối là <code>0</code>, hãy trả về <code>true</code> nếu ký tự cuối bắt buộc là ký tự một bit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> bits = [1,0,0]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Cách duy nhất để giải mã là một ký tự hai bit rồi đến một ký tự một bit.
Vì vậy, ký tự cuối là ký tự một bit.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> bits = [1,1,1,0]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Cách duy nhất để giải mã là hai ký tự hai bit.
Vì vậy, ký tự cuối không phải ký tự một bit.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= bits.length &lt;= 1000</code></li>
	<li><code>bits[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Các ký tự được mã hóa bằng $0$ và $10$/$11$. Ta cần xác định bit cuối có phải là ký tự một bit không. Cách giải mã từ trái sang phải là duy nhất nên không cần thử nhiều khả năng.
>
> Bit $0$ luôn tạo thành ký tự một bit; bit $1$ luôn tiêu thụ thêm bit kế tiếp. Mô phỏng đến ngay trước vị trí cuối rồi kiểm tra xem ta có dừng đúng tại đó không.
>
> Khi $i<n-1$, tăng $i$ thêm $\textit{bits}[i]+1$. Khi kết thúc, $i=n-1$ khi và chỉ khi bit cuối đứng riêng thành một ký tự.

<!-- thinking:end -->

Ta có thể duyệt trực tiếp $n-1$ phần tử đầu của mảng $\textit{bits}$, mỗi lần quyết định số phần tử cần bỏ qua dựa trên giá trị phần tử hiện tại:

- Nếu phần tử hiện tại là $0$, bỏ qua $1$ phần tử (tương ứng với ký tự một bit);
- Nếu phần tử hiện tại là $1$, bỏ qua $2$ phần tử (tương ứng với ký tự hai bit).

Khi kết thúc duyệt, nếu chỉ số hiện tại bằng $n-1$, ký tự cuối là ký tự một bit nên ta trả về $\text{true}$; nếu không thì trả về $\text{false}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{bits}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isOneBitCharacter(self, bits: List[int]) -> bool:
        i, n = 0, len(bits)
        while i < n - 1:
            i += bits[i] + 1
        return i == n - 1
```

#### Java

```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int i = 0, n = bits.length;
        while (i < n - 1) {
            i += bits[i] + 1;
        }
        return i == n - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        int i = 0, n = bits.size();
        while (i < n - 1) {
            i += bits[i] + 1;
        }
        return i == n - 1;
    }
};
```

#### Go

```go
func isOneBitCharacter(bits []int) bool {
	i, n := 0, len(bits)
	for i < n-1 {
		i += bits[i] + 1
	}
	return i == n-1
}
```

#### TypeScript

```ts
function isOneBitCharacter(bits: number[]): boolean {
    let i = 0;
    const n = bits.length;
    while (i < n - 1) {
        i += bits[i] + 1;
    }
    return i === n - 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_one_bit_character(bits: Vec<i32>) -> bool {
        let mut i = 0usize;
        let n = bits.len();
        while i < n - 1 {
            i += (bits[i] + 1) as usize;
        }
        i == n - 1
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} bits
 * @return {boolean}
 */
var isOneBitCharacter = function (bits) {
    let i = 0;
    const n = bits.length;
    while (i < n - 1) {
        i += bits[i] + 1;
    }
    return i === n - 1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
