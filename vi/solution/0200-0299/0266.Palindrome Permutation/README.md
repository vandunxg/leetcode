---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Hash Table
    - String
---

<!-- problem:start -->

# [266. Palindrome Permutation 🔒](https://leetcode.com/problems/palindrome-permutation)

[中文文档](/solution/0200-0299/0266.Palindrome%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về <code>true</code> <em>nếu có thể sắp xếp lại các ký tự trong chuỗi để tạo thành </em><span data-keyword="palindrome-string"><em><strong>palindrome</strong></em></span><em>; nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;code&quot;
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aab&quot;
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;carerac&quot;
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một hoán vị là palindrome khi và chỉ khi có nhiều nhất một ký tự xuất hiện số lần lẻ. Đếm tần suất các ký tự rồi kiểm tra xem số lượng ký tự có tần suất lẻ có nhỏ hơn $2$ hay không.

<!-- thinking:end -->

Nếu một chuỗi là palindrome, nhiều nhất chỉ có một ký tự xuất hiện số lần lẻ; tất cả ký tự còn lại phải xuất hiện số lần chẵn. Vì vậy, ta chỉ cần đếm số lần xuất hiện của từng ký tự rồi kiểm tra điều kiện này.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài chuỗi và $|\Sigma|$ là kích thước bộ ký tự. Với bài này, bộ ký tự gồm các chữ cái viết thường, nên $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPermutePalindrome(self, s: str) -> bool:
        return sum(v & 1 for v in Counter(s).values()) < 2
```

#### Java

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        int odd = 0;
        for (int x : cnt) {
            odd += x & 1;
        }
        return odd < 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPermutePalindrome(string s) {
        vector<int> cnt(26);
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        int odd = 0;
        for (int x : cnt) {
            odd += x & 1;
        }
        return odd < 2;
    }
};
```

#### Go

```go
func canPermutePalindrome(s string) bool {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	odd := 0
	for _, x := range cnt {
		odd += x & 1
	}
	return odd < 2
}
```

#### TypeScript

```ts
function canPermutePalindrome(s: string): boolean {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 97];
    }
    return cnt.filter(c => c % 2 === 1).length < 2;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var canPermutePalindrome = function (s) {
    const cnt = new Map();
    for (const c of s) {
        cnt.set(c, (cnt.get(c) || 0) + 1);
    }
    return [...cnt.values()].filter(v => v % 2 === 1).length < 2;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
