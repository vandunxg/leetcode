---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [05.06. Convert Integer](https://leetcode.cn/problems/convert-integer-lcci)

[中文文档](/lcci/05.06.Convert%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm xác định số bit cần lật để chuyển số nguyên A thành số nguyên B.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>



<strong> Đầu vào</strong>: A = 29 (0b11101), B = 15 (0b01111)



<strong> Đầu ra</strong>: 2



</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>



<strong> Đầu vào</strong>: A = 1，B = 2



<strong> Đầu ra</strong>: 2



</pre>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li><code>-2147483648 &lt;= A, B &lt;= 2147483647</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Để chuyển $A$ thành $B$, ta lật mọi bit khác nhau giữa chúng. Có thể duyệt qua các bit trong $32$ bước.
>
> Các bit đó chính là những bit bằng $1$ trong $A\oplus B$, vì vậy đáp án là Hamming weight của phép XOR.
>
> Áp dụng mask `0xFFFFFFFF` cho cả hai số để các giá trị âm có cùng cách biểu diễn unsigned $32$ bit, sau đó dùng `bit_count`.

<!-- thinking:end -->

Ta thực hiện phép XOR theo bit trên A và B. Số lượng bit bằng $1$ trong kết quả chính là số bit cần thay đổi.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là giá trị lớn nhất của A và B. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def convertInteger(self, A: int, B: int) -> int:
        A &= 0xFFFFFFFF
        B &= 0xFFFFFFFF
        return (A ^ B).bit_count()
```

#### Java

```java
class Solution {
    public int convertInteger(int A, int B) {
        return Integer.bitCount(A ^ B);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int convertInteger(int A, int B) {
        unsigned int c = A ^ B;
        return __builtin_popcount(c);
    }
};
```

#### Go

```go
func convertInteger(A int, B int) int {
	return bits.OnesCount32(uint32(A ^ B))
}
```

#### TypeScript

```ts
function convertInteger(A: number, B: number): number {
    let res = 0;
    while (A !== 0 || B !== 0) {
        if ((A & 1) !== (B & 1)) {
            res++;
        }
        A >>>= 1;
        B >>>= 1;
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn convert_integer(a: i32, b: i32) -> i32 {
        (a ^ b).count_ones() as i32
    }
}
```

#### Swift

```swift
class Solution {
    func convertInteger(_ A: Int, _ B: Int) -> Int {
        return (Int32(A) ^ Int32(B)).nonzeroBitCount
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
