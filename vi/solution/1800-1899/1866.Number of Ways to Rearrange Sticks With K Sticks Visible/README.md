---
comments: true
difficulty: Hard
rating: 2333
source: Weekly Contest 241 Q4
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [1866. Number of Ways to Rearrange Sticks With K Sticks Visible](https://leetcode.com/problems/number-of-ways-to-rearrange-sticks-with-k-sticks-visible)

[中文文档](/solution/1800-1899/1866.Number%20of%20Ways%20to%20Rearrange%20Sticks%20With%20K%20Sticks%20Visible/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> que có độ dài khác nhau, với độ dài là các số nguyên từ <code>1</code> đến <code>n</code>. Bạn muốn sắp xếp các que sao cho <strong>chính xác</strong> <code>k</code> que <strong>có thể nhìn thấy</strong> từ bên trái. Một que <strong>có thể nhìn thấy</strong> từ bên trái nếu không có que nào <strong>dài hơn</strong> nó ở <strong>bên trái</strong>.</p>

<ul>
	<li>Ví dụ, nếu các que được sắp xếp thành <code>[<u>1</u>,<u>3</u>,2,<u>5</u>,4]</code>, thì các que có độ dài <code>1</code>, <code>3</code> và <code>5</code> có thể nhìn thấy từ bên trái.</li>
</ul>

<p>Cho <code>n</code> và <code>k</code>, hãy trả về <em><strong>số</strong> cách sắp xếp như vậy</em>. Vì đáp án có thể lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> [<u>1</u>,<u>3</u>,2], [<u>2</u>,<u>3</u>,1] và [<u>2</u>,1,<u>3</u>] là những cách sắp xếp duy nhất có chính xác 2 que có thể nhìn thấy.
Các que có thể nhìn thấy được gạch chân.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, k = 5
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> [<u>1</u>,<u>2</u>,<u>3</u>,<u>4</u>,<u>5</u>] là cách sắp xếp duy nhất có cả 5 que đều có thể nhìn thấy.
Các que có thể nhìn thấy được gạch chân.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 20, k = 11
<strong>Đầu ra:</strong> 647427950
<strong>Giải thích:</strong> Có 647427950 (mod 10<sup>9 </sup>+ 7) cách sắp xếp lại các que sao cho chính xác 11 que có thể nhìn thấy.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm các hoán vị của $n$ que trong đó chính xác $k$ que có thể nhìn thấy từ bên trái. Một que có thể nhìn thấy khi và chỉ khi nó cao nhất tính đến vị trí đó. Với $n\le 1000$, không thể liệt kê các hoán vị.
>
> Gọi $f[i][j]$ là số cách sắp xếp $i$ que có $j$ que nhìn thấy. Nếu đặt que dài nhất ở cuối, số que nhìn thấy luôn tăng thêm một; nếu que cuối không phải que dài nhất, nó là một trong $i-1$ que còn lại và số que nhìn thấy không đổi. Từ đó ta thu được truy hồi để tính $f[n][k]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số hoán vị độ dài $i$ trong đó có chính xác $j$ que có thể nhìn thấy. Ban đầu, $f[0][0]=1$ và các giá trị còn lại $f[i][j]=0$. Đáp án là $f[n][k]$.

Xét xem que cuối cùng có thể nhìn thấy hay không. Nếu có thể nhìn thấy, nó phải là que dài nhất. Khi đó có $i - 1$ que ở phía trước, và chính xác $j - 1$ que có thể nhìn thấy, tương ứng với $f[i - 1][j - 1]$. Nếu que cuối cùng không thể nhìn thấy, nó có thể là bất kỳ que nào ngoại trừ que dài nhất. Khi đó có $i - 1$ que ở phía trước và chính xác $j$ que có thể nhìn thấy, tương ứng với $f[i - 1][j] \times (i - 1)$.

Vì vậy, công thức chuyển trạng thái là:

$$
f[i][j] = f[i - 1][j - 1] + f[i - 1][j] \times (i - 1)
$$

Đáp án cuối cùng là $f[n][k]$.

Độ phức tạp thời gian là $O(n \times k)$, độ phức tạp không gian là $O(n \times k)$, trong đó $n$ và $k$ là hai số nguyên được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeSticks(self, n: int, k: int) -> int:
        mod = 10**9 + 7
        f = [[0] * (k + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                f[i][j] = (f[i - 1][j - 1] + f[i - 1][j] * (i - 1)) % mod
        return f[n][k]
```

#### Java

```java
class Solution {
    public int rearrangeSticks(int n, int k) {
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[n + 1][k + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                f[i][j] = (int) ((f[i - 1][j - 1] + f[i - 1][j] * (long) (i - 1)) % mod);
            }
        }
        return f[n][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rearrangeSticks(int n, int k) {
        const int mod = 1e9 + 7;
        int f[n + 1][k + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                f[i][j] = (f[i - 1][j - 1] + (i - 1LL) * f[i - 1][j]) % mod;
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func rearrangeSticks(n int, k int) int {
	const mod = 1e9 + 7
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
	}
	f[0][0] = 1
	for i := 1; i <= n; i++ {
		for j := 1; j <= k; j++ {
			f[i][j] = (f[i-1][j-1] + (i-1)*f[i-1][j]) % mod
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function rearrangeSticks(n: number, k: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[][] = Array.from({ length: n + 1 }, () =>
        Array.from({ length: k + 1 }, () => 0),
    );
    f[0][0] = 1;
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= k; ++j) {
            f[i][j] = (f[i - 1][j - 1] + (i - 1) * f[i - 1][j]) % mod;
        }
    }
    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> $f[i][j]$ trong Lời giải 1 chỉ phụ thuộc vào hàng trước đó. Ta nén thành một mảng và cập nhật $j$ theo thứ tự giảm dần để $f[j-1]$ vẫn là giá trị cũ. Không gian phụ thêm giảm còn $O(k)$.

<!-- thinking:end -->

Ta nhận thấy $f[i][j]$ chỉ liên quan đến $f[i - 1][j - 1]$ và $f[i - 1][j]$, nên có thể dùng mảng một chiều để tối ưu độ phức tạp không gian.

Độ phức tạp thời gian là $O(n \times k)$, độ phức tạp không gian là $O(k)$. Ở đây, $n$ và $k$ là hai số nguyên được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeSticks(self, n: int, k: int) -> int:
        mod = 10**9 + 7
        f = [1] + [0] * k
        for i in range(1, n + 1):
            for j in range(k, 0, -1):
                f[j] = (f[j] * (i - 1) + f[j - 1]) % mod
            f[0] = 0
        return f[k]
```

#### Java

```java
class Solution {
    public int rearrangeSticks(int n, int k) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[k + 1];
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = k; j > 0; --j) {
                f[j] = (int) ((f[j] * (i - 1L) + f[j - 1]) % mod);
            }
            f[0] = 0;
        }
        return f[k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rearrangeSticks(int n, int k) {
        const int mod = 1e9 + 7;
        int f[k + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = k; j; --j) {
                f[j] = (f[j - 1] + f[j] * (i - 1LL)) % mod;
            }
            f[0] = 0;
        }
        return f[k];
    }
};
```

#### Go

```go
func rearrangeSticks(n int, k int) int {
	const mod = 1e9 + 7
	f := make([]int, k+1)
	f[0] = 1
	for i := 1; i <= n; i++ {
		for j := k; j > 0; j-- {
			f[j] = (f[j-1] + f[j]*(i-1)) % mod
		}
		f[0] = 0
	}
	return f[k]
}
```

#### TypeScript

```ts
function rearrangeSticks(n: number, k: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[] = Array(n + 1).fill(0);
    f[0] = 1;
    for (let i = 1; i <= n; ++i) {
        for (let j = k; j; --j) {
            f[j] = (f[j] * (i - 1) + f[j - 1]) % mod;
        }
        f[0] = 0;
    }
    return f[k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
