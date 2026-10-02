---
comments: true
difficulty: Medium
rating: 2184
source: Weekly Contest 140 Q4
tags:
    - Stack
    - Greedy
    - String
    - Monotonic Stack
---

<!-- problem:start -->

# [1081. Smallest Subsequence of Distinct Characters](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters)

[中文文档](/solution/1000-1099/1081.Smallest%20Subsequence%20of%20Distinct%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về <em><span data-keyword="lexicographically-smaller-string">chuỗi nhỏ nhất theo thứ tự từ điển</span> là một <span data-keyword="subsequence-string">dãy con</span> của</em> <code>s</code>, <em>chứa mỗi ký tự khác nhau trong</em> <code>s</code> <em>đúng một lần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;bcabc&quot;
<strong>Output:</strong> &quot;abc&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;cbacdcbc&quot;
<strong>Output:</strong> &quot;acdb&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p>&nbsp;</p>
<strong>Lưu ý:</strong> Bài này giống bài 316: <a href="https://leetcode.com/problems/remove-duplicate-letters/" target="_blank">https://leetcode.com/problems/remove-duplicate-letters/</a>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần giữ mỗi ký tự khác nhau đúng một lần trong dãy con nhỏ nhất mà vẫn bảo toàn thứ tự. Với $n\le 1000$, có thể dùng monotonic stack: nếu phần tử trên cùng lớn hơn ký tự hiện tại và vẫn xuất hiện về sau thì có thể pop nó.
>
> Ghi lại chỉ số xuất hiện cuối cùng của mỗi ký tự. Bỏ qua ký tự đã có trong stack; nếu chưa có thì pop các ký tự trên cùng lớn hơn và còn xuất hiện về sau, rồi push ký tự hiện tại.
>
> Dùng một set để theo dõi ký tự nào đã có. Thứ tự trong stack chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestSubsequence(self, s: str) -> str:
        last = {c: i for i, c in enumerate(s)}
        stk = []
        vis = set()
        for i, c in enumerate(s):
            if c in vis:
                continue
            while stk and stk[-1] > c and last[stk[-1]] > i:
                vis.remove(stk.pop())
            stk.append(c)
            vis.add(c)
        return "".join(stk)
```

#### Java

```java
class Solution {
    public String smallestSubsequence(String text) {
        int[] cnt = new int[26];
        for (char c : text.toCharArray()) {
            ++cnt[c - 'a'];
        }
        boolean[] vis = new boolean[26];
        char[] cs = new char[text.length()];
        int top = -1;
        for (char c : text.toCharArray()) {
            --cnt[c - 'a'];
            if (!vis[c - 'a']) {
                while (top >= 0 && c < cs[top] && cnt[cs[top] - 'a'] > 0) {
                    vis[cs[top--] - 'a'] = false;
                }
                cs[++top] = c;
                vis[c - 'a'] = true;
            }
        }
        return String.valueOf(cs, 0, top + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string smallestSubsequence(string s) {
        int n = s.size();
        int last[26] = {0};
        for (int i = 0; i < n; ++i) {
            last[s[i] - 'a'] = i;
        }
        string ans;
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            char c = s[i];
            if ((mask >> (c - 'a')) & 1) {
                continue;
            }
            while (!ans.empty() && ans.back() > c && last[ans.back() - 'a'] > i) {
                mask ^= 1 << (ans.back() - 'a');
                ans.pop_back();
            }
            ans.push_back(c);
            mask |= 1 << (c - 'a');
        }
        return ans;
    }
};
```

#### Go

```go
func smallestSubsequence(s string) string {
	last := make([]int, 26)
	for i, c := range s {
		last[c-'a'] = i
	}
	stk := []rune{}
	vis := make([]bool, 128)
	for i, c := range s {
		if vis[c] {
			continue
		}
		for len(stk) > 0 && stk[len(stk)-1] > c && last[stk[len(stk)-1]-'a'] > i {
			vis[stk[len(stk)-1]] = false
			stk = stk[:len(stk)-1]
		}
		stk = append(stk, c)
		vis[c] = true
	}
	return string(stk)
}
```

#### TypeScript

```ts
function smallestSubsequence(s: string): string {
    const f = (c: string): number => c.charCodeAt(0) - 'a'.charCodeAt(0);
    const last: number[] = new Array(26).fill(0);
    for (const [i, c] of [...s].entries()) {
        last[f(c)] = i;
    }
    const stk: string[] = [];
    let mask = 0;
    for (const [i, c] of [...s].entries()) {
        const x = f(c);
        if ((mask >> x) & 1) {
            continue;
        }
        while (stk.length && stk[stk.length - 1] > c && last[f(stk[stk.length - 1])] > i) {
            mask ^= 1 << f(stk.pop()!);
        }
        stk.push(c);
        mask |= 1 << x;
    }
    return stk.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng hash set để theo dõi phần tử. Vì chỉ có 26 chữ cái nên có thể dùng bit mask, bật hoặc tắt bit tương ứng khi push hoặc pop một ký tự.
>
> Các chỉ số xuất hiện cuối cùng và quy tắc pop đơn điệu vẫn giữ nguyên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Java

```java
class Solution {
    public String smallestSubsequence(String s) {
        int n = s.length();
        int[] last = new int[26];
        for (int i = 0; i < n; ++i) {
            last[s.charAt(i) - 'a'] = i;
        }
        Deque<Character> stk = new ArrayDeque<>();
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            char c = s.charAt(i);
            if (((mask >> (c - 'a')) & 1) == 1) {
                continue;
            }
            while (!stk.isEmpty() && stk.peek() > c && last[stk.peek() - 'a'] > i) {
                mask ^= 1 << (stk.pop() - 'a');
            }
            stk.push(c);
            mask |= 1 << (c - 'a');
        }
        StringBuilder ans = new StringBuilder();
        for (char c : stk) {
            ans.append(c);
        }
        return ans.reverse().toString();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
