---
comments: true
difficulty: Medium
rating: 1848
source: Weekly Contest 152 Q3
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [1177. Can Make Palindrome from Substring](https://leetcode.com/problems/can-make-palindrome-from-substring)

[中文文档](/solution/1100-1199/1177.Can%20Make%20Palindrome%20from%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và mảng <code>queries</code>, trong đó <code>queries[i] = [left<sub>i</sub>, right<sub>i</sub>, k<sub>i</sub>]</code>. Với mỗi query, ta có thể sắp xếp lại chuỗi con <code>s[left<sub>i</sub>...right<sub>i</sub>]</code>, sau đó chọn tối đa <code>k<sub>i</sub></code> ký tự trong đó để thay bằng bất kỳ chữ cái tiếng Anh viết thường nào.</p>

<p>Nếu sau các thao tác trên, chuỗi con có thể trở thành palindrome thì kết quả của query là <code>true</code>; ngược lại là <code>false</code>.</p>

<p>Hãy trả về mảng boolean <code>answer</code>, trong đó <code>answer[i]</code> là kết quả của query thứ <code>i<sup>th</sup></code>, tức <code>queries[i]</code>.</p>

<p>Lưu ý rằng mỗi chữ cái được tính riêng khi thay thế. Ví dụ, nếu <code>s[left<sub>i</sub>...right<sub>i</sub>] = &quot;aaa&quot;</code> và <code>k<sub>i</sub> = 2</code>, ta chỉ có thể thay hai trong số các chữ cái đó. Ngoài ra, không query nào làm thay đổi chuỗi ban đầu <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcda&quot;, queries = [[3,3,0],[1,2,0],[0,3,1],[0,3,2],[0,4,1]]
<strong>Đầu ra:</strong> [true,false,false,true,true]
<strong>Giải thích:</strong>
queries[0]: chuỗi con = &quot;d&quot;, là palindrome.
queries[1]: chuỗi con = &quot;bc&quot;, không phải palindrome.
queries[2]: chuỗi con = &quot;abcd&quot;, không thể thành palindrome nếu chỉ thay 1 ký tự.
queries[3]: chuỗi con = &quot;abcd&quot;, có thể đổi thành &quot;abba&quot; để tạo palindrome. Cũng có thể đổi thành &quot;baab&quot;: trước tiên sắp xếp lại thành &quot;bacd&quot;, sau đó thay &quot;cd&quot; bằng &quot;ab&quot;.
queries[4]: chuỗi con = &quot;abcda&quot;, có thể đổi thành &quot;abcba&quot; để tạo palindrome.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;lyb&quot;, queries = [[0,1,0],[2,2,1]]
<strong>Đầu ra:</strong> [false,true]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= left<sub>i</sub> &lt;= right<sub>i</sub> &lt; s.length</code></li>
	<li><code>0 &lt;= k<sub>i</sub> &lt;= s.length</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi con có thể thành palindrome với tối đa $k$ lần thay thế khi và chỉ khi ta ghép cặp được các ký tự có số lần xuất hiện lẻ; mỗi lần thay thế xử lý được hai ký tự lẻ. Vì có nhiều query, không thể quét lại chuỗi con mỗi lần. Prefix sum gồm $26$ bộ đếm cho biết tính chẵn lẻ trong từng khoảng; so sánh một nửa số ký tự xuất hiện lẻ với $k$ sẽ trả lời từng query.

<!-- thinking:end -->

Trước tiên, xét xem có thể biến chuỗi con thành palindrome với tối đa $k$ lần thay thế hay không. Ta cần đếm số lần xuất hiện của từng ký tự trong chuỗi con, có thể thực hiện bằng prefix sum. Ký tự xuất hiện số lần chẵn không cần thay. Với các ký tự xuất hiện số lần lẻ, số lần thay thế cần thiết là $\lfloor \frac{x}{2} \rfloor$, trong đó $x$ là số ký tự xuất hiện số lần lẻ. Nếu $\lfloor \frac{x}{2} \rfloor \leq k$, chuỗi con có thể trở thành palindrome.

Vì vậy, ta định nghĩa mảng prefix sum $ss$, trong đó $ss[i][j]$ là số lần ký tự $j$ xuất hiện trong $i$ ký tự đầu tiên của chuỗi $s$. Với chuỗi con $s[l..r]$, số lần ký tự $j$ xuất hiện được tính bằng $ss[r + 1][j] - ss[l][j]$. Ta duyệt tất cả query. Với mỗi query $[l, r, k]$, đếm số ký tự $x$ xuất hiện số lần lẻ trong chuỗi con $s[l..r]$. Nếu $\lfloor \frac{x}{2} \rfloor \leq k$, chuỗi con có thể trở thành palindrome.

Độ phức tạp thời gian là $O((n + m) \times C)$ và độ phức tạp không gian là $O(n \times C)$. Trong đó, $n$ và $m$ lần lượt là độ dài chuỗi $s$ và mảng query; $C$ là kích thước bộ ký tự. Trong bài này, bộ ký tự gồm các chữ cái tiếng Anh viết thường nên $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canMakePaliQueries(self, s: str, queries: List[List[int]]) -> List[bool]:
        n = len(s)
        ss = [[0] * 26 for _ in range(n + 1)]
        for i, c in enumerate(s, 1):
            ss[i] = ss[i - 1][:]
            ss[i][ord(c) - ord("a")] += 1
        ans = []
        for l, r, k in queries:
            cnt = sum((ss[r + 1][j] - ss[l][j]) & 1 for j in range(26))
            ans.append(cnt // 2 <= k)
        return ans
```

#### Java

```java
class Solution {
    public List<Boolean> canMakePaliQueries(String s, int[][] queries) {
        int n = s.length();
        int[][] ss = new int[n + 1][26];
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 26; ++j) {
                ss[i][j] = ss[i - 1][j];
            }
            ss[i][s.charAt(i - 1) - 'a']++;
        }
        List<Boolean> ans = new ArrayList<>();
        for (var q : queries) {
            int l = q[0], r = q[1], k = q[2];
            int x = 0;
            for (int j = 0; j < 26; ++j) {
                x += (ss[r + 1][j] - ss[l][j]) & 1;
            }
            ans.add(x / 2 <= k);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> canMakePaliQueries(string s, vector<vector<int>>& queries) {
        int n = s.size();
        int ss[n + 1][26];
        memset(ss, 0, sizeof(ss));
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 26; ++j) {
                ss[i][j] = ss[i - 1][j];
            }
            ss[i][s[i - 1] - 'a']++;
        }
        vector<bool> ans;
        for (auto& q : queries) {
            int l = q[0], r = q[1], k = q[2];
            int x = 0;
            for (int j = 0; j < 26; ++j) {
                x += (ss[r + 1][j] - ss[l][j]) & 1;
            }
            ans.emplace_back(x / 2 <= k);
        }
        return ans;
    }
};
```

#### Go

```go
func canMakePaliQueries(s string, queries [][]int) (ans []bool) {
	n := len(s)
	ss := make([][26]int, n+1)
	for i := 1; i <= n; i++ {
		for j := 0; j < 26; j++ {
			ss[i][j] = ss[i-1][j]
		}
		ss[i][s[i-1]-'a']++
	}
	for _, q := range queries {
		l, r, k := q[0], q[1], q[2]
		x := 0
		for j := 0; j < 26; j++ {
			x += (ss[r+1][j] - ss[l][j]) & 1
		}
		ans = append(ans, x/2 <= k)
	}
	return
}
```

#### TypeScript

```ts
function canMakePaliQueries(s: string, queries: number[][]): boolean[] {
    const n = s.length;
    const ss: number[][] = Array(n + 1)
        .fill(0)
        .map(() => Array(26).fill(0));
    for (let i = 1; i <= n; ++i) {
        ss[i] = ss[i - 1].slice();
        ++ss[i][s.charCodeAt(i - 1) - 97];
    }
    const ans: boolean[] = [];
    for (const [l, r, k] of queries) {
        let x = 0;
        for (let j = 0; j < 26; ++j) {
            x += (ss[r + 1][j] - ss[l][j]) & 1;
        }
        ans.push(x >> 1 <= k);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
