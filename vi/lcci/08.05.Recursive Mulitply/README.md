---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [08.05. Recursive Mulitply](https://leetcode.cn/problems/recursive-mulitply-lcci)

[中文文档](/lcci/08.05.Recursive%20Mulitply/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết một hàm đệ quy để nhân hai số nguyên dương mà không sử dụng toán tử *. Bạn có thể sử dụng phép cộng, phép trừ và phép dịch bit, nhưng cần giảm thiểu số lần thực hiện các phép toán đó.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Đầu vào</strong>: A = 1, B = 10

<strong> Đầu ra</strong>: 10

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Đầu vào</strong>: A = 3, B = 4

<strong> Đầu ra</strong>: 12

</pre>
<p><strong>Lưu ý:</strong></p>
<ol>
	<li>Kết quả sẽ không bị tràn số.</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy + Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Phép nhân $A$ với $B$ bị cấm. Cộng $A$ lặp lại là $O(B)$.
>
> $A\times B = 2A \times \lfloor B/2 \rfloor$, cộng thêm một $A$ khi $B$ là số lẻ. Mỗi bước giảm một nửa $B$, nên độ sâu là $O(\log B)$.
>
> Phép dịch thay thế $\times 2$ và $/2$; $B\&1$ dùng để kiểm tra tính lẻ. Trường hợp cơ sở $B=1$ trả về $A$.

<!-- thinking:end -->

Trước hết, ta kiểm tra xem $B$ có bằng $1$ hay không. Nếu có, ta trả về $A$ ngay.

Nếu không, ta kiểm tra xem $B$ có phải là số lẻ hay không. Nếu phải, ta có thể dịch phải $B$ một bit, sau đó gọi đệ quy hàm, cuối cùng dịch trái kết quả một bit và cộng thêm $A$. Nếu không, ta có thể dịch phải $B$ một bit, sau đó gọi đệ quy hàm, cuối cùng dịch trái kết quả một bit.

Độ phức tạp thời gian là $O(\log n)$, độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là kích thước của $B$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def multiply(self, A: int, B: int) -> int:
        if B == 1:
            return A
        if B & 1:
            return (self.multiply(A, B >> 1) << 1) + A
        return self.multiply(A, B >> 1) << 1
```

#### Java

```java
class Solution {
    public int multiply(int A, int B) {
        if (B == 1) {
            return A;
        }
        if ((B & 1) == 1) {
            return (multiply(A, B >> 1) << 1) + A;
        }
        return multiply(A, B >> 1) << 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int multiply(int A, int B) {
        if (B == 1) {
            return A;
        }
        if ((B & 1) == 1) {
            return (multiply(A, B >> 1) << 1) + A;
        }
        return multiply(A, B >> 1) << 1;
    }
};
```

#### Go

```go
func multiply(A int, B int) int {
	if B == 1 {
		return A
	}
	if B&1 == 1 {
		return (multiply(A, B>>1) << 1) + A
	}
	return multiply(A, B>>1) << 1
}
```

#### TypeScript

```ts
function multiply(A: number, B: number): number {
    if (B === 1) {
        return A;
    }
    if ((B & 1) === 1) {
        return (multiply(A, B >> 1) << 1) + A;
    }
    return multiply(A, B >> 1) << 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn multiply(a: i32, b: i32) -> i32 {
        if b == 1 {
            return a;
        }
        if (b & 1) == 1 {
            return (Self::multiply(a, b >> 1) << 1) + a;
        }
        Self::multiply(a, b >> 1) << 1
    }
}
```

#### Swift

```swift
class Solution {
    func multiply(_ A: Int, _ B: Int) -> Int {
        if B == 1 {
            return A
        }
        if (B & 1) == 1 {
            return (multiply(A, B >> 1) << 1) + A
        }
        return multiply(A, B >> 1) << 1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
