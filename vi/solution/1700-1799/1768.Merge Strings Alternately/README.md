---
comments: true
difficulty: Easy
rating: 1166
source: Weekly Contest 229 Q1
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1768. Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately)

[中文文档](/solution/1700-1799/1768.Merge%20Strings%20Alternately/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>word1</code> và <code>word2</code>. Hãy trộn hai chuỗi bằng cách thêm các ký tự xen kẽ, bắt đầu với <code>word1</code>. Nếu một chuỗi dài hơn chuỗi kia, nối các ký tự còn lại vào cuối chuỗi đã trộn.</p>

<p>Trả về <em>chuỗi đã trộn.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;abc&quot;, word2 = &quot;pqr&quot;
<strong>Đầu ra:</strong> &quot;apbqcr&quot;
<strong>Giải thích:</strong>&nbsp;Chuỗi được trộn như sau:
word1:  a   b   c
word2:    p   q   r
merged: a p b q c r
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;ab&quot;, word2 = &quot;pqrs&quot;
<strong>Đầu ra:</strong> &quot;apbqrs&quot;
<strong>Giải thích:</strong>&nbsp;Vì word2 dài hơn, &quot;rs&quot; được nối vào cuối.
word1:  a   b
word2:    p   q   r   s
merged: a p b q   r   s
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;abcd&quot;, word2 = &quot;pq&quot;
<strong>Đầu ra:</strong> &quot;apbqcd&quot;
<strong>Giải thích:</strong>&nbsp;Vì word1 dài hơn, &quot;cd&quot; được nối vào cuối.
word1:  a   b   c   d
word2:    p   q
merged: a p b q c   d
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length, word2.length &lt;= 100</code></li>
	<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Trộn bằng cách lấy các ký tự xen kẽ rồi nối phần còn lại của chuỗi dài hơn. Chỉ cần một lần duyệt song song là đủ.
>
> $\textit{zip\_longest}$ tạo ra các ký tự tương ứng (rỗng khi một chuỗi không còn ký tự); nối chúng lại sẽ thu được chuỗi trộn.

<!-- thinking:end -->

Ta duyệt hai chuỗi `word1` và `word2`, lấy từng ký tự rồi nối vào chuỗi kết quả. Code Python có thể được viết gọn thành một dòng.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của hai chuỗi. Không tính phần không gian của kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        return ''.join(a + b for a, b in zip_longest(word1, word2, fillvalue=''))
```

#### Java

```java
class Solution {
    public String mergeAlternately(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < m || i < n; ++i) {
            if (i < m) {
                ans.append(word1.charAt(i));
            }
            if (i < n) {
                ans.append(word2.charAt(i));
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string mergeAlternately(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        string ans;
        for (int i = 0; i < m || i < n; ++i) {
            if (i < m) ans += word1[i];
            if (i < n) ans += word2[i];
        }
        return ans;
    }
};
```

#### Go

```go
func mergeAlternately(word1 string, word2 string) string {
	m, n := len(word1), len(word2)
	ans := make([]byte, 0, m+n)
	for i := 0; i < m || i < n; i++ {
		if i < m {
			ans = append(ans, word1[i])
		}
		if i < n {
			ans = append(ans, word2[i])
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function mergeAlternately(word1: string, word2: string): string {
    const ans: string[] = [];
    const [m, n] = [word1.length, word2.length];
    for (let i = 0; i < m || i < n; ++i) {
        if (i < m) {
            ans.push(word1[i]);
        }
        if (i < n) {
            ans.push(word2[i]);
        }
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn merge_alternately(word1: String, word2: String) -> String {
        let s1 = word1.as_bytes();
        let s2 = word2.as_bytes();
        let n = s1.len().max(s2.len());
        let mut res = vec![];
        for i in 0..n {
            if s1.get(i).is_some() {
                res.push(s1[i]);
            }
            if s2.get(i).is_some() {
                res.push(s2[i]);
            }
        }
        String::from_utf8(res).unwrap()
    }
}
```

#### C

```c
char* mergeAlternately(char* word1, char* word2) {
    int m = strlen(word1);
    int n = strlen(word2);
    char* ans = malloc(sizeof(char) * (n + m + 1));
    int i = 0;
    int j = 0;
    while (i + j != m + n) {
        if (i < m) {
            ans[i + j] = word1[i];
            i++;
        }
        if (j < n) {
            ans[i + j] = word2[j];
            j++;
        }
    }
    ans[n + m] = '\0';
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
