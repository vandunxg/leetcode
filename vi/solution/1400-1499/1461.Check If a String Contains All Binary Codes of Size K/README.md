---
comments: true
difficulty: Medium
rating: 1504
source: Biweekly Contest 27 Q2
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Directed Acyclic Graph
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [1461. Check If a String Contains All Binary Codes of Size K](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k)

[中文文档](/solution/1400-1499/1461.Check%20If%20a%20String%20Contains%20All%20Binary%20Codes%20of%20Size%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code> và một số nguyên <code>k</code>, hãy trả về <code>true</code> <em>nếu mọi mã nhị phân có độ dài</em> <code>k</code> <em>đều là chuỗi con của</em> <code>s</code>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00110110&quot;, k = 2
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các mã nhị phân có độ dài 2 là &quot;00&quot;, &quot;01&quot;, &quot;10&quot; và &quot;11&quot;. Lần lượt có thể tìm thấy chúng dưới dạng chuỗi con tại các chỉ số 0, 1, 3 và 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0110&quot;, k = 1
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các mã nhị phân có độ dài 1 là &quot;0&quot; và &quot;1&quot;, rõ ràng cả hai đều tồn tại dưới dạng chuỗi con.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0110&quot;, k = 2
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Mã nhị phân &quot;00&quot; có độ dài 2 và không tồn tại trong mảng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= k &lt;= 20</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Có $2^k$ chuỗi nhị phân có độ dài $k$, và $k\le 20$. Nếu $s$ có ít hơn $2^k$ cửa sổ, ta trả về `false`; nếu không, thu thập mọi chuỗi con có độ dài $k$ và kiểm tra kích thước của set.

<!-- thinking:end -->

Trước hết, với một chuỗi $s$ có độ dài $n$, số chuỗi con có độ dài $k$ là $n - k + 1$. Nếu $n - k + 1 < 2^k$, chắc chắn sẽ tồn tại một chuỗi nhị phân có độ dài $k$ không phải là chuỗi con của $s$, nên ta trả về `false`.

Tiếp theo, ta duyệt chuỗi $s$ và lưu tất cả chuỗi con có độ dài $k$ vào một set $ss$. Cuối cùng, ta kiểm tra xem kích thước của set $ss$ có bằng $2^k$ hay không.

Độ phức tạp thời gian là $O(n \times k)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasAllCodes(self, s: str, k: int) -> bool:
        n = len(s)
        m = 1 << k
        if n - k + 1 < m:
            return False
        ss = {s[i : i + k] for i in range(n - k + 1)}
        return len(ss) == m
```

#### Java

```java
class Solution {
    public boolean hasAllCodes(String s, int k) {
        int n = s.length();
        int m = 1 << k;
        if (n - k + 1 < m) {
            return false;
        }
        Set<String> ss = new HashSet<>();
        for (int i = 0; i < n - k + 1; ++i) {
            ss.add(s.substring(i, i + k));
        }
        return ss.size() == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasAllCodes(string s, int k) {
        int n = s.size();
        int m = 1 << k;
        if (n - k + 1 < m) {
            return false;
        }
        unordered_set<string> ss;
        for (int i = 0; i + k <= n; ++i) {
            ss.insert(move(s.substr(i, k)));
        }
        return ss.size() == m;
    }
};
```

#### Go

```go
func hasAllCodes(s string, k int) bool {
	n, m := len(s), 1<<k
	if n-k+1 < m {
		return false
	}
	ss := map[string]bool{}
	for i := 0; i+k <= n; i++ {
		ss[s[i:i+k]] = true
	}
	return len(ss) == m
}
```

#### TypeScript

```ts
function hasAllCodes(s: string, k: number): boolean {
    const n = s.length;
    const m = 1 << k;
    if (n - k + 1 < m) {
        return false;
    }
    const ss = new Set<string>();
    for (let i = 0; i + k <= n; ++i) {
        ss.add(s.slice(i, i + k));
    }
    return ss.size === m;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 sao chép $k$ ký tự cho mỗi cửa sổ. Ta biểu diễn cửa sổ bằng một số nguyên, dịch bit mới vào, loại bỏ bit cao và chèn vào trong $O(1)$.

<!-- thinking:end -->

Ở Lời giải 1, ta lưu tất cả chuỗi con phân biệt có độ dài $k$, và việc xử lý mỗi chuỗi con cần $O(k)$ thời gian. Thay vào đó, ta có thể sử dụng sliding window: mỗi khi thêm ký tự mới nhất, ta loại bỏ ký tự ngoài cùng bên trái khỏi cửa sổ. Trong quá trình này, ta dùng một số nguyên $x$ để lưu chuỗi con.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasAllCodes(self, s: str, k: int) -> bool:
        n = len(s)
        m = 1 << k
        if n - k + 1 < m:
            return False
        ss = set()
        x = int(s[:k], 2)
        ss.add(x)
        for i in range(k, n):
            a = int(s[i - k]) << (k - 1)
            b = int(s[i])
            x = (x - a) << 1 | b
            ss.add(x)
        return len(ss) == m
```

#### Java

```java
class Solution {
    public boolean hasAllCodes(String s, int k) {
        int n = s.length();
        int m = 1 << k;
        if (n - k + 1 < m) {
            return false;
        }
        boolean[] ss = new boolean[m];
        int x = Integer.parseInt(s.substring(0, k), 2);
        ss[x] = true;
        for (int i = k; i < n; ++i) {
            int a = (s.charAt(i - k) - '0') << (k - 1);
            int b = s.charAt(i) - '0';
            x = (x - a) << 1 | b;
            ss[x] = true;
        }
        for (boolean v : ss) {
            if (!v) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasAllCodes(string s, int k) {
        int n = s.size();
        int m = 1 << k;
        if (n - k + 1 < m) {
            return false;
        }
        bool ss[m];
        memset(ss, false, sizeof(ss));
        int x = stoi(s.substr(0, k), nullptr, 2);
        ss[x] = true;
        for (int i = k; i < n; ++i) {
            int a = (s[i - k] - '0') << (k - 1);
            int b = s[i] - '0';
            x = (x - a) << 1 | b;
            ss[x] = true;
        }
        return all_of(ss, ss + m, [](bool v) { return v; });
    }
};
```

#### Go

```go
func hasAllCodes(s string, k int) bool {
	n, m := len(s), 1<<k
	if n-k+1 < m {
		return false
	}
	ss := make([]bool, m)
	x, _ := strconv.ParseInt(s[:k], 2, 64)
	ss[x] = true
	for i := k; i < n; i++ {
		a := int64(s[i-k]-'0') << (k - 1)
		b := int64(s[i] - '0')
		x = (x-a)<<1 | b
		ss[x] = true
	}
	for _, v := range ss {
		if !v {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function hasAllCodes(s: string, k: number): boolean {
    const n = s.length;
    const m = 1 << k;
    if (n - k + 1 < m) {
        return false;
    }
    let x = +`0b${s.slice(0, k)}`;
    const ss = new Set<number>();
    ss.add(x);
    for (let i = k; i < n; ++i) {
        const a = +s[i - k] << (k - 1);
        const b = +s[i];
        x = ((x - a) << 1) | b;
        ss.add(x);
    }
    return ss.size === m;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
