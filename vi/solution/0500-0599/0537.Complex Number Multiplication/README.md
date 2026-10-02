---
comments: true
difficulty: Medium
tags:
    - Math
    - String
    - Simulation
---

<!-- problem:start -->

# [537. Complex Number Multiplication](https://leetcode.com/problems/complex-number-multiplication)

[中文文档](/solution/0500-0599/0537.Complex%20Number%20Multiplication/README.md)

## Mô tả

<!-- description:start -->

<p><a href="https://en.wikipedia.org/wiki/Complex_number" target="_blank">Số phức</a> có thể được biểu diễn dưới dạng chuỗi theo định dạng <code>&quot;<strong>real</strong>+<strong>imaginary</strong>i&quot;</code>, trong đó:</p>

<ul>
	<li><code>real</code> là phần thực, có giá trị nguyên trong khoảng <code>[-100, 100]</code>.</li>
	<li><code>imaginary</code> là phần ảo, có giá trị nguyên trong khoảng <code>[-100, 100]</code>.</li>
	<li><code>i<sup>2</sup> == -1</code>.</li>
</ul>

<p>Cho hai số phức dạng chuỗi <code>num1</code> và <code>num2</code>, hãy trả về <em>chuỗi biểu diễn tích của hai số phức đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = &quot;1+1i&quot;, num2 = &quot;1+1i&quot;
<strong>Đầu ra:</strong> &quot;0+2i&quot;
<strong>Giải thích:</strong> (1 + i) * (1 + i) = 1 + i2 + 2 * i = 2i; kết quả cần được chuyển sang định dạng 0+2i.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = &quot;1+-1i&quot;, num2 = &quot;1+-1i&quot;
<strong>Đầu ra:</strong> &quot;0+-2i&quot;
<strong>Giải thích:</strong> (1 - i) * (1 - i) = 1 + i2 - 2 * i = -2i; kết quả cần được chuyển sang định dạng 0+-2i.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>num1</code> và <code>num2</code> là các số phức hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Phép nhân số phức có công thức $(a+bi)(c+di)=(ac-bd)+(ad+bc)i$. Đầu vào đã có dạng `a+bi`, nên chỉ cần lấy ra bốn số nguyên.
>
> Bỏ ký tự `i` ở cuối, tách theo dấu `+`, áp dụng công thức rồi định dạng kết quả. Không cần khai triển đa thức.

<!-- thinking:end -->

Ta có thể tách chuỗi số phức thành phần thực $a$ và phần ảo $b$, rồi áp dụng công thức nhân hai số phức $(a_1 + b_1i) \times (a_2 + b_2i) = (a_1a_2 - b_1b_2) + (a_1b_2 + a_2b_1)i$ để tính kết quả.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def complexNumberMultiply(self, num1: str, num2: str) -> str:
        a1, b1 = map(int, num1[:-1].split("+"))
        a2, b2 = map(int, num2[:-1].split("+"))
        return f"{a1 * a2 - b1 * b2}+{a1 * b2 + a2 * b1}i"
```

#### Java

```java
class Solution {
    public String complexNumberMultiply(String num1, String num2) {
        int[] x = parse(num1);
        int[] y = parse(num2);
        int a1 = x[0], b1 = x[1], a2 = y[0], b2 = y[1];
        return (a1 * a2 - b1 * b2) + "+" + (a1 * b2 + a2 * b1) + "i";
    }

    private int[] parse(String s) {
        var cs = s.substring(0, s.length() - 1).split("\\+");
        return new int[] {Integer.parseInt(cs[0]), Integer.parseInt(cs[1])};
    }
}
```

#### C++

```cpp
class Solution {
public:
    string complexNumberMultiply(string num1, string num2) {
        int a1, b1, a2, b2;
        sscanf(num1.c_str(), "%d+%di", &a1, &b1);
        sscanf(num2.c_str(), "%d+%di", &a2, &b2);
        return to_string(a1 * a2 - b1 * b2) + "+" + to_string(a1 * b2 + a2 * b1) + "i";
    }
};
```

#### Go

```go
func complexNumberMultiply(num1 string, num2 string) string {
	x, _ := strconv.ParseComplex(num1, 64)
	y, _ := strconv.ParseComplex(num2, 64)
	return fmt.Sprintf("%d+%di", int(real(x*y)), int(imag(x*y)))
}
```

#### TypeScript

```ts
function complexNumberMultiply(num1: string, num2: string): string {
    const [a1, b1] = num1.slice(0, -1).split('+').map(Number);
    const [a2, b2] = num2.slice(0, -1).split('+').map(Number);
    return `${a1 * a2 - b1 * b2}+${a1 * b2 + a2 * b1}i`;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
