---
comments: true
difficulty: Easy
rating: 1260
source: Weekly Contest 326 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2520. Count the Digits That Divide a Number](https://leetcode.com/problems/count-the-digits-that-divide-a-number)

[中文文档](/solution/2500-2599/2520.Count%20the%20Digits%20That%20Divide%20a%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>num</code>, hãy trả về <em>số chữ số trong <code>num</code> là ước của </em><code>num</code>.</p>

<p>Một số nguyên <code>val</code> là ước của <code>nums</code> nếu <code>nums % val == 0</code>.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 7
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 7 chia hết cho chính nó, nên đáp án là 1.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 121
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 121 chia hết cho 1 nhưng không chia hết cho 2. Vì chữ số 1 xuất hiện hai lần, ta trả về 2.
</pre>

<p><strong>Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 1248
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 1248 chia hết cho tất cả các chữ số của nó, nên đáp án là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>9</sup></code></li>
	<li><code>num</code> không chứa chữ số <code>0</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> Đếm có bao nhiêu chữ số của $num$ là ước của $num$. Có nhiều nhất mười chữ số, nên chỉ cần duyệt qua chúng.
>
> Liên tục lấy $val = num \bmod 10$ rồi kiểm tra $num \bmod val = 0$. Đầu vào không chứa số 0, nên không xảy ra phép chia cho 0.

<!-- thinking:end -->

Ta duyệt trực tiếp từng chữ số $val$ của số nguyên $num$; nếu $val$ là ước của $num$, ta tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(\log num)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDigits(self, num: int) -> int:
        ans, x = 0, num
        while x:
            x, val = divmod(x, 10)
            ans += num % val == 0
        return ans
```

#### Java

```java
class Solution {
    public int countDigits(int num) {
        int ans = 0;
        for (int x = num; x > 0; x /= 10) {
            if (num % (x % 10) == 0) {
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countDigits(int num) {
        int ans = 0;
        for (int x = num; x > 0; x /= 10) {
            if (num % (x % 10) == 0) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countDigits(num int) (ans int) {
	for x := num; x > 0; x /= 10 {
		if num%(x%10) == 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countDigits(num: number): number {
    let ans = 0;
    for (let x = num; x; x = (x / 10) | 0) {
        if (num % (x % 10) === 0) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_digits(num: i32) -> i32 {
        let mut ans = 0;
        let mut cur = num;
        while cur != 0 {
            if num % (cur % 10) == 0 {
                ans += 1;
            }
            cur /= 10;
        }
        ans
    }
}
```

#### C

```c
int countDigits(int num) {
    int ans = 0;
    int cur = num;
    while (cur) {
        if (num % (cur % 10) == 0) {
            ans++;
        }
        cur /= 10;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 trích xuất các chữ số bằng phép toán số học. Chuyển $num$ thành một chuỗi thập phân và kiểm tra từng ký tự cho kết quả tương tự; chỉ thay đổi cách lấy chữ số.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function countDigits(num: number): number {
    let ans = 0;
    for (const s of num.toString()) {
        if (num % Number(s) === 0) {
            ans++;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_digits(num: i32) -> i32 {
        num.to_string()
            .chars()
            .filter(|&c| c != '0')
            .filter(|&c| num % (c.to_digit(10).unwrap() as i32) == 0)
            .count() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
