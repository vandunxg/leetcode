---
comments: true
difficulty: Easy
rating: 1219
source: Weekly Contest 158 Q1
tags:
    - Greedy
    - String
    - Counting
---

<!-- problem:start -->

# [1221. Split a String in Balanced Strings](https://leetcode.com/problems/split-a-string-in-balanced-strings)

[中文文档](/solution/1200-1299/1221.Split%20a%20String%20in%20Balanced%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Chuỗi <strong>cân bằng</strong> là chuỗi có số ký tự <code>&#39;L&#39;</code> và <code>&#39;R&#39;</code> bằng nhau.</p>

<p>Cho chuỗi <code>s</code> <strong>cân bằng</strong>, hãy chia chuỗi thành một số chuỗi con sao cho:</p>

<ul>
	<li>Mỗi chuỗi con đều cân bằng.</li>
</ul>

<p>Hãy trả về <em>số lượng chuỗi cân bằng <strong>lớn nhất</strong> có thể tạo được.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;RLRRLLRLRL&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có thể chia s thành &quot;RL&quot;, &quot;RRLL&quot;, &quot;RL&quot;, &quot;RL&quot;, mỗi chuỗi con có số ký tự &#39;L&#39; và &#39;R&#39; bằng nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;RLRRRLLRLL&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể chia s thành &quot;RL&quot;, &quot;RRRLLRLL&quot;, mỗi chuỗi con có số ký tự &#39;L&#39; và &#39;R&#39; bằng nhau.
Lưu ý rằng không thể chia s thành &quot;RL&quot;, &quot;RR&quot;, &quot;RL&quot;, &quot;LR&quot;, &quot;LL&quot;, vì chuỗi con thứ 2 và thứ 5 không cân bằng.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;LLLLRRRR&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có thể chia s thành &quot;LLLLRRRR&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là <code>&#39;L&#39;</code> hoặc <code>&#39;R&#39;</code>.</li>
	<li><code>s</code> là chuỗi <strong>cân bằng</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Toàn bộ chuỗi cân bằng; ta muốn chia được thành nhiều đoạn cân bằng nhất có thể. Cắt ngay khi một tiền tố trở nên cân bằng sẽ không cản trở các lần cắt tiếp theo: mỗi lần cắt tăng kết quả thêm một và phần hậu tố còn lại vẫn cân bằng.
>
> Một biến đếm theo dõi độ chênh lệch giữa $L$ và $R$, và đạt 0 ở mỗi tiền tố cân bằng. Ta tăng kết quả rồi tiếp tục. Cách cắt tham lam này chỉ cần một lượt duyệt tuyến tính, đồng bộ với biến đếm.

<!-- thinking:end -->

Ta dùng biến $l$ để theo dõi độ cân bằng hiện tại của chuỗi, tức là số ký tự 'L' trừ đi số ký tự 'R' trong đoạn đang xét. Khi $l$ bằng 0, ta đã tìm được một chuỗi cân bằng.

Ta duyệt chuỗi $s$. Khi đến ký tự thứ $i$, nếu $s[i] = L$ thì tăng $l$ lên 1; ngược lại, giảm $l$ đi 1. Khi $l$ bằng 0, ta tăng kết quả thêm 1.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def balancedStringSplit(self, s: str) -> int:
        ans = l = 0
        for c in s:
            if c == 'L':
                l += 1
            else:
                l -= 1
            if l == 0:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int balancedStringSplit(String s) {
        int ans = 0, l = 0;
        for (char c : s.toCharArray()) {
            if (c == 'L') {
                ++l;
            } else {
                --l;
            }
            if (l == 0) {
                ++ans;
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
    int balancedStringSplit(string s) {
        int ans = 0, l = 0;
        for (char c : s) {
            if (c == 'L')
                ++l;
            else
                --l;
            if (l == 0) ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func balancedStringSplit(s string) int {
	ans, l := 0, 0
	for _, c := range s {
		if c == 'L' {
			l++
		} else {
			l--
		}
		if l == 0 {
			ans++
		}
	}
	return ans
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var balancedStringSplit = function (s) {
    let ans = 0;
    let l = 0;
    for (let c of s) {
        if (c == 'L') {
            ++l;
        } else {
            --l;
        }
        if (l == 0) {
            ++ans;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
