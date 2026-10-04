---
comments: true
difficulty: Medium
rating: 1855
source: Weekly Contest 356 Q3
tags:
    - Greedy
    - String
    - Enumeration
---

<!-- problem:start -->

# [2800. Shortest String That Contains Three Strings](https://leetcode.com/problems/shortest-string-that-contains-three-strings)

[中文文档](/solution/2800-2899/2800.Shortest%20String%20That%20Contains%20Three%20Strings/README.md)

## Mô tả

<!-- description:start -->

Cho ba chuỗi <code>a</code>, <code>b</code> và <code>c</code>, hãy tìm một chuỗi có<strong> độ dài nhỏ nhất</strong> và chứa cả ba chuỗi đã cho dưới dạng <strong>chuỗi con</strong>.
<p>Nếu có nhiều chuỗi như vậy, hãy trả về chuỗi<em> </em><strong>nhỏ nhất theo<em> </em>thứ tự từ điển</strong>.</p>

<p>Trả về <em>một chuỗi biểu thị đáp án của bài toán.</em></p>

<p><strong>Ghi chú</strong></p>

<ul>
	<li>Một chuỗi <code>a</code> được xem là <strong>nhỏ hơn theo thứ tự từ điển</strong> chuỗi <code>b</code> (có cùng độ dài) nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, ký tự của chuỗi <code>a</code> xuất hiện <strong>trước</strong> ký tự tương ứng của <code>b</code> trong bảng chữ cái.</li>
	<li><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = &quot;abc&quot;, b = &quot;bca&quot;, c = &quot;aaa&quot;
<strong>Đầu ra:</strong> &quot;aaabca&quot;
<strong>Giải thích:</strong>  Ta có thể thấy &quot;aaabca&quot; chứa tất cả các chuỗi đã cho: a = ans[2...4], b = ans[3..5], c = ans[0..2]. Có thể chứng minh rằng độ dài của chuỗi kết quả ít nhất là 6 và &quot;aaabca&quot; là chuỗi nhỏ nhất theo thứ tự từ điển.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = &quot;ab&quot;, b = &quot;ba&quot;, c = &quot;aba&quot;
<strong>Đầu ra:</strong> &quot;aba&quot;
<strong>Giải thích: </strong>Ta có thể thấy chuỗi &quot;aba&quot; chứa tất cả các chuỗi đã cho: a = ans[0..1], b = ans[1..2], c = ans[0..2]. Vì độ dài của c là 3, độ dài của chuỗi kết quả ít nhất là 3. Có thể chứng minh rằng &quot;aba&quot; là chuỗi nhỏ nhất theo thứ tự từ điển.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length, c.length &lt;= 100</code></li>
	<li><code>a</code>, <code>b</code> và <code>c</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chuỗi dài tối đa $100$, nên ta có thể thử cả sáu thứ tự ghép. Đáp án trước hết phải ngắn nhất, sau đó nhỏ nhất theo thứ tự từ điển, vì vậy cả việc chứa chuỗi và phần chồng lấp đều quan trọng. Nếu một chuỗi đã nằm trong chuỗi khác thì không cần nối thêm; nếu không, một siêu chuỗi tối ưu tương ứng với một hoán vị được gộp theo thứ tự. Hàm $f$ xử lý trường hợp chứa trước, sau đó tìm phần chồng lấp hậu tố–tiền tố dài nhất, rồi chọn chuỗi tốt nhất theo độ dài và thứ tự từ điển.

<!-- thinking:end -->

Ta liệt kê tất cả các hoán vị của ba chuỗi, rồi với mỗi hoán vị, gộp ba chuỗi để tìm chuỗi ngắn nhất và nhỏ nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài lớn nhất của ba chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumString(self, a: str, b: str, c: str) -> str:
        def f(s: str, t: str) -> str:
            if s in t:
                return t
            if t in s:
                return s
            m, n = len(s), len(t)
            for i in range(min(m, n), 0, -1):
                if s[-i:] == t[:i]:
                    return s + t[i:]
            return s + t

        ans = ""
        for a, b, c in permutations((a, b, c)):
            s = f(f(a, b), c)
            if ans == "" or len(s) < len(ans) or (len(s) == len(ans) and s < ans):
                ans = s
        return ans
