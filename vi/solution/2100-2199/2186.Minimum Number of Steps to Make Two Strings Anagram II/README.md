---
comments: true
difficulty: Medium
rating: 1253
source: Weekly Contest 282 Q2
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2186. Minimum Number of Steps to Make Two Strings Anagram II](https://leetcode.com/problems/minimum-number-of-steps-to-make-two-strings-anagram-ii)

[中文文档](/solution/2100-2199/2186.Minimum%20Number%20of%20Steps%20to%20Make%20Two%20Strings%20Anagram%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>s</code> và <code>t</code>. Trong một bước, bạn có thể thêm <strong>bất kỳ ký tự nào</strong> vào cuối <code>s</code> hoặc <code>t</code>.</p>

<p>Hãy trả về <em>số bước ít nhất để biến </em><code>s</code><em> và </em><code>t</code><em> thành <strong>anagram</strong> của nhau.</em></p>

<p><strong>Anagram</strong> của một chuỗi là một chuỗi chứa cùng các ký tự nhưng có thứ tự khác (hoặc giống) chuỗi ban đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;<strong><u>lee</u></strong>tco<u><strong>de</strong></u>&quot;, t = &quot;co<u><strong>a</strong></u>t<u><strong>s</strong></u>&quot;
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
- Trong 2 bước, ta có thể thêm các chữ cái trong &quot;as&quot; vào s = &quot;leetcode&quot;, tạo thành s = &quot;leetcode<strong><u>as</u></strong>&quot;.
- Trong 5 bước, ta có thể thêm các chữ cái trong &quot;leede&quot; vào t = &quot;coats&quot;, tạo thành t = &quot;coats<u><strong>leede</strong></u>&quot;.
&quot;leetcodeas&quot; và &quot;coatsleede&quot; lúc này là anagram của nhau.
Tổng cộng ta đã thực hiện 2 + 5 = 7 bước.
Có thể chứng minh rằng không có cách nào biến chúng thành anagram của nhau với ít hơn 7 bước.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;night&quot;, t = &quot;thing&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Hai chuỗi đã cho vốn đã là anagram của nhau. Vì vậy, ta không cần thực hiện thêm bước nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước chèn một chữ cái vào một trong hai chuỗi. Số lần chèn ít nhất chính là khoảng cách $L_1$ giữa hai vector tần suất, tức số lượng các chữ cái mà mỗi phía còn thiếu.
>
> Trừ số lần xuất hiện của các ký tự trong $t$ khỏi số lần xuất hiện trong $s$, rồi tính tổng các giá trị tuyệt đối.
>
> Chỉ cần duyệt qua hai chuỗi một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSteps(self, s: str, t: str) -> int:
        cnt = Counter(s)
        for c in t:
            cnt[c] -= 1
        return sum(abs(v) for v in cnt.values())
```

#### Java

```java
class Solution {
    public int minSteps(String s, String t) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        for (char c : t.toCharArray()) {
            --cnt[c - 'a'];
        }
        int ans = 0;
        for (int v : cnt) {
            ans += Math.abs(v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSteps(string s, string t) {
        vector<int> cnt(26);
        for (char& c : s) ++cnt[c - 'a'];
        for (char& c : t) --cnt[c - 'a'];
        int ans = 0;
        for (int& v : cnt) ans += abs(v);
        return ans;
    }
};
```

#### Go

```go
func minSteps(s string, t string) int {
	cnt := make([]int, 26)
	for _, c := range s {
		cnt[c-'a']++
	}
	for _, c := range t {
		cnt[c-'a']--
	}
	ans := 0
	for _, v := range cnt {
		ans += abs(v)
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minSteps(s: string, t: string): number {
    let cnt = new Array(128).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0)];
    }
    for (const c of t) {
        --cnt[c.charCodeAt(0)];
    }
    let ans = 0;
    for (const v of cnt) {
        ans += Math.abs(v);
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {number}
 */
var minSteps = function (s, t) {
    let cnt = new Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt() - 'a'.charCodeAt()];
    }
    for (const c of t) {
        --cnt[c.charCodeAt() - 'a'.charCodeAt()];
    }
    let ans = 0;
    for (const v of cnt) {
        ans += Math.abs(v);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
