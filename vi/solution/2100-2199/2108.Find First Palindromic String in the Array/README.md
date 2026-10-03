---
comments: true
difficulty: Easy
rating: 1215
source: Weekly Contest 272 Q1
tags:
    - Array
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2108. Find First Palindromic String in the Array](https://leetcode.com/problems/find-first-palindromic-string-in-the-array)

[中文文档](/solution/2100-2199/2108.Find%20First%20Palindromic%20String%20in%20the%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các chuỗi <code>words</code>, hãy trả về <em>chuỗi <strong>đối xứng</strong> đầu tiên trong mảng</em>. Nếu không có chuỗi nào như vậy, hãy trả về <em><strong>chuỗi rỗng</strong> </em><code>&quot;&quot;</code>.</p>

<p>Một chuỗi là <strong>đối xứng</strong> nếu đọc xuôi hay đọc ngược đều giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abc&quot;,&quot;car&quot;,&quot;ada&quot;,&quot;racecar&quot;,&quot;cool&quot;]
<strong>Đầu ra:</strong> &quot;ada&quot;
<strong>Giải thích:</strong> Chuỗi đối xứng đầu tiên là &quot;ada&quot;.
Lưu ý rằng &quot;racecar&quot; cũng là chuỗi đối xứng, nhưng không phải chuỗi đầu tiên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;notapalindrome&quot;,&quot;racecar&quot;]
<strong>Đầu ra:</strong> &quot;racecar&quot;
<strong>Giải thích:</strong> Chuỗi đối xứng đầu tiên và duy nhất là &quot;racecar&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;def&quot;,&quot;ghi&quot;]
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không có chuỗi đối xứng nào, nên trả về chuỗi rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= words.length &lt;= 100</code></li>
    <li><code>1 &lt;= words[i].length &lt;= 100</code></li>
    <li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm chuỗi đối xứng đầu tiên trong mảng. Tổng độ dài các chuỗi không lớn, nên chỉ cần kiểm tra lần lượt từ trái sang phải.
>
> Một từ là chuỗi đối xứng nếu nó bằng chuỗi đảo ngược của chính nó, hoặc nếu hai con trỏ bắt đầu từ hai đầu luôn trỏ đến các ký tự giống nhau.
>
> Ta duyệt $\textit{words}$ và trả về $w$ đầu tiên thỏa mãn $w=w[::-1]$, hoặc trả về chuỗi rỗng nếu không có chuỗi nào.

<!-- thinking:end -->

Ta duyệt qua mảng `words`; với mỗi chuỗi `w`, ta kiểm tra xem nó có đối xứng hay không. Nếu có, ta trả về `w`; nếu không, ta tiếp tục duyệt.

Để xác định một chuỗi có đối xứng hay không, ta có thể sử dụng hai con trỏ: một con trỏ trỏ đến đầu chuỗi và con trỏ còn lại trỏ đến cuối chuỗi. Hai con trỏ di chuyển về giữa, đồng thời kiểm tra các ký tự tương ứng có bằng nhau hay không. Nếu duyệt hết chuỗi mà không tìm thấy cặp ký tự khác nhau, thì chuỗi đó là chuỗi đối xứng.

Độ phức tạp thời gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả chuỗi trong mảng `words`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def firstPalindrome(self, words: List[str]) -> str:
        return next((w for w in words if w == w[::-1]), "")
```

#### Java

```java
class Solution {
    public String firstPalindrome(String[] words) {
        for (var w : words) {
            boolean ok = true;
            for (int i = 0, j = w.length() - 1; i < j && ok; ++i, --j) {
                if (w.charAt(i) != w.charAt(j)) {
                    ok = false;
                }
            }
            if (ok) {
                return w;
            }
        }
        return "";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string firstPalindrome(vector<string>& words) {
        for (auto& w : words) {
            bool ok = true;
            for (int i = 0, j = w.size() - 1; i < j; ++i, --j) {
                if (w[i] != w[j]) {
                    ok = false;
                }
            }
            if (ok) {
                return w;
            }
        }
        return "";
    }
};
```

#### Go

```go
func firstPalindrome(words []string) string {
	for _, w := range words {
		ok := true
		for i, j := 0, len(w)-1; i < j && ok; i, j = i+1, j-1 {
			if w[i] != w[j] {
				ok = false
			}
		}
		if ok {
			return w
		}
	}
	return ""
}
```

#### TypeScript

```ts
function firstPalindrome(words: string[]): string {
    return words.find(w => w === w.split('').reverse().join('')) || '';
}
```

#### Rust

```rust
impl Solution {
    pub fn first_palindrome(words: Vec<String>) -> String {
        for w in words {
            if w == w.chars().rev().collect::<String>() {
                return w;
            }
        }
        String::new()
    }
}
```

#### C

```c
char* firstPalindrome(char** words, int wordsSize) {
    for (int i = 0; i < wordsSize; ++i) {
        char* w = words[i];
        int len = strlen(w);
        bool ok = true;
        for (int j = 0, k = len - 1; j < k && ok; ++j, --k) {
            if (w[j] != w[k]) {
                ok = false;
            }
        }
        if (ok) {
            return w;
        }
    }
    return "";
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
