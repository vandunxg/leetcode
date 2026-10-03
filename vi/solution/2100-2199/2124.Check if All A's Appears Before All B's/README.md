---
comments: true
difficulty: Easy
rating: 1201
source: Weekly Contest 274 Q1
tags:
    - String
---

<!-- problem:start -->

# [2124. Check if All A's Appears Before All B's](https://leetcode.com/problems/check-if-all-as-appears-before-all-bs)

[中文文档](/solution/2100-2199/2124.Check%20if%20All%20A%27s%20Appears%20Before%20All%20B%27s/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> <strong>chỉ</strong> gồm các ký tự <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code>, hãy trả về <code>true</code> <em>nếu <strong>mọi</strong> </em><code>&#39;a&#39;</code> <em>đều xuất hiện trước <strong>mọi</strong> </em><code>&#39;b&#39;</code><em> trong chuỗi</em>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabbb&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Các ký tự &#39;a&#39; nằm ở các chỉ số 0, 1 và 2, còn các ký tự &#39;b&#39; nằm ở các chỉ số 3, 4 và 5.
Do đó, mọi &#39;a&#39; đều xuất hiện trước mọi &#39;b&#39;, nên ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abab&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Có một &#39;a&#39; ở chỉ số 2 và một &#39;b&#39; ở chỉ số 1.
Do đó, không phải mọi &#39;a&#39; đều xuất hiện trước mọi &#39;b&#39;, nên ta trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bbb&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Không có ký tự &#39;a&#39;, do đó mọi &#39;a&#39; đều xuất hiện trước mọi &#39;b&#39;, nên ta trả về true.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s[i]</code> là <code>&#39;a&#39;</code> hoặc <code>&#39;b&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố tư duy

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi chỉ chứa `a` và `b`. Mọi `a` xuất hiện trước mọi `b` khi và chỉ khi không có `a` nào xuất hiện sau `b`, tức là chuỗi con `ba` không xuất hiện.
>
> Duyệt tuyến tính hoặc kiểm tra chuỗi có thể quyết định điều này trong $O(n)$.
>
> Trả về việc chuỗi `"ba"` không xuất hiện trong $s$.

<!-- thinking:end -->

Theo đề bài, chuỗi $s$ chỉ gồm các ký tự `a` và `b`.

Để đảm bảo mọi `a` đều xuất hiện trước mọi `b`, điều kiện cần là `b` không được xuất hiện trước `a`. Nói cách khác, chuỗi con "ba" không được xuất hiện trong chuỗi $s$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkString(self, s: str) -> bool:
        return "ba" not in s
```

#### Java

```java
class Solution {
    public boolean checkString(String s) {
        return !s.contains("ba");
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkString(string s) {
        return !s.contains("ba");
    }
};
```

#### Go

```go
func checkString(s string) bool {
	return !strings.Contains(s, "ba")
}
```

#### TypeScript

```ts
function checkString(s: string): boolean {
    return !s.includes('ba');
}
```

#### Rust

```rust
impl Solution {
    pub fn check_string(s: String) -> bool {
        !s.contains("ba")
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
