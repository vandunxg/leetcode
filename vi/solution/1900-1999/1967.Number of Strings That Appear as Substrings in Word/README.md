---
comments: true
difficulty: Easy
rating: 1231
source: Weekly Contest 254 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [1967. Number of Strings That Appear as Substrings in Word](https://leetcode.com/problems/number-of-strings-that-appear-as-substrings-in-word)

[中文文档](/solution/1900-1999/1967.Number%20of%20Strings%20That%20Appear%20as%20Substrings%20in%20Word/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>patterns</code> và một chuỗi <code>word</code>, hãy trả về <em><strong>số lượng</strong> chuỗi trong </em><code>patterns</code><em> xuất hiện dưới dạng <strong>chuỗi con</strong> trong </em><code>word</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> patterns = [&quot;a&quot;,&quot;abc&quot;,&quot;bc&quot;,&quot;d&quot;], word = &quot;abc&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- &quot;a&quot; xuất hiện dưới dạng chuỗi con trong &quot;<u>a</u>bc&quot;.
- &quot;abc&quot; xuất hiện dưới dạng chuỗi con trong &quot;<u>abc</u>&quot;.
- &quot;bc&quot; xuất hiện dưới dạng chuỗi con trong &quot;a<u>bc</u>&quot;.
- &quot;d&quot; không xuất hiện dưới dạng chuỗi con trong &quot;abc&quot;.
Có 3 chuỗi trong mảng patterns xuất hiện dưới dạng chuỗi con trong chuỗi word.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> patterns = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;], word = &quot;aaaaabbbbb&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- &quot;a&quot; xuất hiện dưới dạng chuỗi con trong &quot;a<u>a</u>aaabbbbb&quot;.
- &quot;b&quot; xuất hiện dưới dạng chuỗi con trong &quot;aaaaabbbb<u>b</u>&quot;.
- &quot;c&quot; không xuất hiện dưới dạng chuỗi con trong &quot;aaaaabbbbb&quot;.
Có 2 chuỗi trong mảng patterns xuất hiện dưới dạng chuỗi con trong chuỗi word.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> patterns = [&quot;a&quot;,&quot;a&quot;,&quot;a&quot;], word = &quot;ab&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Mỗi chuỗi trong mảng patterns đều xuất hiện dưới dạng chuỗi con trong chuỗi word &quot;<u>a</u>b&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= patterns.length &lt;= 100</code></li>
	<li><code>1 &lt;= patterns[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= word.length &lt;= 100</code></li>
	<li><code>patterns[i]</code> và <code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì mảng patterns và chuỗi word đều ngắn, chỉ cần kiểm tra $p$ có phải là chuỗi con của $\textit{word}$ hay không rồi cộng các kết quả.

<!-- thinking:end -->

Duyệt từng chuỗi $p$ trong mảng $\textit{patterns}$ và kiểm tra xem nó có phải là chuỗi con của $\textit{word}$ hay không. Nếu có, tăng đáp án lên một.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n \times m)$, và độ phức tạp không gian là $O(1)$. Ở đây, $n$ và $m$ lần lượt là độ dài của $\textit{patterns}$ và $\textit{word}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfStrings(self, patterns: List[str], word: str) -> int:
        return sum(p in word for p in patterns)
```

#### Java

```java
class Solution {
    public int numOfStrings(String[] patterns, String word) {
        int ans = 0;
        for (String p : patterns) {
            if (word.contains(p)) {
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
    int numOfStrings(vector<string>& patterns, string word) {
        int ans = 0;
        for (auto& p : patterns) {
            ans += word.find(p) != string::npos;
        }
        return ans;
    }
};
```

#### Go

```go
func numOfStrings(patterns []string, word string) (ans int) {
	for _, p := range patterns {
		if strings.Contains(word, p) {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numOfStrings(patterns: string[], word: string): number {
    return patterns.filter(p => word.includes(p)).length;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_of_strings(patterns: Vec<String>, word: String) -> i32 {
        patterns.iter().filter(|p| word.contains(&**p)).count() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
