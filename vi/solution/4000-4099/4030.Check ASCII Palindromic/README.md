---
comments: true
difficulty: Easy
rating: 1165
source: Weekly Contest 516 Q1
---

<!-- problem:start -->

# [4030. Check ASCII Palindromic](https://leetcode.com/problems/check-ascii-palindromic)

[中文文档](/solution/4000-4099/4030.Check%20ASCII%20Palindromic/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Hãy tạo một <span data-keyword="binary-string"><strong>chuỗi nhị phân</strong></span> bằng cách thay mỗi ký tự trong <code>s</code> bằng biểu diễn nhị phân 8 bit của giá trị ASCII tương ứng, <strong>bao gồm cả các số 0 ở đầu</strong>, đồng thời giữ nguyên thứ tự ban đầu của các ký tự.</p>

<p>Trả về <code>true</code> nếu chuỗi nhị phân thu được là một <span data-keyword="palindrome-string"><strong>palindrome</strong></span>. Ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;ff&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Giá trị ASCII của <code>f</code> là 102, có biểu diễn nhị phân 8 bit là <code>01100110</code>.</li>
	<li>Do đó, chuỗi nhị phân là <code>0110011001100110</code>.</li>
	<li>Vì chuỗi nhị phân này là một <strong>palindrome</strong>, kết quả trả về là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leet&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Giá trị ASCII của <code>l</code>, <code>e</code>, <code>e</code> và <code>t</code> lần lượt là 108, 101, 101 và 116.</li>
	<li>Biểu diễn nhị phân 8 bit của chúng lần lượt là <code>01101100</code>, <code>01100101</code>, <code>01100101</code> và <code>01110100</code>.</li>
	<li>Do đó, chuỗi nhị phân là <code>01101100011001010110010101110100</code>.</li>
	<li>Vì chuỗi nhị phân này không phải là một <strong>palindrome</strong>, kết quả trả về là <code>false</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ký tự được mở rộng thành một chuỗi cố định gồm $8$ bit, bao gồm cả các số 0 ở đầu, rồi ta kiểm tra xem phép nối đó có phải là palindrome hay không. Vì $n$ nhỏ nên không cần viết lại phép kiểm tra theo từng cặp ký tự.
>
> Tạo $t$ theo đúng thứ tự rồi so sánh nó với chuỗi đảo ngược của nó.

<!-- thinking:end -->

Theo đề bài, ta thay mỗi ký tự của $s$ bằng biểu diễn nhị phân $8$ bit của giá trị ASCII tương ứng (bao gồm cả các số 0 ở đầu), nối chúng theo thứ tự để thu được chuỗi nhị phân $t$, sau đó kiểm tra xem $t$ có phải là palindrome hay không.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPalindromic(self, s: str) -> bool:
        t = ''.join(format(ord(c), '08b') for c in s)
        return t == t[::-1]
```

#### Java

```java
class Solution {
    public boolean isPalindromic(String s) {
        StringBuilder t = new StringBuilder();
        for (char c : s.toCharArray()) {
            String b = Integer.toBinaryString(c);
            t.append("0".repeat(8 - b.length())).append(b);
        }
        return t.toString().equals(t.reverse().toString());
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPalindromic(string s) {
        string t;
        for (unsigned char c : s) {
            for (int i = 7; i >= 0; --i) {
                t += char('0' + ((c >> i) & 1));
            }
        }
        return ranges::equal(t, t | views::reverse);
    }
};
```

#### Go

```go
func isPalindromic(s string) bool {
	var t []byte
	for _, c := range []byte(s) {
		for i := 7; i >= 0; i-- {
			t = append(t, '0'+((c>>i)&1))
		}
	}
	for i := range t[:len(t)/2] {
		if t[i] != t[len(t)-1-i] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isPalindromic(s: string): boolean {
    const t = [...s].map(c => c.charCodeAt(0).toString(2).padStart(8, '0')).join('');
    return t === [...t].reverse().join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
