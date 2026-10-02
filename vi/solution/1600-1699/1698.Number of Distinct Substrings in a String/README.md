---
comments: true
difficulty: Medium
tags:
    - Trie
    - String
    - Suffix Array
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [1698. Number of Distinct Substrings in a String 🔒](https://leetcode.com/problems/number-of-distinct-substrings-in-a-string)

[中文文档](/solution/1600-1699/1698.Number%20of%20Distinct%20Substrings%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, hãy trả về <em>số lượng <strong>khác nhau</strong> chuỗi con của</em>&nbsp;<code>s</code>.</p>

<p>Một <strong>chuỗi con</strong> của một chuỗi được tạo ra bằng cách xóa một số ký tự bất kỳ (có thể bằng 0) ở đầu chuỗi và một số ký tự bất kỳ (có thể bằng 0) ở cuối chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabbaba&quot;
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong> Tập hợp các chuỗi khác nhau là [&quot;a&quot;,&quot;b&quot;,&quot;aa&quot;,&quot;bb&quot;,&quot;ab&quot;,&quot;ba&quot;,&quot;aab&quot;,&quot;abb&quot;,&quot;bab&quot;,&quot;bba&quot;,&quot;aba&quot;,&quot;aabb&quot;,&quot;abba&quot;,&quot;bbab&quot;,&quot;baba&quot;,&quot;aabba&quot;,&quot;abbab&quot;,&quot;bbaba&quot;,&quot;aabbab&quot;,&quot;abbaba&quot;,&quot;aabbaba&quot;]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdefg&quot;
<strong>Đầu ra:</strong> 28
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán này với độ phức tạp thời gian <code>O(n)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chuỗi con khác nhau. $n$ nhỏ, nên đưa tất cả $O(n^2)$ đoạn vào một set. Việc cắt chuỗi khiến thời gian khoảng $O(n^3)$, vẫn chấp nhận được trong bài này.

<!-- thinking:end -->

Liệt kê tất cả chuỗi con và dùng hash table để ghi nhận số lượng chuỗi con khác nhau.

Độ phức tạp thời gian là $O(n^3)$, và độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDistinct(self, s: str) -> int:
        n = len(s)
        return len({s[i:j] for i in range(n) for j in range(i + 1, n + 1)})
```

#### Java

```java
class Solution {
    public int countDistinct(String s) {
        Set<String> ss = new HashSet<>();
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j <= n; ++j) {
                ss.add(s.substring(i, j));
            }
        }
        return ss.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countDistinct(string s) {
        unordered_set<string_view> ss;
        int n = s.size();
        string_view t, v = s;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j <= n; ++j) {
                t = v.substr(i, j - i);
                ss.insert(t);
            }
        }
        return ss.size();
    }
};
```

#### Go

```go
func countDistinct(s string) int {
	ss := map[string]struct{}{}
	for i := range s {
		for j := i + 1; j <= len(s); j++ {
			ss[s[i:j]] = struct{}{}
		}
	}
	return len(ss)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Băm chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 so sánh toàn bộ các đoạn. Băm đa thức cho fingerprint của mọi chuỗi con trong $O(1)$; liệt kê hai đầu mút và lưu các hash giúp so sánh số nguyên trong $O(n^2)$.

<!-- thinking:end -->

**Băm chuỗi** là phương pháp ánh xạ một chuỗi bất kỳ độ dài nào thành một số nguyên không âm, với xác suất va chạm gần như bằng không. Băm chuỗi dùng để tính giá trị hash của một chuỗi, qua đó nhanh chóng xác định hai chuỗi có bằng nhau hay không.

Ta chọn một giá trị cố định BASE, xem chuỗi như một số trong hệ cơ số BASE và gán một giá trị lớn hơn 0 cho mỗi ký tự. Thông thường, các giá trị được gán nhỏ hơn nhiều so với BASE. Ví dụ, với chuỗi gồm các chữ cái viết thường, ta có thể đặt a=1, b=2, ..., z=26. Ta chọn một giá trị cố định MOD, tính phần dư của số trong hệ cơ số BASE khi chia cho MOD và dùng nó làm giá trị hash của chuỗi.

Thông thường, ta chọn BASE=131 hoặc BASE=13331, khi đó xác suất va chạm của giá trị hash cực kỳ thấp. Nếu giá trị hash của hai chuỗi giống nhau, ta xem hai chuỗi là bằng nhau. Thông thường, MOD được chọn là 2^64. Trong C++, ta có thể dùng trực tiếp kiểu unsigned long long để lưu giá trị hash này. Khi tính toán, ta không xử lý tràn số học. Khi xảy ra tràn, điều đó tương đương với tự động lấy modulo 2^64, giúp tránh các phép modulo kém hiệu quả.

Ngoại trừ dữ liệu được xây dựng đặc biệt, thuật toán hash trên khó xảy ra va chạm. Nhìn chung, thuật toán hash này có thể xuất hiện trong lời giải chuẩn của bài toán. Ta cũng có thể chọn các giá trị BASE và MOD phù hợp (chẳng hạn các số nguyên tố lớn), thực hiện nhiều nhóm phép hash và chỉ xem các chuỗi ban đầu bằng nhau khi mọi kết quả đều giống nhau, khiến việc tạo dữ liệu làm hash sai trở nên khó hơn.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDistinct(self, s: str) -> int:
        base = 131
        n = len(s)
        p = [0] * (n + 10)
        h = [0] * (n + 10)
        p[0] = 1
        for i, c in enumerate(s):
            p[i + 1] = p[i] * base
            h[i + 1] = h[i] * base + ord(c)
        ss = set()
        for i in range(1, n + 1):
            for j in range(i, n + 1):
                t = h[j] - h[i - 1] * p[j - i + 1]
                ss.add(t)
        return len(ss)
```

#### Java

```java
class Solution {
    public int countDistinct(String s) {
        int base = 131;
        int n = s.length();
        long[] p = new long[n + 10];
        long[] h = new long[n + 10];
        p[0] = 1;
        for (int i = 0; i < n; ++i) {
            p[i + 1] = p[i] * base;
            h[i + 1] = h[i] * base + s.charAt(i);
        }
        Set<Long> ss = new HashSet<>();
        for (int i = 1; i <= n; ++i) {
            for (int j = i; j <= n; ++j) {
                long t = h[j] - h[i - 1] * p[j - i + 1];
                ss.add(t);
            }
        }
        return ss.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countDistinct(string s) {
        using ull = unsigned long long;
        int n = s.size();
        ull p[n + 10];
        ull h[n + 10];
        int base = 131;
        p[0] = 1;
        for (int i = 0; i < n; ++i) {
            p[i + 1] = p[i] * base;
            h[i + 1] = h[i] * base + s[i];
        }
        unordered_set<ull> ss;
        for (int i = 1; i <= n; ++i) {
            for (int j = i; j <= n; ++j) {
                ss.insert(h[j] - h[i - 1] * p[j - i + 1]);
            }
        }
        return ss.size();
    }
};
```

#### Go

```go
func countDistinct(s string) int {
	n := len(s)
	p := make([]int, n+10)
	h := make([]int, n+10)
	p[0] = 1
	base := 131
	for i, c := range s {
		p[i+1] = p[i] * base
		h[i+1] = h[i]*base + int(c)
	}
	ss := map[int]struct{}{}
	for i := 1; i <= n; i++ {
		for j := i; j <= n; j++ {
			ss[h[j]-h[i-1]*p[j-i+1]] = struct{}{}
		}
	}
	return len(ss)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
