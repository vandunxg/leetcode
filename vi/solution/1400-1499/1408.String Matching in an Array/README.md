---
comments: true
difficulty: Easy
rating: 1223
source: Weekly Contest 184 Q1
tags:
    - Array
    - String
    - String Matching
---

<!-- problem:start -->

# [1408. String Matching in an Array](https://leetcode.com/problems/string-matching-in-an-array)

[中文文档](/solution/1400-1499/1408.String%20Matching%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code>, hãy trả về tất cả các chuỗi trong<em> </em><code>words</code><em> </em>là <span data-keyword="substring-nonempty">chuỗi con</span> của một từ khác. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;mass&quot;,&quot;as&quot;,&quot;hero&quot;,&quot;superhero&quot;]
<strong>Đầu ra:</strong> [&quot;as&quot;,&quot;hero&quot;]
<strong>Giải thích:</strong> &quot;as&quot; là chuỗi con của &quot;mass&quot; và &quot;hero&quot; là chuỗi con của &quot;superhero&quot;.
[&quot;hero&quot;,&quot;as&quot;] cũng là một đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;leetcode&quot;,&quot;et&quot;,&quot;code&quot;]
<strong>Đầu ra:</strong> [&quot;et&quot;,&quot;code&quot;]
<strong>Giải thích:</strong> &quot;et&quot;, &quot;code&quot; là các chuỗi con của &quot;leetcode&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;blue&quot;,&quot;green&quot;,&quot;bu&quot;]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không có chuỗi nào trong words là chuỗi con của một chuỗi khác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 30</code></li>
	<li><code>words[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả các chuỗi trong <code>words</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 100$ và mỗi từ có độ dài không quá $30$, vì vậy việc kiểm tra xem mỗi từ có xuất hiện trong một từ khác hay không mất khoảng $O(n^2\cdot L^2)$ và đáp ứng giới hạn.
>
> Các từ là duy nhất, nên chỉ cần dùng hai vòng lặp với phép kiểm tra chuỗi con; không cần trie hay KMP.

<!-- thinking:end -->

Ta liệt kê trực tiếp tất cả các chuỗi $words[i]$ và kiểm tra xem nó có phải là chuỗi con của các chuỗi khác hay không. Nếu có, ta thêm nó vào đáp án.

Độ phức tạp thời gian là $O(n^3)$, còn độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của mảng chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stringMatching(self, words: List[str]) -> List[str]:
        ans = []
        for i, s in enumerate(words):
            if any(i != j and s in t for j, t in enumerate(words)):
                ans.append(s)
        return ans
```

#### Java

```java
class Solution {
    public List<String> stringMatching(String[] words) {
        List<String> ans = new ArrayList<>();
        int n = words.length;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i != j && words[j].contains(words[i])) {
                    ans.add(words[i]);
                    break;
                }
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
    vector<string> stringMatching(vector<string>& words) {
        vector<string> ans;
        int n = words.size();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i != j && words[j].find(words[i]) != string::npos) {
                    ans.push_back(words[i]);
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func stringMatching(words []string) []string {
	ans := []string{}
	for i, w1 := range words {
		for j, w2 := range words {
			if i != j && strings.Contains(w2, w1) {
				ans = append(ans, w1)
				break
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function stringMatching(words: string[]): string[] {
    const ans: string[] = [];
    const n = words.length;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (words[j].includes(words[i]) && i !== j) {
                ans.push(words[i]);
                break;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn string_matching(words: Vec<String>) -> Vec<String> {
        let mut ans = Vec::new();
        let n = words.len();
        for i in 0..n {
            for j in 0..n {
                if i != j && words[j].contains(&words[i]) {
                    ans.push(words[i].clone());
                    break;
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
