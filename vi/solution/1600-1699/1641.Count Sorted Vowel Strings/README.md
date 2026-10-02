---
comments: true
difficulty: Medium
rating: 1519
source: Weekly Contest 213 Q2
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [1641. Count Sorted Vowel Strings](https://leetcode.com/problems/count-sorted-vowel-strings)

[中文文档](/solution/1600-1699/1641.Count%20Sorted%20Vowel%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số chuỗi có độ dài </em><code>n</code><em>, chỉ gồm các nguyên âm (</em><code>a</code><em>, </em><code>e</code><em>, </em><code>i</code><em>, </em><code>o</code><em>, </em><code>u</code><em>) và được </em><strong>sắp xếp theo thứ tự từ điển</strong>.</p>

<p>Chuỗi <code>s</code> được <strong>sắp xếp theo thứ tự từ điển</strong> nếu với mọi <code>i</code> hợp lệ, <code>s[i]</code> đứng trước hoặc trùng với <code>s[i+1]</code> trong bảng chữ cái.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> 5
<strong>Explanation:</strong> 5 chuỗi được sắp xếp chỉ gồm nguyên âm là <code>[&quot;a&quot;,&quot;e&quot;,&quot;i&quot;,&quot;o&quot;,&quot;u&quot;].</code>
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 2
<strong>Output:</strong> 15
<strong>Explanation:</strong> 15 chuỗi được sắp xếp chỉ gồm nguyên âm là
[&quot;aa&quot;,&quot;ae&quot;,&quot;ai&quot;,&quot;ao&quot;,&quot;au&quot;,&quot;ee&quot;,&quot;ei&quot;,&quot;eo&quot;,&quot;eu&quot;,&quot;ii&quot;,&quot;io&quot;,&quot;iu&quot;,&quot;oo&quot;,&quot;ou&quot;,&quot;uu&quot;].
Lưu ý rằng &quot;ea&quot; không hợp lệ vì &#39;e&#39; đứng sau &#39;a&#39; trong bảng chữ cái.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 33
<strong>Output:</strong> 66045
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code>&nbsp;</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi nguyên âm không giảm độ dài $n$ là một cách chọn có lặp từ năm chữ cái. $n$ nhỏ, nên ta tìm kiếm theo trạng thái “đã đặt $i$ chữ cái, chỉ số nguyên âm cuối là $j$”.
>
> $dfs(i,j)$ trả về $1$ khi $i=n$, còn lại tính tổng $dfs(i+1,k)$ với $k \ge j$ và ghi nhớ các trạng thái để tái sử dụng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countVowelStrings(self, n: int) -> int:
        @cache
        def dfs(i, j):
            return 1 if i >= n else sum(dfs(i + 1, k) for k in range(j, 5))

        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int n;

    public int countVowelStrings(int n) {
        this.n = n;
        f = new Integer[n][5];
        return dfs(0, 0);
    }

    private int dfs(int i, int j) {
        if (i >= n) {
            return 1;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = 0;
        for (int k = j; k < 5; ++k) {
            ans += dfs(i + 1, k);
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countVowelStrings(int n) {
        int f[n][5];
        memset(f, 0, sizeof f);
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i >= n) {
                return 1;
            }
            if (f[i][j]) {
                return f[i][j];
            }
            int ans = 0;
            for (int k = j; k < 5; ++k) {
                ans += dfs(i + 1, k);
            }
            return f[i][j] = ans;
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func countVowelStrings(n int) int {
	f := make([][5]int, n)
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i >= n {
			return 1
		}
		if f[i][j] != 0 {
			return f[i][j]
		}
		ans := 0
		for k := j; k < 5; k++ {
			ans += dfs(i+1, k)
		}
		f[i][j] = ans
		return ans
	}
	return dfs(0, 0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Trạng thái hai chiều ở Lời giải 1 có thể rút gọn thành số chuỗi kết thúc ở mỗi nguyên âm. Độ dài tiếp theo là tổng tiền tố của các số đếm trước đó.
>
> Dùng một mảng độ dài-$5$, ghi lại các tổng tiền tố rồi tính tổng các phần tử. Không gian phụ là hằng số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countVowelStrings(self, n: int) -> int:
        f = [1] * 5
        for _ in range(n - 1):
            s = 0
            for j in range(5):
                s += f[j]
                f[j] = s
        return sum(f)
```

#### Java

```java
class Solution {
    public int countVowelStrings(int n) {
        int[] f = {1, 1, 1, 1, 1};
        for (int i = 0; i < n - 1; ++i) {
            int s = 0;
            for (int j = 0; j < 5; ++j) {
                s += f[j];
                f[j] = s;
            }
        }
        return Arrays.stream(f).sum();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countVowelStrings(int n) {
        int f[5] = {1, 1, 1, 1, 1};
        for (int i = 0; i < n - 1; ++i) {
            int s = 0;
            for (int j = 0; j < 5; ++j) {
                s += f[j];
                f[j] = s;
            }
        }
        return accumulate(f, f + 5, 0);
    }
};
```

#### Go

```go
func countVowelStrings(n int) (ans int) {
	f := [5]int{1, 1, 1, 1, 1}
	for i := 0; i < n-1; i++ {
		s := 0
		for j := 0; j < 5; j++ {
			s += f[j]
			f[j] = s
		}
	}
	for _, v := range f {
		ans += v
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
