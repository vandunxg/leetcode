---
comments: true
difficulty: Medium
rating: 1533
source: Weekly Contest 177 Q3
tags:
    - Math
    - Prime Factorization
---

<!-- problem:start -->

# [1362. Closest Divisors](https://leetcode.com/problems/closest-divisors)

[中文文档](/solution/1300-1399/1362.Closest%20Divisors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>num</code>, hãy tìm hai số nguyên có độ chênh lệch tuyệt đối nhỏ nhất sao cho tích của chúng bằng&nbsp;<code>num + 1</code>&nbsp;hoặc <code>num + 2</code>.</p>

<p>Trả về hai số nguyên theo thứ tự bất kỳ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 8
<strong>Đầu ra:</strong> [3,3]
<strong>Giải thích:</strong> Với num + 1 = 9, cặp ước gần nhau nhất là 3 và 3; với num + 2 = 10, cặp ước gần nhau nhất là 2 và 5. Vì vậy, ta chọn 3 và 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 123
<strong>Đầu ra:</strong> [5,25]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 999
<strong>Đầu ra:</strong> [40,25]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10^9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Tìm cặp ước của $num+1$ hoặc $num+2$ có độ chênh lệch nhỏ nhất. Vì $num \le 10^9$, không thể duyệt đến tận $x$. Cặp ước gần nhau nhất nằm quanh $\sqrt{x}$, nên ta duyệt giảm dần từ $\lfloor\sqrt{x}\rfloor$ cho đến khi tìm thấy một ước. Làm vậy với cả $num+1$ và $num+2$, rồi chọn cặp có độ chênh lệch nhỏ hơn.

<!-- thinking:end -->

Ta thiết kế hàm $f(x)$ trả về hai số có tích bằng $x$ và độ chênh lệch tuyệt đối nhỏ nhất. Bắt đầu liệt kê $i$ từ $\sqrt{x}$. Nếu $x$ chia hết cho $i$, thì $\frac{x}{i}$ là ước còn lại. Khi đó, ta đã tìm được cặp ước có tích bằng $x$ và có thể trả về ngay. Nếu không, giảm $i$ rồi tiếp tục tìm.

Tiếp theo, ta lần lượt tính $f(num + 1)$ và $f(num + 2)$, rồi so sánh hai kết quả. Trả về cặp có độ chênh lệch tuyệt đối nhỏ hơn.

Độ phức tạp thời gian là $O(\sqrt{num})$, độ phức tạp không gian là $O(1)$, với $num$ là số nguyên được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def closestDivisors(self, num: int) -> List[int]:
        def f(x):
            for i in range(int(sqrt(x)), 0, -1):
                if x % i == 0:
                    return [i, x // i]

        a = f(num + 1)
        b = f(num + 2)
        return a if abs(a[0] - a[1]) < abs(b[0] - b[1]) else b
```

#### Java

```java
class Solution {
    public int[] closestDivisors(int num) {
        int[] a = f(num + 1);
        int[] b = f(num + 2);
        return Math.abs(a[0] - a[1]) < Math.abs(b[0] - b[1]) ? a : b;
    }

    private int[] f(int x) {
        for (int i = (int) Math.sqrt(x);; --i) {
            if (x % i == 0) {
                return new int[] {i, x / i};
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> closestDivisors(int num) {
        auto f = [](int x) {
            for (int i = sqrt(x);; --i) {
                if (x % i == 0) {
                    return vector<int>{i, x / i};
                }
            }
        };
        vector<int> a = f(num + 1);
        vector<int> b = f(num + 2);
        return abs(a[0] - a[1]) < abs(b[0] - b[1]) ? a : b;
    }
};
```

#### Go

```go
func closestDivisors(num int) []int {
	f := func(x int) []int {
		for i := int(math.Sqrt(float64(x))); ; i-- {
			if x%i == 0 {
				return []int{i, x / i}
			}
		}
	}
	a, b := f(num+1), f(num+2)
	if abs(a[0]-a[1]) < abs(b[0]-b[1]) {
		return a
	}
	return b
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
