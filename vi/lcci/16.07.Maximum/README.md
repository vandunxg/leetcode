---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [16.07. Maximum](https://leetcode.cn/problems/maximum-lcci)

[中文文档](/lcci/16.07.Maximum/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết một phương thức tìm số lớn hơn trong hai số. Bạn không được sử dụng if-else hoặc bất kỳ toán tử so sánh nào khác.</p>
<p><strong>Ví dụ: </strong></p>
<pre>

<strong>Đầu vào: </strong> a = 1, b = 2

<strong>Đầu ra: </strong> 2

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Tìm $\max(a,b)$ mà không dùng toán tử so sánh. Một câu lệnh `if` thường được chuyển thành phép so sánh.
>
> Bit dấu của $a-b$ cho biết số nào lớn hơn: $1$ nghĩa là $a<b$.
>
> Trích xuất bit đó $k$ trong cách biểu diễn $64$ bit rồi trả về $a(k\oplus 1)+b k$, dùng phép chọn bằng số học thay cho phép kiểm tra quan hệ.

<!-- thinking:end -->

Ta có thể trích xuất bit dấu $k$ của $a-b$. Nếu bit dấu là $1$, điều đó có nghĩa là $a \lt b$; nếu bit dấu là $0$, điều đó có nghĩa là $a \ge b$.

Sau đó, kết quả cuối cùng là $a \times (k \oplus 1) + b \times k$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximum(self, a: int, b: int) -> int:
        k = (int(((a - b) & 0xFFFFFFFFFFFFFFFF) >> 63)) & 1
        return a * (k ^ 1) + b * k
```

#### Java

```java
class Solution {
    public int maximum(int a, int b) {
        int k = (int) (((long) a - (long) b) >> 63) & 1;
        return a * (k ^ 1) + b * k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximum(int a, int b) {
        int k = ((static_cast<long long>(a) - static_cast<long long>(b)) >> 63) & 1;
        return a * (k ^ 1) + b * k;
    }
};
```

#### Go

```go
func maximum(a int, b int) int {
	k := (a - b) >> 63 & 1
	return a*(k^1) + b*k
}
```

#### TypeScript

```ts
function maximum(a: number, b: number): number {
    const k: number = Number(((BigInt(a) - BigInt(b)) >> BigInt(63)) & BigInt(1));
    return a * (k ^ 1) + b * k;
}
```

#### Swift

```swift
class Solution {
    func maximum(_ a: Int, _ b: Int) -> Int {
        let diff = Int64(a) - Int64(b)
        let k = Int((diff >> 63) & 1)
        return a * (k ^ 1) + b * k
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
