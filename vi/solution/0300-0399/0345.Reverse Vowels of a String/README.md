---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [345. Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string)

[中文文档](/solution/0300-0399/0345.Reverse%20Vowels%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy đảo ngược thứ tự tất cả nguyên âm trong chuỗi và trả về chuỗi đó.</p>

<p>Các nguyên âm gồm <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>; chúng có thể xuất hiện nhiều lần và ở cả dạng chữ thường lẫn chữ hoa.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;IceCreAm&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;AceCreIm&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các nguyên âm trong <code>s</code> là <code>[&#39;I&#39;, &#39;e&#39;, &#39;e&#39;, &#39;A&#39;]</code>. Sau khi đảo ngược thứ tự nguyên âm, s trở thành <code>&quot;AceCreIm&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leetcode&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;leotcede&quot;</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>s</code> gồm các ký tự <strong>ASCII có thể in được</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two pointers

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ đảo vị trí các nguyên âm, còn phụ âm giữ nguyên. Nếu tách nguyên âm ra trước thì cần thêm bộ nhớ. Có thể dùng cách hoán đổi bằng two pointers, chỉ áp dụng cho nguyên âm.
>
> Bỏ qua các ký tự không phải nguyên âm ở hai đầu, hoán đổi khi $i<j$ rồi thu hẹp khoảng. Một tập nguyên âm nhỏ có thể chứa cả chữ thường lẫn chữ hoa.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$, ban đầu lần lượt trỏ đến đầu và cuối chuỗi.

Trong mỗi vòng lặp, ta kiểm tra ký tự tại $i$ có phải nguyên âm không. Nếu không, tăng $i$. Tương tự, nếu ký tự tại $j$ không phải nguyên âm thì giảm $j$. Nếu lúc này $i < j$, cả hai ký tự tại $i$ và $j$ đều là nguyên âm, nên ta hoán đổi chúng. Sau đó tăng $i$ và giảm $j$. Lặp lại cho đến khi $i \ge j$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là kích thước của tập ký tự.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        vowels = "aeiouAEIOU"
        i, j = 0, len(s) - 1
        cs = list(s)
        while i < j:
            while i < j and cs[i] not in vowels:
                i += 1
            while i < j and cs[j] not in vowels:
                j -= 1
            if i < j:
                cs[i], cs[j] = cs[j], cs[i]
                i, j = i + 1, j - 1
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String reverseVowels(String s) {
        boolean[] vowels = new boolean[128];
        for (char c : "aeiouAEIOU".toCharArray()) {
            vowels[c] = true;
        }
        char[] cs = s.toCharArray();
        int i = 0, j = cs.length - 1;
        while (i < j) {
            while (i < j && !vowels[cs[i]]) {
                ++i;
            }
            while (i < j && !vowels[cs[j]]) {
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
        return String.valueOf(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reverseVowels(string s) {
        bool vowels[128];
        memset(vowels, false, sizeof(vowels));
        for (char c : "aeiouAEIOU") {
            vowels[c] = true;
        }
        int i = 0, j = s.size() - 1;
        while (i < j) {
            while (i < j && !vowels[s[i]]) {
                ++i;
            }
            while (i < j && !vowels[s[j]]) {
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
func reverseVowels(s string) string {
	vowels := [128]bool{}
	for _, c := range "aeiouAEIOU" {
		vowels[c] = true
	}
	cs := []byte(s)
	i, j := 0, len(cs)-1
	for i < j {
		for i < j && !vowels[cs[i]] {
			i++
		}
		for i < j && !vowels[cs[j]] {
			j--
		}
		if i < j {
			cs[i], cs[j] = cs[j], cs[i]
			i, j = i+1, j-1
		}
	}
	return string(cs)
}
```

#### TypeScript

```ts
function reverseVowels(s: string): string {
    const vowels = new Set(['a', 'e', 'i', 'o', 'u']);
    const cs = s.split('');
    for (let i = 0, j = cs.length - 1; i < j; ++i, --j) {
        while (i < j && !vowels.has(cs[i].toLowerCase())) {
            ++i;
        }
        while (i < j && !vowels.has(cs[j].toLowerCase())) {
            --j;
        }
        [cs[i], cs[j]] = [cs[j], cs[i]];
    }
    return cs.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn reverse_vowels(s: String) -> String {
        let vowel = String::from("aeiouAEIOU");
        let mut data: Vec<char> = s.chars().collect();
        let (mut i, mut j) = (0, s.len() - 1);
        while i < j {
            while i < j && !vowel.contains(data[i]) {
                i += 1;
            }
            while i < j && !vowel.contains(data[j]) {
                j -= 1;
            }
            if i < j {
                data.swap(i, j);
                i += 1;
                j -= 1;
            }
        }
        data.iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
