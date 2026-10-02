---
comments: true
difficulty: Hard
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [479. Largest Palindrome Product](https://leetcode.com/problems/largest-palindrome-product)

[中文文档](/solution/0400-0499/0479.Largest%20Palindrome%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về <em><strong>số nguyên đối xứng lớn nhất</strong> có thể biểu diễn thành tích của hai số nguyên có <code>n</code> chữ số</em>. Vì đáp án có thể rất lớn, hãy trả về phần dư khi chia cho <code>1337</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 987
Giải thích: 99 x 91 = 9009, 9009 % 1337 = 987
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 8</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm palindrome lớn nhất là tích của hai số nguyên có $n$ chữ số, rồi lấy modulo $1337$. Vì $n\le 8$, liệt kê mọi tích sẽ rất tốn kém.
>
> Duyệt giảm dần nửa đầu $a$, phản chiếu nó để tạo palindrome $x$, rồi kiểm tra xem có ước $t$ gồm $n$ chữ số hay không (duyệt $t$ giảm dần từ $10^n-1$ khi $t^2\ge x$). Nghiệm đầu tiên tìm được là lớn nhất.
>
> Tạo palindrome trước sẽ gặp các ứng viên lớn nhất sớm hơn so với việc liệt kê các tích. Trường hợp $n=1$ trả về $9$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestPalindrome(self, n: int) -> int:
        mx = 10**n - 1
        for a in range(mx, mx // 10, -1):
            b = x = a
            while b:
                x = x * 10 + b % 10
                b //= 10
            t = mx
            while t * t >= x:
                if x % t == 0:
                    return x % 1337
                t -= 1
        return 9
```

#### Java

```java
class Solution {
    public int largestPalindrome(int n) {
        int mx = (int) Math.pow(10, n) - 1;
        for (int a = mx; a > mx / 10; --a) {
            int b = a;
            long x = a;
            while (b != 0) {
                x = x * 10 + b % 10;
                b /= 10;
            }
            for (long t = mx; t * t >= x; --t) {
                if (x % t == 0) {
                    return (int) (x % 1337);
                }
            }
        }
        return 9;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestPalindrome(int n) {
        int mx = pow(10, n) - 1;
        for (int a = mx; a > mx / 10; --a) {
            int b = a;
            long x = a;
            while (b) {
                x = x * 10 + b % 10;
                b /= 10;
            }
            for (long t = mx; t * t >= x; --t)
                if (x % t == 0)
                    return x % 1337;
        }
        return 9;
    }
};
```

#### Go

```go
func largestPalindrome(n int) int {
	mx := int(math.Pow10(n)) - 1
	for a := mx; a > mx/10; a-- {
		x := a
		for b := a; b != 0; b /= 10 {
			x = x*10 + b%10
		}
		for t := mx; t*t >= x; t-- {
			if x%t == 0 {
				return x % 1337
			}
		}
	}
	return 9
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
