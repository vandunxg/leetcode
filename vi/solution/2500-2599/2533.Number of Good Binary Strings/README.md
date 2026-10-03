---
comments: true
difficulty: Medium
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [2533. Number of Good Binary Strings 🔒](https://leetcode.com/problems/number-of-good-binary-strings)

[中文文档](/solution/2500-2599/2533.Number%20of%20Good%20Binary%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho bạn bốn số nguyên <code>minLength</code>, <code>maxLength</code>, <code>oneGroup</code> và <code>zeroGroup</code>.</p>

<p>Một chuỗi nhị phân được gọi là <strong>tốt</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Độ dài của chuỗi nằm trong khoảng <code>[minLength, maxLength]</code>.</li>
	<li>Độ dài của mỗi khối liên tiếp các <code>1</code> là bội số của <code>oneGroup</code>.
	<ul>
		<li>Ví dụ, trong chuỗi nhị phân <code>00<u>11</u>0<u>1111</u>00</code>, độ dài của mỗi khối các số 1 liên tiếp là <code>[2,4]</code>.</li>
	</ul>
	</li>
	<li>Độ dài của mỗi khối liên tiếp các <code>0</code> là bội số của <code>zeroGroup</code>.
	<ul>
		<li>Ví dụ, trong chuỗi nhị phân <code><u>00</u>11<u>0</u>1111<u>00</u></code>, độ dài của mỗi khối các số 0 liên tiếp là <code>[2,1,2]</code>.</li>
	</ul>
	</li>
</ul>

<p>Trả về <em>số lượng chuỗi nhị phân <strong>tốt</strong></em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý</strong> rằng <code>0</code> được xem là bội số của mọi số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> minLength = 2, maxLength = 3, oneGroup = 1, zeroGroup = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có 5 chuỗi nhị phân tốt trong ví dụ này: &quot;00&quot;, &quot;11&quot;, &quot;001&quot;, &quot;100&quot; và &quot;111&quot;.
Có thể chứng minh rằng chỉ có 5 chuỗi tốt thỏa mãn tất cả các điều kiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> minLength = 4, maxLength = 4, oneGroup = 4, zeroGroup = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có 1 chuỗi nhị phân tốt trong ví dụ này: &quot;1111&quot;.
Có thể chứng minh rằng chỉ có 1 chuỗi tốt thỏa mãn tất cả các điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= minLength &lt;= maxLength &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= oneGroup, zeroGroup &lt;= maxLength</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi tốt là phép nối các khối gồm $\textit{oneGroup}$ số 1 và $\textit{zeroGroup}$ số 0, với tổng độ dài nằm trong $[\textit{minLength},\textit{maxLength}]$. Việc liệt kê tất cả các phép nối là quá lớn.
>
> Gọi $f[i]$ là số chuỗi tốt có độ dài $i$. Khối cuối cùng là khối số 1 hoặc khối số 0, nên $f[i]=f[i-\textit{oneGroup}]+f[i-\textit{zeroGroup}]$ khi các chỉ số đó tồn tại, với $f[0]=1$. Tính tổng $f$ từ $\textit{minLength}$ đến $\textit{maxLength}$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số chuỗi có độ dài $i$ thỏa mãn điều kiện. Công thức chuyển trạng thái là:

$$
f[i] = \begin{cases}
1 & i = 0 \\
f[i - oneGroup] + f[i - zeroGroup] & i \geq 1
\end{cases}
$$

Đáp án cuối cùng là $f[minLength] + f[minLength + 1] + \cdots + f[maxLength]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n=maxLength$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def goodBinaryStrings(
        self, minLength: int, maxLength: int, oneGroup: int, zeroGroup: int
    ) -> int:
        mod = 10**9 + 7
        f = [1] + [0] * maxLength
        for i in range(1, len(f)):
            if i - oneGroup >= 0:
                f[i] += f[i - oneGroup]
            if i - zeroGroup >= 0:
                f[i] += f[i - zeroGroup]
            f[i] %= mod
        return sum(f[minLength:]) % mod
```

#### Java

```java
class Solution {
    public int goodBinaryStrings(int minLength, int maxLength, int oneGroup, int zeroGroup) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[maxLength + 1];
        f[0] = 1;
        for (int i = 1; i <= maxLength; ++i) {
            if (i - oneGroup >= 0) {
                f[i] = (f[i] + f[i - oneGroup]) % mod;
            }
            if (i - zeroGroup >= 0) {
                f[i] = (f[i] + f[i - zeroGroup]) % mod;
            }
        }
        int ans = 0;
        for (int i = minLength; i <= maxLength; ++i) {
            ans = (ans + f[i]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int goodBinaryStrings(int minLength, int maxLength, int oneGroup, int zeroGroup) {
        const int mod = 1e9 + 7;
        int f[maxLength + 1];
        memset(f, 0, sizeof f);
        f[0] = 1;
        for (int i = 1; i <= maxLength; ++i) {
            if (i - oneGroup >= 0) {
                f[i] = (f[i] + f[i - oneGroup]) % mod;
            }
            if (i - zeroGroup >= 0) {
                f[i] = (f[i] + f[i - zeroGroup]) % mod;
            }
        }
        int ans = 0;
        for (int i = minLength; i <= maxLength; ++i) {
            ans = (ans + f[i]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func goodBinaryStrings(minLength int, maxLength int, oneGroup int, zeroGroup int) (ans int) {
	const mod int = 1e9 + 7
	f := make([]int, maxLength+1)
	f[0] = 1
	for i := 1; i <= maxLength; i++ {
		if i-oneGroup >= 0 {
			f[i] += f[i-oneGroup]
		}
		if i-zeroGroup >= 0 {
			f[i] += f[i-zeroGroup]
		}
		f[i] %= mod
	}
	for _, v := range f[minLength:] {
		ans = (ans + v) % mod
	}
	return
}
```

#### TypeScript

```ts
function goodBinaryStrings(
    minLength: number,
    maxLength: number,
    oneGroup: number,
    zeroGroup: number,
): number {
    const mod = 10 ** 9 + 7;
    const f: number[] = Array(maxLength + 1).fill(0);
    f[0] = 1;
    for (let i = 1; i <= maxLength; ++i) {
        if (i >= oneGroup) {
            f[i] += f[i - oneGroup];
        }
        if (i >= zeroGroup) {
            f[i] += f[i - zeroGroup];
        }
        f[i] %= mod;
    }
    return f.slice(minLength).reduce((a, b) => a + b, 0) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
