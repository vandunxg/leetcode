---
comments: true
difficulty: Easy
rating: 1161
source: Weekly Contest 294 Q1
tags:
    - String
---

<!-- problem:start -->

# [2278. Percentage of Letter in String](https://leetcode.com/problems/percentage-of-letter-in-string)

[中文文档](/solution/2200-2299/2278.Percentage%20of%20Letter%20in%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một ký tự <code>letter</code>, hãy trả về <em><strong>phần trăm</strong> số ký tự trong </em><code>s</code><em> bằng </em><code>letter</code><em>, được <strong>làm tròn xuống</strong> đến số phần trăm nguyên gần nhất.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;foobar&quot;, letter = &quot;o&quot;
<strong>Đầu ra:</strong> 33
<strong>Giải thích:</strong>
Phần trăm số ký tự trong s bằng ký tự &#39;o&#39; là 2 / 6 * 100% = 33% khi làm tròn xuống, nên ta trả về 33.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;jjjj&quot;, letter = &quot;k&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Phần trăm số ký tự trong s bằng ký tự &#39;k&#39; là 0%, nên ta trả về 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>letter</code> là một chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính phần trăm làm tròn xuống của một ký tự trong $s$. Độ dài tối đa là $100$, nên chỉ cần đếm và chia nguyên.
>
> $s.\textit{count}(\textit{letter})\times 100 // |s|$ giúp tránh dùng số thực.

<!-- thinking:end -->

Ta có thể duyệt qua chuỗi $\textit{s}$ và đếm số ký tự bằng $\textit{letter}$. Sau đó, ta tính phần trăm bằng công thức $\textit{count} \times 100 \, / \, \textit{len}(\textit{s})$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def percentageLetter(self, s: str, letter: str) -> int:
        return s.count(letter) * 100 // len(s)
```

#### Java

```java
class Solution {
    public int percentageLetter(String s, char letter) {
        int cnt = 0;
        for (char c : s.toCharArray()) {
            if (c == letter) {
                ++cnt;
            }
        }
        return cnt * 100 / s.length();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int percentageLetter(string s, char letter) {
        return 100 * ranges::count(s, letter) / s.size();
    }
};
```

#### Go

```go
func percentageLetter(s string, letter byte) int {
	return strings.Count(s, string(letter)) * 100 / len(s)
}
```

#### TypeScript

```ts
function percentageLetter(s: string, letter: string): number {
    const count = s.split('').filter(c => c === letter).length;
    return Math.floor((100 * count) / s.length);
}
```

#### Rust

```rust
impl Solution {
    pub fn percentage_letter(s: String, letter: char) -> i32 {
        let count = s.chars().filter(|&c| c == letter).count();
        (100 * count as i32 / s.len() as i32) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
