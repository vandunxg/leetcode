---
comments: true
difficulty: Easy
rating: 1139
source: Weekly Contest 491 Q1
tags:
    - String
---

<!-- problem:start -->

# [3856. Trim Trailing Vowels](https://leetcode.com/problems/trim-trailing-vowels)

[中文文档](/solution/3800-3899/3856.Trim%20Trailing%20Vowels/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Hãy trả về chuỗi thu được sau khi xóa <strong>tất cả</strong> <strong>nguyên âm</strong> ở cuối chuỗi <code>s</code>.</p>

<p>Các <strong>nguyên âm</strong> gồm các ký tự <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;idea&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;id&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa <code>&quot;id<u><strong>ea</strong></u>&quot;</code>, ta thu được chuỗi <code>&quot;id&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;day&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;day&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>&quot;day&quot;</code> không có nguyên âm nào ở cuối.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aeiou&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa <code>&quot;<u><strong>aeiou</strong></u>&quot;</code>, ta thu được chuỗi <code>&quot;&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Cần xóa mọi nguyên âm ở cuối chuỗi. Vì $|s| \le 100$, ta chỉ cần duyệt từ phải sang trái.
>
> Đáp án là tiền tố kết thúc tại ký tự không phải nguyên âm cuối cùng, hoặc là chuỗi rỗng nếu mọi ký tự đều là nguyên âm.
>
> Bỏ qua các ký tự thuộc $\texttt{aeiou}$ từ bên phải và trả về $s[:i+1]$.
>
> Chỉ cần một con trỏ và không gian phụ hằng số.

<!-- thinking:end -->

Ta duyệt chuỗi từ cuối về đầu cho đến khi gặp ký tự đầu tiên không phải nguyên âm. Sau đó, ta trả về chuỗi con từ đầu chuỗi đến vị trí đó.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def trimTrailingVowels(self, s: str) -> str:
        i = len(s) - 1
        while i >= 0 and s[i] in "aeiou":
            i -= 1
        return s[: i + 1]
```

#### Java

```java
class Solution {
    public String trimTrailingVowels(String s) {
        int i = s.length() - 1;
        while (i >= 0 && "aeiou".indexOf(s.charAt(i)) != -1) {
            i--;
        }
        return s.substring(0, i + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string trimTrailingVowels(string s) {
        int i = s.size() - 1;
        while (i >= 0 && string("aeiou").find(s[i]) != string::npos) {
            i--;
        }
        return s.substr(0, i + 1);
    }
};
```

#### Go

```go
func trimTrailingVowels(s string) string {
	i := len(s) - 1
	for i >= 0 && strings.IndexByte("aeiou", s[i]) != -1 {
		i--
	}
	return s[:i+1]
}
```

#### TypeScript

```ts
function trimTrailingVowels(s: string): string {
    let i = s.length - 1;
    while (i >= 0 && 'aeiou'.indexOf(s[i]) !== -1) {
        i--;
    }
    return s.slice(0, i + 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
