---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [859. Buddy Strings](https://leetcode.com/problems/buddy-strings)

[中文文档](/solution/0800-0899/0859.Buddy%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>goal</code>, hãy trả về <code>true</code><em> nếu có thể đổi chỗ hai ký tự trong </em><code>s</code><em> để thu được </em><code>goal</code><em>; nếu không, trả về </em><code>false</code><em>.</em></p>

<p>Đổi chỗ hai ký tự được định nghĩa là chọn hai chỉ số <code>i</code> và <code>j</code> (đánh chỉ số từ 0) sao cho <code>i != j</code>, rồi đổi chỗ các ký tự tại <code>s[i]</code> và <code>s[j]</code>.</p>

<ul>
	<li>Ví dụ, đổi chỗ các ký tự ở chỉ số <code>0</code> và <code>2</code> trong <code>&quot;abcd&quot;</code> sẽ thu được <code>&quot;cbad&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ab&quot;, goal = &quot;ba&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bạn có thể đổi chỗ s[0] = &#39;a&#39; và s[1] = &#39;b&#39; để thu được &quot;ba&quot;, đúng bằng goal.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ab&quot;, goal = &quot;ab&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Cặp ký tự duy nhất có thể đổi chỗ là s[0] = &#39;a&#39; và s[1] = &#39;b&#39;; kết quả là &quot;ba&quot; != goal.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aa&quot;, goal = &quot;aa&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bạn có thể đổi chỗ s[0] = &#39;a&#39; và s[1] = &#39;a&#39; để thu được &quot;aa&quot;, đúng bằng goal.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, goal.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>s</code> và <code>goal</code> chỉ gồm các chữ cái thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải đổi chỗ chính xác hai ký tự của $s$ để thu được $\textit{goal}$. Nếu độ dài hoặc số lượng từng chữ cái khác nhau thì không thể thực hiện được. Vì $n\le 2\cdot 10^4$, chỉ cần duyệt một lần để tìm các vị trí khác nhau.
>
> Nếu có đúng hai vị trí khác nhau thì phép đổi chỗ sẽ thành công vì tần suất ký tự đã khớp. Nếu hai chuỗi hoàn toàn giống nhau, cần có một chữ cái xuất hiện ít nhất hai lần để có thể đổi chỗ mà không làm thay đổi chuỗi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def buddyStrings(self, s: str, goal: str) -> bool:
        m, n = len(s), len(goal)
        if m != n:
            return False
        cnt1, cnt2 = Counter(s), Counter(goal)
        if cnt1 != cnt2:
            return False
        diff = sum(s[i] != goal[i] for i in range(n))
        return diff == 2 or (diff == 0 and any(v > 1 for v in cnt1.values()))
```

#### Java

```java
class Solution {
    public boolean buddyStrings(String s, String goal) {
        int m = s.length(), n = goal.length();
        if (m != n) {
            return false;
        }
        int diff = 0;
        int[] cnt1 = new int[26];
        int[] cnt2 = new int[26];
        for (int i = 0; i < n; ++i) {
            int a = s.charAt(i), b = goal.charAt(i);
            ++cnt1[a - 'a'];
            ++cnt2[b - 'a'];
            if (a != b) {
                ++diff;
            }
        }
        boolean f = false;
        for (int i = 0; i < 26; ++i) {
            if (cnt1[i] != cnt2[i]) {
                return false;
            }
            if (cnt1[i] > 1) {
                f = true;
            }
        }
        return diff == 2 || (diff == 0 && f);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool buddyStrings(string s, string goal) {
        int m = s.size(), n = goal.size();
        if (m != n) return false;
        int diff = 0;
        vector<int> cnt1(26);
        vector<int> cnt2(26);
        for (int i = 0; i < n; ++i) {
            ++cnt1[s[i] - 'a'];
            ++cnt2[goal[i] - 'a'];
            if (s[i] != goal[i]) ++diff;
        }
        bool f = false;
        for (int i = 0; i < 26; ++i) {
            if (cnt1[i] != cnt2[i]) return false;
            if (cnt1[i] > 1) f = true;
        }
        return diff == 2 || (diff == 0 && f);
    }
};
```

#### Go

```go
func buddyStrings(s string, goal string) bool {
	m, n := len(s), len(goal)
	if m != n {
		return false
	}
	diff := 0
	cnt1 := make([]int, 26)
	cnt2 := make([]int, 26)
	for i := 0; i < n; i++ {
		cnt1[s[i]-'a']++
		cnt2[goal[i]-'a']++
		if s[i] != goal[i] {
			diff++
		}
	}
	f := false
	for i := 0; i < 26; i++ {
		if cnt1[i] != cnt2[i] {
			return false
		}
		if cnt1[i] > 1 {
			f = true
		}
	}
	return diff == 2 || (diff == 0 && f)
}
```

#### TypeScript

```ts
function buddyStrings(s: string, goal: string): boolean {
    const m = s.length;
    const n = goal.length;
    if (m != n) {
        return false;
    }
    const cnt1 = new Array(26).fill(0);
    const cnt2 = new Array(26).fill(0);
    let diff = 0;
    for (let i = 0; i < n; ++i) {
        cnt1[s.charCodeAt(i) - 'a'.charCodeAt(0)]++;
        cnt2[goal.charCodeAt(i) - 'a'.charCodeAt(0)]++;
        if (s[i] != goal[i]) {
            ++diff;
        }
    }
    for (let i = 0; i < 26; ++i) {
        if (cnt1[i] != cnt2[i]) {
            return false;
        }
    }
    return diff == 2 || (diff == 0 && cnt1.some(v => v > 1));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
