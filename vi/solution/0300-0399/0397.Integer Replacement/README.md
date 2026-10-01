---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Bit Manipulation
    - Memoization
    - Dynamic Programming
---

<!-- problem:start -->

# [397. Integer Replacement](https://leetcode.com/problems/integer-replacement)

[中文文档](/solution/0300-0399/0397.Integer%20Replacement/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương <code>n</code>, bạn có thể thực hiện một trong các phép toán sau:</p>

<ol>
	<li>Nếu <code>n</code> là số chẵn, thay <code>n</code> bằng <code>n / 2</code>.</li>
	<li>Nếu <code>n</code> là số lẻ, thay <code>n</code> bằng <code>n + 1</code> hoặc <code>n - 1</code>.</li>
</ol>

<p>Trả về <em>số phép toán ít nhất cần thực hiện để</em> <code>n</code> <em>trở thành</em> <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 8
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 8 -&gt; 4 -&gt; 2 -&gt; 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>7 -&gt; 8 -&gt; 4 -&gt; 2 -&gt; 1
hoặc 7 -&gt; 6 -&gt; 3 -&gt; 2 -&gt; 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với số chẵn, chia cho $2$; với số lẻ, cộng hoặc trừ $1$. Mục tiêu là đến $1$ với ít bước nhất. Có thể dùng BFS, nhưng greedy dựa trên bit sẽ ngắn gọn hơn. Số chẵn luôn được dịch phải; với số lẻ có hai bit cuối là $11$ (trừ $3$), tăng thêm $1$ để bỏ được nhiều bit $1$ hơn; các trường hợp còn lại thì giảm $1$.
>
> Với $3$, chọn giảm để tránh đi theo đường $3\to 4$. Lặp cho đến khi đạt $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def integerReplacement(self, n: int) -> int:
        ans = 0
        while n != 1:
            if (n & 1) == 0:
                n >>= 1
            elif n != 3 and (n & 3) == 3:
                n += 1
            else:
                n -= 1
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int integerReplacement(int n) {
        int ans = 0;
        while (n != 1) {
            if ((n & 1) == 0) {
                n >>>= 1;
            } else if (n != 3 && (n & 3) == 3) {
                ++n;
            } else {
                --n;
            }
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
    int integerReplacement(int N) {
        int ans = 0;
        long n = N;
        while (n != 1) {
            if ((n & 1) == 0)
                n >>= 1;
            else if (n != 3 && (n & 3) == 3)
                ++n;
            else
                --n;
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func integerReplacement(n int) int {
	ans := 0
	for n != 1 {
		if (n & 1) == 0 {
			n >>= 1
		} else if n != 3 && (n&3) == 3 {
			n++
		} else {
			n--
		}
		ans++
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
