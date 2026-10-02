---
comments: true
difficulty: Easy
rating: 1288
source: Biweekly Contest 41 Q1
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1684. Count the Number of Consistent Strings](https://leetcode.com/problems/count-the-number-of-consistent-strings)

[中文文档](/solution/1600-1699/1684.Count%20the%20Number%20of%20Consistent%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>allowed</code> gồm các ký tự <strong>phân biệt</strong> và mảng chuỗi <code>words</code>. Một chuỗi là <strong>nhất quán</strong> nếu mọi ký tự trong chuỗi đều xuất hiện trong chuỗi <code>allowed</code>.</p>

<p>Hãy trả về <em>số chuỗi <strong>nhất quán</strong> trong mảng </em><code>words</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> allowed = &quot;ab&quot;, words = [&quot;ad&quot;,&quot;bd&quot;,&quot;aaab&quot;,&quot;baa&quot;,&quot;badab&quot;]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Các chuỗi &quot;aaab&quot; và &quot;baa&quot; là nhất quán vì chúng chỉ chứa các ký tự &#39;a&#39; và &#39;b&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> allowed = &quot;abc&quot;, words = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;ab&quot;,&quot;ac&quot;,&quot;bc&quot;,&quot;abc&quot;]
<strong>Output:</strong> 7
<strong>Giải thích:</strong> Tất cả chuỗi đều nhất quán.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> allowed = &quot;cad&quot;, words = [&quot;cc&quot;,&quot;acd&quot;,&quot;b&quot;,&quot;ba&quot;,&quot;bac&quot;,&quot;bad&quot;,&quot;ac&quot;,&quot;d&quot;]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Các chuỗi &quot;cc&quot;, &quot;acd&quot;, &quot;ac&quot; và &quot;d&quot; là nhất quán.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= allowed.length &lt;=<sup> </sup>26</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li>Các ký tự trong <code>allowed</code> là <strong>phân biệt</strong>.</li>
	<li><code>words[i]</code> và <code>allowed</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Một từ nhất quán chỉ dùng các chữ cái trong $\textit{allowed}$. Đưa $\textit{allowed}$ vào một set rồi kiểm tra mọi ký tự của từng từ có nằm trong set hay không.

<!-- thinking:end -->

Cách trực tiếp là dùng hash table hoặc mảng $s$ để lưu các ký tự trong `allowed`. Sau đó duyệt mảng `words`, với mỗi chuỗi $w$, kiểm tra xem nó có chỉ gồm các ký tự trong `allowed` hay không. Nếu có, tăng đáp án.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(C)$. Trong đó, $m$ là tổng độ dài của tất cả chuỗi và $C$ là kích thước tập ký tự `allowed`. Trong bài này, $C \leq 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countConsistentStrings(self, allowed: str, words: List[str]) -> int:
        s = set(allowed)
        return sum(all(c in s for c in w) for w in words)
```

#### Java

```java
class Solution {
    public int countConsistentStrings(String allowed, String[] words) {
        boolean[] s = new boolean[26];
        for (char c : allowed.toCharArray()) {
            s[c - 'a'] = true;
        }
        int ans = 0;
        for (String w : words) {
            if (check(w, s)) {
                ++ans;
            }
        }
        return ans;
    }

    private boolean check(String w, boolean[] s) {
        for (int i = 0; i < w.length(); ++i) {
            if (!s[w.charAt(i) - 'a']) {
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
    int countConsistentStrings(string allowed, vector<string>& words) {
        bitset<26> s;
        for (auto& c : allowed) s[c - 'a'] = 1;
        int ans = 0;
        auto check = [&](string& w) {
            for (auto& c : w)
                if (!s[c - 'a']) return false;
            return true;
        };
        for (auto& w : words) ans += check(w);
        return ans;
    }
};
```

#### Go

```go
func countConsistentStrings(allowed string, words []string) (ans int) {
	s := [26]bool{}
	for _, c := range allowed {
		s[c-'a'] = true
	}
	check := func(w string) bool {
		for _, c := range w {
			if !s[c-'a'] {
				return false
			}
		}
		return true
	}
	for _, w := range words {
		if check(w) {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countConsistentStrings(allowed: string, words: string[]): number {
    const set = new Set([...allowed]);
    const n = words.length;
    let ans = n;
    for (const word of words) {
        for (const c of word) {
            if (!set.has(c)) {
                ans--;
                break;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_consistent_strings(allowed: String, words: Vec<String>) -> i32 {
        let n = words.len();
        let mut make = [false; 26];
        for c in allowed.as_bytes() {
            make[(c - b'a') as usize] = true;
        }
        let mut ans = n as i32;
        for word in words.iter() {
            for c in word.as_bytes().iter() {
                if !make[(c - b'a') as usize] {
                    ans -= 1;
                    break;
                }
            }
        }
        ans
    }
}
```

#### C

```c
int countConsistentStrings(char* allowed, char** words, int wordsSize) {
    int n = strlen(allowed);
    int make[26] = {0};
    for (int i = 0; i < n; i++) {
        make[allowed[i] - 'a'] = 1;
    }
    int ans = wordsSize;
    for (int i = 0; i < wordsSize; i++) {
        char* word = words[i];
        for (int j = 0; j < strlen(word); j++) {
            if (!make[word[j] - 'a']) {
                ans--;
                break;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng hash set. Với $26$ chữ cái, một bit mask có thể mã hóa bảng chữ cái: một từ hợp lệ khi phép OR của hai mask bằng mask của $\textit{allowed}$.

<!-- thinking:end -->

Ta cũng có thể dùng một số nguyên để biểu diễn sự xuất hiện của các ký tự trong mỗi chuỗi. Trong biểu diễn nhị phân của số nguyên này, mỗi bit cho biết một ký tự có xuất hiện hay không.

Ta định nghĩa hàm $f(w)$ để chuyển chuỗi $w$ thành một số nguyên. Mỗi bit trong biểu diễn nhị phân cho biết một ký tự có xuất hiện. Ví dụ, chuỗi `ab` được chuyển thành số nguyên $3$, có biểu diễn nhị phân là $11$. Chuỗi `abd` được chuyển thành số nguyên $11$, có biểu diễn nhị phân là $1011$.

Quay lại bài toán, để xác định chuỗi $w$ có chỉ gồm các ký tự trong `allowed` hay không, ta kiểm tra kết quả phép OR bit giữa $f(allowed)$ và $f(w)$ có bằng $f(allowed)$ hay không. Nếu có, tăng đáp án.

Độ phức tạp thời gian là $O(m)$, trong đó $m$ là tổng độ dài của tất cả chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countConsistentStrings(self, allowed: str, words: List[str]) -> int:
        def f(w):
            return reduce(or_, (1 << (ord(c) - ord('a')) for c in w))

        mask = f(allowed)
        return sum((mask | f(w)) == mask for w in words)
```

#### Java

```java
class Solution {
    public int countConsistentStrings(String allowed, String[] words) {
        int mask = f(allowed);
        int ans = 0;
        for (String w : words) {
            if ((mask | f(w)) == mask) {
                ++ans;
            }
        }
        return ans;
    }

    private int f(String w) {
        int mask = 0;
        for (int i = 0; i < w.length(); ++i) {
            mask |= 1 << (w.charAt(i) - 'a');
        }
        return mask;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countConsistentStrings(string allowed, vector<string>& words) {
        auto f = [](string& w) {
            int mask = 0;
            for (auto& c : w) mask |= 1 << (c - 'a');
            return mask;
        };
        int mask = f(allowed);
        int ans = 0;
        for (auto& w : words) ans += (mask | f(w)) == mask;
        return ans;
    }
};
```

#### Go

```go
func countConsistentStrings(allowed string, words []string) (ans int) {
	f := func(w string) (mask int) {
		for _, c := range w {
			mask |= 1 << (c - 'a')
		}
		return
	}

	mask := f(allowed)
	for _, w := range words {
		if (mask | f(w)) == mask {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countConsistentStrings(allowed: string, words: string[]): number {
    const helper = (s: string) => {
        let res = 0;
        for (const c of s) {
            res |= 1 << (c.charCodeAt(0) - 'a'.charCodeAt(0));
        }
        return res;
    };
    const mask = helper(allowed);
    let ans = 0;
    for (const word of words) {
        if ((mask | helper(word)) === mask) {
            ans++;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    fn helper(s: &String) -> i32 {
        let mut res = 0;
        for c in s.as_bytes().iter() {
            res |= 1 << ((c - b'a') as i32);
        }
        res
    }

    pub fn count_consistent_strings(allowed: String, words: Vec<String>) -> i32 {
        let mask = Self::helper(&allowed);
        let mut ans = 0;
        for word in words.iter() {
            if (mask | Self::helper(word)) == mask {
                ans += 1;
            }
        }
        ans
    }
}
```

#### C

```c
int helper(char* s) {
    int res = 0;
    int n = strlen(s);
    for (int i = 0; i < n; i++) {
        res |= 1 << (s[i] - 'a');
    }
    return res;
}

int countConsistentStrings(char* allowed, char** words, int wordsSize) {
    int mask = helper(allowed);
    int ans = 0;
    for (int i = 0; i < wordsSize; i++) {
        if ((mask | helper(words[i])) == mask) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