```

#### Java

```java
class Solution {
    public String minimumString(String a, String b, String c) {
        String[] s = {a, b, c};
        int[][] perm = {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}};
        String ans = "";
        for (var p : perm) {
            int i = p[0], j = p[1], k = p[2];
            String t = f(f(s[i], s[j]), s[k]);
            if ("".equals(ans) || t.length() < ans.length()
                || (t.length() == ans.length() && t.compareTo(ans) < 0)) {
                ans = t;
            }
        }
        return ans;
    }

    private String f(String s, String t) {
        if (s.contains(t)) {
            return s;
        }
        if (t.contains(s)) {
            return t;
        }
        int m = s.length(), n = t.length();
        for (int i = Math.min(m, n); i > 0; --i) {
            if (s.substring(m - i).equals(t.substring(0, i))) {
                return s + t.substring(i);
            }
        }
        return s + t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minimumString(string a, string b, string c) {
        vector<string> s = {a, b, c};
        vector<vector<int>> perm = {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}};
        string ans = "";
        for (auto& p : perm) {
            int i = p[0], j = p[1], k = p[2];
            string t = f(f(s[i], s[j]), s[k]);
            if (ans == "" || t.size() < ans.size() || (t.size() == ans.size() && t < ans)) {
                ans = t;
            }
        }
        return ans;
    }

    string f(string s, string t) {
        if (s.find(t) != string::npos) {
            return s;
        }
        if (t.find(s) != string::npos) {
            return t;
        }
        int m = s.size(), n = t.size();
        for (int i = min(m, n); i; --i) {
            if (s.substr(m - i) == t.substr(0, i)) {
                return s + t.substr(i);
            }
        }
        return s + t;
    };
};
```

#### Go

```go
func minimumString(a string, b string, c string) string {
	f := func(s, t string) string {
		if strings.Contains(s, t) {
			return s
		}
		if strings.Contains(t, s) {
			return t
		}
		m, n := len(s), len(t)
		for i := min(m, n); i > 0; i-- {
			if s[m-i:] == t[:i] {
				return s + t[i:]
			}
		}
		return s + t
	}
	s := [3]string{a, b, c}
	ans := ""
	for _, p := range [][]int{{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 0, 1}, {2, 1, 0}} {
		i, j, k := p[0], p[1], p[2]
		t := f(f(s[i], s[j]), s[k])
		if ans == "" || len(t) < len(ans) || (len(t) == len(ans) && t < ans) {
			ans = t
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumString(a: string, b: string, c: string): string {
    const f = (s: string, t: string): string => {
        if (s.includes(t)) {
            return s;
        }
        if (t.includes(s)) {
            return t;
        }
        const m = s.length;
        const n = t.length;
        for (let i = Math.min(m, n); i > 0; --i) {
            if (s.slice(-i) === t.slice(0, i)) {
                return s + t.slice(i);
            }
        }
        return s + t;
    };
    const s: string[] = [a, b, c];
    const perm: number[][] = [
        [0, 1, 2],
        [0, 2, 1],
        [1, 0, 2],
        [1, 2, 0],
        [2, 0, 1],
        [2, 1, 0],
    ];
    let ans = '';
    for (const [i, j, k] of perm) {
        const t = f(f(s[i], s[j]), s[k]);
        if (ans === '' || t.length < ans.length || (t.length === ans.length && t < ans)) {
            ans = t;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    fn f(s1: String, s2: String) -> String {
        if s1.contains(&s2) {
            return s1;
        }
        if s2.contains(&s1) {
            return s2;
        }
        for i in 0..s1.len() {
            let s = &s1[i..];
            if s2.starts_with(s) {
                let n = s.len();
                return s1 + &s2[n..];
            }
        }
        s1 + s2.as_str()
    }

    pub fn minimum_string(a: String, b: String, c: String) -> String {
        let s = [&a, &b, &c];
        let perm = [
            [0, 1, 2],
            [0, 2, 1],
            [1, 0, 2],
            [1, 2, 0],
            [2, 0, 1],
            [2, 1, 0],
        ];
        let mut ans = String::new();
        for [i, j, k] in perm.iter() {
            let r = Self::f(Self::f(s[*i].clone(), s[*j].clone()), s[*k].clone());
            if ans == "" || r.len() < ans.len() || (r.len() == ans.len() && r < ans) {
                ans = r;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Liệt kê + KMP

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 duyệt các độ dài phần chồng lấp với độ phức tạp $O(n^2)$ cho mỗi lần gộp. Hàm tiền tố của KMP trên $t\#s$ cho biết tiền tố dài nhất của $t$ cũng là hậu tố của $s$, nên mỗi lần gộp chỉ còn tuyến tính trong khi việc liệt kê các hoán vị vẫn giữ nguyên.

<!-- thinking:end -->

Ta có thể sử dụng thuật toán KMP để tối ưu quá trình gộp chuỗi.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là tổng độ dài của ba chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumString(self, a: str, b: str, c: str) -> str:
        def f(s: str, t: str) -> str:
            if s in t:
                return t
            if t in s:
                return s
            p = t + "#" + s + "$"
            n = len(p)
            next = [0] * n
            next[0] = -1
            i, j = 2, 0
            while i < n:
                if p[i - 1] == p[j]:
                    j += 1
                    next[i] = j
                    i += 1
                elif j:
                    j = next[j]
                else:
                    next[i] = 0
                    i += 1
            return s + t[next[-1] :]

        ans = ""
        for a, b, c in permutations((a, b, c)):
            s = f(f(a, b), c)
            if ans == "" or len(s) < len(ans) or (len(s) == len(ans) and s < ans):
                ans = s
        return ans
```

#### Java

```java
class Solution {
    public String minimumString(String a, String b, String c) {
        String[] s = {a, b, c};
        int[][] perm = {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}};
        String ans = "";
        for (var p : perm) {
            int i = p[0], j = p[1], k = p[2];
            String t = f(f(s[i], s[j]), s[k]);
            if ("".equals(ans) || t.length() < ans.length()
                || (t.length() == ans.length() && t.compareTo(ans) < 0)) {
                ans = t;
            }
        }
        return ans;
    }

    private String f(String s, String t) {
        if (s.contains(t)) {
            return s;
        }
        if (t.contains(s)) {
            return t;
        }
        char[] p = (t + "#" + s + "$").toCharArray();
        int n = p.length;
        int[] next = new int[n];
        next[0] = -1;
        for (int i = 2, j = 0; i < n;) {
            if (p[i - 1] == p[j]) {
                next[i++] = ++j;
            } else if (j > 0) {
                j = next[j];
            } else {
                next[i++] = 0;
            }
        }
        return s + t.substring(next[n - 1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minimumString(string a, string b, string c) {
        vector<string> s = {a, b, c};
        vector<vector<int>> perm = {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}};
        string ans = "";
        for (auto& p : perm) {
            int i = p[0], j = p[1], k = p[2];
            string t = f(f(s[i], s[j]), s[k]);
            if (ans == "" || t.size() < ans.size() || (t.size() == ans.size() && t < ans)) {
                ans = t;
            }
        }
        return ans;
    }

    string f(string s, string t) {
        if (s.find(t) != string::npos) {
            return s;
        }
        if (t.find(s) != string::npos) {
            return t;
        }
        string p = t + "#" + s + "$";
        int n = p.size();
        int next[n];
        next[0] = -1;
        next[1] = 0;
        for (int i = 2, j = 0; i < n;) {
            if (p[i - 1] == p[j]) {
                next[i++] = ++j;
            } else if (j > 0) {
                j = next[j];
            } else {
                next[i++] = 0;
            }
        }
        return s + t.substr(next[n - 1]);
    };
};
```

#### Go

```go
func minimumString(a string, b string, c string) string {
	f := func(s, t string) string {
		if strings.Contains(s, t) {
			return s
		}
		if strings.Contains(t, s) {
			return t
		}
		p := t + "#" + s + "$"
		n := len(p)
		next := make([]int, n)
		next[0] = -1
		for i, j := 2, 0; i < n; {
			if p[i-1] == p[j] {
				j++
				next[i] = j
				i++
			} else if j > 0 {
				j = next[j]
			} else {
				next[i] = 0
				i++
			}
		}
		return s + t[next[n-1]:]
	}
	s := [3]string{a, b, c}
	ans := ""
	for _, p := range [][]int{{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 0, 1}, {2, 1, 0}} {
		i, j, k := p[0], p[1], p[2]
		t := f(f(s[i], s[j]), s[k])
		if ans == "" || len(t) < len(ans) || (len(t) == len(ans) && t < ans) {
			ans = t
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumString(a: string, b: string, c: string): string {
    const f = (s: string, t: string): string => {
        if (s.includes(t)) {
            return s;
        }
        if (t.includes(s)) {
            return t;
        }
        const p = t + '#' + s + '$';
        const n = p.length;
        const next: number[] = Array(n).fill(0);
        next[0] = -1;
        for (let i = 2, j = 0; i < n;) {
            if (p[i - 1] === p[j]) {
                next[i++] = ++j;
            } else if (j) {
                j = next[j];
            } else {
                next[i++] = 0;
            }
        }
        return s + t.slice(next[n - 1]);
    };
    const s: string[] = [a, b, c];
    const perm: number[][] = [
        [0, 1, 2],
        [0, 2, 1],
        [1, 0, 2],
        [1, 2, 0],
        [2, 0, 1],
        [2, 1, 0],
    ];
    let ans = '';
    for (const [i, j, k] of perm) {
        const t = f(f(s[i], s[j]), s[k]);
        if (ans === '' || t.length < ans.length || (t.length === ans.length && t < ans)) {
            ans = t;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
