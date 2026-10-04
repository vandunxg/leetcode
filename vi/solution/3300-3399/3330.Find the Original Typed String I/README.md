---
comments: true
difficulty: Easy
rating: 1338
source: Biweekly Contest 142 Q1
tags:
    - String
---

<!-- problem:start -->

# [3330. Find the Original Typed String I](https://leetcode.com/problems/find-the-original-typed-string-i)

[Tài liệu tiếng Trung](/solution/3300-3399/3330.Find%20the%20Original%20Typed%20String%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Alice đang cố gắng gõ một chuỗi cụ thể trên máy tính. Tuy nhiên, cô ấy khá vụng về và <strong>có thể</strong> nhấn một phím quá lâu, khiến một ký tự được gõ <strong>nhiều</strong> lần.</p>

<p>Mặc dù Alice đã cố gắng tập trung khi gõ, cô ấy biết rằng việc này vẫn có thể xảy ra <strong>nhiều nhất</strong> <em>một</em> lần.</p>

<p>Bạn được cho một chuỗi <code>word</code>, biểu thị kết quả <strong>cuối cùng</strong> hiển thị trên màn hình của Alice.</p>

<p>Hãy trả về tổng số chuỗi ban đầu <em>có thể</em> là chuỗi Alice <em>đã định</em> gõ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;abbcccc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi có thể là: <code>&quot;abbcccc&quot;</code>, <code>&quot;abbccc&quot;</code>, <code>&quot;abbcc&quot;</code>, <code>&quot;abbc&quot;</code> và <code>&quot;abcccc&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;abcd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi duy nhất có thể là <code>&quot;abcd&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aaaa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= word.length &lt;= 100</code></li>
    <li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Alice chỉ nhấn giữ nhiều nhất một phím, nên chuỗi ban đầu hoặc là chính $\textit{word}$ hoặc là $\textit{word}$ với một đoạn liên tiếp bị rút ngắn đi một ký tự.
>
> Với $n \le 100$, ta chỉ cần xét các cặp ký tự bằng nhau liền kề. Mỗi cặp như vậy là một vị trí mà việc nhấn giữ lâu có thể đã ngắn hơn.
>
> Đáp án bằng một cộng với số cặp ký tự bằng nhau liền kề.

<!-- thinking:end -->

Theo mô tả của đề bài, nếu mọi ký tự liền kề đều khác nhau thì chỉ có 1 chuỗi ban đầu có thể có. Nếu có 1 cặp ký tự liền kề giống nhau, chẳng hạn như "abbc", thì có 2 chuỗi ban đầu có thể có: "abc" và "abbc".

Tương tự, nếu có $k$ cặp ký tự liền kề giống nhau thì có $k + 1$ chuỗi ban đầu có thể có.

Vì vậy, ta chỉ cần duyệt chuỗi, đếm số cặp ký tự liền kề giống nhau rồi cộng thêm 1.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def possibleStringCount(self, word: str) -> int:
        return 1 + sum(x == y for x, y in pairwise(word))
```

#### Java

```java
class Solution {
    public int possibleStringCount(String word) {
        int f = 1;
        for (int i = 1; i < word.length(); ++i) {
            if (word.charAt(i) == word.charAt(i - 1)) {
                ++f;
            }
        }
        return f;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int possibleStringCount(string word) {
        int f = 1;
        for (int i = 1; i < word.size(); ++i) {
            f += word[i] == word[i - 1];
        }
        return f;
    }
};
```

#### Go

```go
func possibleStringCount(word string) int {
    f := 1
    for i := 1; i < len(word); i++ {
        if word[i] == word[i-1] {
            f++
        }
    }
    return f
}
```

#### TypeScript

```ts
function possibleStringCount(word: string): number {
    let f = 1;
    for (let i = 1; i < word.length; ++i) {
        f += word[i] === word[i - 1] ? 1 : 0;
    }
    return f;
}
```

#### Rust

```rust
impl Solution {
    pub fn possible_string_count(word: String) -> i32 {
        1 + word.as_bytes().windows(2).filter(|w| w[0] == w[1]).count() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
