---
comments: true
difficulty: Easy
rating: 1151
source: Weekly Contest 359 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2828. Check if a String Is an Acronym of Words](https://leetcode.com/problems/check-if-a-string-is-an-acronym-of-words)

[中文文档](/solution/2800-2899/2828.Check%20if%20a%20String%20Is%20an%20Acronym%20of%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các chuỗi <code>words</code> và một chuỗi <code>s</code>, hãy xác định xem <code>s</code> có phải là <strong>từ viết tắt</strong> của các từ trong mảng hay không.</p>

<p>Chuỗi <code>s</code> được xem là từ viết tắt của <code>words</code> nếu có thể tạo ra nó bằng cách nối <strong>ký tự đầu tiên</strong> của mỗi chuỗi trong <code>words</code> <strong>theo đúng thứ tự</strong>. Ví dụ, <code>&quot;ab&quot;</code> có thể được tạo từ <code>[&quot;apple&quot;, &quot;banana&quot;]</code>, nhưng không thể được tạo từ <code>[&quot;bear&quot;, &quot;aardvark&quot;]</code>.</p>

<p>Trả về <code>true</code><em> nếu </em><code>s</code><em> là từ viết tắt của </em><code>words</code><em>, và </em><code>false</code><em> nếu ngược lại. </em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;alice&quot;,&quot;bob&quot;,&quot;charlie&quot;], s = &quot;abc&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ký tự đầu tiên của các từ &quot;alice&quot;, &quot;bob&quot; và &quot;charlie&quot; lần lượt là &#39;a&#39;, &#39;b&#39; và &#39;c&#39;. Vì vậy, s = &quot;abc&quot; là từ viết tắt cần tìm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;an&quot;,&quot;apple&quot;], s = &quot;a&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Ký tự đầu tiên của hai từ &quot;an&quot; và &quot;apple&quot; lần lượt là &#39;a&#39; và &#39;a&#39;.
Từ viết tắt tạo được bằng cách nối hai ký tự này là &quot;aa&quot;.
Vì vậy, s = &quot;a&quot; không phải là từ viết tắt cần tìm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;never&quot;,&quot;gonna&quot;,&quot;give&quot;,&quot;up&quot;,&quot;on&quot;,&quot;you&quot;], s = &quot;ngguoy&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Nối các ký tự đầu tiên của những từ trong mảng, ta được chuỗi &quot;ngguoy&quot;.
Vì vậy, s = &quot;ngguoy&quot; là từ viết tắt cần tìm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>words[i]</code> và <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Từ viết tắt được tạo bằng cách nối ký tự đầu tiên của từng từ. Chỉ cần duyệt một lần để nối các ký tự đó rồi so sánh với $s$.

<!-- thinking:end -->

Ta có thể duyệt qua từng chuỗi trong mảng $words$, nối các ký tự đầu tiên của chúng để tạo thành chuỗi mới $t$, sau đó kiểm tra xem $t$ có bằng $s$ hay không.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số chuỗi trong mảng $words$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isAcronym(self, words: List[str], s: str) -> bool:
        return "".join(w[0] for w in words) == s
```

#### Java

```java
class Solution {
    public boolean isAcronym(List<String> words, String s) {
        StringBuilder t = new StringBuilder();
        for (var w : words) {
            t.append(w.charAt(0));
        }
        return t.toString().equals(s);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isAcronym(vector<string>& words, string s) {
        string t;
        for (auto& w : words) {
            t += w[0];
        }
        return t == s;
    }
};
```

#### Go

```go
func isAcronym(words []string, s string) bool {
	t := []byte{}
	for _, w := range words {
		t = append(t, w[0])
	}
	return string(t) == s
}
```

#### TypeScript

```ts
function isAcronym(words: string[], s: string): boolean {
    return words.map(w => w[0]).join('') === s;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_acronym(words: Vec<String>, s: String) -> bool {
        words
            .iter()
            .map(|w| w.chars().next().unwrap_or_default())
            .collect::<String>()
            == s
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mô phỏng (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tạo thêm một chuỗi có độ dài $n$. So sánh độ dài trước, sau đó kiểm tra $words[i][0]$ với $s[i]$ sẽ tránh được việc cấp phát chuỗi đó.

<!-- thinking:end -->

Đầu tiên, ta kiểm tra số chuỗi trong $words$ có bằng độ dài của $s$ hay không. Nếu không bằng, chắc chắn $s$ không phải là từ viết tắt được tạo từ các ký tự đầu tiên của $words$, nên ta trả về $false$ ngay.

Tiếp theo, ta duyệt qua từng ký tự trong $s$ và kiểm tra xem nó có bằng chữ cái đầu tiên của chuỗi tương ứng trong $words$ hay không. Nếu không bằng, chắc chắn $s$ không phải là từ viết tắt được tạo từ các ký tự đầu tiên của $words$, nên ta trả về $false$ ngay.

Sau vòng lặp, nếu chưa trả về $false$ thì $s$ là từ viết tắt được tạo từ các ký tự đầu tiên của $words$, và ta trả về $true$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $words$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isAcronym(self, words: List[str], s: str) -> bool:
        return len(words) == len(s) and all(w[0] == c for w, c in zip(words, s))
```

#### Java

```java
class Solution {
    public boolean isAcronym(List<String> words, String s) {
        if (words.size() != s.length()) {
            return false;
        }
        for (int i = 0; i < s.length(); ++i) {
            if (words.get(i).charAt(0) != s.charAt(i)) {
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
    bool isAcronym(vector<string>& words, string s) {
        if (words.size() != s.size()) {
            return false;
        }
        for (int i = 0; i < s.size(); ++i) {
            if (words[i][0] != s[i]) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isAcronym(words []string, s string) bool {
	if len(words) != len(s) {
		return false
	}
	for i := range s {
		if words[i][0] != s[i] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isAcronym(words: string[], s: string): boolean {
    if (words.length !== s.length) {
        return false;
    }
    for (let i = 0; i < words.length; i++) {
        if (words[i][0] !== s[i]) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_acronym(words: Vec<String>, s: String) -> bool {
        if words.len() != s.len() {
            return false;
        }
        for (i, w) in words.iter().enumerate() {
            if w.chars().next().unwrap_or_default() != s.chars().nth(i).unwrap_or_default() {
                return false;
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
