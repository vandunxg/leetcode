---
comments: true
difficulty: Hard
rating: 2428
source: Weekly Contest 136 Q4
tags:
    - String
    - Binary Search
    - Suffix Array
    - Suffix Tree
    - Sliding Window
    - Hash Function
    - Rolling Hash
    - Boyer–Moore
    - Extended KMP
    - Suffix Automato
---

<!-- problem:start -->

# [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring)

[中文文档](/solution/1000-1099/1044.Longest%20Duplicate%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, xét mọi <em>chuỗi con trùng lặp</em>: các chuỗi con liên tiếp xuất hiện từ 2 lần trở lên trong s. Các lần xuất hiện có thể chồng lấn.</p>

<p>Trả về <strong>bất kỳ</strong> chuỗi con trùng lặp nào có độ dài lớn nhất. Nếu <code>s</code> không có chuỗi con trùng lặp, đáp án là <code>&quot;&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "banana"
<strong>Đầu ra:</strong> "ana"
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "abcd"
<strong>Đầu ra:</strong> ""
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đưa mọi chuỗi con vào set có độ phức tạp bậc hai, không đáp ứng được khi $n\le 3\times 10^4$. Nếu tồn tại chuỗi con trùng lặp độ dài $L$, thì cũng tồn tại chuỗi con trùng lặp ngắn hơn, nên tính khả thi đơn điệu theo độ dài.
>
> Dùng binary search trên độ dài. Hàm kiểm tra đưa từng lát cắt độ dài $\textit{mid}$ vào set và trả về khi gặp phần tử trùng. Nếu tìm thấy, thử độ dài lớn hơn; nếu không, giảm độ dài.
>
> Cách cài đặt dùng Python slice thông thường và hash set, đồng thời lưu lần tìm thấy gần nhất làm đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestDupSubstring(self, s: str) -> str:
        def check(l: int) -> str:
            vis = set()
            for i in range(1, n - l + 2):
                j = i + l - 1
                t = h[j] - h[i - 1] * p[j - i + 1]
                if t in vis:
                    return s[i - 1 : j]
                vis.add(t)
            return ''

        base, n = 131, len(s)
        p = [0] * (n + 10)
        h = [0] * (n + 10)
        p[0] = 1
        for i, c in enumerate(s):
            p[i + 1] = p[i] * base
            h[i + 1] = h[i] * base + ord(c)
        left, right = 0, n
        ans = ''
        while left < right:
            mid = (left + right + 1) >> 1
            t = check(mid)
            if t:
                left = mid
                ans = t
            else:
                right = mid - 1
        return ans
```

#### Java

```java
class Solution {
    private long[] p;
    private long[] h;

    public String longestDupSubstring(String s) {
        int base = 131;
        int n = s.length();
        p = new long[n + 10];
        h = new long[n + 10];
        p[0] = 1;
        for (int i = 0; i < n; ++i) {
            p[i + 1] = p[i] * base;
            h[i + 1] = h[i] * base + s.charAt(i);
        }
        String ans = "";
        int left = 0, right = n;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            String t = check(s, mid);
            if (t.length() > 0) {
                left = mid;
                ans = t;
            } else {
                right = mid - 1;
            }
        }
        return ans;
    }

    private String check(String s, int len) {
        int n = s.length();
        Set<Long> vis = new HashSet<>();
        for (int i = 1; i + len - 1 <= n; ++i) {
            int j = i + len - 1;
            long t = h[j] - h[i - 1] * p[j - i + 1];
            if (vis.contains(t)) {
                return s.substring(i - 1, j);
            }
            vis.add(t);
        }
        return "";
    }
}
```

#### C++

```cpp
typedef unsigned long long ULL;

class Solution {
public:
    ULL p[30010];
    ULL h[30010];
    string longestDupSubstring(string s) {
        int base = 131, n = s.size();
        p[0] = 1;
        for (int i = 0; i < n; ++i) {
            p[i + 1] = p[i] * base;
            h[i + 1] = h[i] * base + s[i];
        }
        int left = 0, right = n;
        string ans = "";
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            string t = check(s, mid);
            if (t.empty())
                right = mid - 1;
            else {
                left = mid;
                ans = t;
            }
        }
        return ans;
    }

    string check(string& s, int len) {
        int n = s.size();
        unordered_set<ULL> vis;
        for (int i = 1; i + len - 1 <= n; ++i) {
            int j = i + len - 1;
            ULL t = h[j] - h[i - 1] * p[j - i + 1];
            if (vis.count(t)) return s.substr(i - 1, len);
            vis.insert(t);
        }
        return "";
    }
};
```

#### Go

```go
func longestDupSubstring(s string) string {
	base, n := 131, len(s)
	p := make([]int64, n+10)
	h := make([]int64, n+10)
	p[0] = 1
	for i := 0; i < n; i++ {
		p[i+1] = p[i] * int64(base)
		h[i+1] = h[i]*int64(base) + int64(s[i])
	}
	check := func(l int) string {
		vis := make(map[int64]bool)
		for i := 1; i+l-1 <= n; i++ {
			j := i + l - 1
			t := h[j] - h[i-1]*p[j-i+1]
			if vis[t] {
				return s[i-1 : j]
			}
			vis[t] = true
		}
		return ""
	}
	left, right := 0, n
	ans := ""
	for left < right {
		mid := (left + right + 1) >> 1
		t := check(mid)
		if len(t) > 0 {
			left = mid
			ans = t
		} else {
			right = mid - 1
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
