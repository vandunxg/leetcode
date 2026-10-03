---
comments: true
difficulty: Hard
rating: 2220
source: Biweekly Contest 75 Q4
tags:
    - String
    - Binary Search
    - String Matching
    - Suffix Array
    - Hash Function
    - Rolling Hash
    - KMP
    - Extended KMP
---

<!-- problem:start -->

# [2223. Sum of Scores of Built Strings](https://leetcode.com/problems/sum-of-scores-of-built-strings)

[中文文档](/solution/2200-2299/2223.Sum%20of%20Scores%20of%20Built%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang <strong>xây dựng</strong> một chuỗi <code>s</code> có độ dài <code>n</code> bằng cách thêm <strong>từng</strong> ký tự một, mỗi ký tự mới được <strong>thêm vào</strong> <strong>phía trước</strong> chuỗi. Các chuỗi được đánh số từ <code>1</code> đến <code>n</code>, trong đó chuỗi có độ dài <code>i</code> được ký hiệu là <code>s<sub>i</sub></code>.</p>

<ul>
	<li>Ví dụ, với <code>s = &quot;abaca&quot;</code>, <code>s<sub>1</sub> == &quot;a&quot;</code>, <code>s<sub>2</sub> == &quot;ca&quot;</code>, <code>s<sub>3</sub> == &quot;aca&quot;</code>, v.v.</li>
</ul>

<p><strong>Điểm số</strong> của <code>s<sub>i</sub></code> là độ dài của <strong>tiền tố chung dài nhất</strong> giữa <code>s<sub>i</sub></code> và <code>s<sub>n</sub></code> (lưu ý rằng <code>s == s<sub>n</sub></code>).</p>

<p>Cho chuỗi cuối cùng <code>s</code>, hãy trả về <em><strong>tổng</strong> <strong>điểm số</strong> của mọi </em><code>s<sub>i</sub></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;babab&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Với s<sub>1</sub> == &quot;b&quot;, tiền tố chung dài nhất là &quot;b&quot; nên điểm số là 1.
Với s<sub>2</sub> == &quot;ab&quot;, không có tiền tố chung nên điểm số là 0.
Với s<sub>3</sub> == &quot;bab&quot;, tiền tố chung dài nhất là &quot;bab&quot; nên điểm số là 3.
Với s<sub>4</sub> == &quot;abab&quot;, không có tiền tố chung nên điểm số là 0.
Với s<sub>5</sub> == &quot;babab&quot;, tiền tố chung dài nhất là &quot;babab&quot; nên điểm số là 5.
Tổng điểm số là 1 + 0 + 3 + 0 + 5 = 9, nên ta trả về 9.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;azbazbzaz&quot;
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong>
Với s<sub>2</sub> == &quot;az&quot;, tiền tố chung dài nhất là &quot;az&quot; nên điểm số là 2.
Với s<sub>6</sub> == &quot;azbzaz&quot;, tiền tố chung dài nhất là &quot;azb&quot; nên điểm số là 3.
Với s<sub>9</sub> == &quot;azbazbzaz&quot;, tiền tố chung dài nhất là &quot;azbazbzaz&quot; nên điểm số là 9.
Với mọi s<sub>i</sub> khác, điểm số là 0.
Tổng điểm số là 2 + 3 + 9 = 14, nên ta trả về 14.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi $s_i$ là hậu tố có độ dài $i$ của $s$, và điểm số của nó là LCP của hậu tố đó với chính $s$. Nếu so sánh ngây thơ mọi hậu tố, độ phức tạp là $O(n^2)$ và không đáp ứng được với $n \le 10^5$.
>
> Các LCP này chính là Z-array: $z[i]$ là LCP của $s[i:]$ với $s$, còn $z[0]=n$. Chỉ cần tính tổng Z-array sau khi xây dựng nó trong thời gian tuyến tính. String hashing kết hợp với tìm kiếm nhị phân trên mỗi vị trí bắt đầu là một phương án thay thế có độ phức tạp $O(n\log n)$.

<!-- thinking:end -->

Tính Z-array của $s$, trong đó $z[i]$ là LCP của $s[i:]$ với $s$. Điểm số của hậu tố có độ dài $i$, tức $s_i$, là $z[n-i]$, còn cả chuỗi có điểm số là $n$. Tổng Z-array rồi cộng thêm $n$ sẽ cho đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumScores(self, s: str) -> int:
        n = len(s)
        z = [0] * n
        l = r = 0
        for i in range(1, n):
            if i <= r:
                z[i] = min(r - i + 1, z[i - l])
            while i + z[i] < n and s[z[i]] == s[i + z[i]]:
                z[i] += 1
            if i + z[i] - 1 > r:
                l, r = i, i + z[i] - 1
        return n + sum(z)
```

#### Java

```java
class Solution {
    public long sumScores(String s) {
        int n = s.length();
        int[] z = new int[n];
        for (int i = 1, l = 0, r = 0; i < n; ++i) {
            if (i <= r) {
                z[i] = Math.min(r - i + 1, z[i - l]);
            }
            while (i + z[i] < n && s.charAt(z[i]) == s.charAt(i + z[i])) {
                ++z[i];
            }
            if (i + z[i] - 1 > r) {
                l = i;
                r = i + z[i] - 1;
            }
        }
        long ans = n;
        for (int x : z) {
            ans += x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long sumScores(string s) {
        int n = s.size();
        vector<int> z(n);
        for (int i = 1, l = 0, r = 0; i < n; ++i) {
            if (i <= r) {
                z[i] = min(r - i + 1, z[i - l]);
            }
            while (i + z[i] < n && s[z[i]] == s[i + z[i]]) {
                ++z[i];
            }
            if (i + z[i] - 1 > r) {
                l = i;
                r = i + z[i] - 1;
            }
        }
        return n + accumulate(z.begin(), z.end(), 0LL);
    }
};
```

#### Go

```go
func sumScores(s string) int64 {
	n := len(s)
	z := make([]int, n)
	for i, l, r := 1, 0, 0; i < n; i++ {
		if i <= r {
			z[i] = min(r-i+1, z[i-l])
		}
		for i+z[i] < n && s[z[i]] == s[i+z[i]] {
			z[i]++
		}
		if i+z[i]-1 > r {
			l, r = i, i+z[i]-1
		}
	}
	ans := int64(n)
	for _, x := range z {
		ans += int64(x)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
