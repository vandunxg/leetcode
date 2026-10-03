---
comments: true
difficulty: Easy
rating: 1207
source: Weekly Contest 321 Q1
tags:
    - Math
    - Prefix Sum
---

<!-- problem:start -->

# [2485. Find the Pivot Integer](https://leetcode.com/problems/find-the-pivot-integer)

[中文文档](/solution/2400-2499/2485.Find%20the%20Pivot%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code>, hãy tìm <strong>số nguyên pivot</strong> <code>x</code> sao cho:</p>

<ul>
	<li>Tổng tất cả các phần tử từ <code>1</code> đến <code>x</code>, bao gồm cả hai đầu, bằng tổng tất cả các phần tử từ <code>x</code> đến <code>n</code>, bao gồm cả hai đầu.</li>
</ul>

<p>Trả về <em>số nguyên pivot </em><code>x</code>. Nếu không tồn tại số nguyên như vậy, trả về <code>-1</code>. Bảo đảm rằng với mỗi đầu vào, có nhiều nhất một chỉ số pivot.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 8
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> 6 là số nguyên pivot vì: 1 + 2 + 3 + 4 + 5 + 6 = 6 + 7 + 8 = 21.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 1 là số nguyên pivot vì: 1 = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không tồn tại số nguyên như vậy.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 1000$, số nguyên pivot $x$ thỏa mãn $1+\cdots+x=x+\cdots+n$. Ta thử từng $x$ và so sánh $(1+x)x$ với $(x+n)(n-x+1)$.

<!-- thinking:end -->

Ta có thể duyệt trực tiếp $x$ trong đoạn $[1,..n]$ và kiểm tra xem phương trình sau có đúng hay không. Nếu đúng, $x$ là số nguyên pivot và ta có thể trả về $x$ ngay.

$$
(1 + x) \times x = (x + n) \times (n - x + 1)
$$

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên dương đầu vào (giá trị $n$ đã cho). Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pivotInteger(self, n: int) -> int:
        for x in range(1, n + 1):
            if (1 + x) * x == (x + n) * (n - x + 1):
                return x
        return -1
```

#### Java

```java
class Solution {
    public int pivotInteger(int n) {
        for (int x = 1; x <= n; ++x) {
            if ((1 + x) * x == (x + n) * (n - x + 1)) {
                return x;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int pivotInteger(int n) {
        for (int x = 1; x <= n; ++x) {
            if ((1 + x) * x == (x + n) * (n - x + 1)) {
                return x;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func pivotInteger(n int) int {
	for x := 1; x <= n; x++ {
		if (1+x)*x == (x+n)*(n-x+1) {
			return x
		}
	}
	return -1
}
```

#### TypeScript

```ts
function pivotInteger(n: number): number {
    for (let x = 1; x <= n; ++x) {
        if ((1 + x) * x === (x + n) * (n - x + 1)) {
            return x;
        }
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn pivot_integer(n: i32) -> i32 {
        let y = (n * (n + 1)) / 2;
        let x = (y as f64).sqrt() as i32;

        if x * x == y {
            return x;
        }

        -1
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $n
     * @return Integer
     */
    function pivotInteger($n) {
        $sum = ($n * ($n + 1)) / 2;
        $pre = 0;
        for ($i = 1; $i <= $n; $i++) {
            if ($pre + $i === $sum - $pre) {
                return $i;
            }
            $pre += $i;
        }
        return -1;
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
> Phương pháp 1 duyệt tuyến tính. Đẳng thức trở thành $x^2=n(n+1)/2$, vì vậy ta kiểm tra xem số tam giác đó có phải là một số chính phương hay không: $x=\lfloor\sqrt{y}\rfloor$ và $x^2=y$. Độ phức tạp là hằng số.

<!-- thinking:end -->

Ta có thể biến đổi phương trình trên để được:

$$
n \times (n + 1) = 2 \times x^2
$$

Tức là:

$$
x = \sqrt{\frac{n \times (n + 1)}{2}}
$$

Nếu $x$ là một số nguyên thì $x$ là số nguyên pivot; nếu không thì không tồn tại số nguyên pivot.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pivotInteger(self, n: int) -> int:
        y = n * (n + 1) // 2
        x = int(sqrt(y))
        return x if x * x == y else -1
```

#### Java

```java
class Solution {
    public int pivotInteger(int n) {
        int y = n * (n + 1) / 2;
        int x = (int) Math.sqrt(y);
        return x * x == y ? x : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int pivotInteger(int n) {
        int y = n * (n + 1) / 2;
        int x = sqrt(y);
        return x * x == y ? x : -1;
    }
};
```

#### Go

```go
func pivotInteger(n int) int {
	y := n * (n + 1) / 2
	x := int(math.Sqrt(float64(y)))
	if x*x == y {
		return x
	}
	return -1
}
```

#### TypeScript

```ts
function pivotInteger(n: number): number {
    const y = Math.floor((n * (n + 1)) / 2);
    const x = Math.floor(Math.sqrt(y));
    return x * x === y ? x : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
