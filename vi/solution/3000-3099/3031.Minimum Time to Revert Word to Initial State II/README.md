---
comments: true
difficulty: Hard
rating: 2277
source: Weekly Contest 383 Q4
tags:
    - String
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3031. Minimum Time to Revert Word to Initial State II](https://leetcode.com/problems/minimum-time-to-revert-word-to-initial-state-ii)

[中文文档](/solution/3000-3099/3031.Minimum%20Time%20to%20Revert%20Word%20to%20Initial%20State%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>0-indexed</strong> <code>word</code> và một số nguyên <code>k</code>.</p>

<p>Ở mỗi giây, bạn phải thực hiện các thao tác sau:</p>

<ul>
	<li>Xóa <code>k</code> ký tự đầu tiên của <code>word</code>.</li>
	<li>Thêm bất kỳ <code>k</code> ký tự nào vào cuối <code>word</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng các ký tự được thêm vào không nhất thiết phải giống với các ký tự đã xóa. Tuy nhiên, bạn phải thực hiện <strong>cả hai</strong> thao tác ở mỗi giây.</p>

<p>Trả về <em>thời gian <strong>nhỏ nhất</strong> lớn hơn 0 cần thiết để</em> <code>word</code> <em>trở lại <strong>trạng thái</strong> ban đầu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abacaba&quot;, k = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ở giây thứ 1, ta xóa các ký tự &quot;aba&quot; ở đầu chuỗi và thêm các ký tự &quot;bac&quot; vào cuối chuỗi. Khi đó, word trở thành &quot;cababac&quot;.
Ở giây thứ 2, ta xóa các ký tự &quot;cab&quot; ở đầu chuỗi và thêm &quot;aba&quot; vào cuối chuỗi. Khi đó, word trở thành &quot;abacaba&quot; và trở lại trạng thái ban đầu.
Có thể chứng minh rằng 2 giây là thời gian nhỏ nhất lớn hơn 0 cần thiết để word trở lại trạng thái ban đầu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abacaba&quot;, k = 4
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ở giây thứ 1, ta xóa các ký tự &quot;abac&quot; ở đầu chuỗi và thêm các ký tự &quot;caba&quot; vào cuối chuỗi. Khi đó, word trở thành &quot;abacaba&quot; và trở lại trạng thái ban đầu.
Có thể chứng minh rằng 1 giây là thời gian nhỏ nhất lớn hơn 0 cần thiết để word trở lại trạng thái ban đầu.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abcbabcd&quot;, k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ở mỗi giây, ta xóa 2 ký tự đầu tiên của word rồi thêm chính các ký tự đó vào cuối chuỗi.
Sau 4 giây, word trở thành &quot;abcbabcd&quot; và trở lại trạng thái ban đầu.
Có thể chứng minh rằng 4 giây là thời gian nhỏ nhất lớn hơn 0 cần thiết để word trở lại trạng thái ban đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= word.length</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đề bài giống phần I, nhưng $n \le 10^6$, nên việc so sánh các hậu tố với tiền tố theo cách ngây thơ sẽ bị quá thời gian.
>
> Do đó, phép kiểm tra bằng hash từ phương pháp thứ hai của phần I là bắt buộc.
>
> Sau khi xây dựng bảng hash tiền tố, ta thử các bội số của $k$ và so sánh $\textit{word}[1..n-i]$ với $\textit{word}[i+1..n]$ trong $O(1)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Hashing:
    __slots__ = ["mod", "h", "p"]

    def __init__(self, s: str, base: int, mod: int):
        self.mod = mod
        self.h = [0] * (len(s) + 1)
        self.p = [1] * (len(s) + 1)
        for i in range(1, len(s) + 1):
            self.h[i] = (self.h[i - 1] * base + ord(s[i - 1])) % mod
            self.p[i] = (self.p[i - 1] * base) % mod

    def query(self, l: int, r: int) -> int:
        return (self.h[r] - self.h[l - 1] * self.p[r - l + 1]) % self.mod


class Solution:
    def minimumTimeToInitialState(self, word: str, k: int) -> int:
        hashing = Hashing(word, 13331, 998244353)
        n = len(word)
        for i in range(k, n, k):
            if hashing.query(1, n - i) == hashing.query(i + 1, n):
                return i // k
        return (n + k - 1) // k
```

#### Java

```java
class Hashing {
    private final long[] p;
    private final long[] h;
    private final long mod;

    public Hashing(String word, long base, int mod) {
        int n = word.length();
        p = new long[n + 1];
        h = new long[n + 1];
        p[0] = 1;
        this.mod = mod;
        for (int i = 1; i <= n; i++) {
            p[i] = p[i - 1] * base % mod;
            h[i] = (h[i - 1] * base + word.charAt(i - 1) - 'a') % mod;
        }
    }

    public long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
}

class Solution {
    public int minimumTimeToInitialState(String word, int k) {
        Hashing hashing = new Hashing(word, 13331, 998244353);
        int n = word.length();
        for (int i = k; i < n; i += k) {
            if (hashing.query(1, n - i) == hashing.query(i + 1, n)) {
                return i / k;
            }
        }
        return (n + k - 1) / k;
    }
}
```

#### C++

```cpp
class Hashing {
private:
    vector<long long> p;
    vector<long long> h;
    long long mod;

public:
    Hashing(string word, long long base, int mod) {
        int n = word.size();
        p.resize(n + 1);
        h.resize(n + 1);
        p[0] = 1;
        this->mod = mod;
        for (int i = 1; i <= n; i++) {
            p[i] = (p[i - 1] * base) % mod;
            h[i] = (h[i - 1] * base + word[i - 1] - 'a') % mod;
        }
    }

    long long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
};

class Solution {
public:
    int minimumTimeToInitialState(string word, int k) {
        Hashing hashing(word, 13331, 998244353);
        int n = word.size();
        for (int i = k; i < n; i += k) {
            if (hashing.query(1, n - i) == hashing.query(i + 1, n)) {
                return i / k;
            }
        }
        return (n + k - 1) / k;
    }
};
```

#### Go

```go
type Hashing struct {
	p   []int64
	h   []int64
	mod int64
}

func NewHashing(word string, base int64, mod int64) *Hashing {
	n := len(word)
	p := make([]int64, n+1)
	h := make([]int64, n+1)
	p[0] = 1
	for i := 1; i <= n; i++ {
		p[i] = (p[i-1] * base) % mod
		h[i] = (h[i-1]*base + int64(word[i-1]-'a')) % mod
	}
	return &Hashing{p, h, mod}
}

func (hashing *Hashing) Query(l, r int) int64 {
	return (hashing.h[r] - hashing.h[l-1]*hashing.p[r-l+1]%hashing.mod + hashing.mod) % hashing.mod
}

func minimumTimeToInitialState(word string, k int) int {
	hashing := NewHashing(word, 13331, 998244353)
	n := len(word)
	for i := k; i < n; i += k {
		if hashing.Query(1, n-i) == hashing.Query(i+1, n) {
			return i / k
		}
	}
	return (n + k - 1) / k
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
