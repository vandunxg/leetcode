---
comments: true
difficulty: Medium
rating: 1530
source: Biweekly Contest 23 Q2
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1400. Construct K Palindrome Strings](https://leetcode.com/problems/construct-k-palindrome-strings)

[中文文档](/solution/1400-1499/1400.Construct%20K%20Palindrome%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>, trả về <code>true</code> nếu có thể sử dụng tất cả các ký tự trong <code>s</code> để tạo ra <code>k</code> <strong>chuỗi palindrome không rỗng</strong>, hoặc trả về <code>false</code> trong trường hợp ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;annabelle&quot;, k = 2
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bạn có thể tạo hai palindrome bằng cách sử dụng tất cả các ký tự trong s.
Một số cách tạo có thể là &quot;anna&quot; + &quot;elble&quot;, &quot;anbna&quot; + &quot;elle&quot;, &quot;anellena&quot; + &quot;b&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;, k = 3
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể tạo 3 palindrome bằng cách sử dụng tất cả các ký tự trong s.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;true&quot;, k = 4
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Cách duy nhất có thể là đặt mỗi ký tự vào một chuỗi riêng biệt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê các cách phân chia $s$ thành $k$ palindrome có độ phức tạp cấp số mũ. Với $n,k\le 10^5$, ta chỉ có thể quyết định tính khả thi dựa trên các tính chất về số lượng.
>
> Một palindrome có nhiều nhất một ký tự xuất hiện số lần lẻ ở vị trí trung tâm, nên $k$ palindrome chỉ có thể chứa nhiều nhất $k$ ký tự có số lần xuất hiện lẻ. Mỗi palindrome cũng cần ít nhất một ký tự, vì vậy $|s|<k$ là không thể.
>
> So sánh độ dài với $k$, đếm tần suất và kiểm tra xem số lượng ký tự có số lần xuất hiện lẻ có nhiều hơn $k$ hay không. Các cặp ký tự giống nhau có thể được phân phối tự do; ta không cần tạo ra các chuỗi.

<!-- thinking:end -->

Trước tiên, ta kiểm tra xem độ dài của chuỗi $s$ có nhỏ hơn $k$ hay không. Nếu có, ta không thể tạo ra $k$ chuỗi palindrome, nên có thể trực tiếp trả về `false`.

Sau đó, ta dùng một hash table hoặc một mảng $cnt$ để đếm số lần xuất hiện của mỗi ký tự trong chuỗi $s$. Cuối cùng, ta chỉ cần đếm số ký tự $x$ xuất hiện một số lần lẻ trong $cnt$. Nếu $x$ lớn hơn $k$, ta không thể tạo ra $k$ chuỗi palindrome, nên trả về `false`; nếu không thì trả về `true`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài của chuỗi $s$, còn $C$ là kích thước của tập ký tự; ở đây $C=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canConstruct(self, s: str, k: int) -> bool:
        if len(s) < k:
            return False
        cnt = Counter(s)
        return sum(v & 1 for v in cnt.values()) <= k
```

#### Java

```java
class Solution {
    public boolean canConstruct(String s, int k) {
        int n = s.length();
        if (n < k) {
            return false;
        }
        int[] cnt = new int[26];
        for (int i = 0; i < n; ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        int x = 0;
        for (int v : cnt) {
            x += v & 1;
        }
        return x <= k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canConstruct(string s, int k) {
        if (s.size() < k) {
            return false;
        }
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        int x = 0;
        for (int v : cnt) {
            x += v & 1;
        }
        return x <= k;
    }
};
```

#### Go

```go
func canConstruct(s string, k int) bool {
	if len(s) < k {
		return false
	}
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	x := 0
	for _, v := range cnt {
		x += v & 1
	}
	return x <= k
}
```

#### TypeScript

```ts
function canConstruct(s: string, k: number): boolean {
    if (s.length < k) {
        return false;
    }
    const cnt: number[] = new Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    let x = 0;
    for (const v of cnt) {
        x += v & 1;
    }
    return x <= k;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
