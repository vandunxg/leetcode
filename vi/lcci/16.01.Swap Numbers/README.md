---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.01. Swap Numbers](https://leetcode.cn/problems/swap-numbers-lcci)

[中文文档](/lcci/16.01.Swap%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm để hoán đổi các số ngay tại chỗ (nghĩa là không dùng biến tạm).</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào:</strong> numbers = [1,2]

<strong>Đầu ra:</strong> [2,1]

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>numbers.length == 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Thông thường, ta dùng một biến tạm để hoán đổi hai số; bài toán không cho phép dùng bộ nhớ bổ sung. Phép cộng và phép trừ có thể thực hiện được, nhưng có nguy cơ overflow.
>
> XOR thỏa mãn $a\oplus b\oplus b=a$, nên có thể dùng ba phép XOR để hoán đổi mà không phát sinh carry.
>
> Ba phép gán $a\oplus=b$, $b\oplus=a$, $a\oplus=b$ chính là các bước cập nhật `numbers[0]` và `numbers[1]`.

<!-- thinking:end -->

Ta có thể dùng phép XOR $\oplus$ để hoán đổi hai số.

Phép XOR có ba tính chất sau:

- XOR một số với $0$ cho kết quả không đổi, tức là $a \oplus 0=a$.
- XOR một số với chính nó cho kết quả $0$, tức là $a \oplus a=0$.
- Phép XOR có tính giao hoán và kết hợp, tức là $a \oplus b \oplus a=b \oplus a \oplus a=b \oplus (a \oplus a)=b \oplus 0=b$.

Do đó, ta có thể thực hiện các thao tác sau trên hai số $a$ và $b$ trong mảng $numbers$:

- $a=a \oplus b$, lúc này $a$ lưu kết quả XOR của hai số;
- $b=a \oplus b$, lúc này $b$ lưu giá trị ban đầu của $a$;
- $a=a \oplus b$, lúc này $a$ lưu giá trị ban đầu của $b$;

Như vậy, ta có thể hoán đổi hai số mà không cần dùng biến tạm.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def swapNumbers(self, numbers: List[int]) -> List[int]:
        numbers[0] ^= numbers[1]
        numbers[1] ^= numbers[0]
        numbers[0] ^= numbers[1]
        return numbers
```

#### Java

```java
class Solution {
    public int[] swapNumbers(int[] numbers) {
        numbers[0] ^= numbers[1];
        numbers[1] ^= numbers[0];
        numbers[0] ^= numbers[1];
        return numbers;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> swapNumbers(vector<int>& numbers) {
        numbers[0] ^= numbers[1];
        numbers[1] ^= numbers[0];
        numbers[0] ^= numbers[1];
        return numbers;
    }
};
```

#### Go

```go
func swapNumbers(numbers []int) []int {
	numbers[0] ^= numbers[1]
	numbers[1] ^= numbers[0]
	numbers[0] ^= numbers[1]
	return numbers
}
```

#### TypeScript

```ts
function swapNumbers(numbers: number[]): number[] {
    numbers[0] ^= numbers[1];
    numbers[1] ^= numbers[0];
    numbers[0] ^= numbers[1];
    return numbers;
}
```

#### Swift

```swift
class Solution {
    func swapNumbers(_ numbers: [Int]) -> [Int] {
        var numbers = numbers
        numbers[0] ^= numbers[1]
        numbers[1] ^= numbers[0]
        numbers[0] ^= numbers[1]
        return numbers
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
