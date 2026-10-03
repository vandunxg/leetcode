---
comments: true
difficulty: Easy
rating: 1203
source: Weekly Contest 252 Q1
tags:
    - Math
    - Enumeration
    - Number Theory
    - Sieve
    - Prime Factorization
---

<!-- problem:start -->

# [1952. Three Divisors](https://leetcode.com/problems/three-divisors)

[中文文档](/solution/1900-1999/1952.Three%20Divisors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>, hãy trả về <code>true</code><em> nếu </em><code>n</code><em> có <strong>đúng ba ước dương</strong>. Nếu không, trả về </em><code>false</code>.</p>

<p>Một số nguyên <code>m</code> là một <strong>ước</strong> của <code>n</code> nếu tồn tại một số nguyên <code>k</code> sao cho <code>n = k * m</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> 2 chỉ có hai ước: 1 và 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 4 có ba ước: 1, 2 và 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một số có đúng ba ước dương khi và chỉ khi nó là bình phương của một số nguyên tố. Vì $n$ nhỏ, chỉ cần đếm các ước trong khoảng $2..n-1$ và kiểm tra xem số lượng đó có bằng $1$ hay không.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isThree(self, n: int) -> bool:
        return sum(n % i == 0 for i in range(2, n)) == 1
```

#### Java

```java
class Solution {
    public boolean isThree(int n) {
        int cnt = 0;
        for (int i = 2; i < n; i++) {
            if (n % i == 0) {
                ++cnt;
            }
        }
        return cnt == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isThree(int n) {
        int cnt = 0;
        for (int i = 2; i < n; ++i) {
            cnt += n % i == 0;
        }
        return cnt == 1;
    }
};
```

#### Go

```go
func isThree(n int) bool {
	cnt := 0
	for i := 2; i < n; i++ {
		if n%i == 0 {
			cnt++
		}
	}
	return cnt == 1
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var isThree = function (n) {
    let cnt = 0;
    for (let i = 2; i < n; ++i) {
        if (n % i == 0) {
            ++cnt;
        }
    }
    return cnt == 1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Việc duyệt tuyến tính sẽ lãng phí thời gian khi n gần giới hạn trên. Chỉ cần liệt kê đến $\sqrt{n}$ là có thể đếm mỗi cặp một lần (hoặc một lần đối với số chính phương), rồi kiểm tra xem tổng số ước có bằng $3$ hay không.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isThree(self, n: int) -> bool:
        cnt = 0
        i = 1
        while i <= n // i:
            if n % i == 0:
                cnt += 1 if i == n // i else 2
            i += 1
        return cnt == 3
```

#### Java

```java
class Solution {
    public boolean isThree(int n) {
        int cnt = 0;
        for (int i = 1; i <= n / i; ++i) {
            if (n % i == 0) {
                cnt += n / i == i ? 1 : 2;
            }
        }
        return cnt == 3;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isThree(int n) {
        int cnt = 0;
        for (int i = 1; i <= n / i; ++i) {
            if (n % i == 0) {
                cnt += n / i == i ? 1 : 2;
            }
        }
        return cnt == 3;
    }
};
```

#### Go

```go
func isThree(n int) bool {
	cnt := 0
	for i := 1; i <= n/i; i++ {
		if n%i == 0 {
			if n/i == i {
				cnt++
			} else {
				cnt += 2
			}
		}
	}
	return cnt == 3
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var isThree = function (n) {
    let cnt = 0;
    for (let i = 1; i <= n / i; ++i) {
        if (n % i == 0) {
            cnt += ~~(n / i) == i ? 1 : 2;
        }
    }
    return cnt == 3;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
