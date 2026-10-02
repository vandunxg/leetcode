---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [917. Reverse Only Letters](https://leetcode.com/problems/reverse-only-letters)

[中文文档](/solution/0900-0999/0917.Reverse%20Only%20Letters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy đảo chuỗi theo các quy tắc sau:</p>

<ul>
	<li>Mọi ký tự không phải chữ cái tiếng Anh giữ nguyên vị trí.</li>
	<li>Đảo thứ tự tất cả chữ cái tiếng Anh, dù là chữ thường hay chữ hoa.</li>
</ul>

<p>Trả về <code>s</code><em> sau khi đảo</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "ab-cd"
<strong>Đầu ra:</strong> "dc-ba"
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "a-bC-dEf-ghIj"
<strong>Đầu ra:</strong> "j-Ih-gfE-dCba"
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> s = "Test1ng-Leet=code-Q!"
<strong>Đầu ra:</strong> "Qedo1ct-eeLg=ntse-T!"
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> gồm các ký tự có giá trị ASCII trong khoảng <code>[33, 122]</code>.</li>
	<li><code>s</code> không chứa <code>&#39;\&quot;&#39;</code> hoặc <code>&#39;\\&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ đảo chữ cái, còn các ký tự khác giữ nguyên. Có thể gom các chữ cái rồi ghi ngược trở lại, nhưng dùng hai con trỏ sẽ xử lý trong một lượt. Bỏ qua ký tự không phải chữ cái từ hai đầu rồi hoán đổi. Vì chuỗi không thể thay đổi trực tiếp, trước tiên chuyển nó thành một mảng ký tự.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến đầu và cuối chuỗi. Khi $i < j$, ta liên tục dịch chuyển $i$ và $j$ cho đến khi cả hai cùng trỏ đến chữ cái tiếng Anh, rồi hoán đổi $s[i]$ và $s[j]$. Cuối cùng, trả về chuỗi.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseOnlyLetters(self, s: str) -> str:
        cs = list(s)
        i, j = 0, len(cs) - 1
        while i < j:
            while i < j and not cs[i].isalpha():
                i += 1
            while i < j and not cs[j].isalpha():
                j -= 1
            if i < j:
                cs[i], cs[j] = cs[j], cs[i]
                i, j = i + 1, j - 1
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String reverseOnlyLetters(String s) {
        char[] cs = s.toCharArray();
        int i = 0, j = cs.length - 1;
        while (i < j) {
            while (i < j && !Character.isLetter(cs[i])) {
                ++i;
            }
            while (i < j && !Character.isLetter(cs[j])) {
                --j;
            }
            if (i < j) {
                char t = cs[i];
                cs[i] = cs[j];
                cs[j] = t;
                ++i;
                --j;
            }
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reverseOnlyLetters(string s) {
        int i = 0, j = s.size() - 1;
        while (i < j) {
            while (i < j && !isalpha(s[i])) {
                ++i;
            }
            while (i < j && !isalpha(s[j])) {
                --j;
            }
            if (i < j) {
                swap(s[i++], s[j--]);
            }
        }
        return s;
    }
};
```

#### Go

```go
func reverseOnlyLetters(s string) string {
	cs := []rune(s)
	i, j := 0, len(s)-1
	for i < j {
		for i < j && !unicode.IsLetter(cs[i]) {
			i++
		}
		for i < j && !unicode.IsLetter(cs[j]) {
			j--
		}
		if i < j {
			cs[i], cs[j] = cs[j], cs[i]
			i++
			j--
		}
	}
	return string(cs)
}
```

#### TypeScript

```ts
function reverseOnlyLetters(s: string): string {
    const cs = [...s];
    let [i, j] = [0, cs.length - 1];
    while (i < j) {
        while (!/[a-zA-Z]/.test(cs[i]) && i < j) {
            i++;
        }
        while (!/[a-zA-Z]/.test(cs[j]) && i < j) {
            j--;
        }
        [cs[i], cs[j]] = [cs[j], cs[i]];
        i++;
        j--;
    }
    return cs.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn reverse_only_letters(s: String) -> String {
        let mut cs: Vec<char> = s.chars().collect();
        let n = cs.len();
        let mut l = 0;
        let mut r = n - 1;
        while l < r {
            if !cs[l].is_ascii_alphabetic() {
                l += 1;
            } else if !cs[r].is_ascii_alphabetic() {
                r -= 1;
            } else {
                cs.swap(l, r);
                l += 1;
                r -= 1;
            }
        }
        cs.iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
