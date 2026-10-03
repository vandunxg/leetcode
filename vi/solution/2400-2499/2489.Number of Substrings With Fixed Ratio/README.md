---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - Math
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [2489. Number of Substrings With Fixed Ratio 🔒](https://leetcode.com/problems/number-of-substrings-with-fixed-ratio)

[中文文档](/solution/2400-2499/2489.Number%20of%20Substrings%20With%20Fixed%20Ratio/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code> và hai số nguyên <code>num1</code> và <code>num2</code>. <code>num1</code> và <code>num2</code> là hai số nguyên tố cùng nhau.</p>

<p>Một <strong>chuỗi con theo tỷ lệ</strong> là một chuỗi con của s trong đó tỷ lệ giữa số lượng <code>0</code> và số lượng <code>1</code> trong chuỗi con chính xác bằng <code>num1 : num2</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>num1 = 2</code> và <code>num2 = 3</code> thì <code>&quot;01011&quot;</code> và <code>&quot;1110000111&quot;</code> là các chuỗi con theo tỷ lệ, còn <code>&quot;11000&quot;</code> thì không.</li>
</ul>

<p>Trả về <em>số lượng chuỗi con theo tỷ lệ <strong>khác rỗng</strong> của </em><code>s</code>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li>Một <strong>chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</li>
	<li>Hai giá trị <code>x</code> và <code>y</code> là <strong>nguyên tố cùng nhau</strong> nếu <code>gcd(x, y) == 1</code>, trong đó <code>gcd(x, y)</code> là ước chung lớn nhất của <code>x</code> và <code>y</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0110011&quot;, num1 = 1, num2 = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 chuỗi con theo tỷ lệ khác rỗng.
- Chuỗi con s[0..2]: &quot;<u>011</u>0011&quot;. Chuỗi con này chứa một 0 và hai 1. Tỷ lệ là 1 : 2.
- Chuỗi con s[1..4]: &quot;0<u>110</u>011&quot;. Chuỗi con này chứa một 0 và hai 1. Tỷ lệ là 1 : 2.
- Chuỗi con s[4..6]: &quot;0110<u>011</u>&quot;. Chuỗi con này chứa một 0 và hai 1. Tỷ lệ là 1 : 2.
- Chuỗi con s[1..6]: &quot;0<u>110011</u>&quot;. Chuỗi con này chứa hai 0 và bốn 1. Tỷ lệ là 2 : 4 == 1 : 2.
Có thể chứng minh rằng không còn chuỗi con theo tỷ lệ nào khác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10101&quot;, num1 = 3, num2 = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có chuỗi con theo tỷ lệ nào của s. Kết quả trả về là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= num1, num2 &lt;= s.length</code></li>
	<li><code>num1</code> và <code>num2</code> là các số nguyên tố cùng nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con có tỷ lệ $0$/$1$ là $num1:num2$ khi và chỉ khi giá trị $n_1\cdot num1-n_0\cdot num2$ ở hai đầu bằng nhau. Hai prefix có cùng giá trị như vậy tạo thành một cặp.
>
> Đếm các giá trị prefix đó, bắt đầu với $(0,1)$ cho prefix rỗng. Sau đó cộng rồi tăng giá trị tại mỗi chỉ số.

<!-- thinking:end -->

Ta dùng $one[i]$ để biểu diễn số lượng $1$ trong chuỗi con $s[0,..i]$, và $zero[i]$ để biểu diễn số lượng $0$ trong chuỗi con $s[0,..i]$. Một chuỗi con thỏa mãn điều kiện khi

$$
\frac{zero[j] - zero[i]}{one[j] - one[i]} = \frac{num1}{num2}
$$

với $i < j$. Ta có thể biến đổi phương trình trên thành

$$
one[j] \times num1 - zero[j] \times num2 = one[i] \times num1 - zero[i] \times num2
$$

Khi duyệt đến chỉ số $j$, ta chỉ cần đếm có bao nhiêu chỉ số $i$ thỏa mãn phương trình trên. Vì vậy, ta có thể dùng một hash table để ghi lại số lần xuất hiện của $one[i] \times num1 - zero[i] \times num2$; khi duyệt đến chỉ số $j$, ta chỉ cần đếm số lần xuất hiện của $one[j] \times num1 - zero[j] \times num2$.

Ban đầu, hash table chỉ có một cặp key-value $(0, 1)$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fixedRatio(self, s: str, num1: int, num2: int) -> int:
        n0 = n1 = 0
        ans = 0
        cnt = Counter({0: 1})
        for c in s:
            n0 += c == '0'
            n1 += c == '1'
            x = n1 * num1 - n0 * num2
            ans += cnt[x]
            cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public long fixedRatio(String s, int num1, int num2) {
        long n0 = 0, n1 = 0;
        long ans = 0;
        Map<Long, Long> cnt = new HashMap<>();
        cnt.put(0L, 1L);
        for (char c : s.toCharArray()) {
            n0 += c == '0' ? 1 : 0;
            n1 += c == '1' ? 1 : 0;
            long x = n1 * num1 - n0 * num2;
            ans += cnt.getOrDefault(x, 0L);
            cnt.put(x, cnt.getOrDefault(x, 0L) + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    long long fixedRatio(string s, int num1, int num2) {
        ll n0 = 0, n1 = 0;
        ll ans = 0;
        unordered_map<ll, ll> cnt;
        cnt[0] = 1;
        for (char& c : s) {
            n0 += c == '0';
            n1 += c == '1';
            ll x = n1 * num1 - n0 * num2;
            ans += cnt[x];
            ++cnt[x];
        }
        return ans;
    }
};
```

#### Go

```go
func fixedRatio(s string, num1 int, num2 int) int64 {
	n0, n1 := 0, 0
	ans := 0
	cnt := map[int]int{0: 1}
	for _, c := range s {
		if c == '0' {
			n0++
		} else {
			n1++
		}
		x := n1*num1 - n0*num2
		ans += cnt[x]
		cnt[x]++
	}
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
