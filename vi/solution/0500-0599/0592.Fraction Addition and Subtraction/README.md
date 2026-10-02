---
comments: true
difficulty: Medium
tags:
    - Math
    - String
    - Greatest Common Divisor
    - Simulation
    - Euclidean Algorithm
---

<!-- problem:start -->

# [592. Fraction Addition and Subtraction](https://leetcode.com/problems/fraction-addition-and-subtraction)

[中文文档](/solution/0500-0599/0592.Fraction%20Addition%20and%20Subtraction/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>expression</code> biểu diễn phép cộng và trừ các phân số, hãy trả về kết quả dưới dạng chuỗi.</p>

<p>Kết quả cuối cùng phải là <a href="https://en.wikipedia.org/wiki/Irreducible_fraction" target="_blank">phân số tối giản</a>. Nếu kết quả là số nguyên, hãy biểu diễn dưới dạng phân số có mẫu số <code>1</code>. Ví dụ, <code>2</code> cần được chuyển thành <code>2/1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;-1/2+1/2&quot;
<strong>Đầu ra:</strong> &quot;0/1&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;-1/2+1/2+1/3&quot;
<strong>Đầu ra:</strong> &quot;1/3&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;1/3-1/2&quot;
<strong>Đầu ra:</strong> &quot;-1/6&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Chuỗi đầu vào chỉ chứa các ký tự từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>, <code>&#39;/&#39;</code>, <code>&#39;+&#39;</code> và <code>&#39;-&#39;</code>. Chuỗi kết quả cũng vậy.</li>
	<li>Mỗi phân số (cả đầu vào và đầu ra) có định dạng <code>&plusmn;numerator/denominator</code>. Nếu phân số đầu tiên trong đầu vào hoặc kết quả là số dương thì bỏ ký tự <code>&#39;+&#39;</code>.</li>
	<li>Đầu vào chỉ chứa các <strong>phân số tối giản</strong> hợp lệ; <strong>tử số</strong> và <strong>mẫu số</strong> của mỗi phân số luôn nằm trong khoảng <code>[1, 10]</code>. Nếu mẫu số là <code>1</code>, phân số đó thực chất là số nguyên được viết theo định dạng phân số ở trên.</li>
	<li>Số lượng phân số đầu vào nằm trong khoảng <code>[1, 10]</code>.</li>
	<li>Tử số và mẫu số của <strong>kết quả cuối cùng</strong> được đảm bảo hợp lệ và nằm trong phạm vi số nguyên <strong>32-bit</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần cộng trừ các phân số có mẫu số từ $2$ đến $10$. Chỉ cần mẫu số chung và gcd; không cần tạo kiểu dữ liệu phân số riêng.
>
> Dùng $y = \mathrm{lcm}(2,\ldots,10)$ làm mẫu số chung và duyệt các phân số có dấu $a/b$ để cộng dồn tử số vào $x$. Rút gọn bằng $\gcd(x,y)$. Nếu biểu thức bắt đầu bằng chữ số, thêm dấu `+` ở đầu để việc parse thống nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fractionAddition(self, expression: str) -> str:
        x, y = 0, 6 * 7 * 8 * 9 * 10
        if expression[0].isdigit():
            expression = '+' + expression
        i, n = 0, len(expression)
        while i < n:
            sign = -1 if expression[i] == '-' else 1
            i += 1
            j = i
            while j < n and expression[j] not in '+-':
                j += 1
            s = expression[i:j]
            a, b = s.split('/')
            x += sign * int(a) * y // int(b)
            i = j
        z = gcd(x, y)
        x //= z
        y //= z
        return f'{x}/{y}'
```

#### Java

```java
class Solution {
    public String fractionAddition(String expression) {
        int x = 0, y = 6 * 7 * 8 * 9 * 10;
        if (Character.isDigit(expression.charAt(0))) {
            expression = "+" + expression;
        }
        int i = 0, n = expression.length();
        while (i < n) {
            int sign = expression.charAt(i) == '-' ? -1 : 1;
            ++i;
            int j = i;
            while (j < n && expression.charAt(j) != '+' && expression.charAt(j) != '-') {
                ++j;
            }
            String s = expression.substring(i, j);
            String[] t = s.split("/");
            int a = Integer.parseInt(t[0]), b = Integer.parseInt(t[1]);
            x += sign * a * y / b;
            i = j;
        }
        int z = gcd(Math.abs(x), y);
        x /= z;
        y /= z;
        return x + "/" + y;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### Go

```go
func fractionAddition(expression string) string {
	x, y := 0, 6*7*8*9*10
	if unicode.IsDigit(rune(expression[0])) {
		expression = "+" + expression
	}
	i, n := 0, len(expression)
	for i < n {
		sign := 1
		if expression[i] == '-' {
			sign = -1
		}
		i++
		j := i
		for j < n && expression[j] != '+' && expression[j] != '-' {
			j++
		}
		s := expression[i:j]
		t := strings.Split(s, "/")
		a, _ := strconv.Atoi(t[0])
		b, _ := strconv.Atoi(t[1])
		x += sign * a * y / b
		i = j
	}
	z := gcd(abs(x), y)
	x /= z
	y /= z
	return fmt.Sprintf("%d/%d", x, y)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### JavaScript

```js
/**
 * @param {string} expression
 * @return {string}
 */
var fractionAddition = function (expression) {
    let x = 0,
        y = 1;

    if (!expression.startsWith('-') && !expression.startsWith('+')) {
        expression = '+' + expression;
    }

    let i = 0;
    const n = expression.length;

    while (i < n) {
        const sign = expression[i] === '-' ? -1 : 1;
        i++;

        let j = i;
        while (j < n && expression[j] !== '+' && expression[j] !== '-') {
            j++;
        }

        const [a, b] = expression.slice(i, j).split('/').map(Number);
        x = x * b + sign * a * y;
        y *= b;
        i = j;
    }

    const gcd = (a, b) => {
        while (b !== 0) {
            [a, b] = [b, a % b];
        }
        return Math.abs(a);
    };

    const z = gcd(x, y);
    x = Math.floor(x / z);
    y = Math.floor(y / z);

    return `${x}/${y}`;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
