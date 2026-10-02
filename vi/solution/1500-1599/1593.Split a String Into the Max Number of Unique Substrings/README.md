---
comments: true
difficulty: Medium
rating: 1739
source: Weekly Contest 207 Q2
tags:
    - Hash Table
    - String
    - Backtracking
---

<!-- problem:start -->

# [1593. Split a String Into the Max Number of Unique Substrings](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings)

[中文文档](/solution/1500-1599/1593.Split%20a%20String%20Into%20the%20Max%20Number%20of%20Unique%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi&nbsp;<code>s</code><var>,</var>&nbsp;hãy trả về <em>số lượng mảng con phân biệt lớn nhất mà chuỗi đã cho có thể được tách thành</em>.</p>

<p>Có thể tách chuỗi&nbsp;<code>s</code> thành một danh sách bất kỳ gồm các&nbsp;<strong>mảng con không rỗng</strong>, sao cho phép nối các mảng con tạo thành chuỗi ban đầu.&nbsp;Tuy nhiên, tất cả mảng con phải <strong>phân biệt</strong>.</p>

<p><strong>Mảng con</strong> là một dãy ký tự liên tiếp trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;ababccc&quot;
<strong>Output:</strong> 5
<strong>Giải thích</strong>: Một cách tách tối ưu là [&#39;a&#39;, &#39;b&#39;, &#39;ab&#39;, &#39;c&#39;, &#39;cc&#39;]. Cách tách [&#39;a&#39;, &#39;b&#39;, &#39;a&#39;, &#39;b&#39;, &#39;c&#39;, &#39;cc&#39;] không hợp lệ vì &#39;a&#39; và &#39;b&#39; xuất hiện nhiều lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aba&quot;
<strong>Output:</strong> 2
<strong>Giải thích</strong>: Một cách tách tối ưu là [&#39;a&#39;, &#39;ba&#39;].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aa&quot;
<strong>Output:</strong> 1
<strong>Giải thích</strong>: Không thể tách chuỗi thêm nữa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>
	<p><code>1 &lt;= s.length&nbsp;&lt;= 16</code></p>
	</li>
	<li>
	<p><code>s</code> contains&nbsp;only lower case English letters.</p>
	</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Backtracking + Pruning

<!-- thinking:start -->

> **Tư duy**
>
> Tách chuỗi thành nhiều đoạn phân biệt nhất có thể. Vì $n\le 16$, có thể backtracking toàn bộ; tuy nhiên nếu số đoạn hiện có cộng số ký tự còn lại không thể vượt đáp án tốt nhất, nhánh đó không còn hữu ích.
>
> Từ chỉ số $i$, thử mọi điểm kết thúc $j$, chỉ nhận $s[i:j]$ khi nó chưa xuất hiện rồi đệ quy. Set kiểm tra tính duy nhất; điều kiện $len(st)+n-i\le ans$ loại các tiền tố không thể cải thiện đáp án.

<!-- thinking:end -->

Ta định nghĩa hash table $\textit{st}$ để lưu các mảng con đã tách. Sau đó, ta dùng depth-first search để thử tách chuỗi $\textit{s}$ thành nhiều mảng con phân biệt.

Cụ thể, ta thiết kế hàm $\text{dfs}(i)$, nghĩa là đang xét việc tách phần $\textit{s}[i:]$.

In the function $\text{dfs}(i)$, we first check if the number of substrings already split plus the remaining characters is less than or equal to the current answer. If so, there is no need to continue splitting, and we return directly. If $i \geq n$, it means we have completed the splitting of the entire string, and we update the answer to the maximum of the current number of substrings and the answer. Otherwise, we enumerate the end position $j$ (exclusive) of the current substring and check if $\textit{s}[i..j)$ has already been split. If not, we add it to the hash table $\textit{st}$ and continue to recursively consider splitting the remaining part. After the recursive call, we need to remove $\textit{s}[i..j)$ from the hash table $\textit{st}$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n^2 \times 2^n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{s}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxUniqueSplit(self, s: str) -> int:
        def dfs(i: int):
            nonlocal ans
            if len(st) + len(s) - i <= ans:
                return
            if i >= len(s):
                ans = max(ans, len(st))
                return
            for j in range(i + 1, len(s) + 1):
                if s[i:j] not in st:
                    st.add(s[i:j])
                    dfs(j)
                    st.remove(s[i:j])

        ans = 0
        st = set()
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private Set<String> st = new HashSet<>();
    private int ans;
    private String s;

    public int maxUniqueSplit(String s) {
        this.s = s;
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (st.size() + s.length() - i <= ans) {
            return;
        }
        if (i >= s.length()) {
            ans = Math.max(ans, st.size());
            return;
        }
        for (int j = i + 1; j <= s.length(); ++j) {
            String t = s.substring(i, j);
            if (st.add(t)) {
                dfs(j);
                st.remove(t);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxUniqueSplit(string s) {
        unordered_set<string> st;
        int n = s.size();
        int ans = 0;
        auto dfs = [&](this auto&& dfs, int i) -> void {
            if (st.size() + n - i <= ans) {
                return;
            }
            if (i >= n) {
                ans = max(ans, (int) st.size());
                return;
            }
            for (int j = i + 1; j <= n; ++j) {
                string t = s.substr(i, j - i);
                if (!st.contains(t)) {
                    st.insert(t);
                    dfs(j);
                    st.erase(t);
                }
            }
        };
        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func maxUniqueSplit(s string) (ans int) {
	st := map[string]bool{}
	n := len(s)
	var dfs func(int)
	dfs = func(i int) {
		if len(st)+n-i <= ans {
			return
		}
		if i >= n {
			ans = max(ans, len(st))
			return
		}
		for j := i + 1; j <= n; j++ {
			if t := s[i:j]; !st[t] {
				st[t] = true
				dfs(j)
				delete(st, t)
			}
		}
	}
	dfs(0)
	return
}
```

#### TypeScript

```ts
function maxUniqueSplit(s: string): number {
    const n = s.length;
    const st = new Set<string>();
    let ans = 0;
    const dfs = (i: number): void => {
        if (st.size + n - i <= ans) {
            return;
        }
        if (i >= n) {
            ans = Math.max(ans, st.size);
            return;
        }
        for (let j = i + 1; j <= n; ++j) {
            const t = s.slice(i, j);
            if (!st.has(t)) {
                st.add(t);
                dfs(j);
                st.delete(t);
            }
        }
    };
    dfs(0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
