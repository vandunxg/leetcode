---
comments: true
difficulty: Hard
rating: 1786
source: Weekly Contest 170 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1312. Minimum Insertion Steps to Make a String Palindrome](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome)

[中文文档](/solution/1300-1399/1312.Minimum%20Insertion%20Steps%20to%20Make%20a%20String%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>. Trong một bước, bạn có thể chèn một ký tự bất kỳ vào bất kỳ vị trí nào trong chuỗi.</p>

<p>Trả về <em>số bước ít nhất</em> để biến <code>s</code> thành palindrome.</p>

<p><b>Palindrome</b> là chuỗi đọc xuôi và đọc ngược đều giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;zzazz&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chuỗi &quot;zzazz&quot; đã là palindrome nên không cần chèn thêm ký tự nào.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;mbadm&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chuỗi có thể trở thành &quot;mbdadbm&quot; hoặc &quot;mdbabdm&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Sau khi chèn 5 ký tự, chuỗi trở thành &quot;leetcodocteel&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần số lần chèn ít nhất để biến $s$ thành palindrome. Với $n \le 500$, không thể liệt kê mọi cách chèn. Mỗi bài toán con là một đoạn: nếu hai đầu giống nhau thì xét đoạn nằm giữa; nếu khác nhau, chèn một bản sao của một trong hai ký tự đầu mút ở phía đối diện, rồi lấy kết quả nhỏ hơn giữa hai lựa chọn và cộng $1$.
>
> Các đoạn con chồng lấn nên cùng một cặp $(i,j)$ được tính nhiều lần. Memoize kết quả $dfs(i,j)$ để mỗi đoạn chỉ được xử lý một lần, với độ phức tạp $O(n^2)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minInsertions(self, s: str) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= j:
                return 0
            if s[i] == s[j]:
                return dfs(i + 1, j - 1)
            return 1 + min(dfs(i + 1, j), dfs(i, j - 1))

        return dfs(0, len(s) - 1)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private String s;

    public int minInsertions(String s) {
        this.s = s;
        int n = s.length();
        f = new Integer[n][n];
        return dfs(0, n - 1);
    }

    private int dfs(int i, int j) {
        if (i >= j) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = 1 << 30;
        if (s.charAt(i) == s.charAt(j)) {
            ans = dfs(i + 1, j - 1);
        } else {
            ans = Math.min(dfs(i + 1, j), dfs(i, j - 1)) + 1;
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minInsertions(string s) {
        int n = s.size();
        int f[n][n];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i >= j) {
                return 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            int ans = 1 << 30;
            if (s[i] == s[j]) {
                ans = dfs(i + 1, j - 1);
            } else {
                ans = min(dfs(i + 1, j), dfs(i, j - 1)) + 1;
            }
            return f[i][j] = ans;
        };
        return dfs(0, n - 1);
    }
};
```

#### Go

```go
func minInsertions(s string) int {
	n := len(s)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i >= j {
			return 0
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		ans := 1 << 30
		if s[i] == s[j] {
			ans = dfs(i+1, j-1)
		} else {
			ans = min(dfs(i+1, j), dfs(i, j-1)) + 1
		}
		f[i][j] = ans
		return ans
	}
	return dfs(0, n-1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Memoization vẫn dùng call stack và cache. Dùng cùng công thức truy hồi để điền bảng $f[i][j]$ theo thứ tự $i$ giảm dần, $j$ tăng dần sẽ đảm bảo các trạng thái $f[i+1][j-1]$, $f[i+1][j]$ và $f[i][j-1]$ đã có giá trị, nhờ đó loại bỏ đệ quy.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minInsertions(self, s: str) -> int:
        n = len(s)
        f = [[0] * n for _ in range(n)]
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                if s[i] == s[j]:
                    f[i][j] = f[i + 1][j - 1]
                else:
                    f[i][j] = min(f[i + 1][j], f[i][j - 1]) + 1
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int minInsertions(String s) {
        int n = s.length();
        int[][] f = new int[n][n];
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i + 1][j - 1];
                } else {
                    f[i][j] = Math.min(f[i + 1][j], f[i][j - 1]) + 1;
                }
            }
        }
        return f[0][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minInsertions(string s) {
        int n = s.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s[i] == s[j]) {
                    f[i][j] = f[i + 1][j - 1];
                } else {
                    f[i][j] = min(f[i + 1][j], f[i][j - 1]) + 1;
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func minInsertions(s string) int {
	n := len(s)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			if s[i] == s[j] {
				f[i][j] = f[i+1][j-1]
			} else {
				f[i][j] = min(f[i+1][j], f[i][j-1]) + 1
			}
		}
	}
	return f[0][n-1]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt theo chỉ số hai đầu mút chỉ là một cách điền bảng. Ta cũng có thể xét độ dài đoạn $k$ rồi xét đầu trái $i$ (với $j=i+k-1$), nhờ đó tính các đoạn ngắn trước. Công thức chuyển trạng thái không đổi; chỉ cần thêm vòng lặp bên ngoài theo độ dài đoạn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minInsertions(self, s: str) -> int:
        n = len(s)
        f = [[0] * n for _ in range(n)]
        for k in range(2, n + 1):
            for i in range(n - k + 1):
                j = i + k - 1
                if s[i] == s[j]:
                    f[i][j] = f[i + 1][j - 1]
                else:
                    f[i][j] = min(f[i + 1][j], f[i][j - 1]) + 1
        return f[0][n - 1]
```

#### Java

```java
class Solution {
    public int minInsertions(String s) {
        int n = s.length();
        int[][] f = new int[n][n];
        for (int k = 2; k <= n; ++k) {
            for (int i = 0; i + k - 1 < n; ++i) {
                int j = i + k - 1;
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i + 1][j - 1];
                } else {
                    f[i][j] = Math.min(f[i + 1][j], f[i][j - 1]) + 1;
                }
            }
        }
        return f[0][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minInsertions(string s) {
        int n = s.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        for (int k = 2; k <= n; ++k) {
            for (int i = 0; i + k - 1 < n; ++i) {
                int j = i + k - 1;
                if (s[i] == s[j]) {
                    f[i][j] = f[i + 1][j - 1];
                } else {
                    f[i][j] = min(f[i + 1][j], f[i][j - 1]) + 1;
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func minInsertions(s string) int {
	n := len(s)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
	}
	for k := 2; k <= n; k++ {
		for i := 0; i+k-1 < n; i++ {
			j := i + k - 1
			if s[i] == s[j] {
				f[i][j] = f[i+1][j-1]
			} else {
				f[i][j] = min(f[i+1][j], f[i][j-1]) + 1
			}
		}
	}
	return f[0][n-1]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
