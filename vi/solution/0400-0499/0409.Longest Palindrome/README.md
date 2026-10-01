---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Hash Table
    - String
---

<!-- problem:start -->

# [409. Longest Palindrome](https://leetcode.com/problems/longest-palindrome)

[中文文档](/solution/0400-0499/0409.Longest%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> chỉ gồm chữ cái viết thường hoặc viết hoa, hãy trả về độ dài của <strong><span data-keyword="palindrome-string">palindrome</span> dài nhất</strong> có thể tạo từ các chữ cái đó.</p>

<p>Chữ hoa và chữ thường được xem là khác nhau; ví dụ, <code>&quot;Aa&quot;</code> không được xem là palindrome.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abccccdd&quot;
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Một palindrome dài nhất có thể tạo được là &quot;dccaccd&quot;, có độ dài bằng 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Palindrome dài nhất có thể tạo được là &quot;a&quot;, có độ dài bằng 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> chỉ gồm chữ cái tiếng Anh viết thường và/hoặc viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Palindrome chỉ có thể có tối đa một ký tự xuất hiện số lần lẻ; các ký tự còn lại phải ghép thành cặp. Không cần thử các hoán vị, chỉ cần tính mỗi ký tự tạo được bao nhiêu cặp.
>
> Sau khi đếm, với mỗi tần suất $v$, cộng $v//2\times 2$ vào kết quả. Nếu tổng vẫn nhỏ hơn $|s|$, ta có thể đặt một ký tự còn dư ở chính giữa.
>
> Ghép các cặp trước rồi đặt thêm ký tự ở giữa nếu còn dư sẽ cho độ dài lớn nhất có thể.

<!-- thinking:end -->

Một chuỗi palindrome hợp lệ có thể có tối đa một ký tự xuất hiện số lần lẻ; các ký tự còn lại phải xuất hiện số lần chẵn.

Vì vậy, trước tiên ta duyệt chuỗi $s$, đếm số lần xuất hiện của mỗi ký tự và lưu vào mảng hoặc hash table $cnt$.

Sau đó, ta duyệt $cnt$. Với mỗi số đếm $v$, lấy phần nguyên của $v$ chia cho 2, nhân kết quả với 2 rồi cộng vào đáp án $ans$.

Cuối cùng, nếu đáp án nhỏ hơn độ dài chuỗi $s$, ta tăng đáp án thêm 1 rồi trả về $ans$.

Độ phức tạp thời gian là $O(n + |\Sigma|)$, còn độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi $s$ và $|\Sigma|$ là kích thước của tập ký tự. Trong bài này, $|\Sigma| = 128$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPalindrome(self, s: str) -> int:
        cnt = Counter(s)
        ans = sum(v // 2 * 2 for v in cnt.values())
        ans += int(ans < len(s))
        return ans
```

#### Java

```java
class Solution {
    public int longestPalindrome(String s) {
        int[] cnt = new int[128];
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            ++cnt[s.charAt(i)];
        }
        int ans = 0;
        for (int v : cnt) {
            ans += v / 2 * 2;
        }
        ans += ans < n ? 1 : 0;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestPalindrome(string s) {
        int cnt[128]{};
        for (char c : s) {
            ++cnt[c];
        }
        int ans = 0;
        for (int v : cnt) {
            ans += v / 2 * 2;
        }
        ans += ans < s.size();
        return ans;
    }
};
```

#### Go

```go
func longestPalindrome(s string) (ans int) {
	cnt := [128]int{}
	for _, c := range s {
		cnt[c]++
	}
	for _, v := range cnt {
		ans += v / 2 * 2
	}
	if ans < len(s) {
		ans++
	}
	return
}
```

#### TypeScript

```ts
function longestPalindrome(s: string): number {
    const cnt: Record<string, number> = {};
    for (const c of s) {
        cnt[c] = (cnt[c] || 0) + 1;
    }
    let ans = Object.values(cnt).reduce((acc, v) => acc + Math.floor(v / 2) * 2, 0);
    ans += ans < s.length ? 1 : 0;
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn longest_palindrome(s: String) -> i32 {
        let mut cnt = HashMap::new();
        for ch in s.chars() {
            *cnt.entry(ch).or_insert(0) += 1;
        }

        let mut ans = 0;
        for &v in cnt.values() {
            ans += (v / 2) * 2;
        }

        if ans < (s.len() as i32) {
            ans += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit + đếm

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đếm tần suất rồi tính tổng. Ta có thể XOR một cờ chẵn lẻ khi duyệt và theo dõi số ký tự có tần suất lẻ $\textit{cnt}$; đáp án là $n-\textit{cnt}+1$ (chừa một ký tự ở giữa) hoặc $n$.
>
> Cách này không cần lượt duyệt thứ hai qua bảng tần suất; không gian vẫn bằng kích thước bảng chữ cái.

<!-- thinking:end -->

Ta có thể dùng mảng hoặc hash table $odd$ để ghi lại mỗi ký tự trong chuỗi $s$ xuất hiện số lần lẻ hay chẵn, và dùng biến nguyên $cnt$ để đếm số ký tự xuất hiện số lần lẻ.

Ta duyệt chuỗi $s$. Với mỗi ký tự $c$, ta đảo trạng thái $odd[c]$, tức là $0 \rightarrow 1$, $1 \rightarrow 0$. Nếu $odd[c]$ đổi từ $0$ sang $1$, ta tăng $cnt$ thêm 1; nếu đổi từ $1$ sang $0$, ta giảm $cnt$ đi 1.

Cuối cùng, nếu $cnt$ lớn hơn $0$, đáp án là $n - cnt + 1$; ngược lại, đáp án là $n$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi $s$ và $|\Sigma|$ là kích thước của tập ký tự. Trong bài này, $|\Sigma| = 128$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPalindrome(self, s: str) -> int:
        odd = defaultdict(int)
        cnt = 0
        for c in s:
            odd[c] ^= 1
            cnt += 1 if odd[c] else -1
        return len(s) - cnt + 1 if cnt else len(s)
```

#### Java

```java
class Solution {
    public int longestPalindrome(String s) {
        int[] odd = new int[128];
        int n = s.length();
        int cnt = 0;
        for (int i = 0; i < n; ++i) {
            odd[s.charAt(i)] ^= 1;
            cnt += odd[s.charAt(i)] == 1 ? 1 : -1;
        }
        return cnt > 0 ? n - cnt + 1 : n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestPalindrome(string s) {
        int odd[128]{};
        int n = s.length();
        int cnt = 0;
        for (char& c : s) {
            odd[c] ^= 1;
            cnt += odd[c] ? 1 : -1;
        }
        return cnt ? n - cnt + 1 : n;
    }
};
```

#### Go

```go
func longestPalindrome(s string) (ans int) {
	odd := [128]int{}
	cnt := 0
	for _, c := range s {
		odd[c] ^= 1
		cnt += odd[c]
		if odd[c] == 0 {
			cnt--
		}
	}
	if cnt > 0 {
		return len(s) - cnt + 1
	}
	return len(s)
}
```

#### TypeScript

```ts
function longestPalindrome(s: string): number {
    const odd: Record<string, number> = {};
    let cnt = 0;
    for (const c of s) {
        odd[c] ^= 1;
        cnt += odd[c] ? 1 : -1;
    }
    return cnt ? s.length - cnt + 1 : s.length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
