---
comments: true
difficulty: Easy
rating: 1273
source: Biweekly Contest 65 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2068. Check Whether Two Strings are Almost Equivalent](https://leetcode.com/problems/check-whether-two-strings-are-almost-equivalent)

[中文文档](/solution/2000-2099/2068.Check%20Whether%20Two%20Strings%20are%20Almost%20Equivalent/README.md)

## Mô tả

<!-- description:start -->

<p>Hai chuỗi <code>word1</code> và <code>word2</code> được gọi là <strong>gần tương đương</strong> nếu độ chênh lệch giữa số lần xuất hiện của mỗi chữ cái từ <code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code> trong <code>word1</code> và <code>word2</code> <strong>không vượt quá</strong> <code>3</code>.</p>

<p>Cho hai chuỗi <code>word1</code> và <code>word2</code>, mỗi chuỗi có độ dài <code>n</code>, hãy trả về <code>true</code> <em>nếu </em><code>word1</code> <em>và </em><code>word2</code> <em>là <strong>gần tương đương</strong>, hoặc</em> <code>false</code> <em>ngược lại</em>.</p>

<p><strong>Tần suất</strong> của một chữ cái <code>x</code> là số lần chữ cái đó xuất hiện trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;aaaa&quot;, word2 = &quot;bccb&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có 4 chữ &#39;a&#39; trong &quot;aaaa&quot; nhưng có 0 chữ &#39;a&#39; trong &quot;bccb&quot;.
Độ chênh lệch là 4, lớn hơn 3 là giới hạn cho phép.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;abcdeef&quot;, word2 = &quot;abaaacc&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Độ chênh lệch giữa tần suất của mỗi chữ cái trong word1 và word2 không vượt quá 3:
- Chữ &#39;a&#39; xuất hiện 1 lần trong word1 và 4 lần trong word2. Độ chênh lệch là 3.
- Chữ &#39;b&#39; xuất hiện 1 lần trong word1 và 1 lần trong word2. Độ chênh lệch là 0.
- Chữ &#39;c&#39; xuất hiện 1 lần trong word1 và 2 lần trong word2. Độ chênh lệch là 1.
- Chữ &#39;d&#39; xuất hiện 1 lần trong word1 và 0 lần trong word2. Độ chênh lệch là 1.
- Chữ &#39;e&#39; xuất hiện 2 lần trong word1 và 0 lần trong word2. Độ chênh lệch là 2.
- Chữ &#39;f&#39; xuất hiện 1 lần trong word1 và 0 lần trong word2. Độ chênh lệch là 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;cccddabba&quot;, word2 = &quot;babababab&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Độ chênh lệch giữa tần suất của mỗi chữ cái trong word1 và word2 không vượt quá 3:
- Chữ &#39;a&#39; xuất hiện 2 lần trong word1 và 4 lần trong word2. Độ chênh lệch là 2.
- Chữ &#39;b&#39; xuất hiện 2 lần trong word1 và 5 lần trong word2. Độ chênh lệch là 3.
- Chữ &#39;c&#39; xuất hiện 3 lần trong word1 và 0 lần trong word2. Độ chênh lệch là 3.
- Chữ &#39;d&#39; xuất hiện 2 lần trong word1 và 0 lần trong word2. Độ chênh lệch là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == word1.length == word2.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Với hai chuỗi có cùng độ dài và $n \le 100$, độ chênh lệch tần suất của mỗi chữ cái phải không vượt quá $3$. Một bộ đếm cộng tần suất của $word1$ và trừ tần suất của $word2$; sau đó kiểm tra các giá trị tuyệt đối.

<!-- thinking:end -->

Ta có thể tạo một mảng $cnt$ có độ dài $26$ để ghi lại độ chênh lệch số lần mỗi chữ cái xuất hiện trong hai chuỗi. Sau đó, ta duyệt qua $cnt$; nếu độ chênh lệch của bất kỳ chữ cái nào lớn hơn $3$, ta trả về `false`, ngược lại trả về `true`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài chuỗi và $C$ là kích thước của tập ký tự; trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkAlmostEquivalent(self, word1: str, word2: str) -> bool:
        cnt = Counter(word1)
        for c in word2:
            cnt[c] -= 1
        return all(abs(x) <= 3 for x in cnt.values())
```

#### Java

```java
class Solution {
    public boolean checkAlmostEquivalent(String word1, String word2) {
        int[] cnt = new int[26];
        for (int i = 0; i < word1.length(); ++i) {
            ++cnt[word1.charAt(i) - 'a'];
        }
        for (int i = 0; i < word2.length(); ++i) {
            --cnt[word2.charAt(i) - 'a'];
        }
        for (int x : cnt) {
            if (Math.abs(x) > 3) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkAlmostEquivalent(string word1, string word2) {
        int cnt[26]{};
        for (char& c : word1) {
            ++cnt[c - 'a'];
        }
        for (char& c : word2) {
            --cnt[c - 'a'];
        }
        for (int i = 0; i < 26; ++i) {
            if (abs(cnt[i]) > 3) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkAlmostEquivalent(word1 string, word2 string) bool {
	cnt := [26]int{}
	for _, c := range word1 {
		cnt[c-'a']++
	}
	for _, c := range word2 {
		cnt[c-'a']--
	}
	for _, x := range cnt {
		if x > 3 || x < -3 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function checkAlmostEquivalent(word1: string, word2: string): boolean {
    const cnt: number[] = new Array(26).fill(0);
    for (const c of word1) {
        ++cnt[c.charCodeAt(0) - 97];
    }
    for (const c of word2) {
        --cnt[c.charCodeAt(0) - 97];
    }
    return cnt.every(x => Math.abs(x) <= 3);
}
```

#### JavaScript

```js
/**
 * @param {string} word1
 * @param {string} word2
 * @return {boolean}
 */
var checkAlmostEquivalent = function (word1, word2) {
    const m = new Map();
    for (let i = 0; i < word1.length; i++) {
        m.set(word1[i], (m.get(word1[i]) || 0) + 1);
        m.set(word2[i], (m.get(word2[i]) || 0) - 1);
    }
    for (const v of m.values()) {
        if (Math.abs(v) > 3) {
            return false;
        }
    }
    return true;
};
```

#### C#

```cs
public class Solution {
    public bool CheckAlmostEquivalent(string word1, string word2) {
        int[] cnt = new int[26];
        foreach (var c in word1) {
            cnt[c - 'a']++;
        }
        foreach (var c in word2) {
            cnt[c - 'a']--;
        }
        return cnt.All(x => Math.Abs(x) <= 3);
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $word1
     * @param String $word2
     * @return Boolean
     */
    function checkAlmostEquivalent($word1, $word2) {
        for ($i = 0; $i < strlen($word1); $i++) {
            $hashtable[$word1[$i]] += 1;
            $hashtable[$word2[$i]] -= 1;
        }
        $keys = array_keys($hashtable);
        for ($j = 0; $j < count($keys); $j++) {
            if (abs($hashtable[$keys[$j]]) > 3) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
