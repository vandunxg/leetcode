---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [05.07. Exchange](https://leetcode.cn/problems/exchange-lcci)

[Tài liệu tiếng Trung](/lcci/05.07.Exchange/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một chương trình hoán đổi các bit lẻ và chẵn trong một số nguyên với ít chỉ thị nhất có thể (ví dụ: hoán đổi bit 0 và bit 1, hoán đổi bit 2 và bit 3, cứ như vậy).</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Đầu vào</strong>: num = 2（0b10）

<strong> Đầu ra</strong> 1 (0b01)

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Đầu vào</strong>: num = 3

<strong> Đầu ra</strong>: 3

</pre>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li><code>0 &lt;= num &lt;=</code>&nbsp;2^30 - 1</li>
	<li>Số nguyên kết quả vừa với số nguyên 32 bit.</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Các bit lẻ và chẵn liền kề phải được hoán đổi. Có thể thực hiện 16 lần hoán đổi từng cặp, nhưng khi đó phải dùng vòng lặp.
>
> Có thể gom tất cả bit chẵn và bit lẻ bằng các mask, sau đó dịch chuyển chúng như hai khối.
>
> AND với `$0x55555555` rồi dịch trái; AND với `$0xAAAAAAAA` rồi dịch phải; OR hai nửa lại. Chỉ cần vài phép toán, không cần vòng lặp.

<!-- thinking:end -->

Ta có thể thực hiện phép AND theo bit giữa `num` và `0x55555555` để lấy các bit chẵn của `num`, sau đó dịch chúng sang trái một bit. Tiếp theo, thực hiện phép AND theo bit giữa `num` và `0xaaaaaaaa` để lấy các bit lẻ của `num`, rồi dịch chúng sang phải một bit. Cuối cùng, thực hiện phép OR theo bit trên hai kết quả để thu được đáp án.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def exchangeBits(self, num: int) -> int:
        return ((num & 0x55555555) << 1) | ((num & 0xAAAAAAAA) >> 1)
```

#### Java

```java
class Solution {
    public int exchangeBits(int num) {
        return ((num & 0x55555555) << 1) | ((num & 0xaaaaaaaa)) >> 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int exchangeBits(int num) {
        return ((num & 0x55555555) << 1) | ((num & 0xaaaaaaaa)) >> 1;
    }
};
```

#### Go

```go
func exchangeBits(num int) int {
	return ((num & 0x55555555) << 1) | (num&0xaaaaaaaa)>>1
}
```

#### TypeScript

```ts
function exchangeBits(num: number): number {
    return ((num & 0x55555555) << 1) | ((num & 0xaaaaaaaa) >>> 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn exchange_bits(num: i32) -> i32 {
        let num = num as u32;
        (((num & 0x55555555) << 1) | ((num & 0xaaaaaaaa) >> 1)) as i32
    }
}
```

#### Swift

```swift
class Solution {
    func exchangeBits(_ num: Int) -> Int {
        let oddShifted = (num & 0x55555555) << 1

        let evenShifted = (num & 0xaaaaaaaa) >> 1

        return oddShifted | evenShifted
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
