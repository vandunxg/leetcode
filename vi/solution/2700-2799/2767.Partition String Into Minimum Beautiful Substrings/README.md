---
comments: true
difficulty: Medium
rating: 1864
source: Biweekly Contest 108 Q3
tags:
    - Hash Table
    - String
    - Dynamic Programming
    - Backtracking
---

<!-- problem:start -->

# [2767. Partition String Into Minimum Beautiful Substrings](https://leetcode.com/problems/partition-string-into-minimum-beautiful-substrings)

[中文文档](/solution/2700-2799/2767.Partition%20String%20Into%20Minimum%20Beautiful%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code>, hãy chia chuỗi thành một hoặc nhiều <strong>chuỗi con</strong> sao cho mỗi chuỗi con đều <strong>đẹp</strong>.</p>

<p>Một chuỗi được gọi là <strong>đẹp</strong> nếu:</p>

<ul>
	<li>Không chứa số 0 ở đầu.</li>
	<li>Là <strong>biểu diễn nhị phân</strong> của một số là lũy thừa của <code>5</code>.</li>
</ul>

<p>Trả về <em>số lượng <strong>ít nhất</strong> các chuỗi con trong cách chia như vậy.</em> Nếu không thể chia chuỗi <code>s</code> thành các chuỗi con đẹp, trả về <code>-1</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1011&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể chia chuỗi đã cho thành [&quot;101&quot;, &quot;1&quot;].
- Chuỗi &quot;101&quot; không chứa số 0 ở đầu và là biểu diễn nhị phân của số nguyên 5<sup>1</sup> = 5.
- Chuỗi &quot;1&quot; không chứa số 0 ở đầu và là biểu diễn nhị phân của số nguyên 5<sup>0</sup> = 1.
Có thể chứng minh rằng 2 là số lượng chuỗi con đẹp ít nhất mà chuỗi s có thể được chia thành.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;111&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể chia chuỗi đã cho thành [&quot;1&quot;, &quot;1&quot;, &quot;1&quot;].
- Chuỗi &quot;1&quot; không chứa số 0 ở đầu và là biểu diễn nhị phân của số nguyên 5<sup>0</sup> = 1.
Có thể chứng minh rằng 3 là số lượng chuỗi con đẹp ít nhất mà chuỗi s có thể được chia thành.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0&quot;
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể chia chuỗi s thành các chuỗi con đẹp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 15</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Chia chuỗi nhị phân thành ít phần nhất, trong đó mỗi phần không có số 0 ở đầu và bằng một lũy thừa của $5$. Với $n\le 15$, có thể liệt kê các khả năng, nhưng cần loại ngay các tiền tố bắt đầu bằng số 0.
>
> Ta tiền xử lý các lũy thừa của $5$ có độ dài phù hợp. $dfs(i)$ là số phần ít nhất có thể chia từ chỉ số $i$: nếu $s[i]=0$ thì không hợp lệ; ngược lại, ta tăng dần giá trị số và thử $1+dfs(j+1)$ khi giá trị đó là một lũy thừa của $5$. Nếu giá trị ghi nhớ vẫn là vô hạn, trả về $-1$.

<!-- thinking:end -->

Vì bài toán yêu cầu xác định một chuỗi có phải là biểu diễn nhị phân của một lũy thừa của $5$ hay không, trước hết ta có thể tiền xử lý tất cả các lũy thừa của $5$ và lưu chúng trong một bảng băm $ss$.

Tiếp theo, ta thiết kế hàm $dfs(i)$, biểu thị số lần cắt ít nhất từ ký tự thứ $i$ của chuỗi $s$ đến cuối chuỗi. Khi đó, đáp án là $dfs(0)$.

Cách tính hàm $dfs(i)$ như sau:

- Nếu $i \geq n$, nghĩa là đã xử lý tất cả ký tự, đáp án là $0$;
- Nếu $s[i] = 0$, nghĩa là chuỗi hiện tại có số $0$ ở đầu, không phù hợp với định nghĩa chuỗi đẹp, nên đáp án là vô hạn;
- Ngược lại, ta liệt kê vị trí kết thúc $j$ của chuỗi con bắt đầu từ $i$, dùng $x$ biểu thị giá trị thập phân của chuỗi con $s[i..j]$. Nếu $x$ nằm trong bảng băm $ss$, ta có thể chọn $s[i..j]$ làm một chuỗi con đẹp, và đáp án là $1 + dfs(j + 1)$. Ta cần liệt kê mọi $j$ có thể và lấy giá trị nhỏ nhất trong tất cả các đáp án.

Để tránh tính toán lặp lại, ta có thể sử dụng phương pháp tìm kiếm ghi nhớ.

Trong hàm chính, trước hết ta tiền xử lý tất cả các lũy thừa của $5$, sau đó gọi $dfs(0)$. Nếu giá trị trả về là vô hạn, nghĩa là không thể chia chuỗi $s$ thành các chuỗi con đẹp, khi đó trả về $-1$; ngược lại, trả về giá trị của $dfs(0)$.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumBeautifulSubstrings(self, s: str) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= n:
                return 0
            if s[i] == "0":
                return inf
            x = 0
            ans = inf
            for j in range(i, n):
                x = x << 1 | int(s[j])
                if x in ss:
                    ans = min(ans, 1 + dfs(j + 1))
            return ans

        n = len(s)
        x = 1
        ss = {x}
        for i in range(n):
            x *= 5
            ss.add(x)
        ans = dfs(0)
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    private Integer[] f;
    private String s;
    private Set<Long> ss = new HashSet<>();
    private int n;

    public int minimumBeautifulSubstrings(String s) {
        n = s.length();
        this.s = s;
        f = new Integer[n];
        long x = 1;
        for (int i = 0; i <= n; ++i) {
            ss.add(x);
            x *= 5;
        }
        int ans = dfs(0);
        return ans > n ? -1 : ans;
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (s.charAt(i) == '0') {
            return n + 1;
        }
        if (f[i] != null) {
            return f[i];
        }
        long x = 0;
        int ans = n + 1;
        for (int j = i; j < n; ++j) {
            x = x << 1 | (s.charAt(j) - '0');
            if (ss.contains(x)) {
                ans = Math.min(ans, 1 + dfs(j + 1));
            }
        }
        return f[i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumBeautifulSubstrings(string s) {
        unordered_set<long long> ss;
        int n = s.size();
        long long x = 1;
        for (int i = 0; i <= n; ++i) {
            ss.insert(x);
            x *= 5;
        }
        int f[n];
        memset(f, -1, sizeof(f));
        function<int(int)> dfs = [&](int i) {
            if (i >= n) {
                return 0;
            }
            if (s[i] == '0') {
                return n + 1;
            }
            if (f[i] != -1) {
                return f[i];
            }
            long long x = 0;
            int ans = n + 1;
            for (int j = i; j < n; ++j) {
                x = x << 1 | (s[j] - '0');
                if (ss.count(x)) {
                    ans = min(ans, 1 + dfs(j + 1));
                }
            }
            return f[i] = ans;
        };
        int ans = dfs(0);
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func minimumBeautifulSubstrings(s string) int {
	ss := map[int]bool{}
	n := len(s)
	x := 1
	f := make([]int, n+1)
	for i := 0; i <= n; i++ {
		ss[x] = true
		x *= 5
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if s[i] == '0' {
			return n + 1
		}
		if f[i] != -1 {
			return f[i]
		}
		f[i] = n + 1
		x := 0
		for j := i; j < n; j++ {
			x = x<<1 | int(s[j]-'0')
			if ss[x] {
				f[i] = min(f[i], 1+dfs(j+1))
			}
		}
		return f[i]
	}
	ans := dfs(0)
	if ans > n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumBeautifulSubstrings(s: string): number {
    const ss: Set<number> = new Set();
    const n = s.length;
    const f: number[] = new Array(n).fill(-1);
    for (let i = 0, x = 1; i <= n; ++i) {
        ss.add(x);
        x *= 5;
    }
    const dfs = (i: number): number => {
        if (i === n) {
            return 0;
        }
        if (s[i] === '0') {
            return n + 1;
        }
        if (f[i] !== -1) {
            return f[i];
        }
        f[i] = n + 1;
        for (let j = i, x = 0; j < n; ++j) {
            x = (x << 1) | (s[j] === '1' ? 1 : 0);
            if (ss.has(x)) {
                f[i] = Math.min(f[i], 1 + dfs(j + 1));
            }
        }
        return f[i];
    };
    const ans = dfs(0);
    return ans > n ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
