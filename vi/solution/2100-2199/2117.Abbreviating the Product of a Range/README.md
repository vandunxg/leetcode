---
comments: true
difficulty: Hard
rating: 2476
source: Biweekly Contest 68 Q4
tags:
    - Math
    - Number Theory
---

<!-- problem:start -->

# [2117. Abbreviating the Product of a Range](https://leetcode.com/problems/abbreviating-the-product-of-a-range)

[中文文档](/solution/2100-2199/2117.Abbreviating%20the%20Product%20of%20a%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>left</code> và <code>right</code> với <code>left &lt;= right</code>. Hãy tính <strong>tích</strong> của tất cả các số nguyên trong đoạn <strong>bao gồm cả hai đầu</strong> <code>[left, right]</code>.</p>

<p>Vì tích có thể rất lớn, bạn sẽ <strong>rút gọn</strong> nó theo các bước sau:</p>

<ol>
	<li>Đếm tất cả các chữ số 0 ở <strong>cuối</strong> của tích và <strong>loại bỏ</strong> chúng. Gọi số lượng này là <code>C</code>.

    <ul>
      <li>Ví dụ, <code>1000</code> có <code>3</code> chữ số 0 ở cuối, còn <code>546</code> có <code>0</code> chữ số 0 ở cuối.</li>
    </ul>
    </li>
    <li>Gọi số chữ số còn lại trong tích là <code>d</code>. Nếu <code>d &gt; 10</code>, biểu diễn tích dưới dạng <code>&lt;pre&gt;...&lt;suf&gt;</code>, trong đó <code>&lt;pre&gt;</code> là <strong>đầu tiên</strong> <code>5</code> chữ số của tích, còn <code>&lt;suf&gt;</code> là <strong>cuối cùng</strong> <code>5</code> chữ số của tích <strong>sau khi</strong> loại bỏ tất cả các chữ số 0 ở cuối. Nếu <code>d &lt;= 10</code>, giữ nguyên tích.
    <ul>
      <li>Ví dụ, ta biểu diễn <code>1234567654321</code> thành <code>12345...54321</code>, còn <code>1234567</code> được biểu diễn là <code>1234567</code>.</li>
    </ul>
    </li>
    <li>Cuối cùng, biểu diễn tích dưới dạng một <strong>chuỗi</strong> <code>&quot;&lt;pre&gt;...&lt;suf&gt;eC&quot;</code>.
    <ul>
      <li>Ví dụ, <code>12345678987600000</code> sẽ được biểu diễn thành <code>&quot;12345...89876e5&quot;</code>.</li>
    </ul>
    </li>

</ol>

<p><em>Trả về một chuỗi biểu diễn <strong>tích đã rút gọn</strong> của tất cả các số nguyên trong đoạn <strong>bao gồm cả hai đầu</strong></em> <code>[left, right]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = 1, right = 4
<strong>Đầu ra:</strong> &quot;24e0&quot;
<strong>Giải thích:</strong> Tích là 1 &times; 2 &times; 3 &times; 4 = 24.
Không có chữ số 0 ở cuối, nên 24 được giữ nguyên. Phần rút gọn sẽ kết thúc bằng &quot;e0&quot;.
Vì số chữ số là 2, nhỏ hơn 10, nên ta không cần rút gọn thêm.
Do đó, biểu diễn cuối cùng là &quot;24e0&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = 2, right = 11
<strong>Đầu ra:</strong> &quot;399168e2&quot;
<strong>Giải thích:</strong> Tích là 39916800.
Có 2 chữ số 0 ở cuối, ta loại bỏ chúng để được 399168. Phần rút gọn sẽ kết thúc bằng &quot;e2&quot;.
Số chữ số sau khi loại bỏ các chữ số 0 ở cuối là 6, nên ta không cần rút gọn thêm.
Vì vậy, tích đã rút gọn là &quot;399168e2&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = 371, right = 375
<strong>Đầu ra:</strong> &quot;7219856259e3&quot;
<strong>Giải thích:</strong> Tích là 7219856259000.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= left &lt;= right &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tích của đoạn có thể có hàng chục nghìn chữ số, vì vậy không cần nhân bằng số nguyên lớn đầy đủ nếu ta chỉ cần các chữ số đầu, các chữ số cuối và số mũ của các chữ số 0 ở cuối.
>
> Số chữ số 0 ở cuối là $\min$ của số lượng thừa số $2$ và $5$. Sau khi loại bỏ các thừa số đó, ta có thể giữ phần hậu tố theo modulo $10^{10}$, còn phần tiền tố được thu nhỏ (chia cho $10$ khi tăng quá lớn) để giữ lại một vài chữ số đầu.
>
> Trước tiên, ta loại bỏ $\min(\textit{cnt}_2,\textit{cnt}_5)$ thừa số, sau đó duyệt qua đoạn và cập nhật $\textit{pre}$ cùng $\textit{suf}$. Nếu hậu tố từng vượt quá $10^{10}$, ta xuất dạng rút gọn; nếu không, ta xuất tích chính xác sau khi loại bỏ các chữ số 0 ở cuối.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def abbreviateProduct(self, left: int, right: int) -> str:
        cnt2 = cnt5 = 0
        for x in range(left, right + 1):
            while x % 2 == 0:
                cnt2 += 1
                x //= 2
            while x % 5 == 0:
                cnt5 += 1
                x //= 5
        c = cnt2 = cnt5 = min(cnt2, cnt5)
        pre = suf = 1
        gt = False
        for x in range(left, right + 1):
            suf *= x
            while cnt2 and suf % 2 == 0:
                suf //= 2
                cnt2 -= 1
            while cnt5 and suf % 5 == 0:
                suf //= 5
                cnt5 -= 1
            if suf >= 1e10:
                gt = True
                suf %= int(1e10)
            pre *= x
            while pre > 1e5:
                pre /= 10
        if gt:
            return str(int(pre)) + "..." + str(suf % int(1e5)).zfill(5) + "e" + str(c)
        return str(suf) + "e" + str(c)
```

#### Java

```java
class Solution {

    public String abbreviateProduct(int left, int right) {
        int cnt2 = 0, cnt5 = 0;
        for (int i = left; i <= right; ++i) {
            int x = i;
            for (; x % 2 == 0; x /= 2) {
                ++cnt2;
            }
            for (; x % 5 == 0; x /= 5) {
                ++cnt5;
            }
        }
        int c = Math.min(cnt2, cnt5);
        cnt2 = cnt5 = c;
        long suf = 1;
        double pre = 1;
        boolean gt = false;
        for (int i = left; i <= right; ++i) {
            for (suf *= i; cnt2 > 0 && suf % 2 == 0; suf /= 2) {
                --cnt2;
            }
            for (; cnt5 > 0 && suf % 5 == 0; suf /= 5) {
                --cnt5;
            }
            if (suf >= (long) 1e10) {
                gt = true;
                suf %= (long) 1e10;
            }
            for (pre *= i; pre > 1e5; pre /= 10) {
            }
        }
        if (gt) {
            return (int) pre + "..." + String.format("%05d", suf % (int) 1e5) + "e" + c;
        }
        return suf + "e" + c;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string abbreviateProduct(int left, int right) {
        int cnt2 = 0, cnt5 = 0;
        for (int i = left; i <= right; ++i) {
            int x = i;
            for (; x % 2 == 0; x /= 2) {
                ++cnt2;
            }
            for (; x % 5 == 0; x /= 5) {
                ++cnt5;
            }
        }
        int c = min(cnt2, cnt5);
        cnt2 = cnt5 = c;
        long long suf = 1;
        long double pre = 1;
        bool gt = false;
        for (int i = left; i <= right; ++i) {
            for (suf *= i; cnt2 && suf % 2 == 0; suf /= 2) {
                --cnt2;
            }
            for (; cnt5 && suf % 5 == 0; suf /= 5) {
                --cnt5;
            }
            if (suf >= 1e10) {
                gt = true;
                suf %= (long long) 1e10;
            }
            for (pre *= i; pre > 1e5; pre /= 10) {
            }
        }
        if (gt) {
            char buf[10];
            snprintf(buf, sizeof(buf), "%0*lld", 5, suf % (int) 1e5);
            return to_string((int) pre) + "..." + string(buf) + "e" + to_string(c);
        }
        return to_string(suf) + "e" + to_string(c);
    }
};
```

#### Go

```go
func abbreviateProduct(left int, right int) string {
	cnt2, cnt5 := 0, 0
	for i := left; i <= right; i++ {
		x := i
		for x%2 == 0 {
			cnt2++
			x /= 2
		}
		for x%5 == 0 {
			cnt5++
			x /= 5
		}
	}
	c := int(math.Min(float64(cnt2), float64(cnt5)))
	cnt2 = c
	cnt5 = c
	suf := int64(1)
	pre := float64(1)
	gt := false
	for i := left; i <= right; i++ {
		for suf *= int64(i); cnt2 > 0 && suf%2 == 0; {
			cnt2--
			suf /= int64(2)
		}
		for cnt5 > 0 && suf%5 == 0 {
			cnt5--
			suf /= int64(5)
		}
		if float64(suf) >= 1e10 {
			gt = true
			suf %= int64(1e10)
		}
		for pre *= float64(i); pre > 1e5; {
			pre /= 10
		}
	}
	if gt {
		return fmt.Sprintf("%05d...%05de%d", int(pre), int(suf)%int(1e5), c)
	}
	return fmt.Sprintf("%de%d", suf, c)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
