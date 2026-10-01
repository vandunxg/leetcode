---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [242. Valid Anagram](https://leetcode.com/problems/valid-anagram)

[Tài liệu tiếng Trung](/solution/0200-0299/0242.Valid%20Anagram/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code>, hãy trả về <code>true</code> nếu <code>t</code> là một <span data-keyword="anagram">anagram</span> của <code>s</code>, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;anagram&quot;, t = &quot;nagaram&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;rat&quot;, t = &quot;car&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu đầu vào chứa các ký tự Unicode thì sao? Bạn sẽ điều chỉnh lời giải cho trường hợp đó như thế nào?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Hai anagram có số lần xuất hiện của từng ký tự giống nhau. Nếu độ dài khác nhau thì có thể kết luận ngay.
>
> Đếm các ký tự trong $s$, sau đó giảm bộ đếm khi duyệt $t$; nếu một bộ đếm âm thì hai chuỗi khác nhau.

<!-- thinking:end -->

Trước tiên, ta xác định độ dài của hai chuỗi có bằng nhau hay không. Nếu không bằng nhau, các ký tự trong hai chuỗi chắc chắn khác nhau, nên trả về `false`.

Ngược lại, ta dùng hash table hoặc một mảng có độ dài $26$ để ghi nhận số lần xuất hiện của mỗi ký tự trong chuỗi $s$, sau đó duyệt chuỗi $t$. Mỗi lần duyệt qua một ký tự, ta giảm số lần xuất hiện của ký tự tương ứng trong hash table đi một. Nếu số lần xuất hiện sau khi giảm nhỏ hơn $0$, số lần xuất hiện của ký tự đó trong hai chuỗi là khác nhau, nên trả về `false`. Nếu sau khi duyệt qua hai chuỗi, số lần xuất hiện của mọi ký tự trong hash table đều bằng $0$, điều đó có nghĩa là các ký tự trong hai chuỗi xuất hiện cùng số lần, nên trả về `true`.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài chuỗi; còn $C$ là kích thước của tập ký tự, và trong bài này $C=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        cnt = Counter(s)
        for c in t:
            cnt[c] -= 1
            if cnt[c] < 0:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
            --cnt[t.charAt(i) - 'a'];
        }
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] != 0) {
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
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) {
            return false;
        }
        vector<int> cnt(26);
        for (int i = 0; i < s.size(); ++i) {
            ++cnt[s[i] - 'a'];
            --cnt[t[i] - 'a'];
        }
        return all_of(cnt.begin(), cnt.end(), [](int x) { return x == 0; });
    }
};
```

#### Go

```go
func isAnagram(s string, t string) bool {
	if len(s) != len(t) {
		return false
	}
	cnt := [26]int{}
	for i := 0; i < len(s); i++ {
		cnt[s[i]-'a']++
		cnt[t[i]-'a']--
	}
	for _, v := range cnt {
		if v != 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isAnagram(s: string, t: string): boolean {
    if (s.length !== t.length) {
        return false;
    }
    const cnt = new Array(26).fill(0);
    for (let i = 0; i < s.length; ++i) {
        ++cnt[s.charCodeAt(i) - 'a'.charCodeAt(0)];
        --cnt[t.charCodeAt(i) - 'a'.charCodeAt(0)];
    }
    return cnt.every(x => x === 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn is_anagram(s: String, t: String) -> bool {
        let n = s.len();
        let m = t.len();
        if n != m {
            return false;
        }
        let mut s = s.chars().collect::<Vec<char>>();
        let mut t = t.chars().collect::<Vec<char>>();
        s.sort();
        t.sort();
        for i in 0..n {
            if s[i] != t[i] {
                return false;
            }
        }
        true
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function (s, t) {
    if (s.length !== t.length) {
        return false;
    }
    const cnt = new Array(26).fill(0);
    for (let i = 0; i < s.length; ++i) {
        ++cnt[s.charCodeAt(i) - 'a'.charCodeAt(0)];
        --cnt[t.charCodeAt(i) - 'a'.charCodeAt(0)];
    }
    return cnt.every(x => x === 0);
};
```

#### C#

```cs
public class Solution {
    public bool IsAnagram(string s, string t) {
        if (s.Length != t.Length) {
            return false;
        }
        int[] cnt = new int[26];
        for (int i = 0; i < s.Length; ++i) {
            ++cnt[s[i] - 'a'];
            --cnt[t[i] - 'a'];
        }
        return cnt.All(x => x == 0);
    }
}
```

#### C

```c
int cmp(const void* a, const void* b) {
    return *(char*) a - *(char*) b;
}

bool isAnagram(char* s, char* t) {
    int n = strlen(s);
    int m = strlen(t);
    if (n != m) {
        return 0;
    }
    qsort(s, n, sizeof(char), cmp);
    qsort(t, n, sizeof(char), cmp);
    return !strcmp(s, t);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp đầu tiên tự duy trì bảng đếm. So sánh trực tiếp hai bộ đếm sẽ có cách viết ngắn hơn và cùng độ phức tạp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```

#### Rust

```rust
impl Solution {
    pub fn is_anagram(s: String, t: String) -> bool {
        let n = s.len();
        let m = t.len();
        if n != m {
            return false;
        }
        let (s, t) = (s.as_bytes(), t.as_bytes());
        let mut count = [0; 26];
        for i in 0..n {
            count[(s[i] - b'a') as usize] += 1;
            count[(t[i] - b'a') as usize] -= 1;
        }
        count.iter().all(|&c| c == 0)
    }
}
```

#### C

```c
bool isAnagram(char* s, char* t) {
    int n = strlen(s);
    int m = strlen(t);
    if (n != m) {
        return 0;
    }
    int count[26] = {0};
    for (int i = 0; i < n; i++) {
        count[s[i] - 'a']++;
        count[t[i] - 'a']--;
    }
    for (int i = 0; i < 26; i++) {
        if (count[i]) {
            return 0;
        }
    }
    return 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
