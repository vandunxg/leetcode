---
comments: true
difficulty: Hard
rating: 2558
source: Weekly Contest 259 Q4
tags:
    - Hash Table
    - Two Pointers
    - String
    - Backtracking
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [2014. Longest Subsequence Repeated k Times](https://leetcode.com/problems/longest-subsequence-repeated-k-times)

[中文文档](/solution/2000-2099/2014.Longest%20Subsequence%20Repeated%20k%20Times/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> có độ dài <code>n</code> và một số nguyên <code>k</code>. Nhiệm vụ của bạn là tìm <strong>dãy con dài nhất lặp lại</strong> <code>k</code> lần trong chuỗi <code>s</code>.</p>

<p><strong>Dãy con</strong> là một chuỗi có thể thu được từ một chuỗi khác bằng cách xóa một số hoặc không xóa ký tự nào mà không thay đổi thứ tự của các ký tự còn lại.</p>

<p>Một dãy con <code>seq</code> được gọi là <strong>lặp lại</strong> <code>k</code> lần trong chuỗi <code>s</code> nếu <code>seq * k</code> là một dãy con của <code>s</code>, trong đó <code>seq * k</code> biểu diễn chuỗi được tạo bằng cách nối <code>seq</code> <code>k</code> lần.</p>

<ul>
	<li>Ví dụ, <code>&quot;bba&quot;</code> lặp lại <code>2</code> lần trong chuỗi <code>&quot;bababcba&quot;</code>, vì chuỗi <code>&quot;bbabba&quot;</code>, được tạo bằng cách nối <code>&quot;bba&quot;</code> <code>2</code> lần, là một dãy con của chuỗi <code>&quot;<strong><u>b</u></strong>a<strong><u>bab</u></strong>c<strong><u>ba</u></strong>&quot;</code>.</li>
</ul>

<p>Trả về <em><strong>dãy con dài nhất lặp lại</strong> </em><code>k</code><em> lần trong chuỗi </em><code>s</code><em>. Nếu có nhiều dãy con như vậy, hãy trả về dãy con <strong>lớn nhất theo thứ tự từ điển</strong>. Nếu không có dãy con nào như vậy, hãy trả về <strong>chuỗi rỗng</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="example 1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2014.Longest%20Subsequence%20Repeated%20k%20Times/images/longest-subsequence-repeat-k-times.png" style="width: 457px; height: 99px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;letsleetcode&quot;, k = 2
<strong>Đầu ra:</strong> &quot;let&quot;
<strong>Giải thích:</strong> Có hai dãy con dài nhất lặp lại 2 lần: &quot;let&quot; và &quot;ete&quot;.
&quot;let&quot; là dãy con lớn nhất theo thứ tự từ điển.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bb&quot;, k = 2
<strong>Đầu ra:</strong> &quot;b&quot;
<strong>Giải thích:</strong> Dãy con dài nhất lặp lại 2 lần là &quot;b&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ab&quot;, k = 2
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không có dãy con nào lặp lại 2 lần. Trả về chuỗi rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s.length</code></li>
	<li><code>2 &lt;= k &lt;= 2000</code></li>
	<li><code>2 &lt;= n &lt; min(2001, k * 8)</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n < \min(2001, k \cdot 8)$, một dãy con lặp lại $k$ lần có độ dài nhiều nhất là $\lfloor n/k \rfloor \le 7$. Một chữ cái phải xuất hiện ít nhất $k$ lần mới có thể xuất hiện trong đáp án.
>
> Với các ứng viên ngắn, BFS từ các chuỗi ngắn đến dài khiến lần thành công cuối cùng là chuỗi dài nhất, đồng thời việc thêm các chữ cái theo thứ tự giúp chuỗi có thứ tự từ điển lớn nhất trong số các chuỗi cùng độ dài.
>
> Hàng đợi bắt đầu từ `""` và lần lượt thêm các chữ cái có số lần xuất hiện $\ge k$; `check` khớp $t$ trong $s$ qua $k$ lượt.

<!-- thinking:end -->

Trước tiên, ta đếm số lần xuất hiện của từng ký tự trong chuỗi, sau đó lưu các ký tự xuất hiện ít nhất $k$ lần vào danh sách $\textit{cs}$ theo thứ tự tăng dần. Tiếp theo, ta dùng BFS để liệt kê tất cả dãy con có thể có.

Ta định nghĩa một hàng đợi $\textit{q}$, ban đầu đưa chuỗi rỗng vào hàng đợi. Sau đó, ta lấy một chuỗi $\textit{cur}$ ra khỏi hàng đợi và thử nối từng ký tự $c \in \textit{cs}$ vào cuối $\textit{cur}$ để tạo thành chuỗi mới $\textit{nxt}$. Nếu $\textit{nxt}$ là một dãy con có thể lặp lại $k$ lần, ta cập nhật đáp án và đưa $\textit{nxt}$ trở lại hàng đợi để tiếp tục xử lý.

Ta cần một hàm phụ $\textit{check(t, k)}$ để xác định xem chuỗi $\textit{t}$ có phải là dãy con lặp lại $k$ lần của chuỗi $s$ hay không. Cụ thể, ta có thể dùng hai con trỏ để duyệt qua $s$ và $\textit{t}$. Nếu tìm thấy tất cả ký tự của $\textit{t}$ trong $s$ và lặp lại quá trình này $k$ lần, ta trả về $\textit{true}$; nếu không, trả về $\textit{false}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubsequenceRepeatedK(self, s: str, k: int) -> str:
        def check(t: str, k: int) -> bool:
            i = 0
            for c in s:
                if c == t[i]:
                    i += 1
                    if i == len(t):
                        k -= 1
                        if k == 0:
                            return True
                        i = 0
            return False

        cnt = Counter(s)
        cs = [c for c in ascii_lowercase if cnt[c] >= k]
        q = deque([""])
        ans = ""
        while q:
            cur = q.popleft()
            for c in cs:
                nxt = cur + c
                if check(nxt, k):
                    ans = nxt
                    q.append(nxt)
        return ans
```

#### Java

```java
class Solution {
    private char[] s;

    public String longestSubsequenceRepeatedK(String s, int k) {
        this.s = s.toCharArray();
        int[] cnt = new int[26];
        for (char c : this.s) {
            cnt[c - 'a']++;
        }

        List<Character> cs = new ArrayList<>();
        for (char c = 'a'; c <= 'z'; ++c) {
            if (cnt[c - 'a'] >= k) {
                cs.add(c);
            }
        }
        Deque<String> q = new ArrayDeque<>();
        q.offer("");
        String ans = "";
        while (!q.isEmpty()) {
            String cur = q.poll();
            for (char c : cs) {
                String nxt = cur + c;
                if (check(nxt, k)) {
                    ans = nxt;
                    q.offer(nxt);
                }
            }
        }
        return ans;
    }

    private boolean check(String t, int k) {
        int i = 0;
        for (char c : s) {
            if (c == t.charAt(i)) {
                i++;
                if (i == t.length()) {
                    if (--k == 0) {
                        return true;
                    }
                    i = 0;
                }
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
    string longestSubsequenceRepeatedK(string s, int k) {
        auto check = [&](const string& t, int k) -> bool {
            int i = 0;
            for (char c : s) {
                if (c == t[i]) {
                    i++;
                    if (i == t.size()) {
                        if (--k == 0) {
                            return true;
                        }
                        i = 0;
                    }
                }
            }
            return false;
        };
        int cnt[26] = {};
        for (char c : s) {
            cnt[c - 'a']++;
        }

        vector<char> cs;
        for (char c = 'a'; c <= 'z'; ++c) {
            if (cnt[c - 'a'] >= k) {
                cs.push_back(c);
            }
        }

        queue<string> q;
        q.push("");
        string ans;
        while (!q.empty()) {
            string cur = q.front();
            q.pop();
            for (char c : cs) {
                string nxt = cur + c;
                if (check(nxt, k)) {
                    ans = nxt;
                    q.push(nxt);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubsequenceRepeatedK(s string, k int) string {
	check := func(t string, k int) bool {
		i := 0
		for _, c := range s {
			if byte(c) == t[i] {
				i++
				if i == len(t) {
					k--
					if k == 0 {
						return true
					}
					i = 0
				}
			}
		}
		return false
	}

	cnt := [26]int{}
	for i := 0; i < len(s); i++ {
		cnt[s[i]-'a']++
	}

	cs := []byte{}
	for c := byte('a'); c <= 'z'; c++ {
		if cnt[c-'a'] >= k {
			cs = append(cs, c)
		}
	}

	q := []string{""}
	ans := ""
	for len(q) > 0 {
		cur := q[0]
		q = q[1:]
		for _, c := range cs {
			nxt := cur + string(c)
			if check(nxt, k) {
				ans = nxt
				q = append(q, nxt)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestSubsequenceRepeatedK(s: string, k: number): string {
    const check = (t: string, k: number): boolean => {
        let i = 0;
        for (const c of s) {
            if (c === t[i]) {
                i++;
                if (i === t.length) {
                    k--;
                    if (k === 0) {
                        return true;
                    }
                    i = 0;
                }
            }
        }
        return false;
    };

    const cnt = new Array(26).fill(0);
    for (const c of s) {
        cnt[c.charCodeAt(0) - 97]++;
    }

    const cs: string[] = [];
    for (let i = 0; i < 26; ++i) {
        if (cnt[i] >= k) {
            cs.push(String.fromCharCode(97 + i));
        }
    }

    const q: string[] = [''];
    let ans = '';
    while (q.length > 0) {
        const cur = q.shift()!;
        for (const c of cs) {
            const nxt = cur + c;
            if (check(nxt, k)) {
                ans = nxt;
                q.push(nxt);
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
