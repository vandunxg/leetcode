---
comments: true
difficulty: Medium
tags:
    - Math
    - Two Pointers
    - Binary Search
---

<!-- problem:start -->

# [633. Sum of Square Numbers](https://leetcode.com/problems/sum-of-square-numbers)

[中文文档](/solution/0600-0699/0633.Sum%20of%20Square%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên không âm <code>c</code>, hãy xác định có tồn tại hai số nguyên <code>a</code> và <code>b</code> sao cho <code>a<sup>2</sup> + b<sup>2</sup> = c</code> hay không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> c = 5
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 1 * 1 + 2 * 2 = 5
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> c = 3
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= c &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + two pointers

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xác định liệu $c=a^2+b^2$ hay không. Kiểm tra căn bậc hai cho từng $a$ sẽ lặp lại nhiều phép tính tương tự. Vì $c$ không vượt quá $2^{31}-1$, chỉ cần đặt hai con trỏ tại $0$ và $\sqrt{c}$.
>
> Tăng $a$ khi tổng còn nhỏ hơn mục tiêu, giảm $b$ khi tổng vượt quá mục tiêu. Bình phương tăng đơn điệu theo mỗi con trỏ nên không bỏ sót cặp nào.

<!-- thinking:end -->

Ta có thể dùng two pointers để giải bài này. Đặt hai con trỏ $a$ và $b$ lần lượt tại $0$ và $\sqrt{c}$. Ở mỗi bước, tính $s = a^2 + b^2$ rồi so sánh $s$ với $c$. Nếu $s = c$, ta đã tìm được hai số nguyên $a$, $b$ thỏa mãn $a^2 + b^2 = c$. Nếu $s < c$, tăng $a$ thêm $1$; nếu $s > c$, giảm $b$ đi $1$. Tiếp tục cho đến khi tìm được đáp án hoặc $a > b$, khi đó trả về `false`.

Độ phức tạp thời gian là $O(\sqrt{c})$, trong đó $c$ là số nguyên không âm đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        a, b = 0, int(sqrt(c))
        while a <= b:
            s = a**2 + b**2
            if s == c:
                return True
            if s < c:
                a += 1
            else:
                b -= 1
        return False
```

#### Java

```java
class Solution {
    public boolean judgeSquareSum(int c) {
        long a = 0, b = (long) Math.sqrt(c);
        while (a <= b) {
            long s = a * a + b * b;
            if (s == c) {
                return true;
            }
            if (s < c) {
                ++a;
            } else {
                --b;
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
    bool judgeSquareSum(int c) {
        long long a = 0, b = sqrt(c);
        while (a <= b) {
            long long s = a * a + b * b;
            if (s == c) {
                return true;
            }
            if (s < c) {
                ++a;
            } else {
                --b;
            }
        }
        return false;
    }
};
```

#### Go

```go
func judgeSquareSum(c int) bool {
	a, b := 0, int(math.Sqrt(float64(c)))
	for a <= b {
		s := a*a + b*b
		if s == c {
			return true
		}
		if s < c {
			a++
		} else {
			b--
		}
	}
	return false
}
```

#### TypeScript

```ts
function judgeSquareSum(c: number): boolean {
    let [a, b] = [0, Math.floor(Math.sqrt(c))];
    while (a <= b) {
        const s = a * a + b * b;
        if (s === c) {
            return true;
        }
        if (s < c) {
            ++a;
        } else {
            --b;
        }
    }
    return false;
}
```

#### Rust

```rust
use std::cmp::Ordering;

impl Solution {
    pub fn judge_square_sum(c: i32) -> bool {
        let mut a: i64 = 0;
        let mut b: i64 = (c as f64).sqrt() as i64;
        while a <= b {
            let s = a * a + b * b;
            match s.cmp(&(c as i64)) {
                Ordering::Equal => {
                    return true;
                }
                Ordering::Less => {
                    a += 1;
                }
                Ordering::Greater => {
                    b -= 1;
                }
            }
        }
        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Two pointers vẫn phải tìm kiếm các cặp. Theo định lý Fermat về tổng hai số chính phương, mọi thừa số nguyên tố dạng $4k+3$ phải có số mũ chẵn. Ta phân tích $c$ thành thừa số nguyên tố rồi kiểm tra điều kiện này thay vì liệt kê $(a,b)$.

<!-- thinking:end -->

Bài toán này xoay quanh điều kiện để một số có thể biểu diễn thành tổng của hai số chính phương. Định lý này có từ thời Fermat và Euler, đồng thời là một kết quả kinh điển của lý thuyết số.

Cụ thể, định lý phát biểu như sau:

> Số nguyên dương $n$ có thể biểu diễn thành tổng hai số chính phương khi và chỉ khi mọi thừa số nguyên tố của $n$ có dạng $4k + 3$ đều có số mũ chẵn.

Điều này có nghĩa là nếu phân tích $n$ thành tích các thừa số nguyên tố $n = p_1^{e_1} p_2^{e_2} \cdots p_k^{e_k}$, trong đó $p_i$ là số nguyên tố và $e_i$ là số mũ tương ứng, thì $n$ có thể biểu diễn thành tổng hai số chính phương khi và chỉ khi mọi $p_i$ dạng $4k + 3$ có số mũ $e_i$ chẵn.

Nói chính xác hơn, nếu $p_i$ là số nguyên tố dạng $4k + 3$, thì số mũ $e_i$ của nó phải chẵn.

Ví dụ:

- Số $13$ là số nguyên tố và $13 \equiv 1 \pmod{4}$, nên có thể biểu diễn thành tổng hai số chính phương: $13 = 2^2 + 3^2$.
- Số $21$ có thể phân tích thành $3 \times 7$. Cả $3$ và $7$ đều là thừa số nguyên tố dạng $4k + 3$ với số mũ $1$ (lẻ), nên $21$ không thể biểu diễn thành tổng hai số chính phương.

Tóm lại, định lý này rất hữu ích trong lý thuyết số để xác định một số có thể biểu diễn thành tổng hai số chính phương hay không.

Độ phức tạp thời gian là $O(\sqrt{c})$, trong đó $c$ là số nguyên không âm đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        for i in range(2, int(sqrt(c)) + 1):
            if c % i == 0:
                exp = 0
                while c % i == 0:
                    c //= i
                    exp += 1
                if i % 4 == 3 and exp % 2 != 0:
                    return False
        return c % 4 != 3
```

#### Java

```java
class Solution {
    public boolean judgeSquareSum(int c) {
        int n = (int) Math.sqrt(c);
        for (int i = 2; i <= n; ++i) {
            if (c % i == 0) {
                int exp = 0;
                while (c % i == 0) {
                    c /= i;
                    ++exp;
                }
                if (i % 4 == 3 && exp % 2 != 0) {
                    return false;
                }
            }
        }
        return c % 4 != 3;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool judgeSquareSum(int c) {
        int n = sqrt(c);
        for (int i = 2; i <= n; ++i) {
            if (c % i == 0) {
                int exp = 0;
                while (c % i == 0) {
                    c /= i;
                    ++exp;
                }
                if (i % 4 == 3 && exp % 2 != 0) {
                    return false;
                }
            }
        }
        return c % 4 != 3;
    }
};
```

#### Go

```go
func judgeSquareSum(c int) bool {
	n := int(math.Sqrt(float64(c)))
	for i := 2; i <= n; i++ {
		if c%i == 0 {
			exp := 0
			for c%i == 0 {
				c /= i
				exp++
			}
			if i%4 == 3 && exp%2 != 0 {
				return false
			}
		}
	}
	return c%4 != 3
}
```

#### TypeScript

```ts
function judgeSquareSum(c: number): boolean {
    const n = Math.floor(Math.sqrt(c));
    for (let i = 2; i <= n; ++i) {
        if (c % i === 0) {
            let exp = 0;
            while (c % i === 0) {
                c /= i;
                ++exp;
            }
            if (i % 4 === 3 && exp % 2 !== 0) {
                return false;
            }
        }
    }
    return c % 4 !== 3;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
