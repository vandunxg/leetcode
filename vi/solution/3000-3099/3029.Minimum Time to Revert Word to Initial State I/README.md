---
comments: true
difficulty: Medium
rating: 1659
source: Weekly Contest 383 Q2
tags:
    - String
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3029. Minimum Time to Revert Word to Initial State I](https://leetcode.com/problems/minimum-time-to-revert-word-to-initial-state-i)

[中文文档](/solution/3000-3099/3029.Minimum%20Time%20to%20Revert%20Word%20to%20Initial%20State%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>0-indexed</strong> <code>word</code> và một số nguyên <code>k</code>.</p>

<p>Ở mỗi giây, bạn phải thực hiện các thao tác sau:</p>

<ul>
	<li>Xóa <code>k</code> ký tự đầu tiên của <code>word</code>.</li>
	<li>Thêm bất kỳ <code>k</code> ký tự nào vào cuối <code>word</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng bạn không nhất thiết phải thêm lại những ký tự đã xóa. Tuy nhiên, bạn phải thực hiện <strong>cả hai</strong> thao tác ở mỗi giây.</p>

<p>Trả về <em>thời gian <strong>nhỏ nhất</strong> lớn hơn 0 cần thiết để</em> <code>word</code> <em>trở về trạng thái <strong>ban đầu</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abacaba&quot;, k = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ở giây thứ 1, ta xóa các ký tự &quot;aba&quot; ở đầu word và thêm các ký tự &quot;bac&quot; vào cuối word. Khi đó, word trở thành &quot;cababac&quot;.
Ở giây thứ 2, ta xóa các ký tự &quot;cab&quot; ở đầu word và thêm &quot;aba&quot; vào cuối word. Khi đó, word trở thành &quot;abacaba&quot; và trở về trạng thái ban đầu.
Có thể chứng minh rằng 2 giây là thời gian nhỏ nhất lớn hơn 0 cần thiết để word trở về trạng thái ban đầu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abacaba&quot;, k = 4
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ở giây thứ 1, ta xóa các ký tự &quot;abac&quot; ở đầu word và thêm các ký tự &quot;caba&quot; vào cuối word. Khi đó, word trở thành &quot;abacaba&quot; và trở về trạng thái ban đầu.
Có thể chứng minh rằng 1 giây là thời gian nhỏ nhất lớn hơn 0 cần thiết để word trở về trạng thái ban đầu.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abcbabcd&quot;, k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ở mỗi giây, ta xóa 2 ký tự đầu tiên của word và thêm chính các ký tự đó vào cuối word.
Sau 4 giây, word trở thành &quot;abcbabcd&quot; và trở về trạng thái ban đầu.
Có thể chứng minh rằng 4 giây là thời gian nhỏ nhất lớn hơn 0 cần thiết để word trở về trạng thái ban đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 50 </code></li>
	<li><code>1 &lt;= k &lt;= word.length</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác loại bỏ $k$ ký tự đầu tiên và thêm các ký tự bất kỳ vào cuối. Chuỗi trở về trạng thái ban đầu khi hậu tố còn lại bằng tiền tố có cùng độ dài. Vì $n \le 50$, ta có thể duyệt số lần thao tác.
>
> Sau $i$ thao tác, phần còn lại là $\textit{word}[ik:]$, cần bằng $\textit{word}[:n-ik]$. Nếu không lần nào khớp, $\lceil n/k \rceil$ thao tác sẽ làm chuỗi rỗng.
>
> Ta thử $k,2k,\ldots$ bằng cách so sánh chuỗi trực tiếp; nếu không khớp thì trả về giá trị làm tròn lên.

<!-- thinking:end -->

Giả sử ta có thể đưa `word` về trạng thái ban đầu chỉ sau một thao tác. Khi đó, `word[k:]` phải là tiền tố của `word`, tức là `word[k:] == word[:n-k]`.

Nếu cần nhiều thao tác, giả sử $i$ là số thao tác. Khi đó, `word[k*i:]` phải là tiền tố của `word`, tức là `word[k*i:] == word[:n-k*i]`.

Vì vậy, ta có thể duyệt số thao tác và kiểm tra xem `word[k*i:]` có phải là tiền tố của `word` hay không. Nếu đúng, ta trả về $i$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của `word`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTimeToInitialState(self, word: str, k: int) -> int:
        n = len(word)
        for i in range(k, n, k):
            if word[i:] == word[:-i]:
                return i // k
        return (n + k - 1) // k
```

#### Java

```java
class Solution {
    public int minimumTimeToInitialState(String word, int k) {
        int n = word.length();
        for (int i = k; i < n; i += k) {
            if (word.substring(i).equals(word.substring(0, n - i))) {
                return i / k;
            }
        }
        return (n + k - 1) / k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTimeToInitialState(string word, int k) {
        int n = word.size();
        for (int i = k; i < n; i += k) {
            if (word.substr(i) == word.substr(0, n - i)) {
                return i / k;
            }
        }
        return (n + k - 1) / k;
    }
};
```

#### Go

```go
func minimumTimeToInitialState(word string, k int) int {
	n := len(word)
	for i := k; i < n; i += k {
		if word[i:] == word[:n-i] {
			return i / k
		}
	}
	return (n + k - 1) / k
}
```

#### TypeScript

```ts
function minimumTimeToInitialState(word: string, k: number): number {
    const n = word.length;
    for (let i = k; i < n; i += k) {
        if (word.slice(i) === word.slice(0, -i)) {
            return Math.floor(i / k);
        }
    }
    return Math.floor((n + k - 1) / k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt + Hash chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 so sánh các chuỗi có độ dài $O(n)$ nên tốn $O(n^2)$. Cách này vẫn đủ nhanh với ràng buộc hiện tại, nhưng ta có thể tiền xử lý để kiểm tra bằng $O(1)$.
>
> Hash chuỗi tạo fingerprint cho mỗi chuỗi con, nên mỗi lần kiểm tra tiền tố chỉ cần một truy vấn và toàn bộ quá trình duyệt có độ phức tạp tuyến tính.

<!-- thinking:end -->

Dựa trên Lời giải 1, ta cũng có thể dùng hash chuỗi để xác định hai chuỗi có bằng nhau hay không.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của `word`.

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
