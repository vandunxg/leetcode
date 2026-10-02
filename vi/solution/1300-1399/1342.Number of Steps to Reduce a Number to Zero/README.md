---
comments: true
difficulty: Easy
rating: 1163
source: Biweekly Contest 19 Q1
tags:
    - Bit Manipulation
    - Math
---

<!-- problem:start -->

# [1342. Number of Steps to Reduce a Number to Zero](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-to-zero)

[中文文档](/solution/1300-1399/1342.Number%20of%20Steps%20to%20Reduce%20a%20Number%20to%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>num</code>, hãy trả về <em>số bước cần để đưa nó về 0</em>.</p>

<p>Mỗi bước, nếu số hiện tại là số chẵn thì chia nó cho <code>2</code>; nếu không thì trừ đi <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> num = 14
<strong>Output:</strong> 6
<strong>Giải thích:</strong>&nbsp;
Bước 1) 14 là số chẵn; chia cho 2 được 7.&nbsp;
Bước 2) 7 là số lẻ; trừ 1 được 6.
Bước 3) 6 là số chẵn; chia cho 2 được 3.&nbsp;
Bước 4) 3 là số lẻ; trừ 1 được 2.&nbsp;
Bước 5) 2 là số chẵn; chia cho 2 được 1.&nbsp;
Bước 6) 1 là số lẻ; trừ 1 được 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> num = 8
<strong>Output:</strong> 4
<strong>Giải thích:</strong>&nbsp;
Bước 1) 8 là số chẵn; chia cho 2 được 4.&nbsp;
Bước 2) 4 là số chẵn; chia cho 2 được 2.&nbsp;
Bước 3) 2 là số chẵn; chia cho 2 được 1.&nbsp;
Bước 4) 1 là số lẻ; trừ 1 được 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> num = 123
<strong>Output:</strong> 12
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chia đôi số chẵn, trừ đi một với số lẻ và đếm số bước cho đến khi về $0$. Vì $\textit{num} \le 10^6$, có thể mô phỏng trực tiếp: nếu bit thấp nhất đang bật thì trừ một, nếu không thì dịch phải, cho đến khi giá trị bằng 0.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSteps(self, num: int) -> int:
        ans = 0
        while num:
            if num & 1:
                num -= 1
            else:
                num >>= 1
            ans += 1
        return ans
```

#### Java

```java
class Solution {

    public int numberOfSteps(int num) {
        int ans = 0;
        while (num != 0) {
            num = (num & 1) == 1 ? num - 1 : num >> 1;
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSteps(int num) {
        int ans = 0;
        while (num) {
            num = num & 1 ? num - 1 : num >> 1;
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSteps(num int) int {
	ans := 0
	for num != 0 {
		if (num & 1) == 1 {
			num--
		} else {
			num >>= 1
		}
		ans++
	}
	return ans
}
```

#### TypeScript

```ts
function numberOfSteps(num: number): number {
    let ans = 0;
    while (num) {
        num = num & 1 ? num - 1 : num >>> 1;
        ans++;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_steps(mut num: i32) -> i32 {
        let mut count = 0;
        while num != 0 {
            if num % 2 == 0 {
                num >>= 1;
            } else {
                num -= 1;
            }
            count += 1;
        }
        count
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc tương tự có thể viết đệ quy: số chẵn chuyển thành $n/2$, số lẻ chuyển thành $n-1$, mỗi lời gọi cộng thêm một bước và dừng ở $0$. Ý nghĩa giống vòng lặp; chỉ khác là dùng call stack.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSteps(self, num: int) -> int:
        if num == 0:
            return 0
        return 1 + (
            self.numberOfSteps(num // 2)
            if num % 2 == 0
            else self.numberOfSteps(num - 1)
        )
```

#### Java

```java
class Solution {

    public int numberOfSteps(int num) {
        if (num == 0) {
            return 0;
        }
        return 1 + numberOfSteps((num & 1) == 0 ? num >> 1 : num - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSteps(int num) {
        if (num == 0) return 0;
        return 1 + (num & 1 ? numberOfSteps(num - 1) : numberOfSteps(num >> 1));
    }
};
```

#### Go

```go
func numberOfSteps(num int) int {
	if num == 0 {
		return 0
	}
	if (num & 1) == 0 {
		return 1 + numberOfSteps(num>>1)
	}
	return 1 + numberOfSteps(num-1)
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_steps(mut num: i32) -> i32 {
        if num == 0 {
            0
        } else if num % 2 == 0 {
            1 + Solution::number_of_steps(num >> 1)
        } else {
            1 + Solution::number_of_steps(num - 1)
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
