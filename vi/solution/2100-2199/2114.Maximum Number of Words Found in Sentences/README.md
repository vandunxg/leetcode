---
comments: true
difficulty: Easy
rating: 1257
source: Biweekly Contest 68 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2114. Maximum Number of Words Found in Sentences](https://leetcode.com/problems/maximum-number-of-words-found-in-sentences)

[中文文档](/solution/2100-2199/2114.Maximum%20Number%20of%20Words%20Found%20in%20Sentences/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>câu</strong> là một danh sách các <strong>từ</strong> được ngăn cách bởi một dấu cách duy nhất, không có dấu cách ở đầu hoặc cuối.</p>

<p>Bạn được cho một mảng các chuỗi <code>sentences</code>, trong đó mỗi <code>sentences[i]</code> biểu diễn một <strong>câu</strong>.</p>

<p>Trả về <em><strong>số từ lớn nhất</strong> xuất hiện trong một câu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentences = [&quot;alice and bob love leetcode&quot;, &quot;i think so too&quot;, <u>&quot;this is great thanks very much&quot;</u>]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
- Câu đầu tiên, &quot;alice and bob love leetcode&quot;, có tổng cộng 5 từ.
- Câu thứ hai, &quot;i think so too&quot;, có tổng cộng 4 từ.
- Câu thứ ba, &quot;this is great thanks very much&quot;, có tổng cộng 6 từ.
Vì vậy, số từ lớn nhất trong một câu thuộc về câu thứ ba, với 6 từ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentences = [&quot;please wait&quot;, <u>&quot;continue to fight&quot;</u>, <u>&quot;continue to win&quot;</u>]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có thể có nhiều câu chứa cùng số từ.
Trong ví dụ này, câu thứ hai và câu thứ ba (được gạch chân) có cùng số từ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentences.length &lt;= 100</code></li>
	<li><code>1 &lt;= sentences[i].length &lt;= 100</code></li>
	<li><code>sentences[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường và <code>&#39; &#39;</code>.</li>
	<li><code>sentences[i]</code> không có dấu cách ở đầu hoặc cuối.</li>
	<li>Tất cả các từ trong <code>sentences[i]</code> được ngăn cách bởi một dấu cách duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm dấu cách

<!-- thinking:start -->

> **Tư duy**
>
> Các từ trong một câu được ngăn cách bởi dấu cách, nên số từ bằng số dấu cách cộng một. Tổng độ dài đủ nhỏ để đếm số dấu cách trong từng câu.
>
> Không cần tách thành một danh sách; chỉ cần lấy số dấu cách lớn nhất.
>
> Đáp án là $1+\max_s s.\texttt{count}(\text{' '})$.

<!-- thinking:end -->

Ta duyệt qua mảng `sentences`. Với mỗi câu, ta đếm số dấu cách, sau đó số từ bằng số dấu cách cộng $1$. Cuối cùng, ta trả về số từ lớn nhất.

Độ phức tạp thời gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả chuỗi trong mảng `sentences`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostWordsFound(self, sentences: List[str]) -> int:
        return 1 + max(s.count(' ') for s in sentences)
```

#### Java

```java
class Solution {
    public int mostWordsFound(String[] sentences) {
        int ans = 0;
        for (var s : sentences) {
            int cnt = 1;
            for (int i = 0; i < s.length(); ++i) {
                if (s.charAt(i) == ' ') {
                    ++cnt;
                }
            }
            ans = Math.max(ans, cnt);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mostWordsFound(vector<string>& sentences) {
        int ans = 0;
        for (auto& s : sentences) {
            int cnt = 1 + count(s.begin(), s.end(), ' ');
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func mostWordsFound(sentences []string) (ans int) {
	for _, s := range sentences {
		cnt := 1 + strings.Count(s, " ")
		if ans < cnt {
			ans = cnt
		}
	}
	return
}
```

#### TypeScript

```ts
function mostWordsFound(sentences: string[]): number {
    return sentences.reduce(
        (r, s) =>
            Math.max(
                r,
                [...s].reduce((r, c) => r + (c === ' ' ? 1 : 0), 1),
            ),
        0,
    );
}
```

#### Rust

```rust
impl Solution {
    pub fn most_words_found(sentences: Vec<String>) -> i32 {
        let mut ans = 0;
        for s in sentences.iter() {
            let mut count = 1;
            for c in s.as_bytes() {
                if *c == b' ' {
                    count += 1;
                }
            }
            ans = ans.max(count);
        }
        ans
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int mostWordsFound(char** sentences, int sentencesSize) {
    int ans = 0;
    for (int i = 0; i < sentencesSize; i++) {
        char* s = sentences[i];
        int count = 1;
        for (int j = 0; s[j]; j++) {
            if (s[j] == ' ') {
                count++;
            }
        }
        ans = max(ans, count);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
