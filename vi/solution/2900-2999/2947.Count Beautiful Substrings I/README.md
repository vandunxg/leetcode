---
comments: true
difficulty: Medium
rating: 1450
source: Weekly Contest 373 Q2
tags:
    - Hash Table
    - Math
    - String
    - Enumeration
    - Number Theory
    - Prefix Sum
---

<!-- problem:start -->

# [2947. Count Beautiful Substrings I](https://leetcode.com/problems/count-beautiful-substrings-i)

[中文文档](/solution/2900-2999/2947.Count%20Beautiful%20Substrings%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên dương <code>k</code>.</p>

<p>Gọi <code>vowels</code> và <code>consonants</code> lần lượt là số lượng nguyên âm và phụ âm trong một chuỗi.</p>

<p>Một chuỗi được gọi là <strong>beautiful</strong> nếu:</p>

<ul>
	<li><code>vowels == consonants</code>.</li>
	<li><code>(vowels * consonants) % k == 0</code>, nói cách khác, tích của <code>vowels</code> và <code>consonants</code> chia hết cho <code>k</code>.</li>
</ul>

<p>Hãy trả về <em>số lượng <strong>beautiful substrings</strong> không rỗng</em> trong chuỗi đã cho <code>s</code>.</p>

<p><strong>Substring</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p><strong>Vowel letters</strong> trong tiếng Anh là <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</p>

<p><strong>Consonant letters</strong> trong tiếng Anh là mọi chữ cái ngoại trừ nguyên âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;baeyh&quot;, k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 beautiful substrings trong chuỗi đã cho.
- Substring &quot;b<u>aeyh</u>&quot;, vowels = 2 ([&quot;a&quot;,e&quot;]), consonants = 2 ([&quot;y&quot;,&quot;h&quot;]).
Bạn có thể thấy chuỗi &quot;aeyh&quot; là beautiful vì vowels == consonants và vowels * consonants % k == 0.
- Substring &quot;<u>baey</u>h&quot;, vowels = 2 ([&quot;a&quot;,e&quot;]), consonants = 2 ([&quot;b&quot;,&quot;y&quot;]).
Bạn có thể thấy chuỗi &quot;baey&quot; là beautiful vì vowels == consonants và vowels * consonants % k == 0.
Có thể chứng minh rằng chỉ có 2 beautiful substrings.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abba&quot;, k = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 beautiful substrings trong chuỗi đã cho.
- Substring &quot;<u>ab</u>ba&quot;, vowels = 1 ([&quot;a&quot;]), consonants = 1 ([&quot;b&quot;]).
- Substring &quot;ab<u>ba</u>&quot;, vowels = 1 ([&quot;a&quot;]), consonants = 1 ([&quot;b&quot;]).
- Substring &quot;<u>abba</u>&quot;, vowels = 2 ([&quot;a&quot;,&quot;a&quot;]), consonants = 2 ([&quot;b&quot;,&quot;b&quot;]).
Có thể chứng minh rằng chỉ có 3 beautiful substrings.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bcdf&quot;, k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có beautiful substrings nào trong chuỗi đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> Một beautiful substring có số nguyên âm và phụ âm bằng nhau, đồng thời tích của chúng chia hết cho $k$. Vì $n \le 1000$, ta có thể liệt kê các đầu mút của substring và đếm số nguyên âm; số phụ âm bằng độ dài trừ đi số nguyên âm.
>
> Cả hai điều kiện đều được kiểm tra trong $O(1)$ cho mỗi cặp, tổng cộng là $O(n^2)$. Không cần dùng prefix hash.

<!-- thinking:end -->

Ta liệt kê vị trí bắt đầu $i$ của substring trong khoảng $[0, n)$ và vị trí kết thúc $j$ trong khoảng $[i, n)$, đếm số nguyên âm và phụ âm trong substring $s[i \dots j]$, rồi kiểm tra xem đó có phải là một beautiful substring hay không. Nếu đúng, ta tăng đáp án lên $1$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautifulSubstrings(self, s: str, k: int) -> int:
        n = len(s)
        vs = set("aeiou")
        ans = 0
        for i in range(n):
            vowels = 0
            for j in range(i, n):
                vowels += s[j] in vs
                consonants = j - i + 1 - vowels
                if vowels == consonants and vowels * consonants % k == 0:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int beautifulSubstrings(String s, int k) {
        int n = s.length();
        int[] vs = new int[26];
        for (char c : "aeiou".toCharArray()) {
            vs[c - 'a'] = 1;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int vowels = 0;
            for (int j = i; j < n; ++j) {
                vowels += vs[s.charAt(j) - 'a'];
                int consonants = j - i + 1 - vowels;
                if (vowels == consonants && vowels * consonants % k == 0) {
                    ++ans;
                }
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
    int beautifulSubstrings(string s, int k) {
        int n = s.size();
        int vs[26]{};
        string t = "aeiou";
        for (char c : t) {
            vs[c - 'a'] = 1;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int vowels = 0;
            for (int j = i; j < n; ++j) {
                vowels += vs[s[j] - 'a'];
                int consonants = j - i + 1 - vowels;
                if (vowels == consonants && vowels * consonants % k == 0) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func beautifulSubstrings(s string, k int) (ans int) {
	n := len(s)
	vs := [26]int{}
	for _, c := range "aeiou" {
		vs[c-'a'] = 1
	}
	for i := 0; i < n; i++ {
		vowels := 0
		for j := i; j < n; j++ {
			vowels += vs[s[j]-'a']
			consonants := j - i + 1 - vowels
			if vowels == consonants && vowels*consonants%k == 0 {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function beautifulSubstrings(s: string, k: number): number {
    const n = s.length;
    const vs: number[] = Array(26).fill(0);
    for (const c of 'aeiou') {
        vs[c.charCodeAt(0) - 'a'.charCodeAt(0)] = 1;
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        let vowels = 0;
        for (let j = i; j < n; ++j) {
            vowels += vs[s.charCodeAt(j) - 'a'.charCodeAt(0)];
            const consonants = j - i + 1 - vowels;
            if (vowels === consonants && (vowels * consonants) % k === 0) {
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn beautiful_substrings(s: String, k: i32) -> i32 {
        let n = s.len();
        let s = s.as_bytes();

        let mut vs = [0; 26];
        for &c in b"aeiou" {
            vs[(c - b'a') as usize] = 1;
        }

        let mut ans = 0;

        for i in 0..n {
            let mut vowels = 0;
            for j in i..n {
                vowels += vs[(s[j] - b'a') as usize];
                let consonants = (j - i + 1) as i32 - vowels;
                if vowels == consonants && (vowels * consonants) % k == 0 {
                    ans += 1;
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
