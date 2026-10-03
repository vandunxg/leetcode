---
comments: true
difficulty: Easy
rating: 1260
source: Biweekly Contest 77 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2255. Count Prefixes of a Given String](https://leetcode.com/problems/count-prefixes-of-a-given-string)

[中文文档](/solution/2200-2299/2255.Count%20Prefixes%20of%20a%20Given%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> và một chuỗi <code>s</code>, trong đó <code>words[i]</code> và <code>s</code> chỉ gồm các <strong>chữ cái tiếng Anh viết thường</strong>.</p>

<p>Hãy trả về <em><strong>số lượng chuỗi</strong> trong</em> <code>words</code> <em>là <strong>tiền tố</strong> của</em> <code>s</code>.</p>

<p><strong>Tiền tố</strong> của một chuỗi là một chuỗi con xuất hiện ở đầu chuỗi. <b>Chuỗi con</b> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;ab&quot;,&quot;bc&quot;,&quot;abc&quot;], s = &quot;abc&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Các chuỗi trong words là tiền tố của s = &quot;abc&quot; gồm:
&quot;a&quot;, &quot;ab&quot; và &quot;abc&quot;.
Vì vậy, số lượng chuỗi trong words là tiền tố của s là 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;a&quot;], s = &quot;aa&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:
</strong>Cả hai chuỗi đều là tiền tố của s.
Lưu ý rằng cùng một chuỗi có thể xuất hiện nhiều lần trong words và phải được đếm mỗi lần.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length, s.length &lt;= 10</code></li>
	<li><code>words[i]</code> và <code>s</code> <strong>chỉ</strong> gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số từ là tiền tố của $s$. Có nhiều nhất $10^3$ từ và độ dài tối đa là $10$, nên chỉ cần kiểm tra tiền tố cho từng từ; không cần dùng trie.
>
> $\textit{startswith}$ trả về việc $s$ có bắt đầu bằng $w$ hay không. Khối lượng công việc tỉ lệ với số lượng từ nhân với $|s|$.

<!-- thinking:end -->

Ta duyệt trực tiếp qua mảng words, với mỗi chuỗi w, ta kiểm tra xem s có bắt đầu bằng w hay không. Nếu có, ta tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là độ dài của mảng words và chuỗi s. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPrefixes(self, words: List[str], s: str) -> int:
        return sum(s.startswith(w) for w in words)
```

#### Java

```java
class Solution {
    public int countPrefixes(String[] words, String s) {
        int ans = 0;
        for (String w : words) {
            if (s.startsWith(w)) {
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countPrefixes(vector<string>& words, string s) {
        int ans = 0;
        for (auto& w : words) {
            ans += s.starts_with(w);
        }
        return ans;
    }
};
```

#### Go

```go
func countPrefixes(words []string, s string) (ans int) {
	for _, w := range words {
		if strings.HasPrefix(s, w) {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countPrefixes(words: string[], s: string): number {
    return words.filter(w => s.startsWith(w)).length;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_prefixes(words: Vec<String>, s: String) -> i32 {
        words.iter().filter(|w| s.starts_with(w.as_str())).count() as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int CountPrefixes(string[] words, string s) {
        return words.Count(w => s.StartsWith(w));
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
