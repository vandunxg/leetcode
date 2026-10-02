---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [520. Detect Capital](https://leetcode.com/problems/detect-capital)

[中文文档](/solution/0500-0599/0520.Detect%20Capital/README.md)

## Mô tả

<!-- description:start -->

<p>Việc viết hoa trong một từ được xem là đúng nếu thuộc một trong các trường hợp sau:</p>

<ul>
	<li>Tất cả chữ cái trong từ đều viết hoa, như <code>&quot;USA&quot;</code>.</li>
	<li>Không có chữ cái nào trong từ viết hoa, như <code>&quot;leetcode&quot;</code>.</li>
	<li>Chỉ chữ cái đầu tiên trong từ viết hoa, như <code>&quot;Google&quot;</code>.</li>
</ul>

<p>Cho chuỗi <code>word</code>, hãy trả về <code>true</code> nếu cách viết hoa trong từ là đúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> word = "USA"
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> word = "FlaG"
<strong>Đầu ra:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 100</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường và viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm số chữ cái viết hoa

<!-- thinking:start -->

> **Tư duy**
>
> Cách viết hoa hợp lệ là viết thường toàn bộ, viết hoa toàn bộ hoặc chỉ viết hoa chữ cái đầu tiên. Chỉ cần duyệt một lượt và đếm chữ cái viết hoa để phân biệt ba trường hợp này.
>
> Số lượng chữ cái viết hoa phải bằng $0$, bằng độ dài chuỗi, hoặc bằng $1$ và chữ cái đầu tiên được viết hoa. Không cần tạo bản sao hay dùng regular expression.

<!-- thinking:end -->

Ta có thể đếm số chữ cái viết hoa trong chuỗi, rồi dựa vào số lượng đó để xác định chuỗi có thỏa mãn yêu cầu của bài toán hay không.

- Nếu số chữ cái viết hoa bằng 0 hoặc bằng độ dài chuỗi, trả về `true`.
- Nếu số chữ cái viết hoa bằng 1 và chữ cái đầu tiên được viết hoa, trả về `true`.
- Nếu không thuộc các trường hợp trên, trả về `false`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi `word`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def detectCapitalUse(self, word: str) -> bool:
        cnt = sum(c.isupper() for c in word)
        return cnt == 0 or cnt == len(word) or (cnt == 1 and word[0].isupper())
```

#### Java

```java
class Solution {
    public boolean detectCapitalUse(String word) {
        int cnt = 0;
        for (char c : word.toCharArray()) {
            if (Character.isUpperCase(c)) {
                ++cnt;
            }
        }
        return cnt == 0 || cnt == word.length()
            || (cnt == 1 && Character.isUpperCase(word.charAt(0)));
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool detectCapitalUse(string word) {
        int cnt = count_if(word.begin(), word.end(), [](char c) { return isupper(c); });
        return cnt == 0 || cnt == word.length() || (cnt == 1 && isupper(word[0]));
    }
};
```

#### Go

```go
func detectCapitalUse(word string) bool {
	cnt := 0
	for _, c := range word {
		if unicode.IsUpper(c) {
			cnt++
		}
	}
	return cnt == 0 || cnt == len(word) || (cnt == 1 && unicode.IsUpper(rune(word[0])))
}
```

#### TypeScript

```ts
function detectCapitalUse(word: string): boolean {
    const cnt = word.split('').reduce((acc, c) => acc + (c === c.toUpperCase() ? 1 : 0), 0);
    return cnt === 0 || cnt === word.length || (cnt === 1 && word[0] === word[0].toUpperCase());
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
