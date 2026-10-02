---
comments: true
difficulty: Hard
rating: 1976
source: Weekly Contest 141 Q4
tags:
    - String
    - Dynamic Programming
    - Longest Common Subsequence
---

<!-- problem:start -->

# [1092. Shortest Common Supersequence](https://leetcode.com/problems/shortest-common-supersequence)

[中文文档](/solution/1000-1099/1092.Shortest%20Common%20Supersequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>str1</code> và <code>str2</code>, hãy trả về <em>chuỗi ngắn nhất chứa cả </em><code>str1</code><em> và </em><code>str2</code><em> dưới dạng các <strong>dãy con</strong></em>. Nếu có nhiều chuỗi thỏa mãn, trả về <strong>bất kỳ</strong> chuỗi nào trong số đó.</p>

<p>Chuỗi <code>s</code> là <strong>dãy con</strong> của chuỗi <code>t</code> nếu xóa một số ký tự trong <code>t</code> (có thể là <code>0</code> ký tự) sẽ thu được chuỗi <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> str1 = &quot;abac&quot;, str2 = &quot;cab&quot;
<strong>Đầu ra:</strong> &quot;cabac&quot;
<strong>Giải thích:</strong> 
str1 = &quot;abac&quot; là dãy con của &quot;cabac&quot; vì ta có thể xóa chữ &quot;c&quot; đầu tiên.
str2 = &quot;cab&quot; là dãy con của &quot;cabac&quot; vì ta có thể xóa &quot;ac&quot; ở cuối.
Đáp án đưa ra là chuỗi ngắn nhất thỏa mãn các điều kiện này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> str1 = &quot;aaaaaaaa&quot;, str2 = &quot;aaaaaaaa&quot;
<strong>Đầu ra:</strong> &quot;aaaaaaaa&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= str1.length, str2.length &lt;= 1000</code></li>
	<li><code>str1</code> và <code>str2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Độ dài SCS bằng $|s_1|+|s_2|-\mathrm{LCS}$. Với $m,n\le 1000$, ta lập bảng LCS rồi lần theo các trạng thái ngược lại, thêm những ký tự không chung.
>
> $f[i][j]$ là LCS của hai prefix. Bắt đầu từ $(m,n)$, nếu đi từ ô phía trên hoặc bên trái thì thêm ký tự bị bỏ qua; nếu hai ký tự bằng nhau thì thêm ký tự chung và đi chéo lên.
>
> Quá trình lần ngược thu được SCS theo thứ tự đảo ngược; khi chỉ số một chuỗi về 0, ta thêm các ký tự còn lại của chuỗi kia.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestCommonSupersequence(self, str1: str, str2: str) -> str:
        m, n = len(str1), len(str2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if str1[i - 1] == str2[j - 1]:
                    f[i][j] = f[i - 1][j - 1] + 1
                else:
                    f[i][j] = max(f[i - 1][j], f[i][j - 1])
        ans = []
        i, j = m, n
        while i or j:
            if i == 0:
                j -= 1
                ans.append(str2[j])
            elif j == 0:
                i -= 1
                ans.append(str1[i])
            else:
                if f[i][j] == f[i - 1][j]:
                    i -= 1
                    ans.append(str1[i])
                elif f[i][j] == f[i][j - 1]:
                    j -= 1
                    ans.append(str2[j])
                else:
                    i, j = i - 1, j - 1
                    ans.append(str1[i])
        return ''.join(ans[::-1])
```

#### Java

```java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int m = str1.length(), n = str2.length();
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                } else {
                    f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
                }
            }
        }
        int i = m, j = n;
        StringBuilder ans = new StringBuilder();
        while (i > 0 || j > 0) {
            if (i == 0) {
                ans.append(str2.charAt(--j));
            } else if (j == 0) {
                ans.append(str1.charAt(--i));
            } else {
                if (f[i][j] == f[i - 1][j]) {
                    ans.append(str1.charAt(--i));
                } else if (f[i][j] == f[i][j - 1]) {
                    ans.append(str2.charAt(--j));
                } else {
                    ans.append(str1.charAt(--i));
                    --j;
                }
            }
        }
        return ans.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int m = str1.size(), n = str2.size();
        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (str1[i - 1] == str2[j - 1])
                    f[i][j] = f[i - 1][j - 1] + 1;
                else
                    f[i][j] = max(f[i - 1][j], f[i][j - 1]);
            }
        }
        int i = m, j = n;
        string ans;
        while (i || j) {
            if (i == 0)
                ans += str2[--j];
            else if (j == 0)
                ans += str1[--i];
            else {
                if (f[i][j] == f[i - 1][j])
                    ans += str1[--i];
                else if (f[i][j] == f[i][j - 1])
                    ans += str2[--j];
                else
                    ans += str1[--i], --j;
            }
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func shortestCommonSupersequence(str1 string, str2 string) string {
	m, n := len(str1), len(str2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if str1[i-1] == str2[j-1] {
				f[i][j] = f[i-1][j-1] + 1
			} else {
				f[i][j] = max(f[i-1][j], f[i][j-1])
			}
		}
	}
	ans := []byte{}
	i, j := m, n
	for i > 0 || j > 0 {
		if i == 0 {
			j--
			ans = append(ans, str2[j])
		} else if j == 0 {
			i--
			ans = append(ans, str1[i])
		} else {
			if f[i][j] == f[i-1][j] {
				i--
				ans = append(ans, str1[i])
			} else if f[i][j] == f[i][j-1] {
				j--
				ans = append(ans, str2[j])
			} else {
				i, j = i-1, j-1
				ans = append(ans, str1[i])
			}
		}
	}
	for i, j = 0, len(ans)-1; i < j; i, j = i+1, j-1 {
		ans[i], ans[j] = ans[j], ans[i]
	}
	return string(ans)
}
```

#### TypeScript

```ts
function shortestCommonSupersequence(str1: string, str2: string): string {
    const m = str1.length;
    const n = str2.length;
    const f = new Array(m + 1).fill(0).map(() => new Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (str1[i - 1] == str2[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
            } else {
                f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
            }
        }
    }
    let ans: string[] = [];
    let i = m;
    let j = n;
    while (i > 0 || j > 0) {
        if (i === 0) {
            ans.push(str2[--j]);
        } else if (j === 0) {
            ans.push(str1[--i]);
        } else {
            if (f[i][j] === f[i - 1][j]) {
                ans.push(str1[--i]);
            } else if (f[i][j] === f[i][j - 1]) {
                ans.push(str2[--j]);
            } else {
                ans.push(str1[--i]);
                --j;
            }
        }
    }
    return ans.reverse().join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
