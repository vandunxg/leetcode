---
comments: true
difficulty: Easy
rating: 1344
source: Weekly Contest 201 Q1
tags:
    - Stack
    - String
---

<!-- problem:start -->

# [1544. Make The String Great](https://leetcode.com/problems/make-the-string-great)

[中文文档](/solution/1500-1599/1544.Make%20The%20String%20Great/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và viết hoa.</p>

<p>Chuỗi tốt là chuỗi không có <strong>hai ký tự liền kề</strong> <code>s[i]</code> và <code>s[i + 1]</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt;= s.length - 2</code></li>
	<li><code>s[i]</code> là chữ thường và <code>s[i + 1]</code> là cùng chữ đó nhưng viết hoa hoặc <strong>ngược lại</strong>.</li>
</ul>

<p>Để làm chuỗi tốt, bạn có thể chọn <strong>hai ký tự liền kề</strong> khiến chuỗi không tốt và xóa chúng. Có thể lặp lại cho đến khi chuỗi trở thành chuỗi tốt.</p>

<p>Trả về <em>chuỗi</em> sau khi làm nó tốt. Đáp án được đảm bảo là duy nhất với các ràng buộc đã cho.</p>

<p><strong>Lưu ý</strong> rằng chuỗi rỗng cũng là chuỗi tốt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;leEeetcode&quot;
<strong>Output:</strong> &quot;leetcode&quot;
<strong>Explanation:</strong> In the first step, either you choose i = 1 or i = 2, both will result &quot;leEeetcode&quot; to be reduced to &quot;leetcode&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abBAcC&quot;
<strong>Output:</strong> &quot;&quot;
<strong>Explanation:</strong> Có nhiều cách thực hiện, nhưng tất cả đều dẫn đến cùng một đáp án. Ví dụ:
&quot;abBAcC&quot; --&gt; &quot;aAcC&quot; --&gt; &quot;cC&quot; --&gt; &quot;&quot;
&quot;abBAcC&quot; --&gt; &quot;abBA&quot; --&gt; &quot;aA&quot; --&gt; &quot;&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;s&quot;
<strong>Output:</strong> &quot;s&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> contains only lower and upper case English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi “tốt” không có hai chữ liền kề chỉ khác nhau về hoa thường. Xóa lặp từ trái sang phải vẫn chạy với $n\le 100$, nhưng một lần xóa có thể tạo cặp mới ở bên trái, buộc phải quét thêm.
>
> Stack lưu prefix đã làm sạch. Nếu ký tự mới khác phần tử trên cùng đúng $32$ (chính là một cặp hoa thường), pop; nếu không thì push. Một lượt duyệt tuyến tính sẽ tạo ra đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeGood(self, s: str) -> str:
        stk = []
        for c in s:
            if not stk or abs(ord(stk[-1]) - ord(c)) != 32:
                stk.append(c)
            else:
                stk.pop()
        return "".join(stk)
```

#### Java

```java
class Solution {
    public String makeGood(String s) {
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (sb.length() == 0 || Math.abs(sb.charAt(sb.length() - 1) - c) != 32) {
                sb.append(c);
            } else {
                sb.deleteCharAt(sb.length() - 1);
            }
        }
        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string makeGood(string s) {
        string stk;
        for (char c : s) {
            if (stk.empty() || abs(stk.back() - c) != 32) {
                stk += c;
            } else {
                stk.pop_back();
            }
        }
        return stk;
    }
};
```

#### Go

```go
func makeGood(s string) string {
	stk := []rune{}
	for _, c := range s {
		if len(stk) == 0 || abs(int(stk[len(stk)-1]-c)) != 32 {
			stk = append(stk, c)
		} else {
			stk = stk[:len(stk)-1]
		}
	}
	return string(stk)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
