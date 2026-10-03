---
comments: true
difficulty: Easy
rating: 1471
source: Weekly Contest 264 Q1
tags:
    - String
---

<!-- problem:start -->

# [2047. Number of Valid Words in a Sentence](https://leetcode.com/problems/number-of-valid-words-in-a-sentence)

[中文文档](/solution/2000-2099/2047.Number%20of%20Valid%20Words%20in%20a%20Sentence/README.md)

## Mô tả

<!-- description:start -->

<p>Một câu chỉ gồm các chữ cái viết thường (từ <code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code>), chữ số (từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>), dấu gạch nối (<code>&#39;-&#39;</code>), dấu câu (<code>&#39;!&#39;</code>, <code>&#39;.&#39;</code> và <code>&#39;,&#39;</code>) và dấu cách (<code>&#39; &#39;</code>). Mỗi câu có thể được tách thành <strong>một hoặc nhiều token</strong>, các token được ngăn cách bởi một hoặc nhiều dấu cách <code>&#39; &#39;</code>.</p>

<p>Một token là từ hợp lệ nếu <strong>đồng thời thỏa mãn cả ba</strong> điều kiện sau:</p>

<ul>
	<li>Chỉ chứa các chữ cái viết thường, dấu gạch nối và/hoặc dấu câu (<strong>không</strong> chứa chữ số).</li>
	<li>Có <strong>nhiều nhất một</strong> dấu gạch nối <code>&#39;-&#39;</code>. Nếu có, dấu gạch nối <strong>phải</strong> được bao quanh bởi các chữ cái viết thường (<code>&quot;a-b&quot;</code> là hợp lệ, nhưng <code>&quot;-ab&quot;</code> và <code>&quot;ab-&quot;</code> không hợp lệ).</li>
	<li>Có <strong>nhiều nhất một</strong> dấu câu. Nếu có, dấu câu <strong>phải</strong> nằm ở <strong>cuối</strong> token (<code>&quot;ab,&quot;</code>, <code>&quot;cd!&quot;</code> và <code>&quot;.&quot;</code> là hợp lệ, nhưng <code>&quot;a!b&quot;</code> và <code>&quot;c.,&quot;</code> không hợp lệ).</li>
</ul>

<p>Một số ví dụ về từ hợp lệ: <code>&quot;a-b.&quot;</code>, <code>&quot;afad&quot;</code>, <code>&quot;ba-c&quot;</code>, <code>&quot;a!&quot;</code> và <code>&quot;!&quot;</code>.</p>

<p>Cho một chuỗi <code>sentence</code>, hãy trả về <em><strong>số lượng</strong> từ hợp lệ trong </em><code>sentence</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;<u>cat</u> <u>and</u>  <u>dog</u>&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các từ hợp lệ trong câu là &quot;cat&quot;, &quot;and&quot; và &quot;dog&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;!this  1-s b8d!&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có từ hợp lệ nào trong câu.
&quot;!this&quot; không hợp lệ vì bắt đầu bằng dấu câu.
&quot;1-s&quot; và &quot;b8d&quot; không hợp lệ vì chứa chữ số.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;<u>alice</u> <u>and</u>  <u>bob</u> <u>are</u> <u>playing</u> stone-game10&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các từ hợp lệ trong câu là &quot;alice&quot;, &quot;and&quot;, &quot;bob&quot;, &quot;are&quot; và &quot;playing&quot;.
&quot;stone-game10&quot; không hợp lệ vì chứa chữ số.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 1000</code></li>
	<li><code>sentence</code> chỉ chứa các chữ cái tiếng Anh viết thường, chữ số, <code>&#39; &#39;</code>, <code>&#39;-&#39;</code>, <code>&#39;!&#39;</code>, <code>&#39;.&#39;</code> và <code>&#39;,&#39;</code>.</li>
	<li>Sẽ có ít nhất&nbsp;<code>1</code> token.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi có nhiều nhất $1000$ ký tự. Sau khi tách, mỗi token không được chứa chữ số, dấu câu chỉ được xuất hiện ở cuối và có nhiều nhất một dấu gạch nối nằm giữa các chữ cái.
>
> Dùng một cờ để ghi nhận dấu gạch nối đã xuất hiện, duyệt các ký tự theo những quy tắc trên và đếm các token hợp lệ.

<!-- thinking:end -->

Trước tiên, ta tách câu thành các từ theo dấu cách, sau đó kiểm tra từng từ để xác định đó có phải là từ hợp lệ hay không.

Với mỗi từ, ta có thể dùng một biến boolean $\textit{st}$ để ghi nhận xem dấu gạch nối đã xuất hiện hay chưa, rồi duyệt qua từng ký tự trong từ và kiểm tra theo các quy tắc được mô tả trong đề bài.

Với mỗi ký tự $s[i]$, ta có các trường hợp sau:

- Nếu $s[i]$ là chữ số, thì $s$ không phải là từ hợp lệ và ta trả về $\text{false}$ ngay;
- Nếu $s[i]$ là dấu câu ('!', '.', ',') và $i < \text{len}(s) - 1$, thì $s$ không phải là từ hợp lệ và ta trả về $\text{false}$ ngay;
- Nếu $s[i]$ là dấu gạch nối, ta cần kiểm tra các điều kiện sau:
    - Dấu gạch nối chỉ được xuất hiện một lần;
    - Dấu gạch nối không được xuất hiện ở đầu hoặc cuối từ;
    - Hai phía của dấu gạch nối phải là chữ cái;
- Nếu $s[i]$ là chữ cái, ta không cần thực hiện thao tác nào.

Cuối cùng, ta đếm số từ hợp lệ trong câu.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của câu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countValidWords(self, sentence: str) -> int:
        def check(s: str) -> bool:
            st = False
            for i, c in enumerate(s):
                if c.isdigit() or (c in "!.," and i < len(s) - 1):
                    return False
                if c == "-":
                    if (
                        st
                        or i in (0, len(s) - 1)
                        or not s[i - 1].isalpha()
                        or not s[i + 1].isalpha()
                    ):
                        return False
                    st = True
            return True

        return sum(check(s) for s in sentence.split())
```

#### Java

```java
class Solution {
    public int countValidWords(String sentence) {
        int ans = 0;
        for (String s : sentence.split(" ")) {
            ans += check(s.toCharArray());
        }
        return ans;
    }

    private int check(char[] s) {
        if (s.length == 0) {
            return 0;
        }
        boolean st = false;
        for (int i = 0; i < s.length; ++i) {
            if (Character.isDigit(s[i])) {
                return 0;
            }
            if ((s[i] == '!' || s[i] == '.' || s[i] == ',') && i < s.length - 1) {
                return 0;
            }
            if (s[i] == '-') {
                if (st || i == 0 || i == s.length - 1) {
                    return 0;
                }
                if (!Character.isAlphabetic(s[i - 1]) || !Character.isAlphabetic(s[i + 1])) {
                    return 0;
                }
                st = true;
            }
        }
        return 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countValidWords(string sentence) {
        auto check = [](const string& s) -> int {
            bool st = false;
            for (int i = 0; i < s.length(); ++i) {
                if (isdigit(s[i])) {
                    return 0;
                }
                if ((s[i] == '!' || s[i] == '.' || s[i] == ',') && i < s.length() - 1) {
                    return 0;
                }
                if (s[i] == '-') {
                    if (st || i == 0 || i == s.length() - 1) {
                        return 0;
                    }
                    if (!isalpha(s[i - 1]) || !isalpha(s[i + 1])) {
                        return 0;
                    }
                    st = true;
                }
            }
            return 1;
        };

        int ans = 0;
        stringstream ss(sentence);
        string s;
        while (ss >> s) {
            ans += check(s);
        }
        return ans;
    }
};
```

#### Go

```go
func countValidWords(sentence string) (ans int) {
	check := func(s string) int {
		if len(s) == 0 {
			return 0
		}
		st := false
		for i, r := range s {
			if unicode.IsDigit(r) {
				return 0
			}
			if (r == '!' || r == '.' || r == ',') && i < len(s)-1 {
				return 0
			}
			if r == '-' {
				if st || i == 0 || i == len(s)-1 {
					return 0
				}
				if !unicode.IsLetter(rune(s[i-1])) || !unicode.IsLetter(rune(s[i+1])) {
					return 0
				}
				st = true
			}
		}
		return 1
	}
	for _, s := range strings.Fields(sentence) {
		ans += check(s)
	}
	return ans
}
```

#### TypeScript

```ts
function countValidWords(sentence: string): number {
    const check = (s: string): number => {
        if (s.length === 0) {
            return 0;
        }
        let st = false;
        for (let i = 0; i < s.length; ++i) {
            if (/\d/.test(s[i])) {
                return 0;
            }
            if (['!', '.', ','].includes(s[i]) && i < s.length - 1) {
                return 0;
            }
            if (s[i] === '-') {
                if (st || [0, s.length - 1].includes(i)) {
                    return 0;
                }
                if (!/[a-zA-Z]/.test(s[i - 1]) || !/[a-zA-Z]/.test(s[i + 1])) {
                    return 0;
                }
                st = true;
            }
        }
        return 1;
    };
    return sentence.split(/\s+/).reduce((acc, s) => acc + check(s), 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
