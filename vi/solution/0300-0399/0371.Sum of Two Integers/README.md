---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Math
---

<!-- problem:start -->

# [371. Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers)

[中文文档](/solution/0300-0399/0371.Sum%20of%20Two%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>a</code> và <code>b</code>. Hãy trả về <em>tổng của chúng mà không dùng các toán tử</em> <code>+</code> <em>và</em> <code>-</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> a = 1, b = 2
<strong>Đầu ra:</strong> 3
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> a = 2, b = 3
<strong>Đầu ra:</strong> 5
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-1000 &lt;= a, b &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cộng mà không dùng `+`/`-`. Với thao tác bit, tổng không tính carry là XOR, còn carry là AND rồi dịch trái một bit. Lặp lại đến khi carry bằng 0.
>
> Số nguyên trong Python không giới hạn độ rộng, nên dùng mask $0xFFFFFFFF$ để giữ lại 32 bit. Nếu bit dấu được bật, chuyển giá trị bù hai về số âm.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        a, b = a & 0xFFFFFFFF, b & 0xFFFFFFFF
        while b:
            carry = ((a & b) << 1) & 0xFFFFFFFF
            a, b = a ^ b, carry
        return a if a < 0x80000000 else ~(a ^ 0xFFFFFFFF)
```

#### Java

```java
class Solution {
    public int getSum(int a, int b) {
        return b == 0 ? a : getSum(a ^ b, (a & b) << 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getSum(int a, int b) {
        while (b) {
            unsigned int carry = (unsigned int) (a & b) << 1;
            a = a ^ b;
            b = carry;
        }
        return a;
    }
};
```

#### Go

```go
func getSum(a int, b int) int {
	for b != 0 {
		s := a ^ b
		b = (a & b) << 1
		a = s
	}
	return a
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
