---
comments: true
difficulty: Easy
rating: 1315
source: Weekly Contest 212 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [1629. Slowest Key](https://leetcode.com/problems/slowest-key)

[中文文档](/solution/1600-1699/1629.Slowest%20Key/README.md)

## Mô tả

<!-- description:start -->

<p>Một bàn phím mới được kiểm thử, trong đó người kiểm thử nhấn một dãy <code>n</code> phím, từng phím một.</p>

<p>Cho chuỗi <code>keysPressed</code> độ dài <code>n</code>, trong đó <code>keysPressed[i]</code> là phím thứ <code>i<sup>th</sup></code> được nhấn trong dãy kiểm thử, và danh sách đã sắp xếp <code>releaseTimes</code>, trong đó <code>releaseTimes[i]</code> là thời điểm phím thứ <code>i<sup>th</sup></code> được thả. Cả hai mảng đều <strong>đánh số từ 0</strong>. Phím thứ <code>0<sup>th</sup></code> được nhấn tại thời điểm <code>0</code>,&nbsp;và mỗi phím tiếp theo được nhấn <strong>đúng</strong> vào thời điểm phím trước đó được thả.</p>

<p>Người kiểm thử muốn biết phím có lần nhấn <strong>dài nhất</strong>. Lần nhấn thứ <code>i<sup>th</sup></code><sup> </sup>có <strong>thời lượng</strong> là <code>releaseTimes[i] - releaseTimes[i - 1]</code>, còn lần nhấn thứ <code>0<sup>th</sup></code> có thời lượng là <code>releaseTimes[0]</code>.</p>

<p>Lưu ý rằng cùng một phím có thể được nhấn nhiều lần trong quá trình kiểm thử, và các lần nhấn đó <strong>có thể không</strong> có cùng <strong>thời lượng</strong>.</p>

<p><em>Trả về phím có lần nhấn <strong>dài nhất</strong>. Nếu có nhiều lần nhấn như vậy, trả về phím lớn nhất theo thứ tự từ điển.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> releaseTimes = [9,29,49,50], keysPressed = &quot;cbcd&quot;
<strong>Output:</strong> &quot;c&quot;
<strong>Giải thích:</strong> Các lần nhấn phím như sau:
Keypress for &#39;c&#39; had a duration of 9 (pressed at time 0 and released at time 9).
Lần nhấn &#39;b&#39; có thời lượng 29 - 9 = 20 (được nhấn tại thời điểm 9 ngay sau khi thả ký tự trước đó và được thả tại thời điểm 29).
Lần nhấn &#39;c&#39; có thời lượng 49 - 29 = 20 (được nhấn tại thời điểm 29 ngay sau khi thả ký tự trước đó và được thả tại thời điểm 49).
Lần nhấn &#39;d&#39; có thời lượng 50 - 49 = 1 (được nhấn tại thời điểm 49 ngay sau khi thả ký tự trước đó và được thả tại thời điểm 50).
Dài nhất là lần nhấn &#39;b&#39; và lần nhấn thứ hai của &#39;c&#39;, đều có thời lượng 20.
&#39;c&#39; lớn hơn &#39;b&#39; theo thứ tự từ điển, nên đáp án là &#39;c&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> releaseTimes = [12,23,36,46,62], keysPressed = &quot;spuda&quot;
<strong>Output:</strong> &quot;a&quot;
<strong>Giải thích:</strong> Các lần nhấn phím như sau:
Lần nhấn &#39;s&#39; có thời lượng 12.
Lần nhấn &#39;p&#39; có thời lượng 23 - 12 = 11.
Lần nhấn &#39;u&#39; có thời lượng 36 - 23 = 13.
Lần nhấn &#39;d&#39; có thời lượng 46 - 36 = 10.
Lần nhấn &#39;a&#39; có thời lượng 62 - 46 = 16.
Dài nhất là lần nhấn &#39;a&#39; với thời lượng 16.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>releaseTimes.length == n</code></li>
	<li><code>keysPressed.length == n</code></li>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= releaseTimes[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>releaseTimes[i] &lt; releaseTimes[i+1]</code></li>
	<li><code>keysPressed</code> contains only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thời lượng là hiệu của hai thời điểm thả liên tiếp. Với độ dài nhiều nhất $1000$, chỉ cần một lượt duyệt để theo dõi thời lượng lớn nhất và phím tương ứng.
>
> Thay đáp án khi gặp thời lượng lớn hơn nghiêm ngặt; nếu hòa, chọn phím lớn hơn theo thứ tự từ điển. Phím đầu tiên có thời lượng $\textit{releaseTimes}[0]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def slowestKey(self, releaseTimes: List[int], keysPressed: str) -> str:
        ans = keysPressed[0]
        mx = releaseTimes[0]
        for i in range(1, len(keysPressed)):
            d = releaseTimes[i] - releaseTimes[i - 1]
            if d > mx or (d == mx and ord(keysPressed[i]) > ord(ans)):
                mx = d
                ans = keysPressed[i]
        return ans
```

#### Java

```java
class Solution {
    public char slowestKey(int[] releaseTimes, String keysPressed) {
        char ans = keysPressed.charAt(0);
        int mx = releaseTimes[0];
        for (int i = 1; i < releaseTimes.length; ++i) {
            int d = releaseTimes[i] - releaseTimes[i - 1];
            if (d > mx || (d == mx && keysPressed.charAt(i) > ans)) {
                mx = d;
                ans = keysPressed.charAt(i);
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
    char slowestKey(vector<int>& releaseTimes, string keysPressed) {
        char ans = keysPressed[0];
        int mx = releaseTimes[0];
        for (int i = 1, n = releaseTimes.size(); i < n; ++i) {
            int d = releaseTimes[i] - releaseTimes[i - 1];
            if (d > mx || (d == mx && keysPressed[i] > ans)) {
                mx = d;
                ans = keysPressed[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func slowestKey(releaseTimes []int, keysPressed string) byte {
	ans := keysPressed[0]
	mx := releaseTimes[0]
	for i := 1; i < len(releaseTimes); i++ {
		d := releaseTimes[i] - releaseTimes[i-1]
		if d > mx || (d == mx && keysPressed[i] > ans) {
			mx = d
			ans = keysPressed[i]
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
