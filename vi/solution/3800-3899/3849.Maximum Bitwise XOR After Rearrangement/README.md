---
comments: true
difficulty: Medium
rating: 1556
source: Weekly Contest 490 Q3
tags:
    - Greedy
    - Bit Manipulation
    - String
---

<!-- problem:start -->

# [3849. Maximum Bitwise XOR After Rearrangement](https://leetcode.com/problems/maximum-bitwise-xor-after-rearrangement)

[中文文档](/solution/3800-3899/3849.Maximum%20Bitwise%20XOR%20After%20Rearrangement/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi nhị phân <code>s</code> và <code>t</code>​​​​​​​, mỗi chuỗi có độ dài <code>n</code>.</p>

<p>Bạn có thể <strong>sắp xếp lại</strong> các ký tự của <code>t</code> theo bất kỳ thứ tự nào, nhưng <code>s</code> <strong>phải được giữ nguyên</strong>.</p>

<p>Hãy trả về một <strong>chuỗi nhị phân</strong> có độ dài <code>n</code>, biểu thị giá trị nguyên <strong>lớn nhất</strong> có thể nhận được khi thực hiện phép <strong>XOR</strong> bitwise giữa <code>s</code> và <code>t</code> sau khi sắp xếp lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;101&quot;, t = &quot;011&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;110&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một cách sắp xếp lại tối ưu của <code>t</code> là <code>&quot;011&quot;</code>.</li>
	<li>Phép XOR bitwise giữa <code>s</code> và <code>t</code> sau khi sắp xếp lại là <code>&quot;101&quot; XOR &quot;011&quot; = &quot;110&quot;</code>, đây là giá trị lớn nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0110&quot;, t = &quot;1110&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;1101&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một cách sắp xếp lại tối ưu của <code>t</code> là <code>&quot;1011&quot;</code>.</li>
	<li>Phép XOR bitwise giữa <code>s</code> và <code>t</code> sau khi sắp xếp lại là <code>&quot;0110&quot; XOR &quot;1011&quot; = &quot;1101&quot;</code>, đây là giá trị lớn nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0101&quot;, t = &quot;1001&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;1111&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một cách sắp xếp lại tối ưu của <code>t</code> là <code>&quot;1010&quot;</code>.</li>
	<li>Phép XOR bitwise giữa <code>s</code> và <code>t</code> sau khi sắp xếp lại là <code>&quot;0101&quot; XOR &quot;1010&quot; = &quot;1111&quot;</code>, đây là giá trị lớn nhất có thể.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length == t.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>s[i]</code> và <code>t[i]</code> hoặc là <code>&#39;0&#39;</code>, hoặc là <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể sắp xếp lại $t$ để tối đa hóa số nguyên $s \oplus t$. Vì $s$ cố định, bit $1$ ở vị trí cao hơn sẽ tốt hơn.
>
> Bit $i$ trở thành $1$ nếu ta vẫn còn một ký tự của $t$ đối lập với $s[i]$. Ta nên dùng các ký tự đối lập đó cho các bit ngoài cùng bên trái.
>
> Đếm số lượng ký tự $0$ và $1$ trong $t$. Duyệt từ trái sang phải, dùng một ký tự đối lập nếu còn; nếu không, dùng một ký tự giống nhau và để bit tương ứng bằng $0$.
>
> Cách tiêu thụ tham lam sẽ ưu tiên tạo ra các bit $1$ ở vị trí cao.

<!-- thinking:end -->

Ta sử dụng một mảng $\textit{cnt}$ có độ dài $2$ để đếm số lượng ký tự '0' và ký tự '1' trong chuỗi $t$.

Sau đó, ta duyệt qua chuỗi $s$. Với mỗi ký tự $s[i]$, ta muốn tìm một ký tự trong chuỗi $t$ khác với $s[i]$ để thực hiện phép XOR, nhằm thu được kết quả lớn hơn. Nếu tìm được ký tự như vậy, ta đặt bit thứ $i$ của đáp án thành '1' và giảm số lượng của ký tự đó đi một; nếu không, ta chỉ có thể dùng một ký tự giống với $s[i]$ để thực hiện phép XOR, bit thứ $i$ của đáp án vẫn là '0', và ta giảm số lượng của ký tự đó đi một. Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumXor(self, s: str, t: str) -> str:
        cnt = [0, 0]
        for c in t:
            cnt[int(c)] += 1
        ans = ['0'] * len(s)
        for i, c in enumerate(s):
            x = int(c)
            if cnt[x ^ 1]:
                cnt[x ^ 1] -= 1
                ans[i] = '1'
            else:
                cnt[x] -= 1
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String maximumXor(String s, String t) {
        int[] cnt = new int[2];
        for (char c : t.toCharArray()) {
            cnt[c - '0']++;
        }

        char[] ans = new char[s.length()];
        for (int i = 0; i < s.length(); i++) {
            int x = s.charAt(i) - '0';
            if (cnt[x ^ 1] > 0) {
                cnt[x ^ 1]--;
                ans[i] = '1';
            } else {
                cnt[x]--;
                ans[i] = '0';
            }
        }

        return new String(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string maximumXor(string s, string t) {
        int cnt[2]{};
        for (char c : t) {
            cnt[c - '0']++;
        }

        string ans(s.size(), '0');
        for (int i = 0; i < s.size(); i++) {
            int x = s[i] - '0';
            if (cnt[x ^ 1] > 0) {
                cnt[x ^ 1]--;
                ans[i] = '1';
            } else {
                cnt[x]--;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maximumXor(s string, t string) string {
	cnt := make([]int, 2)
	for _, c := range t {
		cnt[c-'0']++
	}

	ans := make([]byte, len(s))
	for i := 0; i < len(s); i++ {
		x := s[i] - '0'
		if cnt[x^1] > 0 {
			cnt[x^1]--
			ans[i] = '1'
		} else {
			cnt[x]--
			ans[i] = '0'
		}
	}

	return string(ans)
}
```

#### TypeScript

```ts
function maximumXor(s: string, t: string): string {
    const cnt = [0, 0];

    for (const c of t) {
        cnt[Number(c)]++;
    }

    const ans: string[] = new Array(s.length).fill('0');

    for (let i = 0; i < s.length; i++) {
        const x = Number(s[i]);
        if (cnt[x ^ 1] > 0) {
            cnt[x ^ 1]--;
            ans[i] = '1';
        } else {
            cnt[x]--;
        }
    }

    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
