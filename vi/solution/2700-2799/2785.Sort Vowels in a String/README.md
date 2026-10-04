---
comments: true
difficulty: Medium
rating: 1266
source: Biweekly Contest 109 Q2
tags:
    - String
    - Sorting
---

<!-- problem:start -->

# [2785. Sort Vowels in a String](https://leetcode.com/problems/sort-vowels-in-a-string)

[中文文档](/solution/2700-2799/2785.Sort%20Vowels%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>s</code>, hãy <strong>hoán vị</strong> <code>s</code> để nhận được chuỗi mới <code>t</code> sao cho:</p>

<ul>
	<li>Mọi phụ âm giữ nguyên vị trí ban đầu. Cụ thể hơn, nếu có một chỉ số <code>i</code> với <code>0 &lt;= i &lt; s.length</code> sao cho <code>s[i]</code> là một phụ âm, thì <code>t[i] = s[i]</code>.</li>
	<li>Các nguyên âm phải được sắp xếp theo thứ tự <strong>không giảm</strong> của giá trị <strong>ASCII</strong>. Cụ thể hơn, với các cặp chỉ số <code>i</code>, <code>j</code> thỏa mãn <code>0 &lt;= i &lt; j &lt; s.length</code> và <code>s[i]</code>, <code>s[j]</code> là nguyên âm, thì <code>t[i]</code> không được có giá trị ASCII lớn hơn <code>t[j]</code>.</li>
</ul>

<p>Trả về <em>chuỗi kết quả</em>.</p>

<p>Các nguyên âm là <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>, có thể xuất hiện dưới dạng chữ thường hoặc chữ hoa. Phụ âm gồm mọi chữ cái không phải nguyên âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;lEetcOde&quot;
<strong>Đầu ra:</strong> &quot;lEOtcede&quot;
<strong>Giải thích:</strong> &#39;E&#39;, &#39;O&#39; và &#39;e&#39; là các nguyên âm trong s; &#39;l&#39;, &#39;t&#39;, &#39;c&#39; và &#39;d&#39; đều là phụ âm. Các nguyên âm được sắp xếp theo giá trị ASCII, còn các phụ âm giữ nguyên vị trí.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;lYmpH&quot;
<strong>Đầu ra:</strong> &quot;lYmpH&quot;
<strong>Giải thích:</strong> s không có nguyên âm nào (tất cả ký tự đều là phụ âm), nên ta trả về &quot;lYmpH&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái trong bảng chữ cái tiếng Anh, ở dạng <strong>chữ hoa và chữ thường</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các nguyên âm phải được sắp xếp theo ASCII, còn các phụ âm phải giữ nguyên vị trí. Nếu sắp xếp cả chuỗi thì các phụ âm sẽ bị di chuyển.
>
> Ta tách các nguyên âm, sắp xếp chúng, rồi ghi lại vào các vị trí nguyên âm từ trái sang phải.

<!-- thinking:end -->

Trước hết, ta lưu tất cả nguyên âm trong chuỗi vào một mảng hoặc danh sách $vs$, sau đó sắp xếp $vs$.

Tiếp theo, ta duyệt chuỗi $s$ và giữ nguyên các phụ âm. Nếu gặp một nguyên âm, ta lần lượt thay thế nó bằng các ký tự trong mảng $vs$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortVowels(self, s: str) -> str:
        vs = [c for c in s if c.lower() in "aeiou"]
        vs.sort()
        cs = list(s)
        j = 0
        for i, c in enumerate(cs):
            if c.lower() in "aeiou":
                cs[i] = vs[j]
                j += 1
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String sortVowels(String s) {
        List<Character> vs = new ArrayList<>();
        char[] cs = s.toCharArray();
        for (char c : cs) {
            char d = Character.toLowerCase(c);
            if (d == 'a' || d == 'e' || d == 'i' || d == 'o' || d == 'u') {
                vs.add(c);
            }
        }
        Collections.sort(vs);
        for (int i = 0, j = 0; i < cs.length; ++i) {
            char d = Character.toLowerCase(cs[i]);
            if (d == 'a' || d == 'e' || d == 'i' || d == 'o' || d == 'u') {
                cs[i] = vs.get(j++);
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
    string sortVowels(string s) {
        string vs;
        for (auto c : s) {
            char d = tolower(c);
            if (d == 'a' || d == 'e' || d == 'i' || d == 'o' || d == 'u') {
                vs.push_back(c);
            }
        }
        ranges::sort(vs);
        for (int i = 0, j = 0; i < s.size(); ++i) {
            char d = tolower(s[i]);
            if (d == 'a' || d == 'e' || d == 'i' || d == 'o' || d == 'u') {
                s[i] = vs[j++];
            }
        }
        return s;
    }
};
```

#### Go

```go
func sortVowels(s string) string {
	cs := []byte(s)
	vs := []byte{}
	for _, c := range cs {
		d := c | 32
		if d == 'a' || d == 'e' || d == 'i' || d == 'o' || d == 'u' {
			vs = append(vs, c)
		}
	}
	sort.Slice(vs, func(i, j int) bool { return vs[i] < vs[j] })
	j := 0
	for i, c := range cs {
		d := c | 32
		if d == 'a' || d == 'e' || d == 'i' || d == 'o' || d == 'u' {
			cs[i] = vs[j]
			j++
		}
	}
	return string(cs)
}
```

#### TypeScript

```ts
function sortVowels(s: string): string {
    const vowels = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'];
    const vs = s
        .split('')
        .filter(c => vowels.includes(c))
        .sort();
    const ans: string[] = [];
    let j = 0;
    for (const c of s) {
        ans.push(vowels.includes(c) ? vs[j++] : c);
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn sort_vowels(s: String) -> String {
        fn is_vowel(c: char) -> bool {
            matches!(c.to_ascii_lowercase(), 'a' | 'e' | 'i' | 'o' | 'u')
        }

        let mut vs: Vec<char> = s.chars().filter(|&c| is_vowel(c)).collect();
        vs.sort_unstable();

        let mut cs: Vec<char> = s.chars().collect();
        let mut j = 0;

        for (i, c) in cs.clone().into_iter().enumerate() {
            if is_vowel(c) {
                cs[i] = vs[j];
                j += 1;
            }
        }

        cs.into_iter().collect()
    }
}
```

#### C#

```cs
public class Solution {
    public string SortVowels(string s) {
        List<char> vs = new List<char>();
        char[] cs = s.ToCharArray();
        foreach (char c in cs) {
            if (IsVowel(c)) {
                vs.Add(c);
            }
        }
        vs.Sort();
        for (int i = 0, j = 0; i < cs.Length; ++i) {
            if (IsVowel(cs[i])) {
                cs[i] = vs[j++];
            }
        }
        return new string(cs);
    }

    public bool IsVowel(char c) {
        c = char.ToLower(c);
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
