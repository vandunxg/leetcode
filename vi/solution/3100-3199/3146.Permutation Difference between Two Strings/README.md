---
comments: true
difficulty: Easy
rating: 1152
source: Weekly Contest 397 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3146. Permutation Difference between Two Strings](https://leetcode.com/problems/permutation-difference-between-two-strings)

[中文文档](/solution/3100-3199/3146.Permutation%20Difference%20between%20Two%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code>, trong đó mỗi ký tự xuất hiện nhiều nhất một lần trong <code>s</code> và <code>t</code> là một hoán vị của <code>s</code>.</p>

<p><strong>Độ chênh lệch hoán vị</strong> giữa <code>s</code> và <code>t</code> được định nghĩa là <strong>tổng</strong> độ chênh lệch tuyệt đối giữa chỉ số xuất hiện của mỗi ký tự trong <code>s</code> và chỉ số xuất hiện của cùng ký tự đó trong <code>t</code>.</p>

<p>Trả về <strong>độ chênh lệch hoán vị</strong> giữa <code>s</code> và <code>t</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;, t = &quot;bac&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với <code>s = &quot;abc&quot;</code> và <code>t = &quot;bac&quot;</code>, độ chênh lệch hoán vị giữa <code>s</code> và <code>t</code> bằng tổng của:</p>

<ul>
	<li>Độ chênh lệch tuyệt đối giữa chỉ số xuất hiện của <code>&quot;a&quot;</code> trong <code>s</code> và chỉ số xuất hiện của <code>&quot;a&quot;</code> trong <code>t</code>.</li>
	<li>Độ chênh lệch tuyệt đối giữa chỉ số xuất hiện của <code>&quot;b&quot;</code> trong <code>s</code> và chỉ số xuất hiện của <code>&quot;b&quot;</code> trong <code>t</code>.</li>
	<li>Độ chênh lệch tuyệt đối giữa chỉ số xuất hiện của <code>&quot;c&quot;</code> trong <code>s</code> và chỉ số xuất hiện của <code>&quot;c&quot;</code> trong <code>t</code>.</li>
</ul>

<p>Nói cách khác, độ chênh lệch hoán vị giữa <code>s</code> và <code>t</code> bằng <code>|0 - 1| + |1 - 0| + |2 - 2| = 2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcde&quot;, t = &quot;edbac&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong> Độ chênh lệch hoán vị giữa <code>s</code> và <code>t</code> bằng <code>|0 - 3| + |1 - 2| + |2 - 4| + |3 - 1| + |4 - 0| = 12</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 26</code></li>
	<li>Mỗi ký tự xuất hiện nhiều nhất một lần trong <code>s</code>.</li>
	<li><code>t</code> là một hoán vị của <code>s</code>.</li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Độ chênh lệch là tổng khoảng cách chỉ số tuyệt đối của mỗi chữ cái trong $s$ và $t$. Nếu tìm kiếm trong chuỗi còn lại cho từng chữ cái, độ phức tạp sẽ là bậc hai.
>
> Hai chuỗi là các hoán vị, nên ánh xạ từ chữ cái đến chỉ số là song ánh và chỉ cần một map.
>
> Lưu vị trí của $s$, sau đó duyệt qua $t$ và cộng $|d[c]-i|$.

<!-- thinking:end -->

Ta có thể sử dụng một bảng băm hoặc một mảng có độ dài $26$, ký hiệu là $\textit{d}$, để lưu vị trí của mỗi ký tự trong chuỗi $\textit{s}$.

Sau đó, ta duyệt qua chuỗi $\textit{t}$ và tính tổng độ chênh lệch tuyệt đối giữa vị trí của mỗi ký tự trong chuỗi $\textit{t}$ và vị trí của ký tự đó trong chuỗi $\textit{s}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{s}$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự. Ở đây, đó là các chữ cái tiếng Anh viết thường, nên $|\Sigma| \leq 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPermutationDifference(self, s: str, t: str) -> int:
        d = {c: i for i, c in enumerate(s)}
        return sum(abs(d[c] - i) for i, c in enumerate(t))
```

#### Java

```java
class Solution {
    public int findPermutationDifference(String s, String t) {
        int[] d = new int[26];
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            d[s.charAt(i) - 'a'] = i;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += Math.abs(d[t.charAt(i) - 'a'] - i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findPermutationDifference(string s, string t) {
        int d[26]{};
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            d[s[i] - 'a'] = i;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += abs(d[t[i] - 'a'] - i);
        }
        return ans;
    }
};
```

#### Go

```go
func findPermutationDifference(s string, t string) (ans int) {
	d := [26]int{}
	for i, c := range s {
		d[c-'a'] = i
	}
	for i, c := range t {
		ans += max(d[c-'a']-i, i-d[c-'a'])
	}
	return
}
```

#### TypeScript

```ts
function findPermutationDifference(s: string, t: string): number {
    const d: number[] = Array(26).fill(0);
    const n = s.length;
    for (let i = 0; i < n; ++i) {
        d[s.charCodeAt(i) - 97] = i;
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans += Math.abs(d[t.charCodeAt(i) - 97] - i);
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public int FindPermutationDifference(string s, string t) {
        int[] d = new int[26];
        int n = s.Length;
        for (int i = 0; i < n; ++i) {
            d[s[i] - 'a'] = i;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += Math.Abs(d[t[i] - 'a'] - i);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
