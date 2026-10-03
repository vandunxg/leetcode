---
comments: true
difficulty: Medium
rating: 1315
source: Weekly Contest 272 Q2
tags:
    - Array
    - Two Pointers
    - String
    - Simulation
---

<!-- problem:start -->

# [2109. Adding Spaces to a String](https://leetcode.com/problems/adding-spaces-to-a-string)

[中文文档](/solution/2100-2199/2109.Adding%20Spaces%20to%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>s</code> và một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>spaces</code> mô tả các chỉ số trong chuỗi ban đầu, tại đó sẽ thêm khoảng trắng. Mỗi khoảng trắng được chèn <strong>trước</strong> ký tự ở chỉ số tương ứng.</p>

<ul>
	<li>Ví dụ, với <code>s = &quot;EnjoyYourCoffee&quot;</code> và <code>spaces = [5, 9]</code>, ta đặt khoảng trắng trước <code>&#39;Y&#39;</code> và <code>&#39;C&#39;</code>, lần lượt ở các chỉ số <code>5</code> và <code>9</code>. Khi đó, ta nhận được <code>&quot;Enjoy <strong><u>Y</u></strong>our <u><strong>C</strong></u>offee&quot;</code>.</li>
</ul>

<p>Trả về <strong> </strong><em>chuỗi <strong>sau khi</strong> đã thêm các khoảng trắng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;LeetcodeHelpsMeLearn&quot;, spaces = [8,13,15]
<strong>Đầu ra:</strong> &quot;Leetcode Helps Me Learn&quot;
<strong>Giải thích:</strong>
Các chỉ số 8, 13 và 15 tương ứng với các ký tự được gạch chân trong &quot;Leetcode<u><strong>H</strong></u>elps<u><strong>M</strong></u>e<u><strong>L</strong></u>earn&quot;.
Sau đó, ta đặt khoảng trắng trước các ký tự đó.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;icodeinpython&quot;, spaces = [1,5,7,9]
<strong>Đầu ra:</strong> &quot;i code in py thon&quot;
<strong>Giải thích:</strong>
Các chỉ số 1, 5, 7 và 9 tương ứng với các ký tự được gạch chân trong &quot;i<u><strong>c</strong></u>ode<u><strong>i</strong></u>n<u><strong>p</strong></u>y<u><strong>t</strong></u>hon&quot;.
Sau đó, ta đặt khoảng trắng trước các ký tự đó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;spacing&quot;, spaces = [0,1,2,3,4,5,6]
<strong>Đầu ra:</strong> &quot; s p a c i n g&quot;
<strong>Giải thích:</strong>
Ta cũng có thể đặt khoảng trắng trước ký tự đầu tiên của chuỗi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và viết hoa.</li>
	<li><code>1 &lt;= spaces.length &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>0 &lt;= spaces[i] &lt;= s.length - 1</code></li>
	<li>Tất cả các giá trị trong <code>spaces</code> đều <strong>tăng dần</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Các khoảng trắng phải được chèn tại những chỉ số đã cho mà không làm thay đổi thứ tự tương đối của $s$. Nếu chèn trực tiếp vào chuỗi, phần hậu tố sẽ bị dịch chuyển nhiều lần và thời gian chạy có thể trở thành bậc hai.
>
> Vì $\textit{spaces}$ tăng dần, ta có thể duyệt mảng này đồng thời với các chỉ số của $s$ trong thời gian tuyến tính.
>
> Một con trỏ $j$ đánh dấu khoảng trắng tiếp theo. Khi duyệt $s$, nếu chỉ số hiện tại bằng $\textit{spaces}[j]$ thì ta thêm khoảng trắng trước, sau đó thêm ký tự, rồi cuối cùng nối các phần tử trong buffer.

<!-- thinking:end -->

Ta có thể sử dụng hai con trỏ $i$ và $j$ lần lượt trỏ đến đầu chuỗi $s$ và mảng $\textit{spaces}$. Sau đó, ta duyệt chuỗi $s$ từ đầu đến cuối. Khi $i$ bằng $\textit{spaces}[j]$, ta thêm một khoảng trắng vào chuỗi kết quả, rồi tăng $j$ lên $1$. Tiếp theo, ta thêm $s[i]$ vào chuỗi kết quả, rồi tăng $i$ lên $1$. Ta tiếp tục quá trình này cho đến khi duyệt hết chuỗi $s$.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của chuỗi $s$ và mảng $spaces$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def addSpaces(self, s: str, spaces: List[int]) -> str:
        ans = []
        j = 0
        for i, c in enumerate(s):
            if j < len(spaces) and i == spaces[j]:
                ans.append(' ')
                j += 1
            ans.append(c)
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String addSpaces(String s, int[] spaces) {
        StringBuilder ans = new StringBuilder();
        for (int i = 0, j = 0; i < s.length(); ++i) {
            if (j < spaces.length && i == spaces[j]) {
                ans.append(' ');
                ++j;
            }
            ans.append(s.charAt(i));
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string addSpaces(string s, vector<int>& spaces) {
        string ans = "";
        for (int i = 0, j = 0; i < s.size(); ++i) {
            if (j < spaces.size() && i == spaces[j]) {
                ans += ' ';
                ++j;
            }
            ans += s[i];
        }
        return ans;
    }
};
```

#### Go

```go
func addSpaces(s string, spaces []int) string {
	var ans []byte
	for i, j := 0, 0; i < len(s); i++ {
		if j < len(spaces) && i == spaces[j] {
			ans = append(ans, ' ')
			j++
		}
		ans = append(ans, s[i])
	}
	return string(ans)
}
```

#### TypeScript

```ts
function addSpaces(s: string, spaces: number[]): string {
    const ans: string[] = [];
    for (let i = 0, j = 0; i < s.length; i++) {
        if (i === spaces[j]) {
            ans.push(' ');
            j++;
        }
        ans.push(s[i]);
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
