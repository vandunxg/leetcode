---
comments: true
difficulty: Medium
rating: 1459
source: Biweekly Contest 90 Q2
tags:
    - Trie
    - Array
    - String
---

<!-- problem:start -->

# [2452. Words Within Two Edits of Dictionary](https://leetcode.com/problems/words-within-two-edits-of-dictionary)

[中文文档](/solution/2400-2499/2452.Words%20Within%20Two%20Edits%20of%20Dictionary/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng chuỗi <code>queries</code> và <code>dictionary</code>. Tất cả các từ trong mỗi mảng đều chỉ gồm các chữ cái tiếng Anh viết thường và có cùng độ dài.</p>

<p>Trong một lần <strong>chỉnh sửa</strong>, bạn có thể chọn một từ trong <code>queries</code> và thay đổi một chữ cái bất kỳ trong đó thành một chữ cái khác bất kỳ. Hãy tìm tất cả các từ trong <code>queries</code> sao cho sau <strong>tối đa</strong> hai lần chỉnh sửa, chúng bằng một từ nào đó trong <code>dictionary</code>.</p>

<p>Trả về<em> danh sách tất cả các từ trong </em><code>queries</code><em>, </em><em>khớp với một từ nào đó trong </em><code>dictionary</code><em> sau tối đa <strong>hai lần chỉnh sửa</strong></em>. Trả về các từ theo <strong>đúng thứ tự</strong> xuất hiện trong <code>queries</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [&quot;word&quot;,&quot;note&quot;,&quot;ants&quot;,&quot;wood&quot;], dictionary = [&quot;wood&quot;,&quot;joke&quot;,&quot;moat&quot;]
<strong>Đầu ra:</strong> [&quot;word&quot;,&quot;note&quot;,&quot;wood&quot;]
<strong>Giải thích:</strong>
- Thay &#39;r&#39; trong &quot;word&quot; thành &#39;o&#39; để biến nó thành từ &quot;wood&quot; trong từ điển.
- Thay &#39;n&#39; thành &#39;j&#39; và &#39;t&#39; thành &#39;k&#39; trong &quot;note&quot; để biến nó thành &quot;joke&quot;.
- Cần nhiều hơn 2 lần chỉnh sửa để biến &quot;ants&quot; thành một từ trong từ điển.
- &quot;wood&quot; có thể giữ nguyên (0 lần chỉnh sửa) và khớp với từ tương ứng trong từ điển.
Do đó, ta trả về [&quot;word&quot;,&quot;note&quot;,&quot;wood&quot;].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [&quot;yes&quot;], dictionary = [&quot;not&quot;]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong>
Thực hiện bất kỳ hai lần chỉnh sửa nào trên &quot;yes&quot; cũng không thể biến nó thành &quot;not&quot;. Do đó, ta trả về một mảng rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= queries.length, dictionary.length &lt;= 100</code></li>
	<li><code>n == queries[i].length == dictionary[j].length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li>Tất cả <code>queries[i]</code> và <code>dictionary[j]</code> đều chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Số lượng query và từ trong dictionary đều không quá $100$, độ dài mỗi từ $\le 100$. Với mỗi query, ta đếm số vị trí khác nhau khi so sánh với từng từ trong dictionary; số vị trí khác nhau nhỏ hơn ba là đủ điều kiện. Chấp nhận query ngay khi tìm được từ đầu tiên phù hợp.

<!-- thinking:end -->

Ta duyệt trực tiếp từng từ $s$ trong mảng $\textit{queries}$, sau đó duyệt từng từ $t$ trong mảng $\textit{dictionary}$. Nếu tồn tại một từ $t$ có khoảng cách chỉnh sửa với $s$ nhỏ hơn $3$, ta thêm $s$ vào mảng kết quả rồi thoát khỏi vòng lặp bên trong. Nếu không có từ $t$ nào như vậy, ta tiếp tục duyệt từ $s$ tiếp theo.

Độ phức tạp thời gian là $O(m \times n \times l)$, trong đó $m$ và $n$ lần lượt là độ dài của các mảng $\textit{queries}$ và $\textit{dictionary}$, còn $l$ là độ dài của từ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def twoEditWords(self, queries: List[str], dictionary: List[str]) -> List[str]:
        ans = []
        for s in queries:
            for t in dictionary:
                if sum(a != b for a, b in zip(s, t)) < 3:
                    ans.append(s)
                    break
        return ans
```

#### Java

```java
class Solution {
    public List<String> twoEditWords(String[] queries, String[] dictionary) {
        List<String> ans = new ArrayList<>();
        int n = queries[0].length();
        for (var s : queries) {
            for (var t : dictionary) {
                int cnt = 0;
                for (int i = 0; i < n; ++i) {
                    if (s.charAt(i) != t.charAt(i)) {
                        ++cnt;
                    }
                }
                if (cnt < 3) {
                    ans.add(s);
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
    vector<string> twoEditWords(vector<string>& queries, vector<string>& dictionary) {
        vector<string> ans;
        for (auto& s : queries) {
            for (auto& t : dictionary) {
                int cnt = 0;
                for (int i = 0; i < s.size(); ++i) {
                    cnt += s[i] != t[i];
                }
                if (cnt < 3) {
                    ans.emplace_back(s);
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
func twoEditWords(queries []string, dictionary []string) (ans []string) {
	for _, s := range queries {
		for _, t := range dictionary {
			cnt := 0
			for i := range s {
				if s[i] != t[i] {
					cnt++
				}
			}
			if cnt < 3 {
				ans = append(ans, s)
				break
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function twoEditWords(queries: string[], dictionary: string[]): string[] {
    const n = queries[0].length;
    return queries.filter(s => {
        for (const t of dictionary) {
            let diff = 0;
            for (let i = 0; i < n; ++i) {
                if (s[i] !== t[i]) {
                    ++diff;
                }
            }
            if (diff < 3) {
                return true;
            }
        }
        return false;
    });
}
```

#### Rust

```rust
impl Solution {
    pub fn two_edit_words(queries: Vec<String>, dictionary: Vec<String>) -> Vec<String> {
        queries
            .into_iter()
            .filter(|s| {
                dictionary
                    .iter()
                    .any(|t| s.chars().zip(t.chars()).filter(|&(a, b)| a != b).count() < 3)
            })
            .collect()
    }
}
```

#### C#

```cs
public class Solution {
    public IList<string> TwoEditWords(string[] queries, string[] dictionary) {
        var ans = new List<string>();
        foreach (var s in queries) {
            foreach (var t in dictionary) {
                int cnt = 0;
                for (int i = 0; i < s.Length; i++) {
                    if (s[i] != t[i]) {
                        cnt++;
                    }
                }
                if (cnt < 3) {
                    ans.Add(s);
                    break;
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
