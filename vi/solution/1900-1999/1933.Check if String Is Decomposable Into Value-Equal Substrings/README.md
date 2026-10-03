---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [1933. Check if String Is Decomposable Into Value-Equal Substrings 🔒](https://leetcode.com/problems/check-if-string-is-decomposable-into-value-equal-substrings)

[中文文档](/solution/1900-1999/1933.Check%20if%20String%20Is%20Decomposable%20Into%20Value-Equal%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi <strong>đồng giá trị</strong> là chuỗi mà <strong>tất cả</strong> các ký tự đều giống nhau.</p>

<ul>
	<li>Ví dụ, <code>&quot;1111&quot;</code> và <code>&quot;33&quot;</code> là các chuỗi đồng giá trị.</li>
	<li>Ngược lại, <code>&quot;123&quot;</code> không phải là chuỗi đồng giá trị.</li>
</ul>

<p>Cho một chuỗi chữ số <code>s</code>, hãy phân tách chuỗi thành một số chuỗi con <strong>liên tiếp và đồng giá trị</strong>, trong đó <strong>chính xác một</strong> chuỗi con có <strong>độ dài </strong><code>2</code>, còn các chuỗi con còn lại có <strong>độ dài </strong><code>3</code>.</p>

<p>Trả về <code>true</code><em> nếu bạn có thể phân tách </em><code>s</code><em> theo các quy tắc trên. Nếu không, trả về </em><code>false</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;000111000&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích: </strong>s không thể được phân tách theo các quy tắc trên vì [&quot;000&quot;, &quot;111&quot;, &quot;000&quot;] không có chuỗi con nào có độ dài 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00011111222&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>s có thể được phân tách thành [&quot;000&quot;, &quot;111&quot;, &quot;11&quot;, &quot;222&quot;].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;011100022233&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích: </strong>s không thể được phân tách theo các quy tắc trên do ký tự &#39;0&#39; đầu tiên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ chứa các chữ số từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Các đoạn đồng giá trị phải có độ dài $2$ hoặc $3$, và bắt buộc phải có chính xác một đoạn dài $2$. Việc tìm các vị trí cắt có độ phức tạp lũy thừa, nhưng các ký tự giống nhau tạo thành những đoạn liên tiếp.
>
> Hai con trỏ đo từng đoạn: số dư $1$ khi chia cho $3$ không thể ghép thành các đoạn hợp lệ; số dư $2$ sử dụng đoạn duy nhất dài $2$, và nếu có đoạn thứ hai như vậy thì thất bại.
>
> Sau khi duyệt, chỉ chấp nhận nếu đã gặp đúng một đoạn có số dư $2$.

<!-- thinking:end -->

Ta duyệt chuỗi $s$, sử dụng hai con trỏ $i$ và $j$ để đếm độ dài của từng chuỗi con gồm các ký tự giống nhau. Nếu độ dài chia cho $3$ dư $1$, điều đó có nghĩa là độ dài của chuỗi con này không thỏa mãn yêu cầu, nên ta trả về `false`. Nếu độ dài chia cho $3$ dư $2$, điều đó có nghĩa là đã xuất hiện một chuỗi con dài $2$. Nếu trước đó đã xuất hiện một chuỗi con dài $2$, trả về `false`; nếu không, gán giá trị của $j$ cho $i$ và tiếp tục duyệt.

Sau khi duyệt xong, kiểm tra xem đã xuất hiện một chuỗi con dài $2$ hay chưa. Nếu chưa, trả về `false`, ngược lại trả về `true`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isDecomposable(self, s: str) -> bool:
        cnt2 = 0
        for _, g in groupby(s):
            m = len(list(g))
            if m % 3 == 1:
                return False
            cnt2 += m % 3 == 2
            if cnt2 > 1:
                return False
        return cnt2 == 1
```

#### Java

```java
class Solution {
    public boolean isDecomposable(String s) {
        int i = 0, n = s.length();
        int cnt2 = 0;
        while (i < n) {
            int j = i;
            while (j < n && s.charAt(j) == s.charAt(i)) {
                ++j;
            }
            if ((j - i) % 3 == 1) {
                return false;
            }
            if ((j - i) % 3 == 2 && ++cnt2 > 1) {
                return false;
            }
            i = j;
        }
        return cnt2 == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isDecomposable(string s) {
        int cnt2 = 0;
        for (int i = 0, n = s.size(); i < n;) {
            int j = i;
            while (j < n && s[j] == s[i]) {
                ++j;
            }
            if ((j - i) % 3 == 1) {
                return false;
            }
            cnt2 += (j - i) % 3 == 2;
            if (cnt2 > 1) {
                return false;
            }
            i = j;
        }
        return cnt2 == 1;
    }
};
```

#### Go

```go
func isDecomposable(s string) bool {
	i, n := 0, len(s)
	cnt2 := 0
	for i < n {
		j := i
		for j < n && s[j] == s[i] {
			j++
		}
		if (j-i)%3 == 1 {
			return false
		}
		if (j-i)%3 == 2 {
			cnt2++
			if cnt2 > 1 {
				return false
			}
		}
		i = j
	}
	return cnt2 == 1
}
```

#### TypeScript

```ts
function isDecomposable(s: string): boolean {
    const n = s.length;
    let cnt2 = 0;
    for (let i = 0; i < n;) {
        let j = i;
        while (j < n && s[j] === s[i]) {
            ++j;
        }
        if ((j - i) % 3 === 1) {
            return false;
        }
        if ((j - i) % 3 === 2 && ++cnt2 > 1) {
            return false;
        }
        i = j;
    }
    return cnt2 === 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
