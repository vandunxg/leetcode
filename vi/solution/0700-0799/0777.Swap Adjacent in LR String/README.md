---
comments: true
difficulty: Medium
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [777. Swap Adjacent in LR String](https://leetcode.com/problems/swap-adjacent-in-lr-string)

[中文文档](/solution/0700-0799/0777.Swap%20Adjacent%20in%20LR%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Với một chuỗi gồm các ký tự <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> và <code>&#39;X&#39;</code>, chẳng hạn <code>&quot;RXXLRXRXL&quot;</code>, mỗi bước có thể thay một lần xuất hiện của <code>&quot;XL&quot;</code> bằng <code>&quot;LX&quot;</code>, hoặc thay một lần xuất hiện của <code>&quot;RX&quot;</code> bằng <code>&quot;XR&quot;</code>. Cho chuỗi ban đầu <code>start</code> và chuỗi đích <code>result</code>, trả về <code>True</code> khi và chỉ khi tồn tại một chuỗi các bước biến đổi <code>start</code> thành <code>result</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = &quot;RXXLRXRXL&quot;, result = &quot;XRLXXRRLX&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể biến đổi start thành result theo các bước sau:
RXXLRXRXL -&gt;
XRXLRXRXL -&gt;
XRLXRXRXL -&gt;
XRLXXRRXL -&gt;
XRLXXRRLX
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = &quot;X&quot;, result = &quot;L&quot;
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= start.length&nbsp;&lt;= 10<sup>4</sup></code></li>
	<li><code>start.length == result.length</code></li>
	<li>Cả <code>start</code> và <code>result</code> chỉ gồm các ký tự <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> và&nbsp;<code>&#39;X&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các phép đổi `XL`/`RX` đẩy `L` sang trái và `R` sang phải; hai ký tự này không thể vượt qua nhau. Sau khi bỏ các ký tự `X`, dãy ký tự còn lại phải giống nhau.
>
> Mỗi `L` được ghép tương ứng không thể di chuyển sang phải ($i\ge j$), còn mỗi `R` không thể di chuyển sang trái ($i\le j$).
>
> Dùng hai con trỏ bỏ qua `X` và kiểm tra các bất đẳng thức đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canTransform(self, start: str, end: str) -> bool:
        n = len(start)
        i = j = 0
        while 1:
            while i < n and start[i] == 'X':
                i += 1
            while j < n and end[j] == 'X':
                j += 1
            if i >= n and j >= n:
                return True
            if i >= n or j >= n or start[i] != end[j]:
                return False
            if start[i] == 'L' and i < j:
                return False
            if start[i] == 'R' and i > j:
                return False
            i, j = i + 1, j + 1
```

#### Java

```java
class Solution {
    public boolean canTransform(String start, String end) {
        int n = start.length();
        int i = 0, j = 0;
        while (true) {
            while (i < n && start.charAt(i) == 'X') {
                ++i;
            }
            while (j < n && end.charAt(j) == 'X') {
                ++j;
            }
            if (i == n && j == n) {
                return true;
            }
            if (i == n || j == n || start.charAt(i) != end.charAt(j)) {
                return false;
            }
            if (start.charAt(i) == 'L' && i < j || start.charAt(i) == 'R' && i > j) {
                return false;
            }
            ++i;
            ++j;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canTransform(string start, string end) {
        int n = start.size();
        int i = 0, j = 0;
        while (true) {
            while (i < n && start[i] == 'X') ++i;
            while (j < n && end[j] == 'X') ++j;
            if (i == n && j == n) return true;
            if (i == n || j == n || start[i] != end[j]) return false;
            if (start[i] == 'L' && i < j) return false;
            if (start[i] == 'R' && i > j) return false;
            ++i;
            ++j;
        }
    }
};
```

#### Go

```go
func canTransform(start string, end string) bool {
	n := len(start)
	i, j := 0, 0
	for {
		for i < n && start[i] == 'X' {
			i++
		}
		for j < n && end[j] == 'X' {
			j++
		}
		if i == n && j == n {
			return true
		}
		if i == n || j == n || start[i] != end[j] {
			return false
		}
		if start[i] == 'L' && i < j {
			return false
		}
		if start[i] == 'R' && i > j {
			return false
		}
		i, j = i+1, j+1
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
