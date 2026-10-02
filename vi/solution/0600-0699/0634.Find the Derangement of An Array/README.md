---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [634. Find the Derangement of An Array 🔒](https://leetcode.com/problems/find-the-derangement-of-an-array)

[中文文档](/solution/0600-0699/0634.Find%20the%20Derangement%20of%20An%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Trong toán tổ hợp, <strong>derangement</strong> là một hoán vị các phần tử của một tập hợp sao cho không phần tử nào nằm ở vị trí ban đầu.</p>

<p>Cho số nguyên <code>n</code>. Ban đầu có một mảng gồm <code>n</code> số nguyên từ <code>1</code> đến <code>n</code> theo thứ tự tăng dần. Hãy trả về <em>số derangement có thể tạo thành từ mảng đó</em>. Vì kết quả có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Mảng ban đầu là [1,2,3]. Hai derangement có thể tạo thành là [2,3,1] và [3,1,2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Số derangement $!n$ có công thức bao hàm – loại trừ, nhưng phép nghịch đảo modulo khá bất tiện. Dùng công thức truy hồi sẽ đơn giản hơn.
>
> Sau khi đặt $1$ vào vị trí $j$, hoặc đặt $j$ về vị trí đầu hoặc không, từ đó có $f[i]=(i-1)(f[i-1]+f[i-2])$. Tính bảng đến $n$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số derangement của một mảng có độ dài $i$. Ban đầu, $f[0] = 1$, $f[1] = 0$. Đáp án là $f[n]$.

Với mảng có độ dài $i$, ta xét vị trí đặt số $1$. Giả sử ta đặt nó vào vị trí thứ $j$, có $i-1$ lựa chọn. Khi đó, số $j$ có hai cách đặt:

- Đặt vào vị trí đầu tiên: $i - 2$ vị trí còn lại có $f[i - 2]$ derangement, nên tổng cộng có $(i - 1) \times f[i - 2]$ derangement;
- Không đặt vào vị trí đầu tiên, tương đương với bài toán derangement của mảng có độ dài $i - 1$, nên tổng cộng có $(i - 1) \times f[i - 1]$ derangement.

Tóm lại, ta có công thức chuyển trạng thái sau:

$$
f[i] = (i - 1) \times (f[i - 1] + f[i - 2])
$$

Đáp án cuối cùng là $f[n]$. Đừng quên lấy modulo cho kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDerangement(self, n: int) -> int:
        mod = 10**9 + 7
        f = [1] + [0] * n
        for i in range(2, n + 1):
            f[i] = (i - 1) * (f[i - 1] + f[i - 2]) % mod
        return f[n]
```

#### Java

```java
class Solution {
    public int findDerangement(int n) {
        long[] f = new long[n + 1];
        f[0] = 1;
        final int mod = (int) 1e9 + 7;
        for (int i = 2; i <= n; ++i) {
            f[i] = (i - 1) * (f[i - 1] + f[i - 2]) % mod;
        }
        return (int) f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findDerangement(int n) {
        long long f[n + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        const int mod = 1e9 + 7;
        for (int i = 2; i <= n; i++) {
            f[i] = (i - 1LL) * (f[i - 1] + f[i - 2]) % mod;
        }
        return f[n];
    }
};
```

#### Go

```go
func findDerangement(n int) int {
	f := make([]int, n+1)
	f[0] = 1
	const mod = 1e9 + 7
	for i := 2; i <= n; i++ {
		f[i] = (i - 1) * (f[i-1] + f[i-2]) % mod
	}
	return f[n]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ cần hai giá trị trước đó, vì vậy có thể thay mảng bằng hai biến vô hướng.

<!-- thinking:end -->

Ta thấy công thức chuyển trạng thái chỉ phụ thuộc vào $f[i - 1]$ và $f[i - 2]$. Vì vậy, ta có thể dùng hai biến $a$ và $b$ lần lượt biểu diễn $f[i - 1]$ và $f[i - 2]$, qua đó giảm độ phức tạp không gian xuống $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDerangement(self, n: int) -> int:
        mod = 10**9 + 7
        a, b = 1, 0
        for i in range(2, n + 1):
            a, b = b, ((i - 1) * (a + b)) % mod
        return b
```

#### Java

```java
class Solution {
    public int findDerangement(int n) {
        final int mod = (int) 1e9 + 7;
        long a = 1, b = 0;
        for (int i = 2; i <= n; ++i) {
            long c = (i - 1) * (a + b) % mod;
            a = b;
            b = c;
        }
        return (int) b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findDerangement(int n) {
        long long a = 1, b = 0;
        const int mod = 1e9 + 7;
        for (int i = 2; i <= n; ++i) {
            long long c = (i - 1) * (a + b) % mod;
            a = b;
            b = c;
        }
        return b;
    }
};
```

#### Go

```go
func findDerangement(n int) int {
	a, b := 1, 0
	const mod = 1e9 + 7
	for i := 2; i <= n; i++ {
		a, b = b, (i-1)*(a+b)%mod
	}
	return b
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
