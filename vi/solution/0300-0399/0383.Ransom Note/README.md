---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [383. Ransom Note](https://leetcode.com/problems/ransom-note)

[中文文档](/solution/0300-0399/0383.Ransom%20Note/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>ransomNote</code> và <code>magazine</code>, trả về <code>true</code> nếu có thể tạo <code>ransomNote</code> bằng các chữ cái trong <code>magazine</code>, ngược lại trả về <code>false</code>.</p>

<p>Mỗi chữ cái trong <code>magazine</code> chỉ có thể được dùng một lần trong <code>ransomNote</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> ransomNote = "a", magazine = "b"
<strong>Đầu ra:</strong> false
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> ransomNote = "aa", magazine = "ab"
<strong>Đầu ra:</strong> false
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> ransomNote = "aa", magazine = "aab"
<strong>Đầu ra:</strong> true
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= ransomNote.length, magazine.length &lt;= 10<sup>5</sup></code></li>
	<li><code>ransomNote</code> và <code>magazine</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Có thể tạo `ransomNote` từ các chữ cái trong `magazine` không? Ta cần kiểm tra số lượng từng chữ cái có đủ hay không.
>
> Đếm tần suất chữ cái trong magazine, sau đó trừ dần theo từng chữ cái của ransom note; nếu số đếm âm thì không đủ. Bảng chữ cái có $26$ ký tự.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $cnt$ có độ dài $26$ để ghi lại số lần mỗi ký tự xuất hiện trong chuỗi `magazine`. Sau đó duyệt chuỗi `ransomNote`, với mỗi ký tự $c$, ta giảm $cnt[c]$ đi $1$. Nếu số đếm của $c$ nhỏ hơn $0$ sau khi giảm, nghĩa là `magazine` không có đủ ký tự $c$ để tạo `ransomNote`; khi đó trả về $false$.

Nếu không, sau khi duyệt xong, mỗi ký tự trong `ransomNote` đều có thể lấy từ `magazine`. Vì vậy, trả về $true$.

Độ phức tạp thời gian là $O(m + n)$, còn độ phức tạp không gian là $O(C)$, trong đó $m$ và $n$ lần lượt là độ dài của `ransomNote` và `magazine`; $C$ là kích thước của tập ký tự, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        cnt = Counter(magazine)
        for c in ransomNote:
            cnt[c] -= 1
            if cnt[c] < 0:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] cnt = new int[26];
        for (int i = 0; i < magazine.length(); ++i) {
            ++cnt[magazine.charAt(i) - 'a'];
        }
        for (int i = 0; i < ransomNote.length(); ++i) {
            if (--cnt[ransomNote.charAt(i) - 'a'] < 0) {
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
    bool canConstruct(string ransomNote, string magazine) {
        int cnt[26]{};
        for (char& c : magazine) {
            ++cnt[c - 'a'];
        }
        for (char& c : ransomNote) {
            if (--cnt[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canConstruct(ransomNote string, magazine string) bool {
	cnt := [26]int{}
	for _, c := range magazine {
		cnt[c-'a']++
	}
	for _, c := range ransomNote {
		cnt[c-'a']--
		if cnt[c-'a'] < 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function canConstruct(ransomNote: string, magazine: string): boolean {
    const cnt: number[] = Array(26).fill(0);
    for (const c of magazine) {
        ++cnt[c.charCodeAt(0) - 97];
    }
    for (const c of ransomNote) {
        if (--cnt[c.charCodeAt(0) - 97] < 0) {
            return false;
        }
    }
    return true;
}
```

#### C#

```cs
public class Solution {
    public bool CanConstruct(string ransomNote, string magazine) {
        int[] cnt = new int[26];
        foreach (var c in magazine) {
            ++cnt[c - 'a'];
        }
        foreach (var c in ransomNote) {
            if (--cnt[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $ransomNote
     * @param String $magazine
     * @return Boolean
     */
    function canConstruct($ransomNote, $magazine) {
        $arrM = str_split($magazine);
        for ($i = 0; $i < strlen($magazine); $i++) {
            $hashtable[$arrM[$i]] += 1;
        }
        for ($j = 0; $j < strlen($ransomNote); $j++) {
            if (!isset($hashtable[$ransomNote[$j]]) || $hashtable[$ransomNote[$j]] == 0) {
                return false;
            } else {
                $hashtable[$ransomNote[$j]] -= 1;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
