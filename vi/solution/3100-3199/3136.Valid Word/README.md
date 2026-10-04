---
comments: true
difficulty: Easy
rating: 1249
source: Weekly Contest 396 Q1
tags:
    - String
---

<!-- problem:start -->

# [3136. Valid Word](https://leetcode.com/problems/valid-word)

[中文文档](/solution/3100-3199/3136.Valid%20Word/README.md)

## Mô tả

<!-- description:start -->

<p>Một từ được xem là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li>Có <strong>ít nhất</strong> 3 ký tự.</li>
	<li>Chỉ chứa chữ số (0-9) và chữ cái tiếng Anh (chữ hoa và chữ thường).</li>
	<li>Có <strong>ít nhất</strong> một <strong>nguyên âm</strong>.</li>
	<li>Có <strong>ít nhất</strong> một <strong>phụ âm</strong>.</li>
</ul>

<p>Cho một chuỗi <code>word</code>.</p>

<p>Trả về <code>true</code> nếu <code>word</code> hợp lệ, ngược lại trả về <code>false</code>.</p>

<p><strong>Ghi chú:</strong></p>

<ul>
	<li><code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code>, <code>&#39;u&#39;</code> và các chữ hoa tương ứng là <strong>nguyên âm</strong>.</li>
	<li><strong>Phụ âm</strong> là một chữ cái tiếng Anh không phải nguyên âm.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;234Adas&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Từ này thỏa mãn các điều kiện.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;b3&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Độ dài của từ này nhỏ hơn 3 và không có nguyên âm.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;a3$e&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Từ này chứa ký tự <code>&#39;$&#39;</code> và không có phụ âm.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 20</code></li>
	<li><code>word</code> gồm các chữ cái tiếng Anh viết hoa và viết thường, chữ số, <code>&#39;@&#39;</code>, <code>&#39;#&#39;</code> và <code>&#39;$&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một từ hợp lệ có độ dài ít nhất $3$, chỉ chứa chữ và số, đồng thời có ít nhất một nguyên âm và một phụ âm. Các điều kiện này độc lập với nhau và có thể kiểm tra trong một lần duyệt.
>
> Không cần đến automaton. Ký tự không phải chữ hoặc số sẽ khiến từ không hợp lệ ngay; các chữ cái được phân loại dựa trên tập nguyên âm.
>
> Trước hết, loại bỏ các chuỗi quá ngắn, sau đó theo dõi $has\_vowel$ và $has\_consonant$. Cuối cùng, cả hai cờ đều phải là true.

<!-- thinking:end -->

Đầu tiên, chúng ta kiểm tra xem độ dài chuỗi có nhỏ hơn 3 hay không. Nếu có, trả về `false`.

Tiếp theo, chúng ta duyệt qua chuỗi và kiểm tra xem mỗi ký tự là chữ cái hay chữ số. Nếu không phải, trả về `false`. Nếu phải, chúng ta kiểm tra xem ký tự có phải là nguyên âm hay không. Nếu phải, đặt `has_vowel` thành `true`. Nếu không, đặt `has_consonant` thành `true`.

Cuối cùng, nếu cả `has_vowel` và `has_consonant` đều là `true`, trả về `true`. Nếu không, trả về `false`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isValid(self, word: str) -> bool:
        if len(word) < 3:
            return False
        has_vowel = has_consonant = False
        vs = set("aeiouAEIOU")
        for c in word:
            if not c.isalnum():
                return False
            if c.isalpha():
                if c in vs:
                    has_vowel = True
                else:
                    has_consonant = True
        return has_vowel and has_consonant
```

#### Java

```java
class Solution {
    public boolean isValid(String word) {
        if (word.length() < 3) {
            return false;
        }
        boolean hasVowel = false, hasConsonant = false;
        boolean[] vs = new boolean[26];
        for (char c : "aeiou".toCharArray()) {
            vs[c - 'a'] = true;
        }
        for (char c : word.toCharArray()) {
            if (Character.isAlphabetic(c)) {
                if (vs[Character.toLowerCase(c) - 'a']) {
                    hasVowel = true;
                } else {
                    hasConsonant = true;
                }
            } else if (!Character.isDigit(c)) {
                return false;
            }
        }
        return hasVowel && hasConsonant;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isValid(string word) {
        if (word.size() < 3) {
            return false;
        }
        bool has_vowel = false, has_consonant = false;
        bool vs[26]{};
        string vowels = "aeiou";
        for (char c : vowels) {
            vs[c - 'a'] = true;
        }
        for (char c : word) {
            if (isalpha(c)) {
                if (vs[tolower(c) - 'a']) {
                    has_vowel = true;
                } else {
                    has_consonant = true;
                }
            } else if (!isdigit(c)) {
                return false;
            }
        }
        return has_vowel && has_consonant;
    }
};
```

#### Go

```go
func isValid(word string) bool {
	if len(word) < 3 {
		return false
	}
	hasVowel := false
	hasConsonant := false
	vs := make([]bool, 26)
	for _, c := range "aeiou" {
		vs[c-'a'] = true
	}
	for _, c := range word {
		if unicode.IsLetter(c) {
			if vs[unicode.ToLower(c)-'a'] {
				hasVowel = true
			} else {
				hasConsonant = true
			}
		} else if !unicode.IsDigit(c) {
			return false
		}
	}
	return hasVowel && hasConsonant
}
```

#### TypeScript

```ts
function isValid(word: string): boolean {
    if (word.length < 3) {
        return false;
    }
    let hasVowel: boolean = false;
    let hasConsonant: boolean = false;
    const vowels: Set<string> = new Set(['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U']);
    for (const c of word) {
        if (!c.match(/[a-zA-Z0-9]/)) {
            return false;
        }
        if (/[a-zA-Z]/.test(c)) {
            if (vowels.has(c)) {
                hasVowel = true;
            } else {
                hasConsonant = true;
            }
        }
    }
    return hasVowel && hasConsonant;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_valid(word: String) -> bool {
        if word.len() < 3 {
            return false;
        }

        let mut has_vowel = false;
        let mut has_consonant = false;
        let vowels = ['a', 'e', 'i', 'o', 'u'];

        for c in word.chars() {
            if !c.is_alphanumeric() {
                return false;
            }
            if c.is_alphabetic() {
                let lower_c = c.to_ascii_lowercase();
                if vowels.contains(&lower_c) {
                    has_vowel = true;
                } else {
                    has_consonant = true;
                }
            }
        }

        has_vowel && has_consonant
    }
}
```

#### C#

```cs
public class Solution {
    public bool IsValid(string word) {
        if (word.Length < 3) {
            return false;
        }

        bool hasVowel = false, hasConsonant = false;
        bool[] vs = new bool[26];
        foreach (char c in "aeiou") {
            vs[c - 'a'] = true;
        }

        foreach (char c in word) {
            if (char.IsLetter(c)) {
                char lower = char.ToLower(c);
                if (vs[lower - 'a']) {
                    hasVowel = true;
                } else {
                    hasConsonant = true;
                }
            } else if (!char.IsDigit(c)) {
                return false;
            }
        }

        return hasVowel && hasConsonant;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
