---
comments: true
difficulty: Easy
tags:
    - Math
---

<!-- problem:start -->

# [263. Ugly Number](https://leetcode.com/problems/ugly-number)

[中文文档](/solution/0200-0299/0263.Ugly%20Number/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số ugly</strong> là số nguyên <em>dương</em> không có thừa số nguyên tố nào ngoài 2, 3 và 5.</p>

<p>Cho số nguyên <code>n</code>, hãy trả về <code>true</code> <em>nếu</em> <code>n</code> <em>là một <strong>số ugly</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 6 = 2 &times; 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 1 không có thừa số nguyên tố nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 14
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> 14 không phải số ugly vì có thừa số nguyên tố 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-2<sup>31</sup> &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Số ugly chỉ có các thừa số nguyên tố là $2,3,5$. Loại các số không dương; với số dương, lần lượt chia cho ba số nguyên tố này đến khi không còn chia hết, rồi kiểm tra xem kết quả còn lại có bằng $1$ hay không.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isUgly(self, n: int) -> bool:
        if n < 1:
            return False
        for x in [2, 3, 5]:
            while n % x == 0:
                n //= x
        return n == 1
```

#### Java

```java
class Solution {
    public boolean isUgly(int n) {
        if (n < 1) return false;
        while (n % 2 == 0) {
            n /= 2;
        }
        while (n % 3 == 0) {
            n /= 3;
        }
        while (n % 5 == 0) {
            n /= 5;
        }
        return n == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isUgly(int n) {
        if (n < 1) return false;
        while (n % 2 == 0) {
            n /= 2;
        }
        while (n % 3 == 0) {
            n /= 3;
        }
        while (n % 5 == 0) {
            n /= 5;
        }
        return n == 1;
    }
};
```

#### Go

```go
func isUgly(n int) bool {
	if n < 1 {
		return false
	}
	for _, x := range []int{2, 3, 5} {
		for n%x == 0 {
			n /= x
		}
	}
	return n == 1
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var isUgly = function (n) {
    if (n < 1) return false;
    while (n % 2 === 0) {
        n /= 2;
    }
    while (n % 3 === 0) {
        n /= 3;
    }
    while (n % 5 === 0) {
        n /= 5;
    }
    return n === 1;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $n
     * @return Boolean
     */
    function isUgly($n) {
        while ($n) {
            if ($n % 2 == 0) {
                $n = $n / 2;
            } elseif ($n % 3 == 0) {
                $n = $n / 3;
            } elseif ($n % 5 == 0) {
                $n = $n / 5;
            } else {
                break;
            }
        }
        return $n == 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
