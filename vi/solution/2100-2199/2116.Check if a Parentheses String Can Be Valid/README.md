---
comments: true
difficulty: Medium
rating: 2037
source: Biweekly Contest 68 Q3
tags:
    - Stack
    - Greedy
    - String
    - Parentheses
---

<!-- problem:start -->

# [2116. Check if a Parentheses String Can Be Valid](https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid)

[Tài liệu tiếng Trung](/solution/2100-2199/2116.Check%20if%20a%20Parentheses%20String%20Can%20Be%20Valid/README.md)

## Mô tả

<!-- description:start -->

<p>Chuỗi ngoặc là một chuỗi <strong>không rỗng</strong> chỉ gồm <code>&#39;(&#39;</code> và <code>&#39;)&#39;</code>. Chuỗi này hợp lệ nếu <strong>bất kỳ</strong> điều kiện nào sau đây <strong>đúng</strong>:</p>

<ul>
	<li>Chuỗi là <code>()</code>.</li>
	<li>Chuỗi có thể được viết dưới dạng <code>AB</code> (nối <code>A</code> với <code>B</code>), trong đó <code>A</code> và <code>B</code> đều là các chuỗi ngoặc hợp lệ.</li>
	<li>Chuỗi có thể được viết dưới dạng <code>(A)</code>, trong đó <code>A</code> là một chuỗi ngoặc hợp lệ.</li>
</ul>

<p>Cho một chuỗi ngoặc <code>s</code> và một chuỗi <code>locked</code>, cả hai đều có độ dài <code>n</code>. <code>locked</code> là một chuỗi nhị phân chỉ gồm <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>. Với <strong>mỗi</strong> chỉ số <code>i</code> của <code>locked</code>,</p>

<ul>
	<li>Nếu <code>locked[i]</code> là <code>&#39;1&#39;</code>, bạn <strong>không thể</strong> thay đổi <code>s[i]</code>.</li>
	<li>Tuy nhiên, nếu <code>locked[i]</code> là <code>&#39;0&#39;</code>, bạn <strong>có thể</strong> đổi <code>s[i]</code> thành <code>&#39;(&#39;</code> hoặc <code>&#39;)&#39;</code>.</li>
</ul>

<p>Trả về <code>true</code> <em>nếu có thể biến <code>s</code> thành một chuỗi ngoặc hợp lệ</em>. Ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2116.Check%20if%20a%20Parentheses%20String%20Can%20Be%20Valid/images/eg1.png" style="width: 311px; height: 101px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;))()))&quot;, locked = &quot;010100&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> locked[1] == &#39;1&#39; và locked[3] == &#39;1&#39;, nên ta không thể thay đổi s[1] hoặc s[3].
Ta đổi s[0] và s[4] thành &#39;(&#39;, đồng thời giữ nguyên s[2] và s[5], để s trở thành chuỗi hợp lệ.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;()()&quot;, locked = &quot;0000&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta không cần thay đổi gì vì s vốn đã hợp lệ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;)&quot;, locked = &quot;0&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> locked cho phép ta thay đổi s[0].
Dù đổi s[0] thành &#39;(&#39; hay &#39;)&#39; thì cũng không thể làm s trở thành chuỗi hợp lệ.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(((())(((())&quot;, locked = &quot;111111010111&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> locked cho phép ta thay đổi s[6] và s[8].
Ta đổi s[6] và s[8] thành &#39;)&#39; để s trở thành chuỗi hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s.length == locked.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> chỉ có thể là <code>&#39;(&#39;</code> hoặc <code>&#39;)&#39;</code>.</li>
	<li><code>locked[i]</code> chỉ có thể là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Hai lượt duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Các dấu ngoặc chưa khóa có thể được đổi theo cả hai hướng, còn các dấu ngoặc đã khóa thì không thể thay đổi. Chuỗi có độ dài lẻ không thể ghép cặp, còn việc liệt kê mọi cách gán cho các vị trí chưa khóa có độ phức tạp theo cấp số mũ.
