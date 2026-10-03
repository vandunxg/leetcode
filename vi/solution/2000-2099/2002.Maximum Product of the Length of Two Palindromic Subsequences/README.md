---
comments: true
difficulty: Medium
rating: 1869
source: Weekly Contest 258 Q3
tags:
    - Bit Manipulation
    - String
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [2002. Maximum Product of the Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences)

[中文文档](/solution/2000-2099/2002.Maximum%20Product%20of%20the%20Length%20of%20Two%20Palindromic%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, hãy tìm hai <strong>dãy con palindrome rời nhau</strong> của <code>s</code> sao cho <strong>tích</strong> độ dài của chúng là <strong>lớn nhất</strong>. Hai dãy con <strong>rời nhau</strong> nếu chúng không cùng chọn ký tự tại một chỉ số.</p>

<p>Trả về <em><strong>tích</strong> <strong>lớn nhất</strong> có thể có của độ dài hai dãy con palindrome</em>.</p>

<p>Một <strong>dãy con</strong> là một chuỗi có thể thu được từ một chuỗi khác bằng cách xóa một số hoặc không xóa ký tự nào mà không thay đổi thứ tự của các ký tự còn lại. Một chuỗi là <strong>palindrome</strong> nếu đọc xuôi hay ngược đều giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="example-1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2002.Maximum%20Product%20of%20the%20Length%20of%20Two%20Palindromic%20Subsequences/images/two-palindromic-subsequences.png" style="width: 550px; height: 124px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;leetcodecom&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích</strong>: Một lời giải tối ưu là chọn &quot;ete&quot; làm dãy con thứ <sup>1</sup> và &quot;cdc&quot; làm dãy con thứ <sup>2</sup>.
Tích độ dài của chúng là: 3 * 3 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bb&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích</strong>: Một lời giải tối ưu là chọn &quot;b&quot; (ký tự thứ nhất) làm dãy con thứ <sup>1</sup> và &quot;b&quot; (ký tự thứ hai) làm dãy con thứ <sup>2</sup>.
Tích độ dài của chúng là: 1 * 1 = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;accbcaxxcxx&quot;
<strong>Đầu ra:</strong> 25
<strong>Giải thích</strong>: Một lời giải tối ưu là chọn &quot;accca&quot; làm dãy con thứ <sup>1</sup> và &quot;xxcxx&quot; làm dãy con thứ <sup>2</sup>.
Tích độ dài của chúng là: 5 * 5 = 25.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 12</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 12$, chỉ có $2^n$ dãy con nên có thể liệt kê hết. Nếu yêu cầu các tập chỉ số rời nhau, cách duyệt ngây thơ sẽ tốn khoảng $4^n$.
>
> Với mỗi mask, dùng hai con trỏ để bỏ qua các bit không được chọn và kiểm tra xem dãy con có phải palindrome hay không, rồi lưu kết quả vào $p$.
>
> Nếu mask $i$ là palindrome, ta liệt kê các submask $j$ của phần bù của nó và lấy tích số bit 1. Tổng số submask được liệt kê là $3^n$, kết hợp với bước tiền xử lý $2^n n$ vẫn nằm trong giới hạn cho phép.

<!-- thinking:end -->

Ta nhận thấy độ dài của chuỗi $s$ không vượt quá $12$, vì vậy có thể dùng phương pháp liệt kê nhị phân để liệt kê tất cả dãy con của $s$. Giả sử độ dài của $s$ là $n$, ta có thể dùng $2^n$ số nhị phân có độ dài $n$ để biểu diễn tất cả dãy con của $s$. Với mỗi số nhị phân, bit thứ $i$ bằng $1$ nghĩa là ký tự thứ $i$ của $s$ nằm trong dãy con, còn bằng $0$ nghĩa là không nằm trong dãy con. Với mỗi số nhị phân, ta kiểm tra xem nó có biểu diễn một dãy con palindrome hay không và ghi kết quả vào mảng $p$.

Tiếp theo, ta duyệt từng số $i$ trong $p$. Nếu $i$ biểu diễn một dãy con palindrome, ta có thể duyệt một số $j$ từ phần bù của $i$, $mx = (2^n - 1) \oplus i$. Nếu $j$ cũng biểu diễn một dãy con palindrome, thì $i$ và $j$ là hai dãy con palindrome cần tìm. Độ dài của chúng lần lượt là số lượng bit $1$ trong biểu diễn nhị phân của $i$ và $j$, lần lượt ký hiệu là $a$ và $b$. Khi đó, tích độ dài là $a \times b$. Ta lấy giá trị lớn nhất trong tất cả các tích $a \times b$ có thể có.

Độ phức tạp thời gian là $(2^n \times n + 3^n)$, và độ phức tạp không gian là $O(2^n)$. Ở đây, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, s: str) -> int:
        n = len(s)
        p = [True] * (1 << n)
        for k in range(1, 1 << n):
            i, j = 0, n - 1
            while i < j:
                while i < j and (k >> i & 1) == 0:
                    i += 1
                while i < j and (k >> j & 1) == 0:
                    j -= 1
                if i < j and s[i] != s[j]:
                    p[k] = False
                    break
                i, j = i + 1, j - 1
        ans = 0
        for i in range(1, 1 << n):
            if p[i]:
                mx = ((1 << n) - 1) ^ i
                j = mx
                a = i.bit_count()
                while j:
                    if p[j]:
                        b = j.bit_count()
                        ans = max(ans, a * b)
                    j = (j - 1) & mx
        return ans
