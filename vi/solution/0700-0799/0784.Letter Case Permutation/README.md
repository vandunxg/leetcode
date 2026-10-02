---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - String
    - Backtracking
---

<!-- problem:start -->

# [784. Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation)

[中文文档](/solution/0700-0799/0784.Letter%20Case%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, bạn có thể đổi riêng từng chữ cái thành chữ thường hoặc chữ hoa để tạo ra một chuỗi khác.</p>

<p>Hãy trả về <em>danh sách tất cả các chuỗi có thể tạo ra</em>. Có thể trả kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a1b2&quot;
<strong>Đầu ra:</strong> [&quot;a1b2&quot;,&quot;a1B2&quot;,&quot;A1b2&quot;,&quot;A1B2&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;3z4&quot;
<strong>Đầu ra:</strong> [&quot;3z4&quot;,&quot;3Z4&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 12</code></li>
	<li><code>s</code> gồm chữ cái tiếng Anh viết thường, chữ cái tiếng Anh viết hoa và chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ cái có thể đổi kiểu chữ. Chuỗi ngắn nên có thể dùng DFS để liệt kê các khả năng.
>
> Với mỗi chữ cái, trước tiên tiếp tục mà không đổi, sau đó xor với $32$ rồi tiếp tục; chữ số chỉ có một nhánh. Đến cuối chuỗi thì lưu kết quả.

<!-- thinking:end -->

Vì mỗi chữ cái trong $s$ có thể chuyển thành chữ hoa hoặc chữ thường, ta có thể dùng DFS (Depth-First Search) để liệt kê mọi khả năng.

Cụ thể, duyệt chuỗi $s$ từ trái sang phải. Với mỗi chữ cái, ta chọn chuyển nó thành chữ hoa hoặc chữ thường rồi tiếp tục duyệt các ký tự phía sau. Khi đến cuối chuỗi, ta có một phương án chuyển đổi và thêm phương án đó vào đáp án.

Có thể đổi kiểu chữ bằng các phép toán bit. Với một chữ cái, mã ASCII của dạng chữ thường và chữ hoa chênh nhau $32$, vì vậy ta có thể đổi kiểu chữ bằng cách XOR mã ASCII của chữ cái với $32$.

Độ phức tạp thời gian là $O(n \times 2^n)$, trong đó $n$ là độ dài chuỗi $s$. Với mỗi chữ cái, ta có thể chọn chuyển thành chữ hoa hoặc chữ thường nên tổng cộng có $2^n$ phương án. Mỗi phương án cần $O(n)$ thời gian để tạo chuỗi mới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def letterCasePermutation(self, s: str) -> List[str]:
        def dfs(i: int) -> None:
            if i >= len(t):
                ans.append("".join(t))
                return
            dfs(i + 1)
            if t[i].isalpha():
                t[i] = chr(ord(t[i]) ^ 32)
                dfs(i + 1)

        t = list(s)
        ans = []
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private char[] t;

    public List<String> letterCasePermutation(String s) {
        t = s.toCharArray();
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i >= t.length) {
            ans.add(new String(t));
            return;
        }
        dfs(i + 1);
        if (Character.isLetter(t[i])) {
            t[i] ^= 32;
            dfs(i + 1);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> letterCasePermutation(string s) {
        string t = s;
        vector<string> ans;
        auto dfs = [&](this auto&& dfs, int i) -> void {
            if (i >= t.size()) {
                ans.push_back(t);
                return;
            }
            dfs(i + 1);
            if (isalpha(t[i])) {
                t[i] ^= 32;
                dfs(i + 1);
            }
        };
        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func letterCasePermutation(s string) (ans []string) {
	t := []byte(s)
	var dfs func(int)
	dfs = func(i int) {
		if i >= len(t) {
			ans = append(ans, string(t))
			return
		}
		dfs(i + 1)
		if t[i] >= 'A' {
			t[i] ^= 32
			dfs(i + 1)
		}
	}

	dfs(0)
	return ans
}
```

#### TypeScript

```ts
function letterCasePermutation(s: string): string[] {
    const t = s.split('');
    const ans: string[] = [];
    const dfs = (i: number) => {
        if (i >= t.length) {
            ans.push(t.join(''));
            return;
        }
        dfs(i + 1);
        if (t[i].charCodeAt(0) >= 65) {
            t[i] = String.fromCharCode(t[i].charCodeAt(0) ^ 32);
            dfs(i + 1);
        }
    };
    dfs(0);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn letter_case_permutation(s: String) -> Vec<String> {
        fn dfs(i: usize, t: &mut Vec<char>, ans: &mut Vec<String>) {
            if i >= t.len() {
                ans.push(t.iter().collect());
                return;
            }
            dfs(i + 1, t, ans);
            if t[i].is_alphabetic() {
                t[i] = (t[i] as u8 ^ 32) as char;
                dfs(i + 1, t, ans);
            }
        }

        let mut t: Vec<char> = s.chars().collect();
        let mut ans = Vec::new();
        dfs(0, &mut t, &mut ans);
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Nếu có $n$ chữ cái thì có đúng $2^n$ chuỗi. Ta có thể dùng bitmask để chọn dạng chữ thường/hoa cho chữ cái thứ $j$.
>
> Với mỗi mask, đổi kiểu chữ của các chữ cái theo bit tương ứng và giữ nguyên các chữ số.

<!-- thinking:end -->

Mỗi chữ cái có thể được chuyển thành chữ hoa hoặc chữ thường. Vì vậy, ta dùng một bit nhị phân cho mỗi chữ cái để biểu diễn phương án chuyển đổi: $1$ là chữ thường, còn $0$ là chữ hoa.

Trước tiên, đếm số chữ cái trong chuỗi $s$ và gọi số đó là $n$. Khi đó có tổng cộng $2^n$ phương án chuyển đổi. Ta có thể dùng từng bit của một số nhị phân để biểu diễn phương án cho mỗi chữ cái, lần lượt xét các số từ $0$ đến $2^n-1$.

Cụ thể, dùng biến $i$ để biểu diễn số nhị phân đang xét; bit thứ $j$ của $i$ biểu diễn phương án chuyển đổi cho chữ cái thứ $j$. Nghĩa là, nếu bit thứ $j$ của $i$ bằng $1$ thì chuyển chữ cái thứ $j$ thành chữ thường, còn nếu bằng $0$ thì chuyển thành chữ hoa.

Độ phức tạp thời gian là $O(n \times 2^n)$, trong đó $n$ là độ dài chuỗi $s$. Với mỗi chữ cái, ta có thể chọn chuyển thành chữ hoa hoặc chữ thường nên tổng cộng có $2^n$ phương án. Mỗi phương án cần $O(n)$ thời gian để tạo chuỗi mới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def letterCasePermutation(self, s: str) -> List[str]:
        ans = []
        n = sum(c.isalpha() for c in s)
        for i in range(1 << n):
            j, t = 0, []
            for c in s:
                if c.isalpha():
                    c = c.lower() if (i >> j) & 1 else c.upper()
                    j += 1
                t.append(c)
            ans.append(''.join(t))
        return ans
```

#### Java

```java
class Solution {
    public List<String> letterCasePermutation(String s) {
        int n = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) >= 'A') {
                ++n;
            }
        }
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < 1 << n; ++i) {
            int j = 0;
            StringBuilder t = new StringBuilder();
            for (int k = 0; k < s.length(); ++k) {
                char x = s.charAt(k);
                if (x >= 'A') {
                    x = ((i >> j) & 1) == 1 ? Character.toLowerCase(x) : Character.toUpperCase(x);
                    ++j;
                }
                t.append(x);
            }
            ans.add(t.toString());
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> letterCasePermutation(string s) {
        int n = count_if(s.begin(), s.end(), [](char c) { return isalpha(c); });
        vector<string> ans;
        for (int i = 0; i < 1 << n; ++i) {
            int j = 0;
            string t;
            for (char c : s) {
                if (isalpha(c)) {
                    c = (i >> j & 1) ? tolower(c) : toupper(c);
                    ++j;
                }
                t += c;
            }
            ans.emplace_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
func letterCasePermutation(s string) (ans []string) {
	n := 0
	for _, c := range s {
		if c >= 'A' {
			n++
		}
	}
	for i := 0; i < 1<<n; i++ {
		j := 0
		t := []rune{}
		for _, c := range s {
			if c >= 'A' {
				if ((i >> j) & 1) == 1 {
					c = unicode.ToLower(c)
				} else {
					c = unicode.ToUpper(c)
				}
				j++
			}
			t = append(t, c)
		}
		ans = append(ans, string(t))
	}
	return ans
}
```

#### TypeScript

```ts
function letterCasePermutation(s: string): string[] {
    const ans: string[] = [];
    const n: number = Array.from(s).filter(c => /[a-zA-Z]/.test(c)).length;
    for (let i = 0; i < 1 << n; ++i) {
        let j = 0;
        const t: string[] = [];
        for (let c of s) {
            if (/[a-zA-Z]/.test(c)) {
                t.push((i >> j) & 1 ? c.toLowerCase() : c.toUpperCase());
                j++;
            } else {
                t.push(c);
            }
        }
        ans.push(t.join(''));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn letter_case_permutation(s: String) -> Vec<String> {
        let n = s.chars().filter(|&c| c.is_alphabetic()).count();
        let mut ans = Vec::new();
        for i in 0..(1 << n) {
            let mut j = 0;
            let mut t = String::new();
            for c in s.chars() {
                if c.is_alphabetic() {
                    if (i >> j) & 1 == 1 {
                        t.push(c.to_lowercase().next().unwrap());
                    } else {
                        t.push(c.to_uppercase().next().unwrap());
                    }
                    j += 1;
                } else {
                    t.push(c);
                }
            }
            ans.push(t);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