>
> Trong mọi prefix, số vị trí có thể đóng vai trò là `'('` phải ít nhất bằng số dấu `')'` đã khóa; nhìn từ suffix cũng tương tự. Dấu `'('` đã khóa phải được ghép với một dấu ở bên phải, còn dấu `')'` đã khóa phải được ghép với một dấu ở bên trái.
>
> Ta loại ngay trường hợp độ dài lẻ, sau đó duyệt hai lần: lượt từ trái sang phải đếm `'('` hoặc các vị trí chưa khóa và dùng chúng để ghép với `')'` đã khóa; lượt từ phải sang trái thì đổi vai trò hai bên. Nếu số dư âm ở bất kỳ lượt nào, chuỗi không hợp lệ.

<!-- thinking:end -->

Ta nhận thấy một chuỗi có độ dài lẻ không thể là chuỗi ngoặc hợp lệ vì luôn còn lại một dấu ngoặc không được ghép. Do đó, nếu độ dài của chuỗi $s$ là số lẻ, ta trả về $\textit{false}$ ngay lập tức.

Tiếp theo, ta thực hiện hai lượt duyệt.

Lượt đầu tiên duyệt từ trái sang phải, kiểm tra xem mọi dấu ngoặc `'('` có thể được ghép với `')'` hoặc các dấu ngoặc có thể thay đổi hay không. Nếu không thể, ta trả về $\textit{false}$.

Lượt thứ hai duyệt từ phải sang trái, kiểm tra xem mọi dấu ngoặc `')'` có thể được ghép với `'('` hoặc các dấu ngoặc có thể thay đổi hay không. Nếu không thể, ta trả về $\textit{false}$.

Nếu cả hai lượt đều hoàn thành thành công, điều đó có nghĩa là mọi dấu ngoặc đều có thể được ghép, và chuỗi $s$ là một chuỗi ngoặc hợp lệ. Ta trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

Các bài toán tương tự:

- [678. Chuỗi ngoặc hợp lệ](https://github.com/doocs/leetcode/blob/main/solution/0600-0699/0678.Valid%20Parenthesis%20String/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canBeValid(self, s: str, locked: str) -> bool:
        n = len(s)
        if n & 1:
            return False
        x = 0
        for i in range(n):
            if s[i] == '(' or locked[i] == '0':
                x += 1
            elif x:
                x -= 1
            else:
                return False
        x = 0
        for i in range(n - 1, -1, -1):
            if s[i] == ')' or locked[i] == '0':
                x += 1
            elif x:
                x -= 1
            else:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean canBeValid(String s, String locked) {
        int n = s.length();
        if (n % 2 == 1) {
            return false;
        }
        int x = 0;
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '(' || locked.charAt(i) == '0') {
                ++x;
            } else if (x > 0) {
                --x;
            } else {
                return false;
            }
        }
        x = 0;
        for (int i = n - 1; i >= 0; --i) {
            if (s.charAt(i) == ')' || locked.charAt(i) == '0') {
                ++x;
            } else if (x > 0) {
                --x;
            } else {
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
    bool canBeValid(string s, string locked) {
        int n = s.size();
        if (n & 1) {
            return false;
        }
        int x = 0;
        for (int i = 0; i < n; ++i) {
            if (s[i] == '(' || locked[i] == '0') {
                ++x;
            } else if (x) {
                --x;
            } else {
                return false;
            }
        }
        x = 0;
        for (int i = n - 1; i >= 0; --i) {
            if (s[i] == ')' || locked[i] == '0') {
                ++x;
            } else if (x) {
                --x;
            } else {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canBeValid(s string, locked string) bool {
	n := len(s)
	if n%2 == 1 {
		return false
	}
	x := 0
	for i := range s {
		if s[i] == '(' || locked[i] == '0' {
			x++
		} else if x > 0 {
			x--
		} else {
			return false
		}
	}
	x = 0
	for i := n - 1; i >= 0; i-- {
		if s[i] == ')' || locked[i] == '0' {
			x++
		} else if x > 0 {
			x--
		} else {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function canBeValid(s: string, locked: string): boolean {
    const n = s.length;
    if (n & 1) {
        return false;
    }
    let x = 0;
    for (let i = 0; i < n; ++i) {
        if (s[i] === '(' || locked[i] === '0') {
            ++x;
        } else if (x > 0) {
            --x;
        } else {
            return false;
        }
    }
    x = 0;
    for (let i = n - 1; i >= 0; --i) {
        if (s[i] === ')' || locked[i] === '0') {
            ++x;
        } else if (x > 0) {
            --x;
        } else {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
