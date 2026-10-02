---
comments: true
difficulty: Easy
rating: 1315
source: Biweekly Contest 8 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1180. Count Substrings with Only One Distinct Letter 🔒](https://leetcode.com/problems/count-substrings-with-only-one-distinct-letter)

[中文文档](/solution/1100-1199/1180.Count%20Substrings%20with%20Only%20One%20Distinct%20Letter/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về <em>số chuỗi con chỉ chứa <strong>một ký tự khác nhau</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaba&quot;
<strong>Đầu ra:</strong> 8
<strong>Giải thích: </strong>Các chuỗi con chỉ có một ký tự khác nhau là &quot;aaa&quot;, &quot;aa&quot;, &quot;a&quot;, &quot;b&quot;.
&quot;aaa&quot; xuất hiện 1 lần.
&quot;aa&quot; xuất hiện 2 lần.
&quot;a&quot; xuất hiện 4 lần.
&quot;b&quot; xuất hiện 1 lần.
Vậy đáp án là 1 + 2 + 4 + 1 = 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaaaaaaaa&quot;
<strong>Đầu ra:</strong> 55
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi con chỉ có một ký tự nằm trong một đoạn gồm các ký tự giống nhau liên tiếp. Đoạn có độ dài $L$ đóng góp $L(L+1)/2$ chuỗi con. Dùng hai pointer để xác định độ dài từng đoạn rồi cộng số tam giác tương ứng, thay vì liệt kê $O(n^2)$ chuỗi con.

<!-- thinking:end -->

Ta dùng hai pointer: pointer $i$ trỏ đến đầu đoạn hiện tại, còn pointer $j$ dịch sang phải đến vị trí đầu tiên có ký tự khác $s[i]$. Khi đó, đoạn $[i,..j-1]$ chỉ gồm ký tự $s[i]$ và có độ dài $j-i$. Vì vậy, số chuỗi con chỉ chứa ký tự $s[i]$ là $\frac{(j-i+1)(j-i)}{2}$; ta cộng số này vào đáp án. Sau đó đặt $i=j$ và tiếp tục duyệt cho đến khi $i$ vượt khỏi phạm vi chuỗi $s$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countLetters(self, s: str) -> int:
        n = len(s)
        i = ans = 0
        while i < n:
            j = i
            while j < n and s[j] == s[i]:
                j += 1
            ans += (1 + j - i) * (j - i) // 2
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int countLetters(String s) {
        int ans = 0;
        for (int i = 0, n = s.length(); i < n;) {
            int j = i;
            while (j < n && s.charAt(j) == s.charAt(i)) {
                ++j;
            }
            ans += (1 + j - i) * (j - i) / 2;
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countLetters(string s) {
        int ans = 0;
        for (int i = 0, n = s.size(); i < n;) {
            int j = i;
            while (j < n && s[j] == s[i]) {
                ++j;
            }
            ans += (1 + j - i) * (j - i) / 2;
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func countLetters(s string) int {
	ans := 0
	for i, n := 0, len(s); i < n; {
		j := i
		for j < n && s[j] == s[i] {
			j++
		}
		ans += (1 + j - i) * (j - i) / 2
		i = j
	}
	return ans
}
```

#### TypeScript

```ts
function countLetters(s: string): number {
    let ans = 0;
    const n = s.length;
    for (let i = 0; i < n;) {
        let j = i;
        let cnt = 0;
        while (j < n && s[j] === s[i]) {
            ++j;
            ans += ++cnt;
        }
        i = j;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cộng trực tiếp số tam giác cho mỗi đoạn. Lời giải 2 cộng lần lượt $1,2,\ldots,L$ khi đoạn được mở rộng; tổng vẫn là số tam giác đó và tương ứng với số chuỗi con một ký tự kết thúc tại chỉ số hiện tại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countLetters(self, s: str) -> int:
        ans = 0
        i, n = 0, len(s)
        while i < n:
            j = i
            cnt = 0
            while j < n and s[j] == s[i]:
                j += 1
                cnt += 1
                ans += cnt
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int countLetters(String s) {
        int ans = 0;
        int i = 0, n = s.length();
        while (i < n) {
            int j = i;
            int cnt = 0;
            while (j < n && s.charAt(j) == s.charAt(i)) {
                ++j;
                ans += ++cnt;
            }
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countLetters(string s) {
        int ans = 0;
        int i = 0, n = s.size();
        while (i < n) {
            int j = i;
            int cnt = 0;
            while (j < n && s[j] == s[i]) {
                ++j;
                ans += ++cnt;
            }
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func countLetters(s string) (ans int) {
	i, n := 0, len(s)
	for i < n {
		j := i
		cnt := 0
		for j < n && s[j] == s[i] {
			j++
			cnt++
			ans += cnt
		}
		i = j
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
