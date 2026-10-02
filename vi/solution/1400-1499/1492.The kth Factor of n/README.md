---
comments: true
difficulty: Medium
rating: 1231
source: Biweekly Contest 29 Q2
tags:
    - Math
    - Number Theory
    - Prime Factorization
---

<!-- problem:start -->

# [1492. The kth Factor of n](https://leetcode.com/problems/the-kth-factor-of-n)

[中文文档](/solution/1400-1499/1492.The%20kth%20Factor%20of%20n/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>n</code> và <code>k</code>. Một ước của số nguyên <code>n</code> được định nghĩa là số nguyên <code>i</code> sao cho <code>n % i == 0</code>.</p>

<p>Xét danh sách tất cả các ước của <code>n</code> được sắp xếp theo <strong>thứ tự tăng dần</strong>, hãy trả về <em>ước thứ </em><code>k<sup>th</sup></code><em> </em>trong danh sách đó, hoặc trả về <code>-1</code> nếu <code>n</code> có ít hơn <code>k</code> ước.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 12, k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Danh sách các ước là [1, 2, 3, 4, 6, 12], ước thứ 3 là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7, k = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Danh sách các ước là [1, 7], ước thứ 2 là 7.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, k = 4
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Danh sách các ước là [1, 2, 4], chỉ có 3 ước. Ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<p>Bạn có thể giải bài toán này với độ phức tạp nhỏ hơn O(n) không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê bằng vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 1000$. Duyệt từ $1$ đến $n$, giảm $k$ mỗi khi gặp một ước và trả về khi $k$ bằng $0$. Nếu duyệt hết mà vẫn chưa tìm thấy, trả về $-1$.

<!-- thinking:end -->

Một "ước" là một số có thể chia hết một số khác. Vì vậy, ta chỉ cần liệt kê từ $1$ đến $n$, tìm tất cả các số có thể chia hết $n$, rồi trả về số thứ $k$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthFactor(self, n: int, k: int) -> int:
        for i in range(1, n + 1):
            if n % i == 0:
                k -= 1
                if k == 0:
                    return i
        return -1
```

#### Java

```java
class Solution {
    public int kthFactor(int n, int k) {
        for (int i = 1; i <= n; ++i) {
            if (n % i == 0 && (--k == 0)) {
                return i;
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
    int kthFactor(int n, int k) {
        for (int i = 1; i <= n; ++i) {
            if (n % i == 0 && (--k == 0)) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func kthFactor(n int, k int) int {
	for i := 1; i <= n; i++ {
		if n%i == 0 {
			k--
			if k == 0 {
				return i
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function kthFactor(n: number, k: number): number {
    for (let i = 1; i <= n; ++i) {
        if (n % i === 0 && --k === 0) {
            return i;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Liệt kê tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 có độ phức tạp $O(n)$. Các ước xuất hiện theo cặp. Liệt kê các ước nhỏ đến $\lfloor\sqrt{n}\rfloor$; nếu vẫn còn $k$, duyệt $i$ theo chiều giảm dần và tạo ra $n/i$ cho các ước lớn, với độ phức tạp $O(\sqrt{n})$.

<!-- thinking:end -->

Ta có thể nhận thấy rằng nếu $n$ có một ước $x$, thì $n$ cũng có ước $n/x$.

Vì vậy, trước tiên ta cần liệt kê $[1,2,...\left \lfloor \sqrt{n} \right \rfloor]$, tìm tất cả các số có thể chia hết $n$. Nếu tìm thấy ước thứ $k$, ta có thể trả về ngay. Nếu không tìm thấy ước thứ $k$, ta cần liệt kê $[\left \lfloor \sqrt{n} \right \rfloor ,..1]$ theo thứ tự ngược lại và tìm ước thứ $k$.

Độ phức tạp thời gian là $O(\sqrt{n})$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthFactor(self, n: int, k: int) -> int:
        i = 1
        while i * i < n:
            if n % i == 0:
                k -= 1
                if k == 0:
                    return i
            i += 1
        if i * i != n:
            i -= 1
        while i:
            if (n % (n // i)) == 0:
                k -= 1
                if k == 0:
                    return n // i
            i -= 1
        return -1
```

#### Java

```java
class Solution {
    public int kthFactor(int n, int k) {
        int i = 1;
        for (; i < n / i; ++i) {
            if (n % i == 0 && (--k == 0)) {
                return i;
            }
        }
        if (i * i != n) {
            --i;
        }
        for (; i > 0; --i) {
            if (n % (n / i) == 0 && (--k == 0)) {
                return n / i;
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
    int kthFactor(int n, int k) {
        int i = 1;
        for (; i < n / i; ++i) {
            if (n % i == 0 && (--k == 0)) {
                return i;
            }
        }
        if (i * i != n) {
            --i;
        }
        for (; i > 0; --i) {
            if (n % (n / i) == 0 && (--k == 0)) {
                return n / i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func kthFactor(n int, k int) int {
	i := 1
	for ; i < n/i; i++ {
		if n%i == 0 {
			k--
			if k == 0 {
				return i
			}
		}
	}
	if i*i != n {
		i--
	}
	for ; i > 0; i-- {
		if n%(n/i) == 0 {
			k--
			if k == 0 {
				return n / i
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function kthFactor(n: number, k: number): number {
    let i: number = 1;
    for (; i < n / i; ++i) {
        if (n % i === 0 && --k === 0) {
            return i;
        }
    }
    if (i * i !== n) {
        --i;
    }
    for (; i > 0; --i) {
        if (n % Math.floor(n / i) === 0 && --k === 0) {
            return Math.floor(n / i);
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
