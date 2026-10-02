---
comments: true
difficulty: Hard
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [793. Preimage Size of Factorial Zeroes Function](https://leetcode.com/problems/preimage-size-of-factorial-zeroes-function)

[中文文档](/solution/0700-0799/0793.Preimage%20Size%20of%20Factorial%20Zeroes%20Function/README.md)

## Mô tả

<!-- description:start -->

<p>Gọi <code>f(x)</code> là số chữ số 0 ở cuối <code>x!</code>. Nhắc lại rằng <code>x! = 1 * 2 * 3 * ... * x</code> và theo quy ước, <code>0! = 1</code>.</p>

<ul>
	<li>Ví dụ, <code>f(3) = 0</code> vì <code>3! = 6</code> không có chữ số 0 nào ở cuối, còn <code>f(11) = 2</code> vì <code>11! = 39916800</code> có hai chữ số 0 ở cuối.</li>
</ul>

<p>Cho số nguyên <code>k</code>, hãy trả về số lượng số nguyên không âm <code>x</code> thỏa mãn <code>f(x) = k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 0
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> 0!, 1!, 2!, 3! và 4! có k = 0 chữ số 0 ở cuối.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 5
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không tồn tại x sao cho x! có k = 5 chữ số 0 ở cuối.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 3
<strong>Đầu ra:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $f(x)$ là số chữ số 0 ở cuối của $x!$. Tập nghịch ảnh của $k$ rỗng nếu $f$ bỏ qua giá trị $k$; nếu không, đó là một đoạn liên tiếp các giá trị $x$ (thường có độ dài $5$).
>
> $g(k)$ là giá trị $x$ nhỏ nhất sao cho $f(x)\ge k$; đáp án là $g(k+1)-g(k)$. Vì $f(x)\ge x/5$, ta tìm $g$ bằng binary search trong đoạn $[0,5k]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def preimageSizeFZF(self, k: int) -> int:
        def f(x):
            if x == 0:
                return 0
            return x // 5 + f(x // 5)

        def g(k):
            return bisect_left(range(5 * k), k, key=f)

        return g(k + 1) - g(k)
```

#### Java

```java
class Solution {
    public int preimageSizeFZF(int k) {
        return g(k + 1) - g(k);
    }

    private int g(int k) {
        long left = 0, right = 5 * k;
        while (left < right) {
            long mid = (left + right) >> 1;
            if (f(mid) >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return (int) left;
    }

    private int f(long x) {
        if (x == 0) {
            return 0;
        }
        return (int) (x / 5) + f(x / 5);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int preimageSizeFZF(int k) {
        return g(k + 1) - g(k);
    }

    int g(int k) {
        long long left = 0, right = 1ll * 5 * k;
        while (left < right) {
            long long mid = (left + right) >> 1;
            if (f(mid) >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return (int) left;
    }

    int f(long x) {
        int res = 0;
        while (x) {
            x /= 5;
            res += x;
        }
        return res;
    }
};
```

#### Go

```go
func preimageSizeFZF(k int) int {
	f := func(x int) int {
		res := 0
		for x != 0 {
			x /= 5
			res += x
		}
		return res
	}

	g := func(k int) int {
		left, right := 0, k*5
		for left < right {
			mid := (left + right) >> 1
			if f(mid) >= k {
				right = mid
			} else {
				left = mid + 1
			}
		}
		return left
	}

	return g(k+1) - g(k)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
