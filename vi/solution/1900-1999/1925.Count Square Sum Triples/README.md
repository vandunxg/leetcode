---
comments: true
difficulty: Easy
rating: 1323
source: Biweekly Contest 56 Q1
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [1925. Count Square Sum Triples](https://leetcode.com/problems/count-square-sum-triples)

[中文文档](/solution/1900-1999/1925.Count%20Square%20Sum%20Triples/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>bộ ba Pythagore</strong> <code>(a,b,c)</code> là một bộ ba trong đó <code>a</code>, <code>b</code> và <code>c</code> là các <strong>số nguyên</strong>, đồng thời <code>a<sup>2</sup> + b<sup>2</sup> = c<sup>2</sup></code>.</p>

<p>Cho một số nguyên <code>n</code>, hãy trả về <em>số lượng <strong>bộ ba Pythagore</strong> sao cho </em><code>1 &lt;= a, b, c &lt;= n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích</strong>: Các bộ ba Pythagore là (3,4,5) và (4,3,5).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 4
<strong>Giải thích</strong>: Các bộ ba Pythagore là (3,4,5), (4,3,5), (6,8,10) và (8,6,10).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 250</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ nhiều nhất chỉ vài trăm, ta có thể liệt kê $a,b$ và kiểm tra xem $a^2+b^2$ có phải là một số chính phương $\le n$ hay không. Độ phức tạp $O(n^2)$ là chấp nhận được.
>
> Lấy $c=\lfloor\sqrt{a^2+b^2}\rfloor$ và chấp nhận khi $c^2$ bằng tổng cần kiểm tra và $c\le n$. Hai vòng lặp độc lập sẽ đếm cả $(a,b,c)$ và $(b,a,c)$.

<!-- thinking:end -->

Ta liệt kê $a$ và $b$ trong khoảng $[1, n)$, sau đó tính $c = \sqrt{a^2 + b^2}$. Nếu $c$ là số nguyên và $c \leq n$, ta đã tìm thấy một bộ ba Pythagore và tăng đáp án lên một đơn vị.

Sau khi hoàn tất việc liệt kê, trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là số nguyên đầu vào. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTriples(self, n: int) -> int:
        ans = 0
        for a in range(1, n):
            for b in range(1, n):
                x = a * a + b * b
                c = int(sqrt(x))
                if c <= n and c * c == x:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countTriples(int n) {
        int ans = 0;
        for (int a = 1; a < n; a++) {
            for (int b = 1; b < n; b++) {
                int x = a * a + b * b;
                int c = (int) Math.sqrt(x);
                if (c <= n && c * c == x) {
                    ans++;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countTriples(int n) {
        int ans = 0;
        for (int a = 1; a < n; ++a) {
            for (int b = 1; b < n; ++b) {
                int x = a * a + b * b;
                int c = static_cast<int>(sqrt(x));
                if (c <= n && c * c == x) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countTriples(n int) (ans int) {
	for a := 1; a < n; a++ {
		for b := 1; b < n; b++ {
			x := a*a + b*b
			c := int(math.Sqrt(float64(x)))
			if c <= n && c*c == x {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countTriples(n: number): number {
    let ans = 0;
    for (let a = 1; a < n; a++) {
        for (let b = 1; b < n; b++) {
            const x = a * a + b * b;
            const c = Math.floor(Math.sqrt(x));
            if (c <= n && c * c === x) {
                ans++;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
