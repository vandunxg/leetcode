---
comments: true
difficulty: Medium
rating: 1483
source: Weekly Contest 367 Q2
tags:
    - String
    - Sliding Window
---

<!-- problem:start -->

# [2904. Shortest and Lexicographically Smallest Beautiful String](https://leetcode.com/problems/shortest-and-lexicographically-smallest-beautiful-string)

[中文文档](/solution/2900-2999/2904.Shortest%20and%20Lexicographically%20Smallest%20Beautiful%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code> và một số nguyên dương <code>k</code>.</p>

<p>Một chuỗi con của <code>s</code> được gọi là <strong>đẹp</strong> nếu số lượng số <code>1</code> trong đó đúng bằng <code>k</code>.</p>

<p>Gọi <code>len</code> là độ dài của chuỗi con <strong>đẹp ngắn nhất</strong>.</p>

<p>Hãy trả về <em>chuỗi con đẹp <strong>nhỏ nhất theo thứ tự từ điển</strong> của chuỗi </em><code>s</code><em> có độ dài bằng </em><code>len</code>. Nếu <code>s</code> không chứa chuỗi con đẹp, hãy trả về <em>chuỗi <strong>rỗng</strong></em>.</p>

<p>Một chuỗi <code>a</code> được gọi là <strong>lớn hơn theo thứ tự từ điển</strong> chuỗi <code>b</code> (có cùng độ dài) nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, ký tự của <code>a</code> lớn hơn nghiêm ngặt ký tự tương ứng của <code>b</code>.</p>

<ul>
	<li>Ví dụ, <code>&quot;abcd&quot;</code> lớn hơn theo thứ tự từ điển <code>&quot;abcc&quot;</code> vì vị trí đầu tiên chúng khác nhau là ký tự thứ tư, và <code>d</code> lớn hơn <code>c</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;100011001&quot;, k = 3
<strong>Đầu ra:</strong> &quot;11001&quot;
<strong>Giải thích:</strong> Có 7 chuỗi con đẹp trong ví dụ này:
1. Chuỗi con &quot;<u>100011</u>001&quot;.
2. Chuỗi con &quot;<u>1000110</u>01&quot;.
3. Chuỗi con &quot;<u>10001100</u>1&quot;.
4. Chuỗi con &quot;1<u>00011001</u>&quot;.
5. Chuỗi con &quot;10<u>0011001</u>&quot;.
6. Chuỗi con &quot;100<u>011001</u>&quot;.
7. Chuỗi con &quot;1000<u>11001</u>&quot;.
Độ dài của chuỗi con đẹp ngắn nhất là 5.
Chuỗi con đẹp nhỏ nhất theo thứ tự từ điển có độ dài 5 là chuỗi con &quot;11001&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1011&quot;, k = 2
<strong>Đầu ra:</strong> &quot;11&quot;
<strong>Giải thích:</strong> Có 3 chuỗi con đẹp trong ví dụ này:
1. Chuỗi con &quot;<u>101</u>1&quot;.
2. Chuỗi con &quot;1<u>011</u>&quot;.
3. Chuỗi con &quot;10<u>11</u>&quot;.
Độ dài của chuỗi con đẹp ngắn nhất là 2.
Chuỗi con đẹp nhỏ nhất theo thứ tự từ điển có độ dài 2 là chuỗi con &quot;11&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;000&quot;, k = 1
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không có chuỗi con đẹp nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con đẹp chứa đúng $k$ số 1. Vì $n \le 100$, ta có thể liệt kê tất cả $O(n^2)$ chuỗi con và đếm số số 1. Trong các đoạn hợp lệ, ta giữ lại đoạn ngắn hơn, nếu bằng nhau thì chọn theo thứ tự từ điển.
>
> Vòng lặp bên trong có thể bắt đầu từ $i+k$, vì cần ít nhất $k$ ký tự để chứa $k$ số 1.

<!-- thinking:end -->

Có thể liệt kê tất cả các chuỗi con $s[i: j]$, trong đó $i \lt j$, rồi kiểm tra xem chúng có phải là chuỗi con đẹp hay không. Nếu đúng, ta cập nhật đáp án.

Độ phức tạp thời gian là $O(n^3)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestBeautifulSubstring(self, s: str, k: int) -> str:
        n = len(s)
        ans = ""
        for i in range(n):
            for j in range(i + k, n + 1):
                t = s[i:j]
                if t.count("1") == k and (
                    not ans or j - i < len(ans) or (j - i == len(ans) and t < ans)
                ):
                    ans = t
        return ans
```

#### Java

```java
class Solution {
    public String shortestBeautifulSubstring(String s, int k) {
        int n = s.length();
        String ans = "";
        for (int i = 0; i < n; ++i) {
            for (int j = i + k; j <= n; ++j) {
                String t = s.substring(i, j);
                int cnt = 0;
                for (char c : t.toCharArray()) {
                    cnt += c - '0';
                }
                if (cnt == k
                    && ("".equals(ans) || j - i < ans.length()
                        || (j - i == ans.length() && t.compareTo(ans) < 0))) {
                    ans = t;
                }
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
    string shortestBeautifulSubstring(string s, int k) {
        int n = s.size();
        string ans = "";
        for (int i = 0; i < n; ++i) {
            for (int j = i + k; j <= n; ++j) {
                string t = s.substr(i, j - i);
                int cnt = count(t.begin(), t.end(), '1');
                if (cnt == k && (ans == "" || j - i < ans.size() || (j - i == ans.size() && t < ans))) {
                    ans = t;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func shortestBeautifulSubstring(s string, k int) (ans string) {
	n := len(s)
	for i := 0; i < n; i++ {
		for j := i + k; j <= n; j++ {
			t := s[i:j]
			cnt := 0
			for _, c := range t {
				if c == '1' {
					cnt++
				}
			}
			if cnt == k && (ans == "" || j-i < len(ans) || (j-i == len(ans) && t < ans)) {
				ans = t
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function shortestBeautifulSubstring(s: string, k: number): string {
    const n = s.length;
    let ans: string = '';
    for (let i = 0; i < n; ++i) {
        for (let j = i + k; j <= n; ++j) {
            const t = s.slice(i, j);
            const cnt = t.split('').filter(c => c === '1').length;
            if (
                cnt === k &&
                (ans === '' || j - i < ans.length || (j - i === ans.length && t < ans))
            ) {
                ans = t;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn shortest_beautiful_substring(s: String, k: i32) -> String {
        let n = s.len();
        let mut ans = String::new();

        for i in 0..n {
            for j in i + (k as usize)..=n {
                let t = &s[i..j];
                if (t.matches('1').count() as i32) == k
                    && (ans.is_empty() || j - i < ans.len() || (j - i == ans.len() && t < &ans))
                {
                    ans = t.to_string();
                }
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public string ShortestBeautifulSubstring(string s, int k) {
        int n = s.Length;
        string ans = "";

        for (int i = 0; i < n; i++) {
            for (int j = i + k; j <= n; j++) {
                string t = s.Substring(i, j - i);

                int cnt = 0;
                foreach (char c in t.ToCharArray()) {
                    cnt += c - '0';
                }

                if (cnt == k &&
                    (ans == "" ||
                     j - i < ans.Length ||
                     (j - i == ans.Length && string.Compare(t, ans, StringComparison.Ordinal) < 0))) {
                    ans = t;
                }
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Với $n=100$, việc liệt kê vẫn đủ nhanh, nhưng gọi $count$ trên mọi lát cắt sẽ quét lại cùng một đoạn nhiều lần. Các đoạn đẹp là những cửa sổ có đúng $k$ số 1; bỏ số $0$ ở đầu không làm mất số 1, nên một ứng viên ngắn nhất không có số 0 ở đầu.
>
> Hai con trỏ duy trì cửa sổ: mở rộng đầu phải, rồi thu hẹp khi số lượng số 1 vượt quá $k$ hoặc ký tự đầu trái là $0$. Mỗi khi số lượng bằng $k$, ta so sánh độ dài và thứ tự từ điển với đáp án hiện tại.

<!-- thinking:end -->

Ta cũng có thể dùng hai con trỏ để duy trì một cửa sổ trượt, trong đó con trỏ $i$ trỏ đến biên trái của cửa sổ và con trỏ $j$ trỏ đến biên phải của cửa sổ. Ban đầu, cả $i$ và $j$ đều trỏ đến $0$. Ngoài ra, ta dùng biến $cnt$ để ghi nhận số lượng số $1$ trong cửa sổ trượt.

Trước tiên, ta di chuyển con trỏ $j$ sang phải, thêm $s[j]$ vào cửa sổ trượt và cập nhật $cnt$. Nếu $cnt$ lớn hơn $k$, hoặc nếu $i$ nhỏ hơn $j$ và $s[i]$ là $0$, ta di chuyển con trỏ $i$ sang phải và cập nhật $cnt$.

Khi $cnt$ bằng $k$, ta đã tìm được một chuỗi con đẹp. Ta so sánh nó với đáp án hiện tại và cập nhật đáp án nếu cần.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestBeautifulSubstring(self, s: str, k: int) -> str:
        i = j = cnt = 0
        n = len(s)
        ans = ""
        while j < n:
            cnt += s[j] == "1"
            while cnt > k or (i < j and s[i] == "0"):
                cnt -= s[i] == "1"
                i += 1
            j += 1
            if cnt == k and (
                not ans or j - i < len(ans) or (j - i == len(ans) and s[i:j] < ans)
            ):
                ans = s[i:j]
        return ans
```

#### Java

```java
class Solution {
    public String shortestBeautifulSubstring(String s, int k) {
        int i = 0, j = 0, cnt = 0;
        int n = s.length();
        String ans = "";
        while (j < n) {
            cnt += s.charAt(j) - '0';
            while (cnt > k || (i < j && s.charAt(i) == '0')) {
                cnt -= s.charAt(i) - '0';
                ++i;
            }
            ++j;
            String t = s.substring(i, j);
            if (cnt == k
                && ("".equals(ans) || j - i < ans.length()
                    || (j - i == ans.length() && t.compareTo(ans) < 0))) {
                ans = t;
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
    string shortestBeautifulSubstring(string s, int k) {
        int i = 0, j = 0, cnt = 0;
        int n = s.size();
        string ans = "";
        while (j < n) {
            cnt += s[j] == '1';
            while (cnt > k || (i < j && s[i] == '0')) {
                cnt -= s[i++] == '1';
            }
            ++j;
            string t = s.substr(i, j - i);
            if (cnt == k && (ans == "" || j - i < ans.size() || (j - i == ans.size() && t < ans))) {
                ans = t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func shortestBeautifulSubstring(s string, k int) (ans string) {
	i, j, cnt := 0, 0, 0
	n := len(s)
	for j < n {
		cnt += int(s[j] - '0')
		for cnt > k || (i < j && s[i] == '0') {
			cnt -= int(s[i] - '0')
			i++
		}
		j++
		t := s[i:j]
		if cnt == k && (ans == "" || j-i < len(ans) || (j-i == len(ans) && t < ans)) {
			ans = t
		}
	}
	return
}
```

#### TypeScript

```ts
function shortestBeautifulSubstring(s: string, k: number): string {
    let [i, j, cnt] = [0, 0, 0];
    const n = s.length;
    let ans: string = '';
    while (j < n) {
        cnt += s[j] === '1' ? 1 : 0;
        while (cnt > k || (i < j && s[i] === '0')) {
            cnt -= s[i++] === '1' ? 1 : 0;
        }
        ++j;
        const t = s.slice(i, j);
        if (cnt === k && (ans === '' || j - i < ans.length || (j - i === ans.length && t < ans))) {
            ans = t;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn shortest_beautiful_substring(s: String, k: i32) -> String {
        let s_chars: Vec<char> = s.chars().collect();
        let mut i = 0;
        let mut j = 0;
        let mut cnt = 0;
        let mut ans = String::new();
        let n = s.len();

        while j < n {
            if s_chars[j] == '1' {
                cnt += 1;
            }

            while cnt > k || (i < j && s_chars[i] == '0') {
                if s_chars[i] == '1' {
                    cnt -= 1;
                }
                i += 1;
            }

            j += 1;

            if cnt == k
                && (ans.is_empty() || j - i < ans.len() || (j - i == ans.len() && &s[i..j] < &ans))
            {
                ans = s_chars[i..j].iter().collect();
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public string ShortestBeautifulSubstring(string s, int k) {
        int i = 0, j = 0, cnt = 0;
        int n = s.Length;
        string ans = "";

        while (j < n) {
            cnt += s[j] - '0';

            while (cnt > k || (i < j && s[i] == '0')) {
                cnt -= s[i] - '0';
                i++;
            }

            j++;

            string t = s.Substring(i, j - i);

            if (cnt == k &&
                (ans == "" ||
                 j - i < ans.Length ||
                 (j - i == ans.Length && string.Compare(t, ans, StringComparison.Ordinal) < 0))) {
                ans = t;
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
