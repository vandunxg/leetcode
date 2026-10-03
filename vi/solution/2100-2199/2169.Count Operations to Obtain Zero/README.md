---
comments: true
difficulty: Easy
rating: 1199
source: Weekly Contest 280 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [2169. Count Operations to Obtain Zero](https://leetcode.com/problems/count-operations-to-obtain-zero)

[中文文档](/solution/2100-2199/2169.Count%20Operations%20to%20Obtain%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <strong>không âm</strong> là <code>num1</code> và <code>num2</code>.</p>

<p>Trong một <strong>phép toán</strong>, nếu <code>num1 &gt;= num2</code>, bạn phải trừ <code>num2</code> khỏi <code>num1</code>; ngược lại, trừ <code>num1</code> khỏi <code>num2</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>num1 = 5</code> và <code>num2 = 4</code>, ta trừ <code>num2</code> khỏi <code>num1</code>, khi đó thu được <code>num1 = 1</code> và <code>num2 = 4</code>. Tuy nhiên, nếu <code>num1 = 4</code> và <code>num2 = 5</code>, sau một phép toán, <code>num1 = 4</code> và <code>num2 = 1</code>.</li>
</ul>

<p>Trả về <em><strong>số phép toán</strong> cần thực hiện để đưa</em> <code>num1 = 0</code> <em>hoặc</em> <code>num2 = 0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = 2, num2 = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Phép toán 1: num1 = 2, num2 = 3. Vì num1 &lt; num2, ta trừ num1 khỏi num2 và thu được num1 = 2, num2 = 3 - 2 = 1.
- Phép toán 2: num1 = 2, num2 = 1. Vì num1 &gt; num2, ta trừ num2 khỏi num1.
- Phép toán 3: num1 = 1, num2 = 1. Vì num1 == num2, ta trừ num2 khỏi num1.
Khi đó num1 = 0 và num2 = 1. Vì num1 == 0, ta không cần thực hiện thêm phép toán nào.
Vậy tổng số phép toán cần thực hiện là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = 10, num2 = 10
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
- Phép toán 1: num1 = 10, num2 = 10. Vì num1 == num2, ta trừ num2 khỏi num1 và thu được num1 = 10 - 10 = 0.
Khi đó num1 = 0 và num2 = 10. Vì num1 == 0, quá trình kết thúc.
Vậy tổng số phép toán cần thực hiện là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num1, num2 &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước trừ số nhỏ hơn khỏi số lớn hơn cho đến khi một số bằng không, tương tự phép trừ trong thuật toán Euclid. Các giá trị nhiều nhất là $10^4$, nên việc mô phỏng từng phép trừ là phù hợp.
>
> So sánh $\textit{num1}$ và $\textit{num2}$, thực hiện phép trừ rồi đếm số phép toán.
>
> Dừng lại khi một trong hai số bằng $0$.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quá trình này bằng cách liên tục thực hiện các thao tác sau:

- Nếu $\textit{num1} \ge \textit{num2}$, thì $\textit{num1} = \textit{num1} - \textit{num2}$;
- Ngược lại, $\textit{num2} = \textit{num2} - \textit{num1}$.
- Mỗi khi thực hiện một phép toán, tăng số phép toán lên một.

Khi $\textit{num1}$ hoặc $\textit{num2}$ bằng $0$, dừng vòng lặp và trả về số phép toán.

Độ phức tạp thời gian là $O(m)$, trong đó $m$ là giá trị lớn hơn giữa $\textit{num1}$ và $\textit{num2}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOperations(self, num1: int, num2: int) -> int:
        ans = 0
        while num1 and num2:
            if num1 >= num2:
                num1 -= num2
            else:
                num2 -= num1
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countOperations(int num1, int num2) {
        int ans = 0;
        for (; num1 != 0 && num2 != 0; ++ans) {
            if (num1 >= num2) {
                num1 -= num2;
            } else {
                num2 -= num1;
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
    int countOperations(int num1, int num2) {
        int ans = 0;
        for (; num1 && num2; ++ans) {
            if (num1 >= num2) {
                num1 -= num2;
            } else {
                num2 -= num1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countOperations(num1 int, num2 int) (ans int) {
	for ; num1 != 0 && num2 != 0; ans++ {
		if num1 >= num2 {
			num1 -= num2
		} else {
			num2 -= num1
		}
	}
	return
}
```

#### TypeScript

```ts
function countOperations(num1: number, num2: number): number {
    let ans = 0;
    for (; num1 && num2; ++ans) {
        if (num1 >= num2) {
            num1 -= num2;
        } else {
            num2 -= num1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_operations(mut num1: i32, mut num2: i32) -> i32 {
        let mut ans = 0;
        while num1 != 0 && num2 != 0 {
            ans += 1;
            if num1 >= num2 {
                num1 -= num2;
            } else {
                num2 -= num1;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} num1
 * @param {number} num2
 * @return {number}
 */
var countOperations = function (num1, num2) {
    let ans = 0;
    for (; num1 && num2; ++ans) {
        if (num1 >= num2) {
            num1 -= num2;
        } else {
            num2 -= num1;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 thực hiện rất nhiều phép trừ khi một số lớn hơn hẳn số còn lại. Thay vì trừ lặp lại, ta có thể dùng phép chia để cộng thương vào kết quả ngay lập tức rồi tiếp tục với phần dư.
>
> Số lần lặp giảm xuống còn $O(\log m)$, tương tự thuật toán Euclid.

<!-- thinking:end -->

Dựa trên quá trình mô phỏng ở Lời giải 1, ta nhận thấy rằng nếu $\textit{num1}$ lớn hơn nhiều so với $\textit{num2}$, mỗi phép toán chỉ làm giảm $\textit{num1}$ một lượng nhỏ, dẫn đến số phép toán quá lớn. Ta có thể tối ưu bằng cách cộng trực tiếp thương của phép chia $\textit{num1}$ cho $\textit{num2}$ vào kết quả ở mỗi bước, sau đó lấy phần dư của phép chia $\textit{num1}$ cho $\textit{num2}$. Cách này giúp giảm số phép toán.

Độ phức tạp thời gian là $O(\log m)$, trong đó $m$ là giá trị lớn hơn giữa $\textit{num1}$ và $\textit{num2}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOperations(self, num1: int, num2: int) -> int:
        ans = 0
        while num1 and num2:
            if num1 >= num2:
                ans += num1 // num2
                num1 %= num2
            else:
                ans += num2 // num1
                num2 %= num1
        return ans
```

#### Java

```java
class Solution {
    public int countOperations(int num1, int num2) {
        int ans = 0;
        while (num1 != 0 && num2 != 0) {
            if (num1 >= num2) {
                ans += num1 / num2;
                num1 %= num2;
            } else {
                ans += num2 / num1;
                num2 %= num1;
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
    int countOperations(int num1, int num2) {
        int ans = 0;
        while (num1 && num2) {
            if (num1 >= num2) {
                ans += num1 / num2;
                num1 %= num2;
            } else {
                ans += num2 / num1;
                num2 %= num1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countOperations(num1 int, num2 int) (ans int) {
	for num1 != 0 && num2 != 0 {
		if num1 >= num2 {
			ans += num1 / num2
			num1 %= num2
		} else {
			ans += num2 / num1
			num2 %= num1
		}
	}
	return
}
```

#### TypeScript

```ts
function countOperations(num1: number, num2: number): number {
    let ans = 0;
    while (num1 && num2) {
        if (num1 >= num2) {
            ans += (num1 / num2) | 0;
            num1 %= num2;
        } else {
            ans += (num2 / num1) | 0;
            num2 %= num1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_operations(mut num1: i32, mut num2: i32) -> i32 {
        let mut ans = 0;
        while num1 != 0 && num2 != 0 {
            if num1 >= num2 {
                ans += num1 / num2;
                num1 %= num2;
            } else {
                ans += num2 / num1;
                num2 %= num1;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} num1
 * @param {number} num2
 * @return {number}
 */
var countOperations = function (num1, num2) {
    let ans = 0;
    while (num1 && num2) {
        if (num1 >= num2) {
            ans += (num1 / num2) | 0;
            num1 %= num2;
        } else {
            ans += (num2 / num1) | 0;
            num2 %= num1;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
