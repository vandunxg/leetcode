---
comments: true
difficulty: Medium
rating: 1357
source: Weekly Contest 445 Q2
tags:
    - String
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [3517. Smallest Palindromic Rearrangement I](https://leetcode.com/problems/smallest-palindromic-rearrangement-i)

[中文文档](/solution/3500-3599/3517.Smallest%20Palindromic%20Rearrangement%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong><span data-keyword="palindrome-string">đối xứng</span></strong> <code>s</code>.</p>

<p>Hãy trả về <strong><span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span></strong> <span data-keyword="permutation-string">hoán vị đối xứng</span> của <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;z&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;z&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi chỉ có một ký tự đã là chuỗi đối xứng nhỏ nhất theo thứ tự từ điển.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;babab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abbba&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hoán vị <code>&quot;babab&quot;</code> &rarr; <code>&quot;abbba&quot;</code> cho ta chuỗi đối xứng nhỏ nhất theo thứ tự từ điển.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;daccad&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;acddca&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hoán vị <code>&quot;daccad&quot;</code> &rarr; <code>&quot;acddca&quot;</code> cho ta chuỗi đối xứng nhỏ nhất theo thứ tự từ điển.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Đảm bảo <code>s</code> là chuỗi đối xứng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> $s$ đã là một chuỗi đối xứng, và mọi hoán vị vẫn là chuỗi đối xứng đều được xác định bởi multiset của nửa đầu. Đặt một nửa số lần xuất hiện của mỗi ký tự vào bên trái, đặt nhiều nhất một ký tự có số lần xuất hiện lẻ ở giữa, rồi đối xứng sang bên phải.
>
> Nửa bên trái nên được điền từ `a` đến `z` để thu được chuỗi nhỏ nhất theo thứ tự từ điển.

<!-- thinking:end -->

Trước hết, chúng ta đếm số lần xuất hiện của mỗi ký tự trong chuỗi và lưu kết quả vào hash table hoặc mảng $\textit{cnt}$. Vì chuỗi là chuỗi đối xứng, số lần xuất hiện của mỗi ký tự hoặc là số chẵn, hoặc có đúng một ký tự có số lần xuất hiện lẻ.

Tiếp theo, bắt đầu từ ký tự nhỏ nhất theo thứ tự từ điển, chúng ta lần lượt thêm một nửa số lần xuất hiện của mỗi ký tự vào nửa đầu của chuỗi kết quả $\textit{t}$. Nếu một ký tự xuất hiện số lần lẻ, chúng ta lưu nó làm ký tự ở giữa $\textit{ch}$. Cuối cùng, chúng ta nối $\textit{t}$, $\textit{ch}$ và phần đảo ngược của $\textit{t}$ để thu được hoán vị đối xứng nhỏ nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $|\Sigma|$ là kích thước của tập ký tự, bằng $26$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestPalindrome(self, s: str) -> str:
        cnt = Counter(s)
        t = []
        ch = ""
        for c in ascii_lowercase:
            v = cnt[c] // 2
            t.append(c * v)
            cnt[c] -= v * 2
            if cnt[c] == 1:
                ch = c
        ans = "".join(t)
        ans = ans + ch + ans[::-1]
        return ans
```

#### Java

```java
class Solution {
    public String smallestPalindrome(String s) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            cnt[c - 'a']++;
        }

        StringBuilder t = new StringBuilder();
        String ch = "";

        for (char c = 'a'; c <= 'z'; c++) {
            int idx = c - 'a';
            int v = cnt[idx] / 2;
            if (v > 0) {
                t.append(String.valueOf(c).repeat(v));
            }
            cnt[idx] -= v * 2;
            if (cnt[idx] == 1) {
                ch = String.valueOf(c);
            }
        }

        String ans = t.toString();
        ans = ans + ch + new StringBuilder(ans).reverse();
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string smallestPalindrome(string s) {
        vector<int> cnt(26);
        for (char c : s) {
            cnt[c - 'a']++;
        }
        string t = "";
        string ch = "";
        for (char c = 'a'; c <= 'z'; ++c) {
            int v = cnt[c - 'a'] / 2;
            if (v > 0) {
                t.append(v, c);
            }
            cnt[c - 'a'] -= v * 2;
            if (cnt[c - 'a'] == 1) {
                ch = string(1, c);
            }
        }
        string ans = t;
        ans += ch;
        string rev = t;
        reverse(rev.begin(), rev.end());
        ans += rev;
        return ans;
    }
};
```

#### Go

```go
func smallestPalindrome(s string) string {
	cnt := make([]int, 26)
	for i := 0; i < len(s); i++ {
		cnt[s[i]-'a']++
	}

	t := make([]byte, 0, len(s)/2)
	var ch byte
	for c := byte('a'); c <= 'z'; c++ {
		v := cnt[c-'a'] / 2
		for i := 0; i < v; i++ {
			t = append(t, c)
		}
		cnt[c-'a'] -= v * 2
		if cnt[c-'a'] == 1 {
			ch = c
		}
	}

	totalLen := len(t) * 2
	if ch != 0 {
		totalLen++
	}
	var sb strings.Builder
	sb.Grow(totalLen)

	sb.Write(t)
	if ch != 0 {
		sb.WriteByte(ch)
	}
	for i := len(t) - 1; i >= 0; i-- {
		sb.WriteByte(t[i])
	}
	return sb.String()
}
```

#### TypeScript

```ts
function smallestPalindrome(s: string): string {
    const ascii_lowercase = 'abcdefghijklmnopqrstuvwxyz';
    const cnt = new Array<number>(26).fill(0);
    for (const chKey of s) {
        cnt[chKey.charCodeAt(0) - 97]++;
    }

    const t: string[] = [];
    let ch = '';
    for (let i = 0; i < 26; i++) {
        const c = ascii_lowercase[i];
        const v = Math.floor(cnt[i] / 2);
        t.push(c.repeat(v));
        cnt[i] -= v * 2;
        if (cnt[i] === 1) {
            ch = c;
        }
    }

    let ans = t.join('');
    ans = ans + ch + ans.split('').reverse().join('');
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_palindrome(s: String) -> String {
        let mut cnt = vec![0; 26];
        for c in s.bytes() {
            cnt[(c - b'a') as usize] += 1;
        }

        let mut t = String::new();
        let mut ch = String::new();

        for i in 0..26 {
            let v = cnt[i] / 2;
            if v > 0 {
                t.extend(std::iter::repeat((b'a' + i as u8) as char).take(v as usize));
            }
            cnt[i] -= v * 2;
            if cnt[i] == 1 {
                ch.push((b'a' + i as u8) as char);
            }
        }

        let mut ans = t.clone();
        ans.push_str(&ch);

        let rev: String = t.chars().rev().collect();
        ans.push_str(&rev);

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
