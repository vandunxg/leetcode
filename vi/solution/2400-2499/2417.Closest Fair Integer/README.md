---
comments: true
difficulty: Medium
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [2417. Closest Fair Integer 🔒](https://leetcode.com/problems/closest-fair-integer)

[Tài liệu tiếng Trung](/solution/2400-2499/2417.Closest%20Fair%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>số nguyên dương</strong> <code>n</code>.</p>

<p>Một số nguyên <code>k</code> được gọi là số công bằng nếu số <strong>chữ số chẵn</strong> trong <code>k</code> bằng số <strong>chữ số lẻ</strong> trong đó.</p>

<p>Hãy trả về <em>số nguyên công bằng <strong>nhỏ nhất</strong> <strong>lớn hơn hoặc bằng</strong> </em><code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Số nguyên công bằng nhỏ nhất lớn hơn hoặc bằng 2 là 10.
10 là số công bằng vì nó có số chữ số chẵn và lẻ bằng nhau (một chữ số lẻ và một chữ số chẵn).</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 403
<strong>Đầu ra:</strong> 1001
<strong>Giải thích:</strong> Số nguyên công bằng nhỏ nhất lớn hơn hoặc bằng 403 là 1001.
1001 là số công bằng vì nó có số chữ số chẵn và lẻ bằng nhau (hai chữ số lẻ và hai chữ số chẵn).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Một số công bằng cần có số chữ số chẵn và số chữ số lẻ bằng nhau, nên số chữ số của nó phải là số chẵn. Bản thân $n$ có thể đã là số công bằng; nếu không, ta cần tìm số gần nhất lớn hơn hoặc bằng $n$.
>
> Nếu số chữ số hiện tại $k$ là số lẻ thì không có số nào có $k$ chữ số thỏa mãn; ta xây dựng số công bằng nhỏ nhất có $(k+1)$ chữ số (chữ số đầu là $1$, tiếp theo là các số 0, rồi đến các số 1 ở nửa cuối). Nếu $k$ là số chẵn và $n$ chưa công bằng, ta đệ quy trên $n+1$ cho đến khi rơi vào một trong hai trường hợp trên.

<!-- thinking:end -->

Ta ký hiệu số chữ số của $n$ là $k$, số chữ số lẻ và số chữ số chẵn lần lượt là $a$ và $b$.

- Nếu $a = b$, thì bản thân $n$ là `fair`, và ta có thể trả về trực tiếp $n$;
- Ngược lại, nếu $k$ là số lẻ, ta chỉ cần tìm số `fair` nhỏ nhất có $k+1$ chữ số, có dạng `10000111`. Nếu $k$ là số chẵn, ta có thể brute force bằng đệ quy `closestFair(n+1)`.

Độ phức tạp thời gian là $O(\sqrt{n} \times \log_{10} n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def closestFair(self, n: int) -> int:
        a = b = k = 0
        t = n
        while t:
            if (t % 10) & 1:
                a += 1
            else:
                b += 1
            t //= 10
            k += 1
        if k & 1:
            x = 10**k
            y = int('1' * (k >> 1) or '0')
            return x + y
        if a == b:
            return n
        return self.closestFair(n + 1)
```

#### Java

```java
class Solution {
    public int closestFair(int n) {
        int a = 0, b = 0;
        int k = 0, t = n;
        while (t > 0) {
            if ((t % 10) % 2 == 1) {
                ++a;
            } else {
                ++b;
            }
            t /= 10;
            ++k;
        }
        if (k % 2 == 1) {
            int x = (int) Math.pow(10, k);
            int y = 0;
            for (int i = 0; i < k >> 1; ++i) {
                y = y * 10 + 1;
            }
            return x + y;
        }
        if (a == b) {
            return n;
        }
        return closestFair(n + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int closestFair(int n) {
        int a = 0, b = 0;
        int t = n, k = 0;
        while (t) {
            if ((t % 10) & 1) {
                ++a;
            } else {
                ++b;
            }
            ++k;
            t /= 10;
        }
        if (a == b) {
            return n;
        }
        if (k % 2 == 1) {
            int x = pow(10, k);
            int y = 0;
            for (int i = 0; i < k >> 1; ++i) {
                y = y * 10 + 1;
            }
            return x + y;
        }
        return closestFair(n + 1);
    }
};
```

#### Go

```go
func closestFair(n int) int {
	a, b := 0, 0
	t, k := n, 0
	for t > 0 {
		if (t%10)%2 == 1 {
			a++
		} else {
			b++
		}
		k++
		t /= 10
	}
	if a == b {
		return n
	}
	if k%2 == 1 {
		x := int(math.Pow(10, float64(k)))
		y := 0
		for i := 0; i < k>>1; i++ {
			y = y*10 + 1
		}
		return x + y
	}
	return closestFair(n + 1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
