---
comments: true
difficulty: Hard
rating: 2101
source: Weekly Contest 313 Q4
tags:
    - String
    - Dynamic Programming
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [2430. Maximum Deletions on a String](https://leetcode.com/problems/maximum-deletions-on-a-string)

[中文文档](/solution/2400-2499/2430.Maximum%20Deletions%20on%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường. Trong một thao tác, bạn có thể:</p>

<ul>
	<li>Xóa <strong>toàn bộ chuỗi</strong> <code>s</code>, hoặc</li>
	<li>Xóa <strong>đầu tiên</strong> <code>i</code> ký tự của <code>s</code> nếu <code>i</code> ký tự đầu tiên của <code>s</code> <strong>bằng</strong> <code>i</code> ký tự tiếp theo trong <code>s</code>, với mọi <code>i</code> trong khoảng <code>1 &lt;= i &lt;= s.length / 2</code>.</li>
</ul>

<p>Ví dụ, nếu <code>s = &quot;ababc&quot;</code>, trong một thao tác, bạn có thể xóa hai ký tự đầu tiên của <code>s</code> để được <code>&quot;abc&quot;</code>, vì hai ký tự đầu tiên của <code>s</code> và hai ký tự tiếp theo của <code>s</code> đều là <code>&quot;ab&quot;</code>.</p>

<p>Trả về <em><strong>số thao tác lớn nhất</strong> cần dùng để xóa hết </em><code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcabcdabc&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Xóa 3 ký tự đầu tiên (&quot;abc&quot;) vì 3 ký tự tiếp theo cũng giống nhau. Khi đó, s = &quot;abcdabc&quot;.
- Xóa tất cả các ký tự.
Ta đã dùng 2 thao tác, nên trả về 2. Có thể chứng minh rằng 2 là số thao tác lớn nhất cần dùng.
Lưu ý rằng trong thao tác thứ hai, ta không thể xóa lại &quot;abc&quot; vì lần xuất hiện tiếp theo của &quot;abc&quot; không nằm trong 3 ký tự tiếp theo.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabaab&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
- Xóa ký tự đầu tiên (&quot;a&quot;) vì ký tự tiếp theo cũng giống nhau. Khi đó, s = &quot;aabaab&quot;.
- Xóa 3 ký tự đầu tiên (&quot;aab&quot;) vì 3 ký tự tiếp theo cũng giống nhau. Khi đó, s = &quot;aab&quot;.
- Xóa ký tự đầu tiên (&quot;a&quot;) vì ký tự tiếp theo cũng giống nhau. Khi đó, s = &quot;ab&quot;.
- Xóa tất cả các ký tự.
Ta đã dùng 4 thao tác, nên trả về 4. Có thể chứng minh rằng 4 là số thao tác lớn nhất cần dùng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaaa&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Trong mỗi thao tác, ta có thể xóa ký tự đầu tiên của s.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 4000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 4000$, một thao tác sẽ xóa một prefix bằng với block tiếp theo có cùng độ dài. Các trạng thái tìm kiếm chỉ cần xác định bởi vị trí bắt đầu $i$ của suffix còn lại. Từ $i$, ta thử các độ dài $j$ và so sánh các lát $s[i:i+j]$ và $s[i+j:i+2j]$; nếu bằng nhau, ta chuyển đến $i+j$ và tăng số lần xóa thêm một lần. Vì xóa toàn bộ chuỗi cùng lúc luôn hợp lệ, đáp án ít nhất là $1$.
>
> Dùng memoization cho $dfs(i)$. Việc so sánh các lát mất thời gian tuyến tính theo $j$, nên tổng thời gian $O(n^2)$ phù hợp với giới hạn.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$, biểu diễn số thao tác lớn nhất cần dùng để xóa toàn bộ ký tự trong $s[i..]$. Đáp án là $dfs(0)$.

Quá trình tính hàm $dfs(i)$ như sau:

- Nếu $i \geq n$ thì $dfs(i) = 0$, trả về ngay.
- Nếu không, ta duyệt độ dài chuỗi $j$, với $1 \leq j \leq (n-1)/2$. Nếu $s[i..i+j] = s[i+j..i+j+j]$, ta có thể xóa $s[i..i+j]$, khi đó $dfs(i)=max(dfs(i), dfs(i+j)+1)$. Ta cần duyệt mọi $j$ để tìm giá trị lớn nhất của $dfs(i)$.

Ở đây, ta cần nhanh chóng xác định $s[i..i+j]$ có bằng $s[i+j..i+j+j]$ hay không. Ta có thể tiền xử lý toàn bộ longest common prefix của chuỗi $s$, trong đó $g[i][j]$ biểu diễn độ dài tiền tố chung dài nhất của $s[i..]$ và $s[j..]$. Nhờ đó, ta có thể nhanh chóng xác định $s[i..i+j]$ có bằng $s[i+j..i+j+j]$ hay không, tức là $g[i][i+j] \geq j$.

Để tránh tính toán lặp lại, ta có thể dùng memoization search và dùng mảng $f$ để lưu giá trị của hàm $dfs(i)$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def deleteString(self, s: str) -> int:
        @cache
        def dfs(i: int) -> int:
            if i == n:
                return 0
            ans = 1
            for j in range(1, (n - i) // 2 + 1):
                if s[i : i + j] == s[i + j : i + j + j]:
                    ans = max(ans, 1 + dfs(i + j))
            return ans

        n = len(s)
        return dfs(0)
```

#### Java

```java
class Solution {
    private int n;
    private Integer[] f;
    private int[][] g;

    public int deleteString(String s) {
        n = s.length();
        f = new Integer[n];
        g = new int[n + 1][n + 1];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s.charAt(i) == s.charAt(j)) {
                    g[i][j] = g[i + 1][j + 1] + 1;
                }
            }
        }
        return dfs(0);
    }

    private int dfs(int i) {
        if (i == n) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        f[i] = 1;
        for (int j = 1; j <= (n - i) / 2; ++j) {
            if (g[i][i + j] >= j) {
                f[i] = Math.max(f[i], 1 + dfs(i + j));
            }
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int deleteString(string s) {
        int n = s.size();
        int g[n + 1][n + 1];
        memset(g, 0, sizeof(g));
        for (int i = n - 1; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s[i] == s[j]) {
                    g[i][j] = g[i + 1][j + 1] + 1;
                }
            }
        }
        int f[n];
        memset(f, 0, sizeof(f));
        function<int(int)> dfs = [&](int i) -> int {
            if (i == n) {
                return 0;
            }
            if (f[i]) {
                return f[i];
            }
            f[i] = 1;
            for (int j = 1; j <= (n - i) / 2; ++j) {
                if (g[i][i + j] >= j) {
                    f[i] = max(f[i], 1 + dfs(i + j));
                }
            }
            return f[i];
        };
        return dfs(0);
    }
};
```

#### Go

```go
func deleteString(s string) int {
	n := len(s)
	g := make([][]int, n+1)
	for i := range g {
		g[i] = make([]int, n+1)
	}
	for i := n - 1; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			if s[i] == s[j] {
				g[i][j] = g[i+1][j+1] + 1
			}
		}
	}
	f := make([]int, n)
	var dfs func(int) int
	dfs = func(i int) int {
		if i == n {
			return 0
		}
		if f[i] > 0 {
			return f[i]
		}
		f[i] = 1
		for j := 1; j <= (n-i)/2; j++ {
			if g[i][i+j] >= j {
				f[i] = max(f[i], dfs(i+j)+1)
			}
		}
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function deleteString(s: string): number {
    const n = s.length;
    const f: number[] = new Array(n).fill(0);
    const dfs = (i: number): number => {
        if (i == n) {
            return 0;
        }
        if (f[i] > 0) {
            return f[i];
        }
        f[i] = 1;
        for (let j = 1; j <= (n - i) >> 1; ++j) {
            if (s.slice(i, i + j) == s.slice(i + j, i + j + j)) {
                f[i] = Math.max(f[i], dfs(i + j) + 1);
            }
        }
        return f[i];
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 phải so sánh các lát nhiều lần và chịu thêm overhead của đệ quy. Ta tiền xử lý LCP $g[i][j]$ để kiểm tra hai đoạn có bằng nhau hay không bằng điều kiện $g[i][i+j]\ge j$ trong $O(1)$. Tính $f[i]$ từ cuối về đầu; ngoài bảng LCP, phần DP chỉ cần một chiều.

<!-- thinking:end -->

Ta có thể chuyển tìm kiếm có ghi nhớ ở Lời giải 1 thành quy hoạch động. Định nghĩa $f[i]$ là số thao tác lớn nhất cần dùng để xóa toàn bộ ký tự trong $s[i..]$. Ban đầu, $f[i]=1$, và đáp án là $f[0]$.

Ta có thể duyệt $i$ từ cuối về đầu. Với mỗi $i$, ta duyệt độ dài chuỗi $j$, với $1 \leq j \leq (n-1)/2$. Nếu $s[i..i+j] = s[i+j..i+j+j]$, ta có thể xóa $s[i..i+j]$, khi đó $f[i]=max(f[i], f[i+j]+1)$. Ta cần duyệt mọi $j$ để tìm giá trị lớn nhất của $f[i]$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def deleteString(self, s: str) -> int:
        n = len(s)
        g = [[0] * (n + 1) for _ in range(n + 1)]
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                if s[i] == s[j]:
                    g[i][j] = g[i + 1][j + 1] + 1

        f = [1] * n
        for i in range(n - 1, -1, -1):
            for j in range(1, (n - i) // 2 + 1):
                if g[i][i + j] >= j:
                    f[i] = max(f[i], f[i + j] + 1)
        return f[0]
```

#### Java

```java
class Solution {
    public int deleteString(String s) {
        int n = s.length();
        int[][] g = new int[n + 1][n + 1];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s.charAt(i) == s.charAt(j)) {
                    g[i][j] = g[i + 1][j + 1] + 1;
                }
            }
        }
        int[] f = new int[n];
        for (int i = n - 1; i >= 0; --i) {
            f[i] = 1;
            for (int j = 1; j <= (n - i) / 2; ++j) {
                if (g[i][i + j] >= j) {
                    f[i] = Math.max(f[i], f[i + j] + 1);
                }
            }
        }
        return f[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int deleteString(string s) {
        int n = s.size();
        int g[n + 1][n + 1];
        memset(g, 0, sizeof(g));
        for (int i = n - 1; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s[i] == s[j]) {
                    g[i][j] = g[i + 1][j + 1] + 1;
                }
            }
        }
        int f[n];
        for (int i = n - 1; ~i; --i) {
            f[i] = 1;
            for (int j = 1; j <= (n - i) / 2; ++j) {
                if (g[i][i + j] >= j) {
                    f[i] = max(f[i], f[i + j] + 1);
                }
            }
        }
        return f[0];
    }
};
```

#### Go

```go
func deleteString(s string) int {
	n := len(s)
	g := make([][]int, n+1)
	for i := range g {
		g[i] = make([]int, n+1)
	}
	for i := n - 1; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			if s[i] == s[j] {
				g[i][j] = g[i+1][j+1] + 1
			}
		}
	}
	f := make([]int, n)
	for i := n - 1; i >= 0; i-- {
		f[i] = 1
		for j := 1; j <= (n-i)/2; j++ {
			if g[i][i+j] >= j {
				f[i] = max(f[i], f[i+j]+1)
			}
		}
	}
	return f[0]
}
```

#### TypeScript

```ts
function deleteString(s: string): number {
    const n = s.length;
    const f: number[] = new Array(n).fill(1);
    for (let i = n - 1; i >= 0; --i) {
        for (let j = 1; j <= (n - i) >> 1; ++j) {
            if (s.slice(i, i + j) === s.slice(i + j, i + j + j)) {
                f[i] = Math.max(f[i], f[i + j] + 1);
            }
        }
    }
    return f[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
