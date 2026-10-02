---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Math
    - Prime Factorization
---

<!-- problem:start -->

# [625. Minimum Factorization 🔒](https://leetcode.com/problems/minimum-factorization)

[中文文档](/solution/0600-0699/0625.Minimum%20Factorization/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương num, hãy trả về <em>số nguyên dương nhỏ nhất </em><code>x</code><em> sao cho tích các chữ số của nó bằng </em><code>num</code>. Nếu không có đáp án hoặc đáp án không vừa kiểu số nguyên có dấu <strong>32-bit</strong>, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> num = 48
<strong>Đầu ra:</strong> 68
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> num = 15
<strong>Đầu ra:</strong> 35
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần biểu diễn $num$ thành tích của các chữ số từ $2$ đến $9$ và tạo thành số nguyên nhỏ nhất. Không cần thử mọi cách phân tích thừa số.
>
> Số có ít chữ số hơn và chữ số ở hàng cao nhỏ hơn sẽ nhỏ hơn. Vì vậy, lần lượt chia cho các số từ $9$ xuống $2$, rồi ghép các thừa số theo thứ tự từ hàng thấp lên. Nếu còn dư lớn hơn $1$ hoặc kết quả vượt giới hạn $32-bit$, trả về $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestFactorization(self, num: int) -> int:
        if num < 2:
            return num
        ans, mul = 0, 1
        for i in range(9, 1, -1):
            while num % i == 0:
                num //= i
                ans = mul * i + ans
                mul *= 10
        return ans if num < 2 and ans <= 2**31 - 1 else 0
```

#### Java

```java
class Solution {
    public int smallestFactorization(int num) {
        if (num < 2) {
            return num;
        }
        long ans = 0, mul = 1;
        for (int i = 9; i >= 2; --i) {
            if (num % i == 0) {
                while (num % i == 0) {
                    num /= i;
                    ans = mul * i + ans;
                    mul *= 10;
                }
            }
        }
        return num < 2 && ans <= Integer.MAX_VALUE ? (int) ans : 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestFactorization(int num) {
        if (num < 2) {
            return num;
        }
        long long ans = 0, mul = 1;
        for (int i = 9; i >= 2; --i) {
            if (num % i == 0) {
                while (num % i == 0) {
                    num /= i;
                    ans = mul * i + ans;
                    mul *= 10;
                }
            }
        }
        return num < 2 && ans <= INT_MAX ? ans : 0;
    }
};
```

#### Go

```go
func smallestFactorization(num int) int {
	if num < 2 {
		return num
	}
	ans, mul := 0, 1
	for i := 9; i >= 2; i-- {
		if num%i == 0 {
			for num%i == 0 {
				num /= i
				ans = mul*i + ans
				mul *= 10
			}
		}
	}
	if num < 2 && ans <= math.MaxInt32 {
		return ans
	}
	return 0
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
