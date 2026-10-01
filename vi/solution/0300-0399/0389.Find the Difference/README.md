---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [389. Find the Difference](https://leetcode.com/problems/find-the-difference)

[中文文档](/solution/0300-0399/0389.Find%20the%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code>.</p>

<p>Chuỗi <code>t</code> được tạo bằng cách xáo trộn ngẫu nhiên chuỗi <code>s</code>, sau đó thêm một chữ cái vào vị trí bất kỳ.</p>

<p>Hãy trả về chữ cái được thêm vào <code>t</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, t = &quot;abcde&quot;
<strong>Đầu ra:</strong> &quot;e&quot;
<strong>Giải thích:</strong> &#39;e&#39; là chữ cái được thêm vào.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;&quot;, t = &quot;y&quot;
<strong>Đầu ra:</strong> &quot;y&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 1000</code></li>
	<li><code>t.length == s.length + 1</code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> $t$ là chuỗi $s$ sau khi xáo trộn và được thêm một chữ cái. Có thể tìm chữ cái đó bằng cách so sánh số lần xuất hiện.
>
> Đếm các chữ cái trong $s$, rồi trừ dần theo các chữ cái trong $t$; tần suất đầu tiên âm chính là chữ cái được thêm.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $cnt$ để đếm số lần xuất hiện của từng ký tự trong chuỗi $s$, sau đó duyệt chuỗi $t$. Với mỗi ký tự, ta giảm số đếm tương ứng trong $cnt$. Nếu số đếm trở thành âm, nghĩa là ký tự này xuất hiện trong $t$ nhiều hơn trong $s$; đó chính là ký tự được thêm vào.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi và $\Sigma$ là tập ký tự. Ở đây, tập ký tự gồm các chữ cái viết thường nên $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheDifference(self, s: str, t: str) -> str:
        cnt = Counter(s)
        for c in t:
            cnt[c] -= 1
            if cnt[c] < 0:
                return c
```

#### Java

```java
class Solution {
    public char findTheDifference(String s, String t) {
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        for (int i = 0;; ++i) {
            if (--cnt[t.charAt(i) - 'a'] < 0) {
                return t.charAt(i);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    char findTheDifference(string s, string t) {
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        for (char& c : t) {
            if (--cnt[c - 'a'] < 0) {
                return c;
            }
        }
        return ' ';
    }
};
```

#### Go

```go
func findTheDifference(s, t string) byte {
	cnt := [26]int{}
	for _, ch := range s {
		cnt[ch-'a']++
	}
	for i := 0; ; i++ {
		ch := t[i]
		cnt[ch-'a']--
		if cnt[ch-'a'] < 0 {
			return ch
		}
	}
}
```

#### TypeScript

```ts
function findTheDifference(s: string, t: string): string {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    for (const c of t) {
        --cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    for (let i = 0; ; ++i) {
        if (cnt[i] < 0) {
            return String.fromCharCode(i + 'a'.charCodeAt(0));
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_difference(s: String, t: String) -> char {
        let s = s.as_bytes();
        let t = t.as_bytes();
        let n = s.len();
        let mut count = [0; 26];
        for i in 0..n {
            count[(s[i] - b'a') as usize] += 1;
            count[(t[i] - b'a') as usize] -= 1;
        }
        count[(t[n] - b'a') as usize] -= 1;
        char::from(b'a' + (count.iter().position(|&v| v != 0).unwrap() as u8))
    }
}
```

#### C

```c
char findTheDifference(char* s, char* t) {
    int n = strlen(s);
    int cnt[26] = {0};
    for (int i = 0; i < n; i++) {
        cnt[s[i] - 'a']++;
        cnt[t[i] - 'a']--;
    }
    cnt[t[n] - 'a']--;
    for (int i = 0;; i++) {
        if (cnt[i]) {
            return 'a' + i;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Cách đếm cần $O(\Sigma)$ không gian. Lấy hiệu tổng mã ASCII sẽ cho mã ký tự được thêm, chỉ cần $O(1)$ không gian.

<!-- thinking:end -->

Ta tính tổng giá trị ASCII của các ký tự trong chuỗi $t$, rồi trừ đi tổng giá trị ASCII của các ký tự trong chuỗi $s$. Kết quả thu được là giá trị ASCII của ký tự được thêm vào.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheDifference(self, s: str, t: str) -> str:
        a = sum(ord(c) for c in s)
        b = sum(ord(c) for c in t)
        return chr(b - a)
```

#### Java

```java
class Solution {
    public char findTheDifference(String s, String t) {
        int ss = 0;
        for (int i = 0; i < t.length(); ++i) {
            ss += t.charAt(i);
        }
        for (int i = 0; i < s.length(); ++i) {
            ss -= s.charAt(i);
        }
        return (char) ss;
    }
}
```

#### C++

```cpp
class Solution {
public:
    char findTheDifference(string s, string t) {
        int a = 0, b = 0;
        for (char& c : s) {
            a += c;
        }
        for (char& c : t) {
            b += c;
        }
        return b - a;
    }
};
```

#### Go

```go
func findTheDifference(s string, t string) byte {
	ss := 0
	for _, c := range s {
		ss -= int(c)
	}
	for _, c := range t {
		ss += int(c)
	}
	return byte(ss)
}
```

#### TypeScript

```ts
function findTheDifference(s: string, t: string): string {
    return String.fromCharCode(
        [...t].reduce((r, v) => r + v.charCodeAt(0), 0) -
            [...s].reduce((r, v) => r + v.charCodeAt(0), 0),
    );
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_difference(s: String, t: String) -> char {
        let mut ans = 0;
        for c in s.as_bytes() {
            ans ^= c;
        }
        for c in t.as_bytes() {
            ans ^= c;
        }
        char::from(ans)
    }
}
```

#### C

```c
char findTheDifference(char* s, char* t) {
    int n = strlen(s);
    char ans = 0;
    for (int i = 0; i < n; i++) {
        ans ^= s[i];
        ans ^= t[i];
    }
    ans ^= t[n];
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
