---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
    - Counting
    - Prefix Sum
---

<!-- problem:start -->

# [2955. Number of Same-End Substrings 🔒](https://leetcode.com/problems/number-of-same-end-substrings)

[中文文档](/solution/2900-2999/2955.Number%20of%20Same-End%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> được đánh chỉ số từ <strong>0</strong> và một mảng số nguyên 2 chiều <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> biểu diễn một substring của <code>s</code> bắt đầu tại chỉ số <code>l<sub>i</sub></code> và kết thúc tại chỉ số <code>r<sub>i</sub></code> (cả hai đều <strong>bao gồm</strong>), tức là <code>s[l<sub>i</sub>..r<sub>i</sub>]</code>.</p>

<p>Trả về <em>một mảng </em><code>ans</code><em>, trong đó</em> <code>ans[i]</code> <em>là số lượng substring <strong>cùng ký tự ở hai đầu</strong> của</em> <code>queries[i]</code>.</p>

<p>Một chuỗi <code>t</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>, được gọi là <strong>cùng ký tự ở hai đầu</strong> nếu hai đầu của nó có cùng ký tự, tức là <code>t[0] == t[n - 1]</code>.</p>

<p><b>Substring</b> là một dãy ký tự liên tiếp, không rỗng trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcaab&quot;, queries = [[0,0],[1,4],[2,5],[0,5]]
<strong>Đầu ra:</strong> [1,5,5,10]
<strong>Giải thích:</strong> Các substring có cùng ký tự ở hai đầu của mỗi query như sau:
query thứ 1<sup>st</sup>: s[0..0] là &quot;a&quot;, có 1 substring cùng ký tự ở hai đầu: &quot;<strong><u>a</u></strong>&quot;.
query thứ 2<sup>nd</sup>: s[1..4] là &quot;bcaa&quot;, có 5 substring cùng ký tự ở hai đầu: &quot;<strong><u>b</u></strong>caa&quot;, &quot;b<strong><u>c</u></strong>aa&quot;, &quot;bc<strong><u>a</u></strong>a&quot;, &quot;bca<strong><u>a</u></strong>&quot;, &quot;bc<strong><u>aa</u></strong>&quot;.
query thứ 3<sup>rd</sup>: s[2..5] là &quot;caab&quot;, có 5 substring cùng ký tự ở hai đầu: &quot;<strong><u>c</u></strong>aab&quot;, &quot;c<strong><u>a</u></strong>ab&quot;, &quot;ca<strong><u>a</u></strong>b&quot;, &quot;caa<strong><u>b</u></strong>&quot;, &quot;c<strong><u>aa</u></strong>b&quot;.
query thứ 4<sup>th</sup>: s[0..5] là &quot;abcaab&quot;, có 10 substring cùng ký tự ở hai đầu: &quot;<strong><u>a</u></strong>bcaab&quot;, &quot;a<strong><u>b</u></strong>caab&quot;, &quot;ab<strong><u>c</u></strong>aab&quot;, &quot;abc<strong><u>a</u></strong>ab&quot;, &quot;abca<strong><u>a</u></strong>b&quot;, &quot;abcaa<strong><u>b</u></strong>&quot;, &quot;abc<strong><u>aa</u></strong>b&quot;, &quot;<strong><u>abca</u></strong>ab&quot;, &quot;<strong><u>abcaa</u></strong>b&quot;, &quot;a<strong><u>bcaab</u></strong>&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, queries = [[0,3]]
<strong>Đầu ra:</strong> [4]
<strong>Giải thích:</strong> Query duy nhất là s[0..3], tức &quot;abcd&quot;. Nó có 4 substring cùng ký tự ở hai đầu: &quot;<strong><u>a</u></strong>bcd&quot;, &quot;a<strong><u>b</u></strong>cd&quot;, &quot;ab<strong><u>c</u></strong>d&quot;, &quot;abc<strong><u>d</u></strong>&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= queries.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một substring có cùng ký tự ở hai đầu tương ứng với một cặp lần xuất hiện của cùng một chữ cái trong đoạn, bao gồm cả substring chỉ có một ký tự. Số lượng query có thể lên tới $n$, nên không thể duyệt lại toàn bộ chuỗi cho từng query. Prefix count của từng chữ cái biến tần suất $x$ trong một đoạn thành $x + C(x,2)$, được triển khai bằng độ dài đoạn cộng với $C(x,2)$.
>
> $cs=set(s)$ giúp tránh cấp phát các bảng chữ cái rỗng. Mỗi query chỉ duyệt qua những chữ cái thực sự xuất hiện.

<!-- thinking:end -->

Ta có thể tính trước prefix sum cho từng chữ cái và lưu vào mảng $cnt$, trong đó $cnt[i][j]$ biểu diễn số lần xuất hiện của chữ cái thứ $i$ trong $j$ ký tự đầu tiên. Nhờ đó, với mỗi đoạn $[l, r]$, ta có thể liệt kê từng chữ cái $c$ trong đoạn và nhanh chóng tính số lần xuất hiện $x$ của $c$ bằng mảng prefix sum. Chọn tùy ý hai lần xuất hiện trong số đó để tạo thành một substring có cùng ký tự ở hai đầu, số lượng là $C_x^2=\frac{x(x-1)}{2}$. Ngoài ra, mỗi chữ cái trong đoạn cũng có thể tự tạo thành một substring có cùng ký tự ở hai đầu, tổng cộng có $r - l + 1$ ký tự. Vì vậy, với mỗi query $[l, r]$, số substring có cùng ký tự ở hai đầu là $r - l + 1 + \sum_{c \in \Sigma} \frac{x_c(x_c-1)}{2}$, trong đó $x_c$ là số lần chữ cái $c$ xuất hiện trong đoạn $[l, r]$.

Độ phức tạp thời gian là $O((n + m) \times |\Sigma|)$, còn độ phức tạp không gian là $O(n \times |\Sigma|)$. Trong đó, $n$ và $m$ lần lượt là độ dài của chuỗi $s$ và số lượng query, còn $\Sigma$ là tập các chữ cái xuất hiện trong chuỗi $s$; trong bài này, $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sameEndSubstringCount(self, s: str, queries: List[List[int]]) -> List[int]:
        n = len(s)
        cs = set(s)
        cnt = {c: [0] * (n + 1) for c in cs}
        for i, a in enumerate(s, 1):
            for c in cs:
                cnt[c][i] = cnt[c][i - 1]
            cnt[a][i] += 1
        ans = []
        for l, r in queries:
            t = r - l + 1
            for c in cs:
                x = cnt[c][r + 1] - cnt[c][l]
                t += x * (x - 1) // 2
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    public int[] sameEndSubstringCount(String s, int[][] queries) {
        int n = s.length();
        int[][] cnt = new int[26][n + 1];
        for (int j = 1; j <= n; ++j) {
            for (int i = 0; i < 26; ++i) {
                cnt[i][j] = cnt[i][j - 1];
            }
            cnt[s.charAt(j - 1) - 'a'][j]++;
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int k = 0; k < m; ++k) {
            int l = queries[k][0], r = queries[k][1];
            ans[k] = r - l + 1;
            for (int i = 0; i < 26; ++i) {
                int x = cnt[i][r + 1] - cnt[i][l];
                ans[k] += x * (x - 1) / 2;
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
    vector<int> sameEndSubstringCount(string s, vector<vector<int>>& queries) {
        int n = s.size();
        int cnt[26][n + 1];
        memset(cnt, 0, sizeof(cnt));
        for (int j = 1; j <= n; ++j) {
            for (int i = 0; i < 26; ++i) {
                cnt[i][j] = cnt[i][j - 1];
            }
            cnt[s[j - 1] - 'a'][j]++;
        }
        vector<int> ans;
        for (auto& q : queries) {
            int l = q[0], r = q[1];
            ans.push_back(r - l + 1);
            for (int i = 0; i < 26; ++i) {
                int x = cnt[i][r + 1] - cnt[i][l];
                ans.back() += x * (x - 1) / 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sameEndSubstringCount(s string, queries [][]int) []int {
	n := len(s)
	cnt := make([][]int, 26)
	for i := 0; i < 26; i++ {
		cnt[i] = make([]int, n+1)
	}

	for j := 1; j <= n; j++ {
		for i := 0; i < 26; i++ {
			cnt[i][j] = cnt[i][j-1]
		}
		cnt[s[j-1]-'a'][j]++
	}

	var ans []int
	for _, q := range queries {
		l, r := q[0], q[1]
		ans = append(ans, r-l+1)
		for i := 0; i < 26; i++ {
			x := cnt[i][r+1] - cnt[i][l]
			ans[len(ans)-1] += x * (x - 1) / 2
		}
	}

	return ans
}
```

#### TypeScript

```ts
function sameEndSubstringCount(s: string, queries: number[][]): number[] {
    const n: number = s.length;
    const cnt: number[][] = Array.from({ length: 26 }, () => Array(n + 1).fill(0));
    for (let j = 1; j <= n; j++) {
        for (let i = 0; i < 26; i++) {
            cnt[i][j] = cnt[i][j - 1];
        }
        cnt[s.charCodeAt(j - 1) - 'a'.charCodeAt(0)][j]++;
    }
    const ans: number[] = [];
    for (const [l, r] of queries) {
        ans.push(r - l + 1);
        for (let i = 0; i < 26; i++) {
            const x: number = cnt[i][r + 1] - cnt[i][l];
            ans[ans.length - 1] += (x * (x - 1)) / 2;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn same_end_substring_count(s: String, queries: Vec<Vec<i32>>) -> Vec<i32> {
        let n = s.len();
        let mut cnt: Vec<Vec<i32>> = vec![vec![0; n + 1]; 26];
        for j in 1..=n {
            for i in 0..26 {
                cnt[i][j] = cnt[i][j - 1];
            }
            cnt[(s.as_bytes()[j - 1] as usize) - (b'a' as usize)][j] += 1;
        }
        let mut ans: Vec<i32> = Vec::new();
        for q in queries.iter() {
            let l = q[0] as usize;
            let r = q[1] as usize;
            let mut t = (r - l + 1) as i32;
            for i in 0..26 {
                let x = cnt[i][r + 1] - cnt[i][l];
                t += (x * (x - 1)) / 2;
            }
            ans.push(t);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
