---
comments: true
difficulty: Medium
rating: 1376
source: Weekly Contest 315 Q3
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [2443. Sum of Number and Its Reverse](https://leetcode.com/problems/sum-of-number-and-its-reverse)

[中文文档](/solution/2400-2499/2443.Sum%20of%20Number%20and%20Its%20Reverse/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>không âm</strong> <code>num</code>, hãy trả về <code>true</code><em> nếu </em><code>num</code><em> có thể được biểu diễn dưới dạng tổng của một số nguyên <strong>không âm</strong> bất kỳ và số đảo ngược của nó, hoặc </em><code>false</code><em> nếu không thể.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 443
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 172 + 271 = 443 nên ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 63
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> 63 không thể được biểu diễn dưới dạng tổng của một số nguyên không âm và số đảo ngược của nó, nên ta trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 181
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 140 + 041 = 181 nên ta trả về true. Lưu ý rằng khi đảo ngược một số, có thể xuất hiện các số 0 ở đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Với $num\le 10^5$, ta thử mọi $k\in[0,num]$ và kiểm tra điều kiện $k+reverse(k)=num$. Khoảng giá trị này đủ nhỏ; ta đảo ngược số bằng chuỗi thập phân.

<!-- thinking:end -->

Duyệt $k$ trong đoạn $[0,.., num]$ và kiểm tra xem $k + reverse(k)$ có bằng $num$ hay không.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số chữ số của $num$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfNumberAndReverse(self, num: int) -> bool:
        return any(k + int(str(k)[::-1]) == num for k in range(num + 1))
```

#### Java

```java
class Solution {
    public boolean sumOfNumberAndReverse(int num) {
        for (int x = 0; x <= num; ++x) {
            int k = x;
            int y = 0;
            while (k > 0) {
                y = y * 10 + k % 10;
                k /= 10;
            }
            if (x + y == num) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool sumOfNumberAndReverse(int num) {
        for (int x = 0; x <= num; ++x) {
            int k = x;
            int y = 0;
            while (k > 0) {
                y = y * 10 + k % 10;
                k /= 10;
            }
            if (x + y == num) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func sumOfNumberAndReverse(num int) bool {
	for x := 0; x <= num; x++ {
		k, y := x, 0
		for k > 0 {
			y = y*10 + k%10
			k /= 10
		}
		if x+y == num {
			return true
		}
	}
	return false
}
```

#### TypeScript

```ts
function sumOfNumberAndReverse(num: number): boolean {
    for (let i = 0; i <= num; i++) {
        if (i + Number([...(i + '')].reverse().join('')) === num) {
            return true;
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_of_number_and_reverse(num: i32) -> bool {
        for i in 0..=num {
            if i + ({
                let mut t = i;
                let mut j = 0;
                while t > 0 {
                    j = j * 10 + (t % 10);
                    t /= 10;
                }
                j
            }) == num
            {
                return true;
            }
        }
        false
    }
}
```

#### C

```c
bool sumOfNumberAndReverse(int num) {
    for (int i = 0; i <= num; i++) {
        int t = i;
        int j = 0;
        while (t > 0) {
            j = j * 10 + t % 10;
            t /= 10;
        }
        if (i + j == num) {
            return 1;
        }
    }
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
