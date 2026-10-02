---
comments: true
difficulty: Medium
rating: 1631
source: Biweekly Contest 32 Q2
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1540. Can Convert String in K Moves](https://leetcode.com/problems/can-convert-string-in-k-moves)

[中文文档](/solution/1500-1599/1540.Can%20Convert%20String%20in%20K%20Moves/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi&nbsp;<code>s</code>&nbsp;và&nbsp;<code>t</code>, mục tiêu là chuyển&nbsp;<code>s</code>&nbsp;thành&nbsp;<code>t</code>&nbsp;trong không quá&nbsp;<code>k</code><strong>&nbsp;</strong>lần di chuyển.</p>

<p>Trong lần di chuyển thứ&nbsp;<code>i<sup>th</sup></code>&nbsp;(<font face="monospace"><code>1 &lt;= i &lt;= k</code>)&nbsp;</font>, bạn có thể:</p>

<ul>
	<li>Chọn một chỉ số&nbsp;<code>j</code>&nbsp;(đánh số từ 1) trong&nbsp;<code>s</code>, sao cho&nbsp;<code>1 &lt;= j &lt;= s.length</code>&nbsp;và <code>j</code>&nbsp;chưa được chọn ở lần di chuyển trước, rồi dịch ký tự tại chỉ số đó&nbsp;<code>i</code>&nbsp;lần.</li>
	<li>Không làm gì.</li>
</ul>

<p>Shifting a character means replacing it by the next letter in the alphabet&nbsp;(wrapping around so that&nbsp;<code>&#39;z&#39;</code>&nbsp;becomes&nbsp;<code>&#39;a&#39;</code>). Shifting a character by&nbsp;<code>i</code>&nbsp;means applying the shift operations&nbsp;<code>i</code>&nbsp;times.</p>

<p>Lưu ý rằng mỗi chỉ số&nbsp;<code>j</code>&nbsp;chỉ được chọn nhiều nhất một lần.</p>

<p>Trả về&nbsp;<code>true</code>&nbsp;nếu có thể chuyển&nbsp;<code>s</code>&nbsp;thành&nbsp;<code>t</code>&nbsp;trong không quá&nbsp;<code>k</code>&nbsp;lần di chuyển, ngược lại trả về&nbsp;<code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;input&quot;, t = &quot;ouput&quot;, k = 9
<strong>Output:</strong> true
<b>Explanation: </b>Ở lần thứ 6, ta dịch &#39;i&#39; 6 lần để được &#39;o&#39;. Ở lần thứ 7, ta dịch &#39;n&#39; để được &#39;u&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abc&quot;, t = &quot;bcd&quot;, k = 10
<strong>Output:</strong> false
<strong>Explanation: </strong>Ta cần dịch mỗi ký tự trong s một lần để chuyển thành t. Có thể dịch &#39;a&#39; thành &#39;b&#39; ở lần thứ 1. Tuy nhiên, không thể dịch các ký tự còn lại trong những lần còn lại để biến s thành t.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aab&quot;, t = &quot;bbb&quot;, k = 27
<strong>Output:</strong> true
<b>Explanation: </b>Ở lần thứ 1, ta dịch &#39;a&#39; đầu tiên 1 lần để được &#39;b&#39;. Ở lần thứ 27, ta dịch &#39;a&#39; thứ hai 27 lần để được &#39;b&#39;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= k &lt;= 10^9</code></li>
	<li><code>s</code>, <code>t</code> contain&nbsp;only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Để chuyển $s$ thành $t$, lần di chuyển thứ $i$ có thể dịch một chữ cái $i$ vị trí và mỗi $i$ chỉ dùng được một lần. Vì $n\le 10^5$ và $k\le 10^9$, ta không thể mô phỏng từng lần di chuyển.
>
> Hai chuỗi khác độ dài thì thất bại ngay. Mỗi vị trí có độ dịch nhỏ nhất $x\in[1,25]$; các vị trí có cùng $x$ lần lượt phải dùng $x,x+26,x+52,\ldots$. Lần xuất hiện cuối cần $x+26(cnt[x]-1)$ và giá trị này không được vượt quá $k$. Dịch 0 vị trí không tốn lượt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canConvertString(self, s: str, t: str, k: int) -> bool:
        if len(s) != len(t):
            return False
        cnt = [0] * 26
        for a, b in zip(s, t):
            x = (ord(b) - ord(a) + 26) % 26
            cnt[x] += 1
        for i in range(1, 26):
            if i + 26 * (cnt[i] - 1) > k:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean canConvertString(String s, String t, int k) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            int x = (t.charAt(i) - s.charAt(i) + 26) % 26;
            ++cnt[x];
        }
        for (int i = 1; i < 26; ++i) {
            if (i + 26 * (cnt[i] - 1) > k) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canConvertString(string s, string t, int k) {
        if (s.size() != t.size()) {
            return false;
        }
        int cnt[26]{};
        for (int i = 0; i < s.size(); ++i) {
            int x = (t[i] - s[i] + 26) % 26;
            ++cnt[x];
        }
        for (int i = 1; i < 26; ++i) {
            if (i + 26 * (cnt[i] - 1) > k) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canConvertString(s string, t string, k int) bool {
	if len(s) != len(t) {
		return false
	}
	cnt := [26]int{}
	for i := range s {
		x := (t[i] - s[i] + 26) % 26
		cnt[x]++
	}
	for i := 1; i < 26; i++ {
		if i+26*(cnt[i]-1) > k {
			return false
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
