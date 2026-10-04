---
comments: true
difficulty: Medium
rating: 1772
source: Weekly Contest 400 Q3
tags:
    - Stack
    - Greedy
    - Hash Table
    - String
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3170. Lexicographically Minimum String After Removing Stars](https://leetcode.com/problems/lexicographically-minimum-string-after-removing-stars)

[中文文档](/solution/3100-3199/3170.Lexicographically%20Minimum%20String%20After%20Removing%20Stars/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code>. Chuỗi có thể chứa bất kỳ số lượng ký tự <code>&#39;*&#39;</code> nào. Nhiệm vụ của bạn là xóa tất cả ký tự <code>&#39;*&#39;</code>.</p>

<p>Trong khi vẫn còn ký tự <code>&#39;*&#39;</code>, hãy thực hiện thao tác sau:</p>

<ul>
	<li>Xóa ký tự <code>&#39;*&#39;</code> ngoài cùng bên trái và ký tự không phải <code>&#39;*&#39;</code> <strong>nhỏ nhất</strong> ở bên <em>trái</em> nó. Nếu có nhiều ký tự nhỏ nhất, bạn có thể xóa bất kỳ ký tự nào trong số đó.</li>
</ul>

<p>Trả về chuỗi kết quả <span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span> sau khi xóa tất cả ký tự <code>&#39;*&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaba*&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aab&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta nên xóa một trong các ký tự <code>&#39;a&#39;</code> cùng với <code>&#39;*&#39;</code>. Nếu chọn <code>s[3]</code>, <code>s</code> sẽ trở thành chuỗi nhỏ nhất theo thứ tự từ điển.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abc&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi không chứa ký tự <code>&#39;*&#39;</code>.<!-- notionvc: ff07e34f-b1d6-41fb-9f83-5d0ba3c1ecde --></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh thường và <code>&#39;*&#39;</code>.</li>
	<li>Dữ liệu đầu vào được tạo sao cho có thể xóa tất cả ký tự <code>&#39;*&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ghi lại chỉ số theo ký tự

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi `*` xóa chính nó và chữ cái nhỏ nhất ở bên trái (nếu bằng nhau thì ưu tiên chỉ số lớn hơn). Có thể dùng heap, nhưng khi đó phải lưu các chỉ số để dựng lại kết quả.
>
> Gom các chỉ số theo ký tự. Khi gặp dấu sao, duyệt `a`..`z` và lấy ra chỉ số cuối cùng của bucket không rỗng đầu tiên — bản sao ở bên phải nhất của ký tự nhỏ nhất.
>
> Đánh dấu các vị trí bị xóa trong $rem$ rồi nối các ký tự chưa được đánh dấu theo thứ tự. Mỗi dấu sao kiểm tra nhiều nhất $26$ bucket.

<!-- thinking:end -->

Ta định nghĩa một mảng $g$ để lưu danh sách chỉ số của từng ký tự, và một mảng boolean $rem$ có độ dài $n$ để ghi nhận ký tự nào cần bị xóa.

Ta duyệt qua chuỗi $s$:

Nếu ký tự hiện tại là dấu sao, ta cần xóa nó, nên đánh dấu $rem[i]$ là đã bị xóa. Đồng thời, ta cần xóa ký tự có thứ tự từ điển nhỏ nhất và chỉ số lớn nhất tại thời điểm này. Ta duyệt 26 chữ cái thường theo thứ tự tăng dần. Nếu $g[a]$ không rỗng, ta xóa chỉ số cuối cùng trong $g[a]$ và đánh dấu chỉ số tương ứng trong $rem$ là đã bị xóa.

Nếu ký tự hiện tại không phải là dấu sao, ta thêm chỉ số của ký tự hiện tại vào $g$.

Cuối cùng, ta duyệt qua $s$ và nối các ký tự chưa bị xóa.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài chuỗi $s$, còn $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def clearStars(self, s: str) -> str:
        g = defaultdict(list)
        n = len(s)
        rem = [False] * n
        for i, c in enumerate(s):
            if c == "*":
                rem[i] = True
                for a in ascii_lowercase:
                    if g[a]:
                        rem[g[a].pop()] = True
                        break
            else:
                g[c].append(i)
        return "".join(c for i, c in enumerate(s) if not rem[i])
```

#### Java

```java
class Solution {
    public String clearStars(String s) {
        Deque<Integer>[] g = new Deque[26];
        Arrays.setAll(g, k -> new ArrayDeque<>());
        int n = s.length();
        boolean[] rem = new boolean[n];
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '*') {
                rem[i] = true;
                for (int j = 0; j < 26; ++j) {
                    if (!g[j].isEmpty()) {
                        rem[g[j].pop()] = true;
                        break;
                    }
                }
            } else {
                g[s.charAt(i) - 'a'].push(i);
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; ++i) {
            if (!rem[i]) {
                sb.append(s.charAt(i));
            }
        }
        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string clearStars(string s) {
        stack<int> g[26];
        int n = s.length();
        vector<bool> rem(n);
        for (int i = 0; i < n; ++i) {
            if (s[i] == '*') {
                rem[i] = true;
                for (int j = 0; j < 26; ++j) {
                    if (!g[j].empty()) {
                        rem[g[j].top()] = true;
                        g[j].pop();
                        break;
                    }
                }
            } else {
                g[s[i] - 'a'].push(i);
            }
        }
        string ans;
        for (int i = 0; i < n; ++i) {
            if (!rem[i]) {
                ans.push_back(s[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func clearStars(s string) string {
	g := make([][]int, 26)
	n := len(s)
	rem := make([]bool, n)
	for i, c := range s {
		if c == '*' {
			rem[i] = true
			for j := 0; j < 26; j++ {
				if len(g[j]) > 0 {
					rem[g[j][len(g[j])-1]] = true
					g[j] = g[j][:len(g[j])-1]
					break
				}
			}
		} else {
			g[c-'a'] = append(g[c-'a'], i)
		}
	}
	ans := []byte{}
	for i := range s {
		if !rem[i] {
			ans = append(ans, s[i])
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function clearStars(s: string): string {
    const g: number[][] = Array.from({ length: 26 }, () => []);
    const n = s.length;
    const rem: boolean[] = Array(n).fill(false);
    for (let i = 0; i < n; ++i) {
        if (s[i] === '*') {
            rem[i] = true;
            for (let j = 0; j < 26; ++j) {
                if (g[j].length) {
                    rem[g[j].pop()!] = true;
                    break;
                }
            }
        } else {
            g[s.charCodeAt(i) - 97].push(i);
        }
    }
    return s
        .split('')
        .filter((_, i) => !rem[i])
        .join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn clear_stars(s: String) -> String {
        let n = s.len();
        let s_bytes = s.as_bytes();
        let mut g: Vec<Vec<usize>> = vec![vec![]; 26];
        let mut rem = vec![false; n];
        let chars: Vec<char> = s.chars().collect();

        for (i, &ch) in chars.iter().enumerate() {
            if ch == '*' {
                rem[i] = true;
                for j in 0..26 {
                    if let Some(idx) = g[j].pop() {
                        rem[idx] = true;
                        break;
                    }
                }
            } else {
                g[(ch as u8 - b'a') as usize].push(i);
            }
        }

        chars
            .into_iter()
            .enumerate()
            .filter_map(|(i, ch)| if !rem[i] { Some(ch) } else { None })
            .collect()
    }
}
```

#### C#

```cs
public class Solution {
    public string ClearStars(string s) {
        int n = s.Length;
        List<int>[] g = new List<int>[26];
        for (int i = 0; i < 26; i++) {
            g[i] = new List<int>();
        }

        bool[] rem = new bool[n];
        for (int i = 0; i < n; i++) {
            char ch = s[i];
            if (ch == '*') {
                rem[i] = true;
                for (int j = 0; j < 26; j++) {
                    if (g[j].Count > 0) {
                        int idx = g[j][g[j].Count - 1];
                        g[j].RemoveAt(g[j].Count - 1);
                        rem[idx] = true;
                        break;
                    }
                }
            } else {
                g[ch - 'a'].Add(i);
            }
        }

        var ans = new System.Text.StringBuilder();
        for (int i = 0; i < n; i++) {
            if (!rem[i]) {
                ans.Append(s[i]);
            }
        }

        return ans.ToString();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
