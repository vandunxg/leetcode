---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.15. Longest Word](https://leetcode.cn/problems/longest-word-lcci)

[中文文档](/lcci/17.15.Longest%20Word/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một danh sách các từ, hãy viết chương trình tìm từ dài nhất được tạo thành từ các từ khác trong danh sách. Nếu có nhiều đáp án, hãy trả về đáp án có thứ tự từ điển nhỏ nhất. Nếu không có đáp án, trả về một chuỗi rỗng.</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong> [&quot;cat&quot;,&quot;banana&quot;,&quot;dog&quot;,&quot;nana&quot;,&quot;walk&quot;,&quot;walker&quot;,&quot;dogwalker&quot;]

<strong>Đầu ra: </strong> &quot;dogwalker&quot;

<strong>Giải thích: </strong> &quot;dogwalker&quot; có thể được tạo thành từ &quot;dog&quot; và &quot;walker&quot;.

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>0 &lt;= len(words) &lt;= 100</code></li>
	<li><code>1 &lt;= len(words[i]) &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sorting + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Từ dài nhất được tạo bằng cách nối các từ khác trong từ điển (cho phép dùng lại), và nếu hòa thì chọn theo thứ tự từ điển. Chỉ cần phân tách từng từ; xét các từ dài hơn trước thì có thể dừng sớm.
>
> Sắp xếp theo độ dài giảm dần, sau đó theo thứ tự từ điển tăng dần; từ đầu tiên có thể nối được chính là đáp án.
>
> Xóa chính từ đó trước khi tìm kiếm để nó không thể tự sử dụng chính mình. $dfs$ thử các tiền tố trong từ điển. Nếu không tìm thấy, trả về chuỗi rỗng.

<!-- thinking:end -->

Lưu ý rằng trong bài toán này, mỗi từ thực sự có thể được sử dụng lại.

Ta có thể dùng hash table $\textit{s}$ để lưu tất cả các từ, sau đó sắp xếp các từ theo độ dài giảm dần; nếu độ dài bằng nhau thì sắp xếp theo thứ tự từ điển tăng dần.

Tiếp theo, ta duyệt danh sách từ đã sắp xếp. Với mỗi từ $\textit{w}$, trước tiên ta xóa nó khỏi hash table $\textit{s}$, sau đó dùng tìm kiếm theo chiều sâu $\textit{dfs}$ để xác định xem $\textit{w}$ có thể được tạo thành từ các từ khác hay không. Nếu có, ta trả về $\textit{w}$.

Logic thực thi của hàm $\textit{dfs}$ như sau:

- Nếu $\textit{w}$ rỗng, trả về $\text{true}$;
- Duyệt qua tất cả tiền tố của $\textit{w}$. Nếu một tiền tố nằm trong hash table $\textit{s}$ và $\textit{dfs}$ trả về $\text{true}$, thì trả về $\text{true}$;
- Nếu không có tiền tố nào thỏa mãn điều kiện, trả về $\text{false}$.

Nếu không có từ nào thỏa mãn điều kiện, trả về một chuỗi rỗng.

Độ phức tạp thời gian là $O(m \times n \times \log n + n \times 2^M)$, và độ phức tạp không gian là $O(m \times n)$. Trong đó, $n$ và $m$ lần lượt là số lượng từ và độ dài trung bình của các từ, còn $M$ là độ dài của từ dài nhất.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestWord(self, words: List[str]) -> str:
        def dfs(w: str) -> bool:
            if not w:
                return True
            for k in range(1, len(w) + 1):
                if w[:k] in s and dfs(w[k:]):
                    return True
            return False

        s = set(words)
        words.sort(key=lambda x: (-len(x), x))
        for w in words:
            s.remove(w)
            if dfs(w):
                return w
        return ""
```

#### Java

```java
class Solution {
    private Set<String> s = new HashSet<>();

    public String longestWord(String[] words) {
        for (String w : words) {
            s.add(w);
        }
        Arrays.sort(words, (a, b) -> {
            if (a.length() != b.length()) {
                return b.length() - a.length();
            }
            return a.compareTo(b);
        });
        for (String w : words) {
            s.remove(w);
            if (dfs(w)) {
                return w;
            }
        }
        return "";
    }

    private boolean dfs(String w) {
        if (w.length() == 0) {
            return true;
        }
        for (int k = 1; k <= w.length(); ++k) {
            if (s.contains(w.substring(0, k)) && dfs(w.substring(k))) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string longestWord(vector<string>& words) {
        unordered_set<string> s(words.begin(), words.end());
        ranges::sort(words, [&](const string& a, const string& b) {
            return a.size() > b.size() || (a.size() == b.size() && a < b);
        });
        auto dfs = [&](this auto&& dfs, string w) -> bool {
            if (w.empty()) {
                return true;
            }
            for (int k = 1; k <= w.size(); ++k) {
                if (s.contains(w.substr(0, k)) && dfs(w.substr(k))) {
                    return true;
                }
            }
            return false;
        };
        for (const string& w : words) {
            s.erase(w);
            if (dfs(w)) {
                return w;
            }
        }
        return "";
    }
};
```

#### Go

```go
func longestWord(words []string) string {
	s := map[string]bool{}
	for _, w := range words {
		s[w] = true
	}
	sort.Slice(words, func(i, j int) bool {
		return len(words[i]) > len(words[j]) || (len(words[i]) == len(words[j]) && words[i] < words[j])
	})
	var dfs func(string) bool
	dfs = func(w string) bool {
		if len(w) == 0 {
			return true
		}
		for k := 1; k <= len(w); k++ {
			if s[w[:k]] && dfs(w[k:]) {
				return true
			}
		}
		return false
	}
	for _, w := range words {
		s[w] = false
		if dfs(w) {
			return w
		}
	}
	return ""
}
```

#### TypeScript

```ts
function longestWord(words: string[]): string {
    const s = new Set(words);

    words.sort((a, b) => (a.length === b.length ? a.localeCompare(b) : b.length - a.length));

    const dfs = (w: string): boolean => {
        if (w === '') {
            return true;
        }
        for (let k = 1; k <= w.length; ++k) {
            if (s.has(w.substring(0, k)) && dfs(w.substring(k))) {
                return true;
            }
        }
        return false;
    };

    for (const w of words) {
        s.delete(w);
        if (dfs(w)) {
            return w;
        }
    }

    return '';
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn longest_word(words: Vec<String>) -> String {
        let mut s: HashSet<String> = words.iter().cloned().collect();
        let mut words = words;
        words.sort_by(|a, b| b.len().cmp(&a.len()).then(a.cmp(b)));

        fn dfs(w: String, s: &mut HashSet<String>) -> bool {
            if w.is_empty() {
                return true;
            }
            for k in 1..=w.len() {
                if s.contains(&w[0..k]) && dfs(w[k..].to_string(), s) {
                    return true;
                }
            }
            false
        }
        for w in words {
            s.remove(&w);
            if dfs(w.clone(), &mut s) {
                return w;
            }
        }
        String::new()
    }
}
```

#### Swift

```swift
class Solution {
    func longestWord(_ words: [String]) -> String {
        var s: Set<String> = Set(words)
        var words = words
        words.sort { (a, b) -> Bool in
            if a.count == b.count {
                return a < b
            } else {
                return a.count > b.count
            }
        }

        func dfs(_ w: String) -> Bool {
            if w.isEmpty {
                return true
            }
            for k in 1...w.count {
                let prefix = String(w.prefix(k))
                if s.contains(prefix) && dfs(String(w.dropFirst(k))) {
                    return true
                }
            }
            return false
        }

        for w in words {
            s.remove(w)
            if dfs(w) {
                return w
            }
        }

        return ""
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