```

#### Java

```java
class Solution {
    public int maxProduct(String s) {
        int n = s.length();
        boolean[] p = new boolean[1 << n];
        Arrays.fill(p, true);
        for (int k = 1; k < 1 << n; ++k) {
            for (int i = 0, j = n - 1; i < n; ++i, --j) {
                while (i < j && (k >> i & 1) == 0) {
                    ++i;
                }
                while (i < j && (k >> j & 1) == 0) {
                    --j;
                }
                if (i < j && s.charAt(i) != s.charAt(j)) {
                    p[k] = false;
                    break;
                }
            }
        }
        int ans = 0;
        for (int i = 1; i < 1 << n; ++i) {
            if (p[i]) {
                int a = Integer.bitCount(i);
                int mx = ((1 << n) - 1) ^ i;
                for (int j = mx; j > 0; j = (j - 1) & mx) {
                    if (p[j]) {
                        int b = Integer.bitCount(j);
                        ans = Math.max(ans, a * b);
                    }
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
    int maxProduct(string s) {
        int n = s.size();
        vector<bool> p(1 << n, true);
        for (int k = 1; k < 1 << n; ++k) {
            for (int i = 0, j = n - 1; i < j; ++i, --j) {
                while (i < j && !(k >> i & 1)) {
                    ++i;
                }
                while (i < j && !(k >> j & 1)) {
                    --j;
                }
                if (i < j && s[i] != s[j]) {
                    p[k] = false;
                    break;
                }
            }
        }
        int ans = 0;
        for (int i = 1; i < 1 << n; ++i) {
            if (p[i]) {
                int a = __builtin_popcount(i);
                int mx = ((1 << n) - 1) ^ i;
                for (int j = mx; j; j = (j - 1) & mx) {
                    if (p[j]) {
                        int b = __builtin_popcount(j);
                        ans = max(ans, a * b);
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxProduct(s string) (ans int) {
	n := len(s)
	p := make([]bool, 1<<n)
	for i := range p {
		p[i] = true
	}
	for k := 1; k < 1<<n; k++ {
		for i, j := 0, n-1; i < j; i, j = i+1, j-1 {
			for i < j && (k>>i&1) == 0 {
				i++
			}
			for i < j && (k>>j&1) == 0 {
				j--
			}
			if i < j && s[i] != s[j] {
				p[k] = false
				break
			}
		}
	}
	for i := 1; i < 1<<n; i++ {
		if p[i] {
			a := bits.OnesCount(uint(i))
			mx := (1<<n - 1) ^ i
			for j := mx; j > 0; j = (j - 1) & mx {
				if p[j] {
					b := bits.OnesCount(uint(j))
					ans = max(ans, a*b)
				}
			}
		}
	}
	return

}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
