---
comments: true
difficulty: Medium
tags:
    - Math
    - Number Theory
    - Primality Test
---

<!-- problem:start -->

# [866. Prime Palindrome](https://leetcode.com/problems/prime-palindrome)

[中文文档](/solution/0800-0899/0866.Prime%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên n, hãy trả về <em>số <strong>nguyên tố đối xứng</strong> nhỏ nhất lớn hơn hoặc bằng </em><code>n</code>.</p>

<p>Một số nguyên là <strong>số nguyên tố</strong> nếu có đúng hai ước là <code>1</code> và chính nó. Lưu ý <code>1</code> không phải số nguyên tố.</p>

<ul>
	<li>Ví dụ, <code>2</code>, <code>3</code>, <code>5</code>, <code>7</code>, <code>11</code> và <code>13</code> đều là số nguyên tố.</li>
</ul>

<p>Một số nguyên là <strong>số đối xứng</strong> nếu đọc từ trái sang phải hay từ phải sang trái đều giống nhau.</p>

<ul>
	<li>Ví dụ, <code>101</code> và <code>12321</code> là các số đối xứng.</li>
</ul>

<p>Các bộ test được tạo sao cho đáp án luôn tồn tại và nằm trong khoảng <code>[2, 2 * 10<sup>8</sup>]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> n = 6
<strong>Đầu ra:</strong> 7
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> n = 8
<strong>Đầu ra:</strong> 11
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> n = 13
<strong>Đầu ra:</strong> 101
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nguyên tố đối xứng nhỏ nhất $\ge n$. $n$ có thể bằng $10^8$; kiểm tra lần lượt mọi số sẽ tốn công vì các số đối xứng có độ dài chẵn đều chia hết cho $11$.
>
> Bỏ qua toàn bộ khoảng $(10^7,10^8)$ bằng cách nhảy thẳng đến $10^8$. Ở các khoảng khác, tăng dần giá trị, kiểm tra tính đối xứng và tính nguyên tố, rồi trả về số đầu tiên thỏa mãn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def primePalindrome(self, n: int) -> int:
        def is_prime(x):
            if x < 2:
                return False
            v = 2
            while v * v <= x:
                if x % v == 0:
                    return False
                v += 1
            return True

        def reverse(x):
            res = 0
            while x:
                res = res * 10 + x % 10
                x //= 10
            return res

        while 1:
            if reverse(n) == n and is_prime(n):
                return n
            if 10**7 < n < 10**8:
                n = 10**8
            n += 1
```

#### Java

```java
class Solution {
    public int primePalindrome(int n) {
        while (true) {
            if (reverse(n) == n && isPrime(n)) {
                return n;
            }
            if (n > 10000000 && n < 100000000) {
                n = 100000000;
            }
            ++n;
        }
    }

    private boolean isPrime(int x) {
        if (x < 2) {
            return false;
        }
        for (int v = 2; v * v <= x; ++v) {
            if (x % v == 0) {
                return false;
            }
        }
        return true;
    }

    private int reverse(int x) {
        int res = 0;
        while (x != 0) {
            res = res * 10 + x % 10;
            x /= 10;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int primePalindrome(int n) {
        while (1) {
            if (reverse(n) == n && isPrime(n)) return n;
            if (n > 10000000 && n < 100000000) n = 100000000;
            ++n;
        }
    }

    bool isPrime(int x) {
        if (x < 2) return false;
        for (int v = 2; v * v <= x; ++v)
            if (x % v == 0)
                return false;
        return true;
    }

    int reverse(int x) {
        int res = 0;
        while (x) {
            res = res * 10 + x % 10;
            x /= 10;
        }
        return res;
    }
};
```

#### Go

```go
func primePalindrome(n int) int {
	isPrime := func(x int) bool {
		if x < 2 {
			return false
		}
		for v := 2; v*v <= x; v++ {
			if x%v == 0 {
				return false
			}
		}
		return true
	}

	reverse := func(x int) int {
		res := 0
		for x != 0 {
			res = res*10 + x%10
			x /= 10
		}
		return res
	}
	for {
		if reverse(n) == n && isPrime(n) {
			return n
		}
		if n > 10000000 && n < 100000000 {
			n = 100000000
		}
		n++
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
