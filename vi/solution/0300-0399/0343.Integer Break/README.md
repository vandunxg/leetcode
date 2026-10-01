---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [343. Integer Break](https://leetcode.com/problems/integer-break)

[中文文档](/solution/0300-0399/0343.Integer%20Break/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy phân tích nó thành tổng của <code>k</code> <strong>số nguyên dương</strong>, với <code>k &gt;= 2</code>, sao cho tích của các số đó lớn nhất.</p>

<p>Hãy trả về <em>tích lớn nhất có thể đạt được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 2 = 1 + 1, 1 &times; 1 = 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 36
<strong>Giải thích:</strong> 10 = 3 + 3 + 4, 3 &times; 3 &times; 4 = 36.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 58</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> $n$ cần được tách thành ít nhất hai số nguyên dương sao cho tích lớn nhất. Có quá nhiều cách phân hoạch để liệt kê hết. Nếu phần cuối là $j$, phần còn lại có thể giữ nguyên hoặc được phân tách theo cách tối ưu.
>
> $f[i]=\max_j \max(f[i-j]\cdot j,\,(i-j)\cdot j)$ với $f[1]=1$. Tính lần lượt theo $i$ tăng dần; đáp án là $f[n]$. Vì $n\le 58$, độ phức tạp $O(n^2)$ là phù hợp.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là tích lớn nhất có thể đạt được khi phân tách số nguyên dương $i$, với điều kiện ban đầu $f[1] = 1$. Đáp án là $f[n]$.

Xét số cuối cùng $j$ được tách ra từ $i$, với $j \in [1, i)$. Có hai trường hợp:

1. Tách $i$ thành tổng của $i - j$ và $j$, không tách thêm, khi đó tích là $(i - j) \times j$;
2. Tách $i$ thành tổng của $i - j$ và $j$, rồi tiếp tục tách $i-j$, khi đó tích là $f[i - j] \times j$. 

Do đó, ta có công thức chuyển trạng thái:

$$
f[i] = \max(f[i], f[i - j] \times j, (i - j) \times j) \quad (j \in [0, i))
$$

Cuối cùng, chỉ cần trả về $f[n]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số nguyên dương đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        f = [1] * (n + 1)
        for i in range(2, n + 1):
            for j in range(1, i):
                f[i] = max(f[i], f[i - j] * j, (i - j) * j)
        return f[n]
```

#### Java

```java
class Solution {
    public int integerBreak(int n) {
        int[] f = new int[n + 1];
        f[1] = 1;
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j < i; ++j) {
                f[i] = Math.max(Math.max(f[i], f[i - j] * j), (i - j) * j);
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int> f(n + 1);
        f[1] = 1;
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j < i; ++j) {
                f[i] = max({f[i], f[i - j] * j, (i - j) * j});
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func integerBreak(n int) int {
	f := make([]int, n+1)
	f[1] = 1
	for i := 2; i <= n; i++ {
		for j := 1; j < i; j++ {
			f[i] = max(max(f[i], f[i-j]*j), (i-j)*j)
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function integerBreak(n: number): number {
    const f = Array(n + 1).fill(1);
    for (let i = 3; i <= n; i++) {
        for (let j = 1; j < i; j++) {
            f[i] = Math.max(f[i], j * (i - j), j * f[i - j]);
        }
    }
    return f[n];
}
```

#### Rust

```rust
impl Solution {
    pub fn integer_break(n: i32) -> i32 {
        let n = n as usize;
        let mut f = vec![0; n + 1];
        f[1] = 1;
        for i in 2..=n {
            for j in 1..i {
                f[i] = f[i].max(f[i - j] * j).max((i - j) * j);
            }
        }
        f[n] as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var integerBreak = function (n) {
    const f = Array(n + 1).fill(1);
    for (let i = 2; i <= n; ++i) {
        for (let j = 1; j < i; ++j) {
            f[i] = Math.max(f[i], f[i - j] * j, (i - j) * j);
        }
    }
    return f[n];
};
```

#### C#

```cs
public class Solution {
    public int IntegerBreak(int n) {
        int[] f = new int[n + 1];
        f[1] = 1;
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j < i; ++j) {
                f[i] = Math.Max(Math.Max(f[i], f[i - j] * j), (i - j) * j);
            }
        }
        return f[n];
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int integerBreak(int n) {
    int* f = (int*) malloc((n + 1) * sizeof(int));
    f[1] = 1;
    for (int i = 2; i <= n; ++i) {
        f[i] = 0;
        for (int j = 1; j < i; ++j) {
            f[i] = max(f[i], max(f[i - j] * j, (i - j) * j));
        }
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 không chỉ ra cụ thể các phần tối ưu. Với $n\ge 4$, dùng nhiều số $3$ nhất có thể; nếu dư $1$ thì đổi thành $2+2$, còn nếu dư $2$ thì thêm một số $2$. Với $n<4$, tích lớn nhất là $n-1$. Có thể tính trực tiếp trong $O(1)$.

<!-- thinking:end -->

Khi $n < 4$, vì đề bài yêu cầu tách thành ít nhất hai số nguyên nên tích lớn nhất đạt được là $n - 1$. Khi $n \geq 4$, ta tách được nhiều số $3$ nhất có thể. Nếu phần còn lại cuối cùng là $4$, hãy tách thành $2 + 2$ để đạt tích lớn nhất.

Độ phức tạp thời gian và không gian đều là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        if n < 4:
            return n - 1
        if n % 3 == 0:
            return pow(3, n // 3)
        if n % 3 == 1:
            return pow(3, n // 3 - 1) * 4
        return pow(3, n // 3) * 2
```

#### Java

```java
class Solution {
    public int integerBreak(int n) {
        if (n < 4) {
            return n - 1;
        }
        if (n % 3 == 0) {
            return (int) Math.pow(3, n / 3);
        }
        if (n % 3 == 1) {
            return (int) Math.pow(3, n / 3 - 1) * 4;
        }
        return (int) Math.pow(3, n / 3) * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int integerBreak(int n) {
        if (n < 4) {
            return n - 1;
        }
        if (n % 3 == 0) {
            return pow(3, n / 3);
        }
        if (n % 3 == 1) {
            return pow(3, n / 3 - 1) * 4;
        }
        return pow(3, n / 3) * 2;
    }
};
```

#### Go

```go
func integerBreak(n int) int {
	if n < 4 {
		return n - 1
	}
	if n%3 == 0 {
		return int(math.Pow(3, float64(n/3)))
	}
	if n%3 == 1 {
		return int(math.Pow(3, float64(n/3-1))) * 4
	}
	return int(math.Pow(3, float64(n/3))) * 2
}
```

#### TypeScript

```ts
function integerBreak(n: number): number {
    if (n < 4) {
        return n - 1;
    }
    const m = Math.floor(n / 3);
    if (n % 3 == 0) {
        return 3 ** m;
    }
    if (n % 3 == 1) {
        return 3 ** (m - 1) * 4;
    }
    return 3 ** m * 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn integer_break(n: i32) -> i32 {
        if n < 4 {
            return n - 1;
        }
        match n % 3 {
            0 => return (3 as i32).pow((n / 3) as u32),
            1 => return (3 as i32).pow((n / 3 - 1) as u32) * 4,
            _ => return (3 as i32).pow((n / 3) as u32) * 2,
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var integerBreak = function (n) {
    if (n < 4) {
        return n - 1;
    }
    const m = Math.floor(n / 3);
    if (n % 3 == 0) {
        return 3 ** m;
    }
    if (n % 3 == 1) {
        return 3 ** (m - 1) * 4;
    }
    return 3 ** m * 2;
};
```

#### C#

```cs
public class Solution {
    public int IntegerBreak(int n) {
        if (n < 4) {
            return n - 1;
        }
        if (n % 3 == 0) {
            return (int)Math.Pow(3, n / 3);
        }
        if (n % 3 == 1) {
            return (int)Math.Pow(3, n / 3 - 1) * 4;
        }
        return (int)Math.Pow(3, n / 3) * 2;
    }
}
```

#### C

```c
int integerBreak(int n) {
    if (n < 4) {
        return n - 1;
    }
    if (n % 3 == 0) {
        return (int) pow(3, n / 3);
    }
    if (n % 3 == 1) {
        return (int) pow(3, n / 3 - 1) * 4;
    }
    return (int) pow(3, n / 3) * 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
