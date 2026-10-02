---
comments: true
difficulty: Medium
rating: 1575
source: Biweekly Contest 24 Q3
tags:
    - String
    - Backtracking
---

<!-- problem:start -->

# [1415. The k-th Lexicographical String of All Happy Strings of Length n](https://leetcode.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n)

[中文文档](/solution/1400-1499/1415.The%20k-th%20Lexicographical%20String%20of%20All%20Happy%20Strings%20of%20Length%20n/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>happy string</strong> là một chuỗi:</p>

<ul>
	<li>chỉ bao gồm các chữ cái thuộc tập <code>[&#39;a&#39;, &#39;b&#39;, &#39;c&#39;]</code>.</li>
	<li><code>s[i] != s[i + 1]</code> với mọi giá trị <code>i</code> từ <code>1</code> đến <code>s.length - 1</code> (chuỗi được đánh chỉ số từ 1).</li>
</ul>

<p>Ví dụ, các chuỗi <strong>&quot;abc&quot;, &quot;ac&quot;, &quot;b&quot;</strong> và <strong>&quot;abcbabcbcb&quot;</strong> đều là happy string, còn các chuỗi <strong>&quot;aa&quot;, &quot;baa&quot;</strong> và <strong>&quot;ababbc&quot;</strong> thì không.</p>

<p>Cho hai số nguyên <code>n</code> và <code>k</code>, xét danh sách tất cả happy string có độ dài <code>n</code>, được sắp xếp theo thứ tự từ điển.</p>

<p>Trả về <em>chuỗi thứ k</em> trong danh sách này, hoặc trả về <strong>chuỗi rỗng</strong> nếu có ít hơn <code>k</code> happy string có độ dài <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, k = 3
<strong>Đầu ra:</strong> &quot;c&quot;
<strong>Giải thích:</strong> Danh sách [&quot;a&quot;, &quot;b&quot;, &quot;c&quot;] chứa tất cả happy string có độ dài 1. Chuỗi thứ ba là &quot;c&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, k = 4
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Chỉ có 3 happy string có độ dài 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 9
<strong>Đầu ra:</strong> &quot;cab&quot;
<strong>Giải thích:</strong> Có 12 happy string khác nhau có độ dài 3 [&quot;aba&quot;, &quot;abc&quot;, &quot;aca&quot;, &quot;acb&quot;, &quot;bab&quot;, &quot;bac&quot;, &quot;bca&quot;, &quot;bcb&quot;, &quot;cab&quot;, &quot;cac&quot;, &quot;cba&quot;, &quot;cbc&quot;]. Chuỗi thứ 9 là &quot;cab&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10$, nên có nhiều nhất $3\cdot 2^{n-1}\le 1536$ happy string. DFS theo thứ tự từ điển có thể sinh ra chúng và trả về chuỗi thứ $k$.
>
> Ghi nhận một chuỗi khi độ dài đạt $n$, đồng thời dừng ngay khi đã tìm thấy $k$ chuỗi. Hai ký tự liền kề phải khác nhau; alphabet là $a,b,c$.

<!-- thinking:end -->

Chúng ta dùng một chuỗi $\textit{s}$ để lưu chuỗi hiện tại, ban đầu là chuỗi rỗng. Sau đó, xây dựng hàm $\text{dfs}$ để sinh tất cả happy string có độ dài $n$.

Cách triển khai hàm $\text{dfs}$ như sau:

1. Nếu độ dài của chuỗi hiện tại bằng $n$, thêm chuỗi hiện tại vào mảng đáp án $\textit{ans}$ rồi trả về;
2. Nếu độ dài của mảng đáp án lớn hơn hoặc bằng $k$, trả về ngay;
3. Nếu không, duyệt qua tập ký tự $\{a, b, c\}$. Với mỗi ký tự $c$, nếu chuỗi hiện tại rỗng hoặc ký tự cuối của chuỗi hiện tại khác $c$, thêm ký tự $c$ vào chuỗi hiện tại, rồi gọi đệ quy $\text{dfs}$. Sau khi đệ quy kết thúc, xóa ký tự cuối khỏi chuỗi hiện tại.

Cuối cùng, kiểm tra xem độ dài của mảng đáp án có nhỏ hơn $k$ hay không. Nếu có, trả về chuỗi rỗng; nếu không, trả về phần tử thứ $k$ của mảng đáp án.

Độ phức tạp thời gian là $O(n \times 2^n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getHappyString(self, n: int, k: int) -> str:
        def dfs():
            if len(s) == n:
                ans.append("".join(s))
                return
            if len(ans) >= k:
                return
            for c in "abc":
                if not s or s[-1] != c:
                    s.append(c)
                    dfs()
                    s.pop()

        ans = []
        s = []
        dfs()
        return "" if len(ans) < k else ans[k - 1]
```

#### Java

```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private StringBuilder s = new StringBuilder();
    private int n, k;

    public String getHappyString(int n, int k) {
        this.n = n;
        this.k = k;
        dfs();
        return ans.size() < k ? "" : ans.get(k - 1);
    }

    private void dfs() {
        if (s.length() == n) {
            ans.add(s.toString());
            return;
        }
        if (ans.size() >= k) {
            return;
        }
        for (char c : "abc".toCharArray()) {
            if (s.isEmpty() || s.charAt(s.length() - 1) != c) {
                s.append(c);
                dfs();
                s.deleteCharAt(s.length() - 1);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    string getHappyString(int n, int k) {
        vector<string> ans;
        string s = "";
        auto dfs = [&](this auto&& dfs) -> void {
            if (s.size() == n) {
                ans.emplace_back(s);
                return;
            }
            if (ans.size() >= k) {
                return;
            }
            for (char c = 'a'; c <= 'c'; ++c) {
                if (s.empty() || s.back() != c) {
                    s.push_back(c);
                    dfs();
                    s.pop_back();
                }
            }
        };
        dfs();
        return ans.size() < k ? "" : ans[k - 1];
    }
};
```

#### Go

```go
func getHappyString(n int, k int) string {
    ans := []string{}
    var s []byte

    var dfs func()
    dfs = func() {
        if len(s) == n {
            ans = append(ans, string(s))
            return
        }
        if len(ans) >= k {
            return
        }
        for c := byte('a'); c <= 'c'; c++ {
            if len(s) == 0 || s[len(s)-1] != c {
                s = append(s, c)
                dfs()
                s = s[:len(s)-1]
            }
        }
    }

    dfs()
    if len(ans) < k {
        return ""
    }
    return ans[k-1]
}
```

#### TypeScript

```ts
function getHappyString(n: number, k: number): string {
    const ans: string[] = [];
    const s: string[] = [];
    const dfs = () => {
        if (s.length === n) {
            ans.push(s.join(''));
            return;
        }
        if (ans.length >= k) {
            return;
        }
        for (const c of 'abc') {
            if (!s.length || s.at(-1)! !== c) {
                s.push(c);
                dfs();
                s.pop();
            }
        }
    };
    dfs();
    return ans[k - 1] ?? '';
}
```

#### Rust

```rust
impl Solution {
    pub fn get_happy_string(n: i32, k: i32) -> String {
        let mut ans = Vec::new();
        let mut s = String::new();
        let mut k = k;

        fn dfs(n: i32, s: &mut String, ans: &mut Vec<String>, k: &mut i32) {
            if s.len() == n as usize {
                ans.push(s.clone());
                return;
            }
            if ans.len() >= *k as usize {
                return;
            }
            for c in "abc".chars() {
                if s.is_empty() || s.chars().last() != Some(c) {
                    s.push(c);
                    dfs(n, s, ans, k);
                    s.pop();
                }
            }
        }

        dfs(n, &mut s, &mut ans, &mut k);
        if ans.len() < k as usize {
            "".to_string()
        } else {
            ans[(k - 1) as usize].clone()
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {string}
 */
var getHappyString = function (n, k) {
    const ans = [];
    const s = [];
    const dfs = () => {
        if (s.length === n) {
            ans.push(s.join(''));
            return;
        }
        if (ans.length >= k) {
            return;
        }
        for (const c of 'abc') {
            if (!s.length || s.at(-1) !== c) {
                s.push(c);
                dfs();
                s.pop();
            }
        }
    };
    dfs();
    return ans[k - 1] ?? '';
};
```

#### C#

```cs
public class Solution {
    public string GetHappyString(int n, int k) {
        List<string> ans = new List<string>();
        StringBuilder s = new StringBuilder();

        void Dfs() {
            if (s.Length == n) {
                ans.Add(s.ToString());
                return;
            }
            if (ans.Count >= k) {
                return;
            }
            foreach (char c in "abc") {
                if (s.Length == 0 || s[s.Length - 1] != c) {
                    s.Append(c);
                    Dfs();
                    s.Length--;
                }
            }
        }

        Dfs();
        return ans.Count < k ? "" : ans[k - 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tạo ra từng happy string. Sau một prefix, $n-i-1$ vị trí còn lại có $2$ lựa chọn mỗi vị trí, nên có thể bỏ qua một block kích thước $2^{n-i-1}$ bằng cách sử dụng $k$.
>
> Nếu $k$ lớn hơn $3\cdot 2^{n-1}$, trả về chuỗi rỗng. Nếu không, duyệt từ trái sang phải, thử các chữ cái khác ký tự trước đó và phát ra hoặc trừ kích thước block khỏi $k$.

<!-- thinking:end -->

Chúng ta có thể trực tiếp tính happy string thứ $k$ mà không cần sinh tất cả happy string.

Bắt đầu từ happy string đầu tiên có độ dài $n$, chúng ta có thể xác định từng vị trí ký tự.

Với một happy string có độ dài $n$, ký tự đầu tiên có $3$ lựa chọn, ký tự thứ hai có $2$ lựa chọn (không được giống ký tự đầu tiên), ký tự thứ ba cũng có $2$ lựa chọn (không được giống ký tự thứ hai), và tiếp tục như vậy cho đến ký tự thứ $n$, cũng có $2$ lựa chọn (không được giống ký tự thứ $(n-1)$). Vì vậy, tổng số happy string có độ dài $n$ là $3 \times 2^{n-1}$.

Nếu $k$ lớn hơn tổng số happy string có độ dài $n$, chúng ta trả về chuỗi rỗng ngay.

Nếu không, bắt đầu từ ký tự đầu tiên và lần lượt xác định từng vị trí ký tự. Với ký tự thứ $i$, duyệt qua tập ký tự $\{a, b, c\}$. Nếu ký tự cuối của chuỗi hiện tại khác $c$, tính số happy string còn lại. Nếu $k$ nhỏ hơn hoặc bằng số lượng đó, thêm ký tự $c$ vào chuỗi hiện tại và chuyển sang vị trí tiếp theo; nếu không, trừ số lượng đó khỏi $k$ rồi tiếp tục duyệt ký tự kế tiếp.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getHappyString(self, n: int, k: int) -> str:
        if k > 3 * (1 << (n - 1)):
            return ""
        cs = "abc"
        ans = []
        for i in range(n):
            remain = 1 << (n - i - 1)
            for c in cs:
                if ans and ans[-1] == c:
                    continue
                if k <= remain:
                    ans.append(c)
                    break
                k -= remain
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String getHappyString(int n, int k) {
        if (k > 3 * (1 << (n - 1))) {
            return "";
        }
        String cs = "abc";
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < n; i++) {
            int remain = 1 << (n - i - 1);
            for (char c : cs.toCharArray()) {
                if (ans.length() > 0 && ans.charAt(ans.length() - 1) == c) {
                    continue;
                }
                if (k <= remain) {
                    ans.append(c);
                    break;
                }
                k -= remain;
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string getHappyString(int n, int k) {
        if (k > 3 * (1 << (n - 1))) {
            return "";
        }
        string cs = "abc";
        string ans;
        for (int i = 0; i < n; ++i) {
            int remain = 1 << (n - i - 1);
            for (char c : cs) {
                if (!ans.empty() && ans.back() == c) {
                    continue;
                }
                if (k <= remain) {
                    ans.push_back(c);
                    break;
                }
                k -= remain;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getHappyString(n int, k int) string {
	if k > 3*(1<<(n-1)) {
		return ""
	}
	cs := "abc"
	ans := make([]byte, 0, n)
	for i := 0; i < n; i++ {
		remain := 1 << (n - i - 1)
		for j := 0; j < len(cs); j++ {
			c := cs[j]
			if len(ans) > 0 && ans[len(ans)-1] == c {
				continue
			}
			if k <= remain {
				ans = append(ans, c)
				break
			}
			k -= remain
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function getHappyString(n: number, k: number): string {
    if (k > 3 * (1 << (n - 1))) {
        return '';
    }
    const cs = 'abc';
    const ans: string[] = [];
    for (let i = 0; i < n; i++) {
        const remain = 1 << (n - i - 1);
        for (const c of cs) {
            if (ans.at(-1) === c) {
                continue;
            }
            if (k <= remain) {
                ans.push(c);
                break;
            }
            k -= remain;
        }
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn get_happy_string(n: i32, mut k: i32) -> String {
        if k > 3 * (1 << (n - 1)) {
            return String::new();
        }
        let cs = ['a', 'b', 'c'];
        let mut ans: Vec<char> = Vec::with_capacity(n as usize);
        for i in 0..n {
            let remain = 1 << (n - i - 1);
            for &c in &cs {
                if !ans.is_empty() && *ans.last().unwrap() == c {
                    continue;
                }
                if k <= remain {
                    ans.push(c);
                    break;
                }
                k -= remain;
            }
        }
        ans.into_iter().collect()
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {string}
 */
var getHappyString = function (n, k) {
    if (k > 3 * (1 << (n - 1))) {
        return '';
    }
    const cs = 'abc';
    const ans = [];
    for (let i = 0; i < n; i++) {
        const remain = 1 << (n - i - 1);
        for (let j = 0; j < cs.length; j++) {
            const c = cs[j];
            if (ans.at(-1) === c) {
                continue;
            }
            if (k <= remain) {
                ans.push(c);
                break;
            }
            k -= remain;
        }
    }
    return ans.join('');
};
```

#### C#

```cs
public class Solution {
    public string GetHappyString(int n, int k) {
        if (k > 3 * (1 << (n - 1))) {
            return "";
        }
        string cs = "abc";
        var ans = new System.Text.StringBuilder();
        for (int i = 0; i < n; i++) {
            int remain = 1 << (n - i - 1);
            foreach (char c in cs) {
                if (ans.Length > 0 && ans[ans.Length - 1] == c) {
                    continue;
                }
                if (k <= remain) {
                    ans.Append(c);
                    break;
                }
                k -= remain;
            }
        }
        return ans.ToString();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
