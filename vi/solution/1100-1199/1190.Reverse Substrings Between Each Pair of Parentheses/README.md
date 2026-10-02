---
comments: true
difficulty: Medium
rating: 1485
source: Weekly Contest 154 Q2
tags:
    - Stack
    - String
    - Parentheses
---

<!-- problem:start -->

# [1190. Reverse Substrings Between Each Pair of Parentheses](https://leetcode.com/problems/reverse-substrings-between-each-pair-of-parentheses)

[中文文档](/solution/1100-1199/1190.Reverse%20Substrings%20Between%20Each%20Pair%20of%20Parentheses/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và dấu ngoặc.</p>

<p>Đảo ngược chuỗi nằm trong từng cặp ngoặc khớp nhau, bắt đầu từ cặp trong cùng.</p>

<p>Kết quả <strong>không được</strong> chứa dấu ngoặc.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(abcd)&quot;
<strong>Đầu ra:</strong> &quot;dcba&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(u(love)i)&quot;
<strong>Đầu ra:</strong> &quot;iloveu&quot;
<strong>Giải thích:</strong> Chuỗi con &quot;love&quot; được đảo ngược trước, sau đó đảo ngược toàn bộ chuỗi.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(ed(et(oc))el)&quot;
<strong>Đầu ra:</strong> &quot;leetcode&quot;
<strong>Giải thích:</strong> Trước tiên, ta đảo ngược chuỗi con &quot;oc&quot;, tiếp theo là &quot;etco&quot;, cuối cùng đảo ngược toàn bộ chuỗi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường và dấu ngoặc đơn.</li>
	<li>Đảm bảo mọi dấu ngoặc đều cân bằng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp ngoặc được xử lý từ trong ra ngoài. Khi gặp `)`, stack lấy các ký tự ra cho đến dấu `'('` khớp rồi đẩy chúng trở lại theo thứ tự đảo ngược. Mỗi lớp ngoặc có thể di chuyển $O(n)$ ký tự, nên trường hợp xấu nhất là $O(n^2)$; mức này chấp nhận được với $n\le 2000$.

<!-- thinking:end -->

Ta có thể dùng trực tiếp stack để mô phỏng quá trình đảo ngược.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseParentheses(self, s: str) -> str:
        stk = []
        for c in s:
            if c == ")":
                t = []
                while stk[-1] != "(":
                    t.append(stk.pop())
                stk.pop()
                stk.extend(t)
            else:
                stk.append(c)
        return "".join(stk)
```

#### Java

```java
class Solution {
    public String reverseParentheses(String s) {
        StringBuilder stk = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c == ')') {
                StringBuilder t = new StringBuilder();
                while (stk.charAt(stk.length() - 1) != '(') {
                    t.append(stk.charAt(stk.length() - 1));
                    stk.deleteCharAt(stk.length() - 1);
                }
                stk.deleteCharAt(stk.length() - 1);
                stk.append(t);
            } else {
                stk.append(c);
            }
        }
        return stk.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reverseParentheses(string s) {
        string stk;
        for (char& c : s) {
            if (c == ')') {
                string t;
                while (stk.back() != '(') {
                    t.push_back(stk.back());
                    stk.pop_back();
                }
                stk.pop_back();
                stk += t;
            } else {
                stk.push_back(c);
            }
        }
        return stk;
    }
};
```

#### Go

```go
func reverseParentheses(s string) string {
	stk := []byte{}
	for i := range s {
		if s[i] == ')' {
			t := []byte{}
			for stk[len(stk)-1] != '(' {
				t = append(t, stk[len(stk)-1])
				stk = stk[:len(stk)-1]
			}
			stk = stk[:len(stk)-1]
			stk = append(stk, t...)
		} else {
			stk = append(stk, s[i])
		}
	}
	return string(stk)
}
```

#### TypeScript

```ts
function reverseParentheses(s: string): string {
    const stk: string[] = [];
    for (const c of s) {
        if (c === ')') {
            const t: string[] = [];
            while (stk.at(-1)! !== '(') {
                t.push(stk.pop()!);
            }
            stk.pop();
            stk.push(...t);
        } else {
            stk.push(c);
        }
    }
    return stk.join('');
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var reverseParentheses = function (s) {
    const stk = [];
    for (const c of s) {
        if (c === ')') {
            const t = [];
            while (stk.at(-1) !== '(') {
                t.push(stk.pop());
            }
            stk.pop();
            stk.push(...t);
        } else {
            stk.push(c);
        }
    }
    return stk.join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Nhận xét

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sắp xếp lại ký tự ở từng cặp ngoặc. Có thể xem mỗi cặp như một lần nhảy đến dấu ngoặc khớp và đổi hướng duyệt: tính trước vị trí ngoặc tương ứng, khi gặp ngoặc thì nhảy đến vị trí đó và đảo chiều bước di chuyển, chỉ ghi lại các chữ cái. Chỉ cần duyệt chuỗi một lần.

<!-- thinking:end -->

Ta nhận thấy khi duyệt chuỗi, mỗi lần gặp `(` hoặc `)`, ta nhảy đến dấu ngoặc khớp tương ứng rồi đảo chiều duyệt để tiếp tục.

Vì vậy, ta có thể dùng mảng $d$ để lưu vị trí của dấu ngoặc khớp với mỗi dấu `(` hoặc `)`. Cụ thể, $d[i]$ là vị trí của dấu ngoặc tương ứng với dấu ngoặc tại vị trí $i$. Ta có thể dùng trực tiếp stack để tính mảng $d$.

Sau đó, ta duyệt chuỗi từ trái sang phải. Khi gặp `(` hoặc `)`, ta nhảy đến vị trí tương ứng theo mảng $d$, đảo chiều rồi tiếp tục duyệt cho đến hết chuỗi.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseParentheses(self, s: str) -> str:
        n = len(s)
        d = [0] * n
        stk = []
        for i, c in enumerate(s):
            if c == "(":
                stk.append(i)
            elif c == ")":
                j = stk.pop()
                d[i], d[j] = j, i
        i, x = 0, 1
        ans = []
        while i < n:
            if s[i] in "()":
                i = d[i]
                x = -x
            else:
                ans.append(s[i])
            i += x
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String reverseParentheses(String s) {
        int n = s.length();
        int[] d = new int[n];
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '(') {
                stk.push(i);
            } else if (s.charAt(i) == ')') {
                int j = stk.pop();
                d[i] = j;
                d[j] = i;
            }
        }
        StringBuilder ans = new StringBuilder();
        int i = 0, x = 1;
        while (i < n) {
            if (s.charAt(i) == '(' || s.charAt(i) == ')') {
                i = d[i];
                x = -x;
            } else {
                ans.append(s.charAt(i));
            }
            i += x;
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reverseParentheses(string s) {
        int n = s.size();
        vector<int> d(n);
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            if (s[i] == '(') {
                stk.push(i);
            } else if (s[i] == ')') {
                int j = stk.top();
                stk.pop();
                d[i] = j;
                d[j] = i;
            }
        }
        int i = 0, x = 1;
        string ans;
        while (i < n) {
            if (s[i] == '(' || s[i] == ')') {
                i = d[i];
                x = -x;
            } else {
                ans.push_back(s[i]);
            }
            i += x;
        }
        return ans;
    }
};
```

#### Go

```go
func reverseParentheses(s string) string {
	n := len(s)
	d := make([]int, n)
	stk := []int{}
	for i, c := range s {
		if c == '(' {
			stk = append(stk, i)
		} else if c == ')' {
			j := stk[len(stk)-1]
			stk = stk[:len(stk)-1]
			d[i], d[j] = j, i
		}
	}
	ans := []byte{}
	i, x := 0, 1
	for i < n {
		if s[i] == '(' || s[i] == ')' {
			i = d[i]
			x = -x
		} else {
			ans = append(ans, s[i])
		}
		i += x
	}
	return string(ans)
}
```

#### TypeScript

```ts
function reverseParentheses(s: string): string {
    const n = s.length;
    const d: number[] = Array(n).fill(0);
    const stk: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (s[i] === '(') {
            stk.push(i);
        } else if (s[i] === ')') {
            const j = stk.pop()!;
            d[i] = j;
            d[j] = i;
        }
    }
    let i = 0;
    let x = 1;
    const ans: string[] = [];
    while (i < n) {
        const c = s.charAt(i);
        if ('()'.includes(c)) {
            i = d[i];
            x = -x;
        } else {
            ans.push(c);
        }
        i += x;
    }
    return ans.join('');
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var reverseParentheses = function (s) {
    const n = s.length;
    const d = Array(n).fill(0);
    const stk = [];
    for (let i = 0; i < n; ++i) {
        if (s[i] === '(') {
            stk.push(i);
        } else if (s[i] === ')') {
            const j = stk.pop();
            d[i] = j;
            d[j] = i;
        }
    }
    let i = 0;
    let x = 1;
    const ans = [];
    while (i < n) {
        const c = s.charAt(i);
        if ('()'.includes(c)) {
            i = d[i];
            x = -x;
        } else {
            ans.push(c);
        }
        i += x;
    }
    return ans.join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
