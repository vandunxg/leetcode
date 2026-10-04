---
comments: true
difficulty: Easy
rating: 1178
source: Weekly Contest 336 Q1
tags:
    - Array
    - String
    - Counting
---

<!-- problem:start -->

# [2586. Count the Number of Vowel Strings in Range](https://leetcode.com/problems/count-the-number-of-vowel-strings-in-range)

[中文文档](/solution/2500-2599/2586.Count%20the%20Number%20of%20Vowel%20Strings%20in%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>words</code> được đánh chỉ số từ <strong>0</strong> và hai số nguyên <code>left</code> và <code>right</code>.</p>

<p>Một chuỗi được gọi là <strong>chuỗi nguyên âm</strong> nếu bắt đầu bằng một ký tự nguyên âm và kết thúc bằng một ký tự nguyên âm, trong đó các ký tự nguyên âm là <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</p>

<p>Hãy trả về <em>số lượng chuỗi nguyên âm </em><code>words[i]</code><em> với </em><code>i</code><em> thuộc đoạn đóng </em><code>[left, right]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;are&quot;,&quot;amy&quot;,&quot;u&quot;], left = 0, right = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- &quot;are&quot; là một chuỗi nguyên âm vì bắt đầu bằng &#39;a&#39; và kết thúc bằng &#39;e&#39;.
- &quot;amy&quot; không phải là một chuỗi nguyên âm vì không kết thúc bằng một nguyên âm.
- &quot;u&quot; là một chuỗi nguyên âm vì bắt đầu bằng &#39;u&#39; và kết thúc bằng &#39;u&#39;.
Số lượng chuỗi nguyên âm trong đoạn được đề cập là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;hey&quot;,&quot;aeo&quot;,&quot;mu&quot;,&quot;ooo&quot;,&quot;artro&quot;], left = 1, right = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- &quot;aeo&quot; là một chuỗi nguyên âm vì bắt đầu bằng &#39;a&#39; và kết thúc bằng &#39;o&#39;.
- &quot;mu&quot; không phải là một chuỗi nguyên âm vì không bắt đầu bằng một nguyên âm.
- &quot;ooo&quot; là một chuỗi nguyên âm vì bắt đầu bằng &#39;o&#39; và kết thúc bằng &#39;o&#39;.
- &quot;artro&quot; là một chuỗi nguyên âm vì bắt đầu bằng &#39;a&#39; và kết thúc bằng &#39;o&#39;.
Số lượng chuỗi nguyên âm trong đoạn được đề cập là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= left &lt;= right &lt; words.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Hãy đếm các chuỗi có chỉ số nằm trong $[\textit{left},\textit{right}]$ và bắt đầu, kết thúc bằng một nguyên âm. Đoạn cần xét có độ dài $O(n)$, nên chỉ cần kiểm tra từng chuỗi trong đoạn; không cần dùng tổng tiền tố.

<!-- thinking:end -->

Ta chỉ cần duyệt qua các chuỗi trong đoạn $[left,.. right]$ và kiểm tra xem chuỗi có bắt đầu và kết thúc bằng một nguyên âm hay không. Nếu có, tăng đáp án lên một.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(1)$. Trong đó $m = right - left + 1$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def vowelStrings(self, words: List[str], left: int, right: int) -> int:
        return sum(
            w[0] in 'aeiou' and w[-1] in 'aeiou' for w in words[left : right + 1]
        )
```

#### Java

```java
class Solution {
    public int vowelStrings(String[] words, int left, int right) {
        int ans = 0;
        for (int i = left; i <= right; ++i) {
            var w = words[i];
            if (check(w.charAt(0)) && check(w.charAt(w.length() - 1))) {
                ++ans;
            }
        }
        return ans;
    }

    private boolean check(char c) {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }
}
```

#### C++

```cpp
class Solution {
public:
    int vowelStrings(vector<string>& words, int left, int right) {
        auto check = [](char c) -> bool {
            return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
        };
        int ans = 0;
        for (int i = left; i <= right; ++i) {
            auto w = words[i];
            ans += check(w[0]) && check(w[w.size() - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func vowelStrings(words []string, left int, right int) (ans int) {
	check := func(c byte) bool {
		return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u'
	}
	for _, w := range words[left : right+1] {
		if check(w[0]) && check(w[len(w)-1]) {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function vowelStrings(words: string[], left: number, right: number): number {
    let ans = 0;
    const check: string[] = ['a', 'e', 'i', 'o', 'u'];
    for (let i = left; i <= right; ++i) {
        const w = words[i];
        if (check.includes(w[0]) && check.includes(w.at(-1))) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn vowel_strings(words: Vec<String>, left: i32, right: i32) -> i32 {
        let check =
            |c: u8| -> bool { c == b'a' || c == b'e' || c == b'i' || c == b'o' || c == b'u' };

        let mut ans = 0;
        for i in left..=right {
            let w = words[i as usize].as_bytes();
            if check(w[0]) && check(w[w.len() - 1]) {
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
