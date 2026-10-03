---
comments: true
difficulty: Easy
rating: 1187
source: Weekly Contest 243 Q1
tags:
    - String
---

<!-- problem:start -->

# [1880. Check if Word Equals Summation of Two Words](https://leetcode.com/problems/check-if-word-equals-summation-of-two-words)

[中文文档](/solution/1800-1899/1880.Check%20if%20Word%20Equals%20Summation%20of%20Two%20Words/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Giá trị chữ cái</strong> của một chữ cái là vị trí của nó trong bảng chữ cái, <strong>bắt đầu từ 0</strong> (tức là <code>&#39;a&#39; -&gt; 0</code>, <code>&#39;b&#39; -&gt; 1</code>, <code>&#39;c&#39; -&gt; 2</code>, v.v.).</p>

<p><strong>Giá trị số</strong> của một chuỗi gồm các chữ cái tiếng Anh viết thường <code>s</code> là kết quả <strong>nối</strong> <strong>giá trị chữ cái</strong> của từng chữ cái trong <code>s</code>, sau đó <strong>chuyển đổi</strong> kết quả thành một số nguyên.</p>

<ul>
	<li>Ví dụ, nếu <code>s = &quot;acb&quot;</code>, ta nối giá trị chữ cái của từng chữ cái và nhận được <code>&quot;021&quot;</code>. Sau khi chuyển đổi, ta được <code>21</code>.</li>
</ul>

<p>Cho ba chuỗi <code>firstWord</code>, <code>secondWord</code> và <code>targetWord</code>, mỗi chuỗi chỉ gồm các chữ cái tiếng Anh viết thường từ <code>&#39;a&#39;</code> đến <code>&#39;j&#39;</code> <strong>bao gồm cả hai đầu mút</strong>.</p>

<p>Trả về <code>true</code> <em>nếu <strong>tổng</strong> <strong>các giá trị số</strong> của </em><code>firstWord</code><em> và </em><code>secondWord</code><em> bằng <strong>giá trị số</strong> của </em><code>targetWord</code><em>, ngược lại trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> firstWord = &quot;acb&quot;, secondWord = &quot;cba&quot;, targetWord = &quot;cdb&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Giá trị số của firstWord là &quot;acb&quot; -&gt; &quot;021&quot; -&gt; 21.
Giá trị số của secondWord là &quot;cba&quot; -&gt; &quot;210&quot; -&gt; 210.
Giá trị số của targetWord là &quot;cdb&quot; -&gt; &quot;231&quot; -&gt; 231.
Ta trả về true vì 21 + 210 == 231.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> firstWord = &quot;aaa&quot;, secondWord = &quot;a&quot;, targetWord = &quot;aab&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Giá trị số của firstWord là &quot;aaa&quot; -&gt; &quot;000&quot; -&gt; 0.
Giá trị số của secondWord là &quot;a&quot; -&gt; &quot;0&quot; -&gt; 0.
Giá trị số của targetWord là &quot;aab&quot; -&gt; &quot;001&quot; -&gt; 1.
Ta trả về false vì 0 + 0 != 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> firstWord = &quot;aaa&quot;, secondWord = &quot;a&quot;, targetWord = &quot;aaaa&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Giá trị số của firstWord là &quot;aaa&quot; -&gt; &quot;000&quot; -&gt; 0.
Giá trị số của secondWord là &quot;a&quot; -&gt; &quot;0&quot; -&gt; 0.
Giá trị số của targetWord là &quot;aaaa&quot; -&gt; &quot;0000&quot; -&gt; 0.
Ta trả về true vì 0 + 0 == 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= firstWord.length, </code><code>secondWord.length, </code><code>targetWord.length &lt;= 8</code></li>
	<li><code>firstWord</code>, <code>secondWord</code> và <code>targetWord</code> chỉ gồm các chữ cái tiếng Anh viết thường từ <code>&#39;a&#39;</code> đến <code>&#39;j&#39;</code> <strong>bao gồm cả hai đầu mút</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Chuỗi sang Số

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái $a\ldots j$ tương ứng với các chữ số $0\ldots 9$; ta kiểm tra xem giá trị của hai từ có cộng lại bằng giá trị của từ thứ ba hay không. Giá trị của một từ là phép nối các chữ số tương ứng.
>
> $f$ cộng dồn $ord(c)-ord('a')$ theo cơ số $10$. So sánh $f(first)+f(second)$ với $f(target)$.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{f}(s)$ để tính giá trị số của chuỗi $s$. Với mỗi ký tự $c$ trong chuỗi $s$, ta chuyển nó thành số tương ứng $x$, sau đó lần lượt nối $x$ vào kết quả và cuối cùng chuyển kết quả thành số nguyên.

Cuối cùng, ta chỉ cần kiểm tra xem $\textit{f}(\textit{firstWord}) + \textit{f}(\textit{secondWord})$ có bằng $\textit{f}(\textit{targetWord})$ hay không.

Độ phức tạp thời gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả chuỗi trong bài toán. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isSumEqual(self, firstWord: str, secondWord: str, targetWord: str) -> bool:
        def f(s: str) -> int:
            ans, a = 0, ord("a")
            for c in map(ord, s):
                x = c - a
                ans = ans * 10 + x
            return ans

        return f(firstWord) + f(secondWord) == f(targetWord)
```

#### Java

```java
class Solution {
    public boolean isSumEqual(String firstWord, String secondWord, String targetWord) {
        return f(firstWord) + f(secondWord) == f(targetWord);
    }

    private int f(String s) {
        int ans = 0;
        for (char c : s.toCharArray()) {
            ans = ans * 10 + (c - 'a');
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isSumEqual(string firstWord, string secondWord, string targetWord) {
        auto f = [](string& s) -> int {
            int ans = 0;
            for (char c : s) {
                ans = ans * 10 + (c - 'a');
            }
            return ans;
        };
        return f(firstWord) + f(secondWord) == f(targetWord);
    }
};
```

#### Go

```go
func isSumEqual(firstWord string, secondWord string, targetWord string) bool {
	f := func(s string) (ans int) {
		for _, c := range s {
			ans = ans*10 + int(c-'a')
		}
		return
	}
	return f(firstWord)+f(secondWord) == f(targetWord)
}
```

#### TypeScript

```ts
function isSumEqual(firstWord: string, secondWord: string, targetWord: string): boolean {
    const f = (s: string): number => {
        let ans = 0;
        for (const c of s) {
            ans = ans * 10 + c.charCodeAt(0) - 97;
        }
        return ans;
    };
    return f(firstWord) + f(secondWord) == f(targetWord);
}
```

#### Rust

```rust
impl Solution {
    pub fn is_sum_equal(first_word: String, second_word: String, target_word: String) -> bool {
        fn f(s: &str) -> i64 {
            let mut ans = 0;
            let a = 'a' as i64;
            for c in s.chars() {
                let x = c as i64 - a;
                ans = ans * 10 + x;
            }
            ans
        }
        f(&first_word) + f(&second_word) == f(&target_word)
    }
}
```

#### JavaScript

```js
/**
 * @param {string} firstWord
 * @param {string} secondWord
 * @param {string} targetWord
 * @return {boolean}
 */
var isSumEqual = function (firstWord, secondWord, targetWord) {
    const f = s => {
        let ans = 0;
        for (const c of s) {
            ans = ans * 10 + c.charCodeAt(0) - 97;
        }
        return ans;
    };
    return f(firstWord) + f(secondWord) == f(targetWord);
};
```

#### C

```c
int f(const char* s) {
    int ans = 0;
    while (*s) {
        ans = ans * 10 + (*s - 'a');
        s++;
    }
    return ans;
}

bool isSumEqual(char* firstWord, char* secondWord, char* targetWord) {
    return f(firstWord) + f(secondWord) == f(targetWord);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
