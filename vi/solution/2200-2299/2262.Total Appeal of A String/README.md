---
comments: true
difficulty: Hard
rating: 2033
source: Weekly Contest 291 Q4
tags:
    - Hash Table
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2262. Total Appeal of A String](https://leetcode.com/problems/total-appeal-of-a-string)

[中文文档](/solution/2200-2299/2262.Total%20Appeal%20of%20A%20String/README.md)

## Mô tả

<!-- description:start -->

<p><b>Độ hấp dẫn</b> của một chuỗi là số lượng ký tự <strong>phân biệt</strong> xuất hiện trong chuỗi đó.</p>

<ul>
	<li>Ví dụ, độ hấp dẫn của <code>&quot;abbca&quot;</code> là <code>3</code> vì chuỗi có <code>3</code> ký tự phân biệt: <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>.</li>
</ul>

<p>Cho một chuỗi <code>s</code>, hãy trả về <em><strong>tổng độ hấp dẫn của tất cả <strong>chuỗi con</strong>.</strong></em></p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbca&quot;
<strong>Đầu ra:</strong> 28
<strong>Giải thích:</strong> Các chuỗi con của &quot;abbca&quot; là:
- Các chuỗi con có độ dài 1: &quot;a&quot;, &quot;b&quot;, &quot;b&quot;, &quot;c&quot;, &quot;a&quot; có độ hấp dẫn lần lượt là 1, 1, 1, 1 và 1. Tổng là 5.
- Các chuỗi con có độ dài 2: &quot;ab&quot;, &quot;bb&quot;, &quot;bc&quot;, &quot;ca&quot; có độ hấp dẫn lần lượt là 2, 1, 2 và 2. Tổng là 7.
- Các chuỗi con có độ dài 3: &quot;abb&quot;, &quot;bbc&quot;, &quot;bca&quot; có độ hấp dẫn lần lượt là 2, 2 và 3. Tổng là 7.
- Các chuỗi con có độ dài 4: &quot;abbc&quot;, &quot;bbca&quot; có độ hấp dẫn lần lượt là 3 và 3. Tổng là 6.
- Các chuỗi con có độ dài 5: &quot;abbca&quot; có độ hấp dẫn là 3. Tổng là 3.
Tổng cộng là 5 + 7 + 7 + 6 + 3 = 28.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;code&quot;
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Các chuỗi con của &quot;code&quot; là:
- Các chuỗi con có độ dài 1: &quot;c&quot;, &quot;o&quot;, &quot;d&quot;, &quot;e&quot; có độ hấp dẫn lần lượt là 1, 1, 1 và 1. Tổng là 4.
- Các chuỗi con có độ dài 2: &quot;co&quot;, &quot;od&quot;, &quot;de&quot; có độ hấp dẫn lần lượt là 2, 2 và 2. Tổng là 6.
- Các chuỗi con có độ dài 3: &quot;cod&quot;, &quot;ode&quot; có độ hấp dẫn lần lượt là 3 và 3. Tổng là 6.
- Các chuỗi con có độ dài 4: &quot;code&quot; có độ hấp dẫn là 4. Tổng là 4.
Tổng cộng là 4 + 6 + 6 + 4 = 20.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Độ hấp dẫn của một chuỗi con là số lượng chữ cái phân biệt trong chuỗi đó; ta cần tính tổng trên tất cả chuỗi con. Vì $n \le 10^5$, không thể liệt kê chúng. Với các chuỗi con kết thúc tại $i$, một ký tự $c$ mới sẽ làm tăng độ hấp dẫn của mọi chuỗi con có vị trí bắt đầu nằm sau lần xuất hiện trước đó của $c$.
>
> Lưu chỉ số xuất hiện gần nhất $pos[c]$ (ban đầu là $-1$). Tại $i$, cộng $i-pos[c]$ vào biến tổng $t$, sau đó cộng $t$ vào đáp án. $t$ là tổng độ hấp dẫn của các chuỗi con kết thúc tại đây.

<!-- thinking:end -->

Ta có thể duyệt qua tất cả các chuỗi con kết thúc tại từng ký tự $s[i]$ và tính tổng độ hấp dẫn của chúng là $t$. Cuối cùng, cộng tất cả các giá trị $t$ để nhận được tổng độ hấp dẫn.

Khi xét $s[i]$, được thêm vào cuối chuỗi con kết thúc tại $s[i-1]$, ta xem xét sự thay đổi của tổng độ hấp dẫn $t$:

Nếu $s[i]$ chưa từng xuất hiện, độ hấp dẫn của mọi chuỗi con kết thúc tại $s[i-1]$ sẽ tăng thêm $1$, và có tổng cộng $i$ chuỗi con như vậy. Vì thế, $t$ tăng thêm $i$, cộng với độ hấp dẫn của riêng $s[i]$ là $1$. Do đó, $t$ tăng tổng cộng $i+1$.

Nếu $s[i]$ đã xuất hiện, gọi vị trí xuất hiện gần nhất là $j$. Khi đó, ta thêm $s[i]$ vào cuối các chuỗi con $s[0..i-1]$, $[1..i-1]$, $s[2..i-1]$, $\cdots$, $s[j..i-1]$. Độ hấp dẫn của các chuỗi con này không thay đổi vì $s[i]$ đã xuất hiện trong chúng. Độ hấp dẫn của các chuỗi con $s[j+1..i-1]$, $s[j+2..i-1]$, $\cdots$, $s[i-1]$ sẽ tăng thêm $1$, và có tổng cộng $i-j-1$ chuỗi con như vậy. Vì thế, $t$ tăng thêm $i-j-1$, cộng với độ hấp dẫn của riêng $s[i]$ là $1$. Do đó, $t$ tăng tổng cộng $i-j$.

Vì vậy, ta có thể dùng một mảng $pos$ để lưu vị trí xuất hiện gần nhất của mỗi ký tự. Ban đầu, tất cả các vị trí được gán bằng $-1$.

Tiếp theo, ta duyệt qua chuỗi. Mỗi lần, ta cập nhật tổng độ hấp dẫn $t$ của chuỗi con kết thúc tại ký tự hiện tại theo công thức $t = t + i - pos[c]$, trong đó $c$ là ký tự hiện tại. Ta cộng $t$ vào đáp án, rồi cập nhật $pos[c]$ thành vị trí hiện tại $i$. Tiếp tục duyệt cho đến hết chuỗi.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi $s$ và $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def appealSum(self, s: str) -> int:
        ans = t = 0
        pos = [-1] * 26
        for i, c in enumerate(s):
            c = ord(c) - ord('a')
            t += i - pos[c]
            ans += t
            pos[c] = i
        return ans
```

#### Java

```java
class Solution {
    public long appealSum(String s) {
        long ans = 0;
        long t = 0;
        int[] pos = new int[26];
        Arrays.fill(pos, -1);
        for (int i = 0; i < s.length(); ++i) {
            int c = s.charAt(i) - 'a';
            t += i - pos[c];
            ans += t;
            pos[c] = i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long appealSum(string s) {
        long long ans = 0, t = 0;
        vector<int> pos(26, -1);
        for (int i = 0; i < s.size(); ++i) {
            int c = s[i] - 'a';
            t += i - pos[c];
            ans += t;
            pos[c] = i;
        }
        return ans;
    }
};
```

#### Go

```go
func appealSum(s string) int64 {
	var ans, t int64
	pos := make([]int, 26)
	for i := range pos {
		pos[i] = -1
	}
	for i, c := range s {
		c -= 'a'
		t += int64(i - pos[c])
		ans += t
		pos[c] = i
	}
	return ans
}
```

#### TypeScript

```ts
function appealSum(s: string): number {
    const pos: number[] = Array(26).fill(-1);
    const n = s.length;
    let ans = 0;
    let t = 0;
    for (let i = 0; i < n; ++i) {
        const c = s.charCodeAt(i) - 97;
        t += i - pos[c];
        ans += t;
        pos[c] = i;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
