---
comments: true
difficulty: Easy
rating: 1232
source: Biweekly Contest 4 Q2
tags:
    - String
---

<!-- problem:start -->

# [1119. Remove Vowels from a String 🔒](https://leetcode.com/problems/remove-vowels-from-a-string)

[中文文档](/solution/1100-1199/1119.Remove%20Vowels%20from%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy xóa các nguyên âm <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>, rồi trả về chuỗi mới.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;leetcodeisacommunityforcoders&quot;
<strong>Output:</strong> &quot;ltcdscmmntyfrcdrs&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aeiou&quot;
<strong>Output:</strong> &quot;&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Việc bỏ nguyên âm là phép kiểm tra từng ký tự: chỉ cần duyệt một lần, bỏ qua `aeiou` và tạo chuỗi kết quả. Gọi replace nhiều lần sẽ khiến chuỗi bị duyệt lại không cần thiết.

<!-- thinking:end -->

Ta có thể duyệt trực tiếp chuỗi theo yêu cầu của đề bài và nối các ký tự không phải nguyên âm vào chuỗi kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Không tính bộ nhớ của chuỗi kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeVowels(self, s: str) -> str:
        return "".join(c for c in s if c not in "aeiou")
```

#### Java

```java
class Solution {
    public String removeVowels(String s) {
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (!(c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u')) {
                ans.append(c);
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeVowels(string s) {
        string ans;
        for (char& c : s) {
            if (!(c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u')) {
                ans += c;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeVowels(s string) string {
	ans := []rune{}
	for _, c := range s {
		if !(c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
			ans = append(ans, c)
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function removeVowels(s: string): string {
    return s.replace(/[aeiou]/g, '');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
