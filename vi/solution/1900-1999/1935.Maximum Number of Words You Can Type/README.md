---
comments: true
difficulty: Easy
rating: 1226
source: Weekly Contest 250 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1935. Maximum Number of Words You Can Type](https://leetcode.com/problems/maximum-number-of-words-you-can-type)

[中文文档](/solution/1900-1999/1935.Maximum%20Number%20of%20Words%20You%20Can%20Type/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bàn phím bị hỏng, trong đó một số phím chữ không hoạt động. Tất cả các phím còn lại trên bàn phím đều hoạt động bình thường.</p>

<p>Cho một chuỗi <code>text</code> gồm các từ được ngăn cách bởi một dấu cách duy nhất (không có dấu cách ở đầu hoặc cuối) và một chuỗi <code>brokenLetters</code> chứa tất cả các phím chữ bị hỏng và <strong>không trùng nhau</strong>, hãy trả về <em><strong>số lượng từ</strong> trong</em> <code>text</code> <em>mà bạn có thể gõ đầy đủ bằng bàn phím này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;hello world&quot;, brokenLetters = &quot;ad&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Không thể gõ &quot;world&quot; vì phím &#39;d&#39; bị hỏng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;leet code&quot;, brokenLetters = &quot;lt&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Không thể gõ &quot;leet&quot; vì các phím &#39;l&#39; và &#39;t&#39; bị hỏng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;leet code&quot;, brokenLetters = &quot;e&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể gõ cả hai từ vì phím &#39;e&#39; bị hỏng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= brokenLetters.length &lt;= 26</code></li>
	<li><code>text</code> gồm các từ được ngăn cách bởi một dấu cách duy nhất, không có dấu cách ở đầu hoặc cuối.</li>
	<li>Mỗi từ chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>brokenLetters</code> gồm các chữ cái tiếng Anh viết thường <strong>không trùng nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một từ có thể gõ được khi và chỉ khi nó không chứa chữ cái bị hỏng. Ta đưa các chữ cái bị hỏng vào một set rồi tách $\textit{text}$ theo dấu cách.
>
> Đếm các từ mà mọi ký tự đều không thuộc set. Vì bảng chữ cái có kích thước cố định nên việc duyệt có độ phức tạp tuyến tính theo độ dài của chuỗi.

<!-- thinking:end -->

Ta có thể dùng một hash table hoặc một mảng $s$ có độ dài $26$ để ghi lại tất cả các phím chữ bị hỏng.

Sau đó, ta duyệt qua từng từ $w$ trong chuỗi $text$; nếu có ký tự $c$ nào trong $w$ xuất hiện trong $s$, điều đó có nghĩa là không thể gõ từ này, và ta không cần cộng thêm một vào đáp án. Ngược lại, ta cộng thêm một vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài của chuỗi $text$, và $|\Sigma|$ là kích thước của bảng chữ cái. Trong bài này, $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canBeTypedWords(self, text: str, brokenLetters: str) -> int:
        s = set(brokenLetters)
        return sum(all(c not in s for c in w) for w in text.split())
```

#### Java

```java
class Solution {
    public int canBeTypedWords(String text, String brokenLetters) {
        boolean[] s = new boolean[26];
        for (char c : brokenLetters.toCharArray()) {
            s[c - 'a'] = true;
        }
        int ans = 0;
        for (String w : text.split(" ")) {
            for (char c : w.toCharArray()) {
                if (s[c - 'a']) {
                    --ans;
                    break;
                }
            }
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int canBeTypedWords(string text, string brokenLetters) {
        bool s[26]{};
        for (char c : brokenLetters) {
            s[c - 'a'] = true;
        }
        int ans = 0;
        stringstream ss(text);
        string w;
        while (ss >> w) {
            for (char c : w) {
                if (s[c - 'a']) {
                    --ans;
                    break;
                }
            }
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func canBeTypedWords(text string, brokenLetters string) (ans int) {
	s := [26]bool{}
	for _, c := range brokenLetters {
		s[c-'a'] = true
	}
	for _, w := range strings.Split(text, " ") {
		for _, c := range w {
			if s[c-'a'] {
				ans--
				break
			}
		}
		ans++
	}
	return
}
```

#### TypeScript

```ts
function canBeTypedWords(text: string, brokenLetters: string): number {
    const s: boolean[] = Array(26).fill(false);
    for (const c of brokenLetters) {
        s[c.charCodeAt(0) - 'a'.charCodeAt(0)] = true;
    }
    let ans = 0;
    for (const w of text.split(' ')) {
        for (const c of w) {
            if (s[c.charCodeAt(0) - 'a'.charCodeAt(0)]) {
                --ans;
                break;
            }
        }
        ++ans;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_be_typed_words(text: String, broken_letters: String) -> i32 {
        let mut s = vec![false; 26];
        for c in broken_letters.chars() {
            s[(c as usize) - ('a' as usize)] = true;
        }
        let mut ans = 0;
        let words = text.split_whitespace();
        for w in words {
            for c in w.chars() {
                if s[(c as usize) - ('a' as usize)] {
                    ans -= 1;
                    break;
                }
            }
            ans += 1;
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int CanBeTypedWords(string text, string brokenLetters) {
        bool[] s = new bool[26];
        foreach (char c in brokenLetters) {
            s[c - 'a'] = true;
        }
        int ans = 0;
        string[] words = text.Split(' ');
        foreach (string w in words) {
            foreach (char c in w) {
                if (s[c - 'a']) {
                    --ans;
                    break;
                }
            }
            ++ans;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
