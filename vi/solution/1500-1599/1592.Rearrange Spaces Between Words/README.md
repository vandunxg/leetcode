---
comments: true
difficulty: Easy
rating: 1362
source: Weekly Contest 207 Q1
tags:
    - String
---

<!-- problem:start -->

# [1592. Rearrange Spaces Between Words](https://leetcode.com/problems/rearrange-spaces-between-words)

[中文文档](/solution/1500-1599/1592.Rearrange%20Spaces%20Between%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>text</code> gồm các từ và một số khoảng trắng. Mỗi từ gồm một hoặc nhiều chữ cái tiếng Anh viết thường và được ngăn cách bởi ít nhất một khoảng trắng. Đảm bảo <code>text</code> <strong>chứa ít nhất một từ</strong>.</p>

<p>Sắp xếp lại các khoảng trắng sao cho giữa mọi cặp từ kề nhau có số khoảng trắng <strong>bằng nhau</strong> và số đó được <strong>tối đa hóa</strong>. Nếu không thể phân phối đều tất cả khoảng trắng, hãy đặt <strong>các khoảng trắng dư ở cuối</strong>, nghĩa là chuỗi trả về phải có cùng độ dài với <code>text</code>.</p>

<p>Trả về <em>chuỗi sau khi sắp xếp lại các khoảng trắng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;  this   is  a sentence &quot;
<strong>Output:</strong> &quot;this   is   a   sentence&quot;
<strong>Giải thích:</strong> Có tổng cộng 9 khoảng trắng và 4 từ. Ta có thể chia đều 9 khoảng trắng giữa các từ: 9 / (4-1) = 3 khoảng trắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> text = &quot; practice   makes   perfect&quot;
<strong>Output:</strong> &quot;practice   makes   perfect &quot;
<strong>Giải thích:</strong> Có tổng cộng 7 khoảng trắng và 3 từ. 7 / (3-1) = 3 khoảng trắng và dư 1 khoảng trắng. Ta đặt khoảng trắng dư này ở cuối chuỗi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 100</code></li>
	<li><code>text</code> consists of lowercase English letters and <code>&#39; &#39;</code>.</li>
	<li><code>text</code> contains at least one word.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Phân phối đều các khoảng trắng ban đầu giữa các từ và đưa phần dư ra cuối chuỗi. Danh sách từ và số khoảng trắng đều có thể lấy trong một lần duyệt.
>
> Đếm khoảng trắng và dùng $split$ để tách các từ. Nếu chỉ có một từ, mọi khoảng trắng được đặt làm hậu tố; nếu không, $divmod$ cho biết độ dài khoảng cách và phần dư, ta nối các từ rồi thêm phần dư.

<!-- thinking:end -->

Trước hết, ta đếm số khoảng trắng trong chuỗi $\textit{text}$, gọi là $\textit{spaces}$. Sau đó, ta tách $\textit{text}$ theo khoảng trắng thành mảng chuỗi $\textit{words}$. Tiếp theo, ta tính số khoảng trắng cần chèn giữa các từ kề nhau và thực hiện nối chuỗi. Cuối cùng, ta thêm các khoảng trắng còn lại vào cuối.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{text}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reorderSpaces(self, text: str) -> str:
        spaces = text.count(" ")
        words = text.split()
        if len(words) == 1:
            return words[0] + " " * spaces
        cnt, mod = divmod(spaces, len(words) - 1)
        return (" " * cnt).join(words) + " " * mod
```

#### Java

```java
class Solution {
    public String reorderSpaces(String text) {
        int spaces = 0;
        for (char c : text.toCharArray()) {
            if (c == ' ') {
                ++spaces;
            }
        }
        String[] words = text.trim().split("\\s+");
        if (words.length == 1) {
            return words[0] + " ".repeat(spaces);
        }
        int cnt = spaces / (words.length - 1);
        int mod = spaces % (words.length - 1);
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < words.length; ++i) {
            sb.append(words[i]);
            if (i < words.length - 1) {
                sb.append(" ".repeat(cnt));
            }
        }
        sb.append(" ".repeat(mod));
        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reorderSpaces(string text) {
        int spaces = ranges::count(text, ' ');
        auto words = split(text);

        if (words.size() == 1) {
            return words[0] + string(spaces, ' ');
        }

        int cnt = spaces / (words.size() - 1);
        int mod = spaces % (words.size() - 1);

        string result = join(words, string(cnt, ' '));
        result += string(mod, ' ');

        return result;
    }

private:
    vector<string> split(const string& text) {
        vector<string> words;
        istringstream stream(text);
        string word;
        while (stream >> word) {
            words.push_back(word);
        }
        return words;
    }

    string join(const vector<string>& words, const string& separator) {
        ostringstream result;
        for (size_t i = 0; i < words.size(); ++i) {
            result << words[i];
            if (i < words.size() - 1) {
                result << separator;
            }
        }
        return result.str();
    }
};
```

#### Go

```go
func reorderSpaces(text string) string {
	cnt := strings.Count(text, " ")
	words := strings.Fields(text)
	m := len(words) - 1
	if m == 0 {
		return words[0] + strings.Repeat(" ", cnt)
	}
	return strings.Join(words, strings.Repeat(" ", cnt/m)) + strings.Repeat(" ", cnt%m)
}
```

#### TypeScript

```ts
function reorderSpaces(text: string): string {
    const spaces = (text.match(/ /g) || []).length;
    const words = text.split(/\s+/).filter(Boolean);
    if (words.length === 1) {
        return words[0] + ' '.repeat(spaces);
    }
    const cnt = Math.floor(spaces / (words.length - 1));
    const mod = spaces % (words.length - 1);
    const result = words.join(' '.repeat(cnt));
    return result + ' '.repeat(mod);
}
```

#### Rust

```rust
impl Solution {
    pub fn reorder_spaces(text: String) -> String {
        let spaces = text.chars().filter(|&c| c == ' ').count();
        let words: Vec<&str> = text.split_whitespace().collect();
        if words.len() == 1 {
            return format!("{}{}", words[0], " ".repeat(spaces));
        }
        let cnt = spaces / (words.len() - 1);
        let mod_spaces = spaces % (words.len() - 1);
        let result = words.join(&" ".repeat(cnt));
        result + &" ".repeat(mod_spaces)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
