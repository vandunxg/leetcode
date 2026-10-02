---
comments: true
difficulty: Medium
tags:
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings)

[中文文档](/solution/0600-0699/0647.Palindromic%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về <em>số lượng <strong>chuỗi con đối xứng</strong> trong đó</em>.</p>

<p>Một chuỗi là <strong>palindrome</strong> nếu đọc xuôi hay đọc ngược đều giống nhau.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp bên trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abc&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có ba chuỗi đối xứng: &quot;a&quot;, &quot;b&quot;, &quot;c&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaa&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có sáu chuỗi đối xứng: &quot;a&quot;, &quot;a&quot;, &quot;a&quot;, &quot;aa&quot;, &quot;aa&quot;, &quot;aaa&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mở rộng từ tâm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm các chuỗi con đối xứng. Có thể dùng bảng DP kích thước $n^2$, nhưng không cần thiết nếu chỉ cần số lượng.
>
> Mở rộng từ $2n-1$ tâm (cho cả độ dài lẻ và chẵn), mỗi lần mở rộng thành công thì tăng kết quả lên một.

<!-- thinking:end -->

Ta có thể lần lượt xét vị trí tâm của mỗi palindrome rồi mở rộng ra hai phía để đếm các chuỗi con đối xứng. Với chuỗi độ dài $n$, có $2n-1$ vị trí tâm có thể có, bao gồm tâm của palindrome độ dài lẻ và chẵn. Với mỗi tâm, ta tiếp tục mở rộng cho đến khi điều kiện palindrome không còn đúng, đồng thời đếm số chuỗi con đối xứng.

Độ phức tạp thời gian là $O(n^2)$, với $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        ans, n = 0, len(s)
        for k in range(n * 2 - 1):
            i, j = k // 2, (k + 1) // 2
            while ~i and j < n and s[i] == s[j]:
                ans += 1
                i, j = i - 1, j + 1
        return ans
```

#### Java

```java
class Solution {
    public int countSubstrings(String s) {
        int ans = 0;
        int n = s.length();
        for (int k = 0; k < n * 2 - 1; ++k) {
            int i = k / 2, j = (k + 1) / 2;
            while (i >= 0 && j < n && s.charAt(i) == s.charAt(j)) {
                ++ans;
                --i;
                ++j;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSubstrings(string s) {
        int ans = 0;
        int n = s.size();
        for (int k = 0; k < n * 2 - 1; ++k) {
            int i = k / 2, j = (k + 1) / 2;
            while (~i && j < n && s[i] == s[j]) {
                ++ans;
                --i;
                ++j;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSubstrings(s string) int {
	ans, n := 0, len(s)
	for k := 0; k < n*2-1; k++ {
		i, j := k/2, (k+1)/2
		for i >= 0 && j < n && s[i] == s[j] {
			ans++
			i, j = i-1, j+1
		}
	}
	return ans
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var countSubstrings = function (s) {
    let ans = 0;
    const n = s.length;
    for (let k = 0; k < n * 2 - 1; ++k) {
        let i = k >> 1;
        let j = (k + 1) >> 1;
        while (~i && j < n && s[i] == s[j]) {
            ++ans;
            --i;
            ++j;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thuật toán Manacher

<!-- thinking:start -->

> **Tư duy**
>
> Trong trường hợp xấu nhất, mở rộng từ tâm có độ phức tạp $O(n^2)$. Sau khi chèn ký tự phân tách, Manacher tính độ dài mỗi nhánh $p[i]$ trong thời gian tuyến tính; tâm đó đóng góp $\lfloor p[i]/2\rfloor$ palindrome.

<!-- thinking:end -->

Trong thuật toán Manacher, $p[i] - 1$ biểu diễn độ dài palindrome lớn nhất có tâm tại vị trí $i$, còn số chuỗi con đối xứng có tâm tại vị trí $i$ là $\left \lceil \frac{p[i]-1}{2} \right \rceil$.

Độ phức tạp thời gian và không gian lần lượt là $O(n)$ và $O(n)$, với $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        t = '^#' + '#'.join(s) + '#$'
        n = len(t)
        p = [0 for _ in range(n)]
        pos, maxRight = 0, 0
        ans = 0
        for i in range(1, n - 1):
            p[i] = min(maxRight - i, p[2 * pos - i]) if maxRight > i else 1
            while t[i - p[i]] == t[i + p[i]]:
                p[i] += 1
            if i + p[i] > maxRight:
                maxRight = i + p[i]
                pos = i
            ans += p[i] // 2
        return ans
```

#### Java

```java
class Solution {
    public int countSubstrings(String s) {
        StringBuilder sb = new StringBuilder("^#");
        for (char ch : s.toCharArray()) {
            sb.append(ch).append('#');
        }
        String t = sb.append('$').toString();
        int n = t.length();
        int[] p = new int[n];
        int pos = 0, maxRight = 0;
        int ans = 0;
        for (int i = 1; i < n - 1; i++) {
            p[i] = maxRight > i ? Math.min(maxRight - i, p[2 * pos - i]) : 1;
            while (t.charAt(i - p[i]) == t.charAt(i + p[i])) {
                p[i]++;
            }
            if (i + p[i] > maxRight) {
                maxRight = i + p[i];
                pos = i;
            }
            ans += p[i] / 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSubstrings(string s) {
        string t = "^#";
        for (char c : s) {
            t += c;
            t += '#';
        }
        t += "$";

        int n = t.size();
        vector<int> p(n, 0);
        int pos = 0, maxRight = 0;
        int ans = 0;

        for (int i = 1; i < n - 1; ++i) {
            if (maxRight > i) {
                p[i] = min(maxRight - i, p[2 * pos - i]);
            } else {
                p[i] = 1;
            }

            while (t[i - p[i]] == t[i + p[i]]) {
                ++p[i];
            }

            if (i + p[i] > maxRight) {
                maxRight = i + p[i];
                pos = i;
            }

            ans += p[i] / 2;
        }

        return ans;
    }
};
```

#### Go

```go
func countSubstrings(s string) int {
	t := "^#"
	for _, c := range s {
		t += string(c)
		t += "#"
	}
	t += "$"

	n := len(t)
	p := make([]int, n)
	pos, maxRight := 0, 0
	ans := 0

	for i := 1; i < n-1; i++ {
		if maxRight > i {
			mirror := 2*pos - i
			if p[mirror] < maxRight-i {
				p[i] = p[mirror]
			} else {
				p[i] = maxRight - i
			}
		} else {
			p[i] = 1
		}

		for t[i-p[i]] == t[i+p[i]] {
			p[i]++
		}

		if i+p[i] > maxRight {
			maxRight = i + p[i]
			pos = i
		}

		ans += p[i] / 2
	}

	return ans
}
```

#### TypeScript

```ts
function countSubstrings(s: string): number {
    let t = '^#';
    for (const c of s) {
        t += c + '#';
    }
    t += '$';

    const n = t.length;
    const p: number[] = new Array(n).fill(0);
    let pos = 0,
        maxRight = 0;
    let ans = 0;

    for (let i = 1; i < n - 1; i++) {
        if (maxRight > i) {
            p[i] = Math.min(maxRight - i, p[2 * pos - i]);
        } else {
            p[i] = 1;
        }

        while (t[i - p[i]] === t[i + p[i]]) {
            p[i]++;
        }

        if (i + p[i] > maxRight) {
            maxRight = i + p[i];
            pos = i;
        }

        ans += Math.floor(p[i] / 2);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
