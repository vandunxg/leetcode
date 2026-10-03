---
comments: true
difficulty: Easy
rating: 1242
source: Biweekly Contest 57 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1941. Check if All Characters Have Equal Number of Occurrences](https://leetcode.com/problems/check-if-all-characters-have-equal-number-of-occurrences)

[中文文档](/solution/1900-1999/1941.Check%20if%20All%20Characters%20Have%20Equal%20Number%20of%20Occurrences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, hãy trả về <code>true</code><em> nếu </em><code>s</code><em> là một chuỗi <strong>tốt</strong>, hoặc </em><code>false</code><em> nếu ngược lại</em>.</p>

<p>Chuỗi <code>s</code> được gọi là <strong>tốt</strong> nếu <strong>tất cả</strong> các ký tự xuất hiện trong <code>s</code> có <strong>cùng</strong> số lần xuất hiện (tức là cùng tần suất).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abacbc&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các ký tự xuất hiện trong s là &#39;a&#39;, &#39;b&#39; và &#39;c&#39;. Mỗi ký tự xuất hiện 2 lần trong s.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabb&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Các ký tự xuất hiện trong s là &#39;a&#39; và &#39;b&#39;.
&#39;a&#39; xuất hiện 3 lần còn &#39;b&#39; xuất hiện 2 lần, nên số lần xuất hiện không bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần mọi tần suất ký tự bằng nhau. Sau khi đếm, tập các tần suất phải chỉ có một phần tử.
>
> Chỉ cần một lượt duyệt qua các chữ cái viết thường.

<!-- thinking:end -->

Ta dùng một hash table hoặc một mảng có độ dài $26$ gọi là $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi ký tự trong chuỗi $s$.

Tiếp theo, ta duyệt qua từng giá trị trong $\textit{cnt}$ và kiểm tra xem tất cả các giá trị khác 0 có bằng nhau hay không.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(|\Sigma|)$. Ở đây, $n$ là độ dài của chuỗi $s$, còn $\Sigma$ là kích thước của tập ký tự. Trong bài này, tập ký tự gồm các chữ cái tiếng Anh viết thường, nên $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def areOccurrencesEqual(self, s: str) -> bool:
        return len(set(Counter(s).values())) == 1
```

#### Java

```java
class Solution {
    public boolean areOccurrencesEqual(String s) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        int v = 0;
        for (int x : cnt) {
            if (x == 0) {
                continue;
            }
            if (v > 0 && v != x) {
                return false;
            }
            v = x;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool areOccurrencesEqual(string s) {
        vector<int> cnt(26);
        for (char c : s) {
            ++cnt[c - 'a'];
        }
        int v = 0;
        for (int x : cnt) {
            if (x == 0) {
                continue;
            }
            if (v && v != x) {
                return false;
            }
            v = x;
        }
        return true;
    }
};
```

#### Go

```go
func areOccurrencesEqual(s string) bool {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	v := 0
	for _, x := range cnt {
		if x == 0 {
			continue
		}
		if v > 0 && v != x {
			return false
		}
		v = x
	}
	return true
}
```

#### TypeScript

```ts
function areOccurrencesEqual(s: string): boolean {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    const v = cnt.find(v => v);
    return cnt.every(x => !x || v === x);
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return Boolean
     */
    function areOccurrencesEqual($s) {
        $cnt = array_fill(0, 26, 0);
        for ($i = 0; $i < strlen($s); $i++) {
            $cnt[ord($s[$i]) - ord('a')]++;
        }
        $v = 0;
        foreach ($cnt as $x) {
            if ($x == 0) {
                continue;
            }
            if ($v && $v != $x) {
                return false;
            }
            $v = $x;
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
