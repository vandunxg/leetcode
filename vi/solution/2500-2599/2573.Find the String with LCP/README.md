---
comments: true
difficulty: Hard
rating: 2681
source: Weekly Contest 333 Q4
tags:
    - Greedy
    - Union Find
    - Array
    - String
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [2573. Find the String with LCP](https://leetcode.com/problems/find-the-string-with-lcp)

[中文文档](/solution/2500-2599/2573.Find%20the%20String%20with%20LCP/README.md)

## Mô tả

<!-- description:start -->

<p>Ta định nghĩa ma trận <code>lcp</code> của một chuỗi <code>word</code> gồm <code>n</code> chữ cái tiếng Anh viết thường, được đánh chỉ số từ <strong>0</strong>, là một lưới <code>n x n</code> sao cho:</p>

<ul>
	<li><code>lcp[i][j]</code> bằng độ dài của <strong>tiền tố chung dài nhất</strong> giữa hai chuỗi con <code>word[i,n-1]</code> và <code>word[j,n-1]</code>.</li>
</ul>

<p>Cho một ma trận <code>lcp</code> kích thước <code>n x n</code>, hãy trả về chuỗi <code>word</code> nhỏ nhất theo thứ tự từ điển tương ứng với <code>lcp</code>. Nếu không tồn tại chuỗi như vậy, hãy trả về một chuỗi rỗng.</p>

<p>Một chuỗi <code>a</code> nhỏ hơn một chuỗi <code>b</code> theo thứ tự từ điển (với cùng độ dài) nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, chuỗi <code>a</code> có ký tự xuất hiện sớm hơn trong bảng chữ cái so với ký tự tương ứng trong <code>b</code>. Ví dụ, <code>&quot;aabd&quot;</code> nhỏ hơn <code>&quot;aaca&quot;</code> theo thứ tự từ điển vì vị trí khác nhau đầu tiên là ký tự thứ ba, và <code>&#39;b&#39;</code> đứng trước <code>&#39;c&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> lcp = [[4,0,2,0],[0,3,0,1],[2,0,2,0],[0,1,0,1]]
<strong>Đầu ra:</strong> &quot;abab&quot;
<strong>Giải thích:</strong> lcp tương ứng với mọi chuỗi gồm 4 chữ cái và có hai chữ cái xen kẽ nhau. Trong số đó, chuỗi nhỏ nhất theo thứ tự từ điển là &quot;abab&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> lcp = [[4,3,2,1],[3,3,2,1],[2,2,2,1],[1,1,1,1]]
<strong>Đầu ra:</strong> &quot;aaaa&quot;
<strong>Giải thích:</strong> lcp tương ứng với mọi chuỗi gồm 4 chữ cái và chỉ có một chữ cái phân biệt. Trong số đó, chuỗi nhỏ nhất theo thứ tự từ điển là &quot;aaaa&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> lcp = [[4,3,2,1],[3,3,2,1],[2,2,2,1],[1,1,1,3]]
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> lcp[3][3] không thể bằng 3 vì word[3,...,3] chỉ gồm một chữ cái; do đó không tồn tại đáp án.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n ==&nbsp;</code><code>lcp.length == </code><code>lcp[i].length</code>&nbsp;<code>&lt;= 1000</code></li>
	<li><code><font face="monospace">0 &lt;= lcp[i][j] &lt;= n</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Dựng chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Dựng lại chuỗi nhỏ nhất theo thứ tự từ điển từ ma trận $lcp$. $lcp[i][j]>0$ khi và chỉ khi $s[i]=s[j]$, vì vậy các lớp chỉ số bằng nhau cần được gán các chữ cái từ `'a'` trở đi.
>
> Mỗi khi gặp một chỉ số chưa được điền, ta gán ký tự hiện tại cho mọi $j$ thỏa mãn $lcp[i][j]\ne 0$. Nếu vẫn còn vị trí trống sau khi dùng đến `'z'` thì bài toán không có nghiệm. Cuối cùng, ta kiểm tra công thức truy hồi của $lcp$ từ cuối về đầu: các ký tự bằng nhau phải có giá trị bằng $lcp$ của các hậu tố cộng một; các ký tự khác nhau phải lưu giá trị $0$.

<!-- thinking:end -->

Vì chuỗi được dựng phải nhỏ nhất theo thứ tự từ điển, ta có thể bắt đầu bằng cách điền ký tự `'a'` vào chuỗi $s$.

Nếu vị trí hiện tại $i$ chưa được điền ký tự, ta điền ký tự `'a'` vào vị trí $i$. Sau đó, ta duyệt qua mọi vị trí $j > i$. Nếu $lcp[i][j] > 0$, vị trí $j$ cũng phải được điền ký tự `'a'`. Tiếp theo, ta tăng mã ASCII của ký tự `'a'` lên một đơn vị và tiếp tục điền các vị trí còn trống.

Sau khi điền xong, nếu chuỗi vẫn còn vị trí trống, điều đó có nghĩa là không thể dựng được chuỗi tương ứng, nên ta trả về một chuỗi rỗng.

Tiếp theo, ta duyệt các vị trí $i$ và $j$ trong chuỗi từ lớn đến nhỏ, rồi kiểm tra xem $s[i]$ và $s[j]$ có bằng nhau hay không:

- Nếu $s[i] = s[j]$, ta cần kiểm tra xem $i$ và $j$ có phải là vị trí cuối cùng của chuỗi hay không. Nếu đúng, thì $lcp[i][j]$ phải bằng $1$; nếu không, $lcp[i][j]$ phải bằng $0$. Nếu các điều kiện trên không thỏa mãn, điều đó có nghĩa là không thể dựng được chuỗi tương ứng, nên ta trả về một chuỗi rỗng. Nếu $i$ và $j$ không phải là vị trí cuối cùng của chuỗi, thì $lcp[i][j]$ phải bằng $lcp[i + 1][j + 1] + 1$; nếu không, điều đó có nghĩa là không thể dựng được chuỗi tương ứng, nên ta trả về một chuỗi rỗng.
- Ngược lại, nếu $lcp[i][j] > 0$, điều đó có nghĩa là không thể dựng được chuỗi tương ứng, nên ta trả về một chuỗi rỗng.

Nếu mọi vị trí trong chuỗi đều thỏa mãn các điều kiện trên, ta có thể dựng chuỗi tương ứng và trả về chuỗi đó.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheString(self, lcp: List[List[int]]) -> str:
        n = len(lcp)
        s = [""] * n
        i = 0
        for c in ascii_lowercase:
            while i < n and s[i]:
                i += 1
            if i == n:
                break
            for j in range(i, n):
                if lcp[i][j]:
                    s[j] = c
        if "" in s:
            return ""
        for i in range(n - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                if s[i] == s[j]:
                    if i == n - 1 or j == n - 1:
                        if lcp[i][j] != 1:
                            return ""
                    elif lcp[i][j] != lcp[i + 1][j + 1] + 1:
                        return ""
                elif lcp[i][j]:
                    return ""
        return "".join(s)
```

#### Java

```java
class Solution {
    public String findTheString(int[][] lcp) {
        int n = lcp.length;
        char[] s = new char[n];
        int i = 0;
        for (char c = 'a'; c <= 'z'; ++c) {
            while (i < n && s[i] != '\0') {
                ++i;
            }
            if (i == n) {
                break;
            }
            for (int j = i; j < n; ++j) {
                if (lcp[i][j] > 0) {
                    s[j] = c;
                }
            }
        }
        for (i = 0; i < n; ++i) {
            if (s[i] == '\0') {
                return "";
            }
        }
        for (i = n - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (s[i] == s[j]) {
                    if (i == n - 1 || j == n - 1) {
                        if (lcp[i][j] != 1) {
                            return "";
                        }
                    } else if (lcp[i][j] != lcp[i + 1][j + 1] + 1) {
                        return "";
                    }
                } else if (lcp[i][j] > 0) {
                    return "";
                }
            }
        }
        return String.valueOf(s);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findTheString(vector<vector<int>>& lcp) {
        int i = 0, n = lcp.size();
        string s(n, '\0');
        for (char c = 'a'; c <= 'z'; ++c) {
            while (i < n && s[i]) {
                ++i;
            }
            if (i == n) {
                break;
            }
            for (int j = i; j < n; ++j) {
                if (lcp[i][j]) {
                    s[j] = c;
                }
            }
        }
        if (s.find('\0') != -1) {
            return "";
        }
        for (i = n - 1; ~i; --i) {
            for (int j = n - 1; ~j; --j) {
                if (s[i] == s[j]) {
                    if (i == n - 1 || j == n - 1) {
                        if (lcp[i][j] != 1) {
                            return "";
                        }
                    } else if (lcp[i][j] != lcp[i + 1][j + 1] + 1) {
                        return "";
                    }
                } else if (lcp[i][j]) {
                    return "";
                }
            }
        }
        return s;
    }
};
```

#### Go

```go
func findTheString(lcp [][]int) string {
	i, n := 0, len(lcp)
	s := make([]byte, n)
	for c := 'a'; c <= 'z'; c++ {
		for i < n && s[i] != 0 {
			i++
		}
		if i == n {
			break
		}
		for j := i; j < n; j++ {
			if lcp[i][j] > 0 {
				s[j] = byte(c)
			}
		}
	}
	if bytes.IndexByte(s, 0) >= 0 {
		return ""
	}
	for i := n - 1; i >= 0; i-- {
		for j := n - 1; j >= 0; j-- {
			if s[i] == s[j] {
				if i == n-1 || j == n-1 {
					if lcp[i][j] != 1 {
						return ""
					}
				} else if lcp[i][j] != lcp[i+1][j+1]+1 {
					return ""
				}
			} else if lcp[i][j] > 0 {
				return ""
			}
		}
	}
	return string(s)
}
```

#### TypeScript

```ts
function findTheString(lcp: number[][]): string {
    let i: number = 0;
    const n: number = lcp.length;
    let s: string = '\0'.repeat(n);
    for (let ascii = 97; ascii < 123; ++ascii) {
        const c: string = String.fromCharCode(ascii);
        while (i < n && s[i] !== '\0') {
            ++i;
        }
        if (i === n) {
            break;
        }
        for (let j = i; j < n; ++j) {
            if (lcp[i][j]) {
                s = s.substring(0, j) + c + s.substring(j + 1);
            }
        }
    }
    if (s.indexOf('\0') !== -1) {
        return '';
    }
    for (i = n - 1; ~i; --i) {
        for (let j = n - 1; ~j; --j) {
            if (s[i] === s[j]) {
                if (i === n - 1 || j === n - 1) {
                    if (lcp[i][j] !== 1) {
                        return '';
                    }
                } else if (lcp[i][j] !== lcp[i + 1][j + 1] + 1) {
                    return '';
                }
            } else if (lcp[i][j]) {
                return '';
            }
        }
    }
    return s;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_string(lcp: Vec<Vec<i32>>) -> String {
        let n = lcp.len();
        let mut s = vec!['\0'; n];
        let mut i = 0;

        for c in b'a'..=b'z' {
            while i < n && s[i] != '\0' {
                i += 1;
            }
            if i == n {
                break;
            }
            for j in i..n {
                if lcp[i][j] > 0 {
                    s[j] = c as char;
                }
            }
        }

        for i in 0..n {
            if s[i] == '\0' {
                return "".to_string();
            }
        }

        for i in (0..n).rev() {
            for j in (0..n).rev() {
                if s[i] == s[j] {
                    if i == n - 1 || j == n - 1 {
                        if lcp[i][j] != 1 {
                            return "".to_string();
                        }
                    } else if lcp[i][j] != lcp[i + 1][j + 1] + 1 {
                        return "".to_string();
                    }
                } else if lcp[i][j] > 0 {
                    return "".to_string();
                }
            }
        }

        s.into_iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
