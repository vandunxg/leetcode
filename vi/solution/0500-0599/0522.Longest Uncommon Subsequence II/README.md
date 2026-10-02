---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Two Pointers
    - String
    - Sorting
---

<!-- problem:start -->

# [522. Longest Uncommon Subsequence II](https://leetcode.com/problems/longest-uncommon-subsequence-ii)

[中文文档](/solution/0500-0599/0522.Longest%20Uncommon%20Subsequence%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>strs</code>, hãy trả về <em>độ dài của <strong>dãy con không phổ biến dài nhất</strong> trong các chuỗi</em>. Nếu không tồn tại dãy con không phổ biến, hãy trả về <code>-1</code>.</p>

<p><strong>Dãy con không phổ biến</strong> trong một mảng chuỗi là chuỗi <strong>là dãy con của một chuỗi nhưng không phải của các chuỗi còn lại</strong>.</p>

<p><strong>Dãy con</strong> của chuỗi <code>s</code> là chuỗi có thể thu được bằng cách xóa một số ký tự bất kỳ khỏi <code>s</code>.</p>

<ul>
	<li>Ví dụ, <code>&quot;abc&quot;</code> là dãy con của <code>&quot;aebdc&quot;</code> vì có thể xóa các ký tự được gạch chân trong <code>&quot;a<u>e</u>b<u>d</u>c&quot;</code> để thu được <code>&quot;abc&quot;</code>. Các dãy con khác của <code>&quot;aebdc&quot;</code> gồm <code>&quot;aebdc&quot;</code>, <code>&quot;aeb&quot;</code> và <code>&quot;&quot;</code> (chuỗi rỗng).</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> strs = ["aba","cdc","eae"]
<strong>Đầu ra:</strong> 3
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> strs = ["aaa","aaa","aa"]
<strong>Đầu ra:</strong> -1
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= strs.length &lt;= 50</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 10</code></li>
	<li><code>strs[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Kiểm tra dãy con

<!-- thinking:start -->

> **Tư duy**
>
> Khi có nhiều chuỗi, một chuỗi dài hơn vẫn có thể là dãy con của chuỗi khác, nên chỉ dựa vào độ dài là chưa đủ. Vì $n \le 50$ và các chuỗi ngắn, ta có thể kiểm tra từng chuỗi với các chuỗi còn lại.
>
> Duyệt bằng hai con trỏ để kiểm tra $s$ có phải dãy con của $t$ hay không. Nếu $s$ không phải dãy con của bất kỳ chuỗi nào khác, thì đó là dãy con không phổ biến và ta dùng độ dài của nó để cập nhật đáp án. Nếu không có chuỗi nào thỏa mãn, trả về $-1$.

<!-- thinking:end -->

Ta định nghĩa hàm $check(s, t)$ để kiểm tra chuỗi $s$ có phải là dãy con của chuỗi $t$ hay không. Dùng hai con trỏ $i$ và $j$ ban đầu trỏ đến đầu chuỗi $s$ và $t$. Sau đó, liên tục dịch con trỏ $j$. Nếu $s[i]$ bằng $t[j]$, dịch con trỏ $i$. Cuối cùng, kiểm tra xem $i$ có bằng độ dài của $s$ hay không. Nếu có, $s$ là dãy con của $t$.

Để xác định $s$ có phải dãy con không phổ biến hay không, ta so sánh chính chuỗi $s$ với các chuỗi khác trong danh sách. Nếu tồn tại chuỗi mà $s$ là dãy con của nó thì $s$ không phải dãy con không phổ biến. Ngược lại, $s$ là dãy con không phổ biến. Ta chọn chuỗi dài nhất trong số các chuỗi như vậy.

Độ phức tạp thời gian là $O(n^2 \times m)$, trong đó $n$ là số chuỗi trong danh sách và $m$ là độ dài trung bình của các chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLUSlength(self, strs: List[str]) -> int:
        def check(s: str, t: str):
            i = j = 0
            while i < len(s) and j < len(t):
                if s[i] == t[j]:
                    i += 1
                j += 1
            return i == len(s)

        ans = -1
        for i, s in enumerate(strs):
            for j, t in enumerate(strs):
                if i != j and check(s, t):
                    break
            else:
                ans = max(ans, len(s))
        return ans
```

#### Java

```java
class Solution {
    public int findLUSlength(String[] strs) {
        int ans = -1;
        int n = strs.length;
        for (int i = 0, j; i < n; ++i) {
            int x = strs[i].length();
            for (j = 0; j < n; ++j) {
                if (i != j && check(strs[i], strs[j])) {
                    x = -1;
                    break;
                }
            }
            ans = Math.max(ans, x);
        }
        return ans;
    }

    private boolean check(String s, String t) {
        int m = s.length(), n = t.length();
        int i = 0;
        for (int j = 0; i < m && j < n; ++j) {
            if (s.charAt(i) == t.charAt(j)) {
                ++i;
            }
        }
        return i == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findLUSlength(vector<string>& strs) {
        int ans = -1;
        int n = strs.size();
        auto check = [&](const string& s, const string& t) {
            int m = s.size(), n = t.size();
            int i = 0;
            for (int j = 0; i < m && j < n; ++j) {
                if (s[i] == t[j]) {
                    ++i;
                }
            }
            return i == m;
        };
        for (int i = 0, j; i < n; ++i) {
            int x = strs[i].size();
            for (j = 0; j < n; ++j) {
                if (i != j && check(strs[i], strs[j])) {
                    x = -1;
                    break;
                }
            }
            ans = max(ans, x);
        }
        return ans;
    }
};
```

#### Go

```go
func findLUSlength(strs []string) int {
	ans := -1
	check := func(s, t string) bool {
		m, n := len(s), len(t)
		i := 0
		for j := 0; i < m && j < n; j++ {
			if s[i] == t[j] {
				i++
			}
		}
		return i == m
	}
	for i, s := range strs {
		x := len(s)
		for j, t := range strs {
			if i != j && check(s, t) {
				x = -1
				break
			}
		}
		ans = max(ans, x)
	}
	return ans
}
```

#### TypeScript

```ts
function findLUSlength(strs: string[]): number {
    const n = strs.length;
    let ans = -1;
    const check = (s: string, t: string): boolean => {
        const [m, n] = [s.length, t.length];
        let i = 0;
        for (let j = 0; i < m && j < n; ++j) {
            if (s[i] === t[j]) {
                ++i;
            }
        }
        return i === m;
    };
    for (let i = 0; i < n; ++i) {
        let x = strs[i].length;
        for (let j = 0; j < n; ++j) {
            if (i !== j && check(strs[i], strs[j])) {
                x = -1;
                break;
            }
        }
        ans = Math.max(ans, x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
