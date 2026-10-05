---
comments: true
difficulty: Medium
rating: 1364
source: Weekly Contest 478 Q2
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3760. Maximum Substrings With Distinct Start](https://leetcode.com/problems/maximum-substrings-with-distinct-start)

[中文文档](/solution/3700-3799/3760.Maximum%20Substrings%20With%20Distinct%20Start/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường.</p>

<p>Trả về một số nguyên biểu thị <strong>số lượng</strong> <span data-keyword="substring-nonempty">chuỗi con</span> tối đa mà bạn có thể tách <code>s</code> thành, sao cho mỗi <strong>chuỗi con</strong> bắt đầu bằng một <strong>ký tự khác nhau</strong> (tức là không có hai chuỗi con nào bắt đầu bằng cùng một ký tự).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tách <code>&quot;abab&quot;</code> thành <code>&quot;a&quot;</code> và <code>&quot;bab&quot;</code>.</li>
	<li>Mỗi chuỗi con bắt đầu bằng một ký tự khác nhau, cụ thể là <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code>. Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tách <code>&quot;abcd&quot;</code> thành <code>&quot;a&quot;</code>, <code>&quot;b&quot;</code>, <code>&quot;c&quot;</code> và <code>&quot;d&quot;</code>.</li>
	<li>Mỗi chuỗi con bắt đầu bằng một ký tự khác nhau. Do đó, đáp án là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaaa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tất cả ký tự trong <code>&quot;aaaa&quot;</code> đều là <code>&#39;a&#39;</code>.</li>
	<li>Chỉ có thể có một chuỗi con bắt đầu bằng <code>&#39;a&#39;</code>. Do đó, đáp án là 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phần phải bắt đầu bằng một ký tự khác nhau, nên có nhiều nhất $|\Sigma|$ phần và mỗi ký tự xuất hiện có thể bắt đầu nhiều nhất một phần. Mỗi ký tự khác nhau đều có thể tạo thành một phần riêng, vì vậy đáp án là số lượng chữ cái khác nhau trong $s$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistinct(self, s: str) -> int:
        return len(set(s))
```

#### Java

```java
class Solution {
    public int maxDistinct(String s) {
        int ans = 0;
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            if (++cnt[s.charAt(i) - 'a'] == 1) {
                ++ans;
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
    int maxDistinct(string s) {
        int ans = 0;
        int cnt[26]{};
        for (char c : s) {
            if (++cnt[c - 'a'] == 1) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxDistinct(s string) (ans int) {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
		if cnt[c-'a'] == 1 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function maxDistinct(s: string): number {
    let ans = 0;
    const cnt: number[] = Array(26).fill(0);
    for (const ch of s) {
        const idx = ch.charCodeAt(0) - 97;
        if (++cnt[idx] === 1) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
