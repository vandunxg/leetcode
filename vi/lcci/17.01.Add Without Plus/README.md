---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [17.01. Add Without Plus](https://leetcode.cn/problems/add-without-plus-lcci)

[中文文档](/lcci/17.01.Add%20Without%20Plus/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm cộng hai số. Không được sử dụng + hoặc bất kỳ toán tử số học nào.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong>Đầu vào:</strong> a = 1, b = 1

<strong>Đầu ra:</strong> 2</pre>

<p>&nbsp;</p>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>a</code>&nbsp;và&nbsp;<code>b</code>&nbsp;có thể bằng 0 hoặc là số âm.</li>
	<li>Kết quả nằm trong phạm vi số nguyên 32-bit.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cộng mà không dùng $+,-,*,/$. Một vòng lặp thực hiện phép cộng sẽ vi phạm quy tắc; mô phỏng cách cộng trên giấy bằng chuỗi sẽ dài hơn.
>
> XOR là tổng không có bit nhớ; AND dịch trái là bit nhớ. Lặp lại cho đến khi bit nhớ biến mất.
>
> $sum=a\oplus b$ và $carry=(a\& b)\ll 1$ được ghi ngược lại vào `a` và `b`. Bit dấu tuân theo phép dịch số học; vòng lặp có độ phức tạp $O$(bit width).

<!-- thinking:end -->

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int add(int a, int b) {
        int sum = 0, carry = 0;
        while (b != 0) {
            sum = a ^ b;
            carry = (a & b) << 1;
            a = sum;
            b = carry;
        }
        return a;
    }
}
```

#### Swift

```swift
class Solution {
    func add(_ a: Int, _ b: Int) -> Int {
        var a = a
        var b = b
        var sum = 0
        var carry = 0

        while b != 0 {
            sum = a ^ b
            carry = (a & b) << 1
            a = sum
            b = carry
        }

        return a
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
