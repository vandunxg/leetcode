---
comments: true
difficulty: Easy
rating: 1155
source: Weekly Contest 303 Q1
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2351. First Letter to Appear Twice](https://leetcode.com/problems/first-letter-to-appear-twice)

[中文文档](/solution/2300-2399/2351.First%20Letter%20to%20Appear%20Twice/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường, hãy trả về <em>chữ cái đầu tiên xuất hiện <strong>hai lần</strong></em>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li>Một chữ cái <code>a</code> xuất hiện hai lần trước chữ cái <code>b</code> nếu lần xuất hiện <strong>thứ hai</strong> của <code>a</code> đứng trước lần xuất hiện <strong>thứ hai</strong> của <code>b</code>.</li>
	<li><code>s</code> sẽ chứa ít nhất một chữ cái xuất hiện hai lần.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abccbaacz&quot;
<strong>Đầu ra:</strong> &quot;c&quot;
<strong>Giải thích:</strong>
Chữ cái &#39;a&#39; xuất hiện tại các chỉ số 0, 5 và 6.
Chữ cái &#39;b&#39; xuất hiện tại các chỉ số 1 và 4.
Chữ cái &#39;c&#39; xuất hiện tại các chỉ số 2, 3 và 7.
Chữ cái &#39;z&#39; xuất hiện tại chỉ số 8.
Chữ cái &#39;c&#39; là chữ cái đầu tiên xuất hiện hai lần, vì trong tất cả các chữ cái, chỉ số xuất hiện lần thứ hai của nó là nhỏ nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdd&quot;
<strong>Đầu ra:</strong> &quot;d&quot;
<strong>Giải thích:</strong>
Chỉ có chữ cái &#39;d&#39; xuất hiện hai lần, nên ta trả về &#39;d&#39;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>s</code> có ít nhất một chữ cái lặp lại.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm chữ cái đầu tiên có số lần xuất hiện đạt hai. Vì $s$ ngắn và chắc chắn có chữ cái lặp lại, chỉ cần duyệt qua chuỗi một lần.
>
> Tăng bộ đếm trong map hoặc mảng; trả về ngay khi một khóa đạt giá trị $2$.

<!-- thinking:end -->

Ta duyệt qua chuỗi $s$, sử dụng mảng hoặc hash table `cnt` để ghi lại số lần xuất hiện của mỗi chữ cái. Khi một chữ cái xuất hiện hai lần, ta trả về chữ cái đó.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài của chuỗi $s$, còn $C$ là kích thước của tập ký tự. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def repeatedCharacter(self, s: str) -> str:
        cnt = Counter()
        for c in s:
            cnt[c] += 1
            if cnt[c] == 2:
                return c
```

#### Java

```java
class Solution {
    public char repeatedCharacter(String s) {
        int[] cnt = new int[26];
        for (int i = 0;; ++i) {
            char c = s.charAt(i);
            if (++cnt[c - 'a'] == 2) {
                return c;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    char repeatedCharacter(string s) {
        int cnt[26]{};
        for (int i = 0;; ++i) {
            if (++cnt[s[i] - 'a'] == 2) {
                return s[i];
            }
        }
    }
};
```

#### Go

```go
func repeatedCharacter(s string) byte {
	cnt := [26]int{}
	for i := 0; ; i++ {
		cnt[s[i]-'a']++
		if cnt[s[i]-'a'] == 2 {
			return s[i]
		}
	}
}
```

#### TypeScript

```ts
function repeatedCharacter(s: string): string {
    const vis = new Array(26).fill(false);
    for (const c of s) {
        const i = c.charCodeAt(0) - 'a'.charCodeAt(0);
        if (vis[i]) {
            return c;
        }
        vis[i] = true;
    }
    return ' ';
}
```

#### Rust

```rust
impl Solution {
    pub fn repeated_character(s: String) -> char {
        let mut vis = [false; 26];
        for &c in s.as_bytes() {
            if vis[(c - b'a') as usize] {
                return c as char;
            }
            vis[(c - b'a') as usize] = true;
        }
        ' '
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return String
     */
    function repeatedCharacter($s) {
        for ($i = 0; ; $i++) {
            $hashtable[$s[$i]] += 1;
            if ($hashtable[$s[$i]] == 2) {
                return $s[$i];
            }
        }
    }
}
```

#### C

```c
char repeatedCharacter(char* s) {
    int vis[26] = {0};
    for (int i = 0; s[i]; i++) {
        if (vis[s[i] - 'a']) {
            return s[i];
        }
        vis[s[i] - 'a']++;
    }
    return ' ';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 lưu số lần xuất hiện. Ta chỉ cần biết một chữ cái đã xuất hiện hay chưa, nên có thể dùng một bit cho mỗi chữ cái trong một số nguyên để đạt không gian hằng số.

<!-- thinking:end -->

Ta cũng có thể sử dụng một số nguyên `mask` để ghi lại việc mỗi chữ cái đã xuất hiện hay chưa, trong đó bit thứ $i$ của `mask` cho biết chữ cái thứ $i$ đã xuất hiện hay chưa. Khi một chữ cái xuất hiện hai lần, ta trả về chữ cái đó.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def repeatedCharacter(self, s: str) -> str:
        mask = 0
        for c in s:
            i = ord(c) - ord('a')
            if mask >> i & 1:
                return c
            mask |= 1 << i
```

#### Java

```java
class Solution {
    public char repeatedCharacter(String s) {
        int mask = 0;
        for (int i = 0;; ++i) {
            char c = s.charAt(i);
            if ((mask >> (c - 'a') & 1) == 1) {
                return c;
            }
            mask |= 1 << (c - 'a');
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    char repeatedCharacter(string s) {
        int mask = 0;
        for (int i = 0;; ++i) {
            if (mask >> (s[i] - 'a') & 1) {
                return s[i];
            }
            mask |= 1 << (s[i] - 'a');
        }
    }
};
```

#### Go

```go
func repeatedCharacter(s string) byte {
	mask := 0
	for i := 0; ; i++ {
		if mask>>(s[i]-'a')&1 == 1 {
			return s[i]
		}
		mask |= 1 << (s[i] - 'a')
	}
}
```

#### TypeScript

```ts
function repeatedCharacter(s: string): string {
    let mask = 0;
    for (const c of s) {
        const i = c.charCodeAt(0) - 'a'.charCodeAt(0);
        if (mask & (1 << i)) {
            return c;
        }
        mask |= 1 << i;
    }
    return ' ';
}
```

#### Rust

```rust
impl Solution {
    pub fn repeated_character(s: String) -> char {
        let mut mask = 0;
        for &c in s.as_bytes() {
            if (mask & (1 << ((c - b'a') as i32))) != 0 {
                return c as char;
            }
            mask |= 1 << ((c - b'a') as i32);
        }
        ' '
    }
}
```

#### C

```c
char repeatedCharacter(char* s) {
    int mask = 0;
    for (int i = 0; s[i]; i++) {
        if (mask & (1 << s[i] - 'a')) {
            return s[i];
        }
        mask |= 1 << s[i] - 'a';
    }
    return ' ';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
