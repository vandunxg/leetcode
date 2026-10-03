---
comments: true
difficulty: Easy
rating: 1292
source: Biweekly Contest 93 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2496. Maximum Value of a String in an Array](https://leetcode.com/problems/maximum-value-of-a-string-in-an-array)

[中文文档](/solution/2400-2499/2496.Maximum%20Value%20of%20a%20String%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Giá trị</strong> của một chuỗi chữ và số được định nghĩa như sau:</p>

<ul>
	<li>Biểu diễn <strong>số</strong> của chuỗi trong cơ số <code>10</code>, nếu chuỗi chỉ gồm <strong>các chữ số</strong>.</li>
	<li><strong>Độ dài</strong> của chuỗi, nếu không.</li>
</ul>

<p>Cho một mảng <code>strs</code> gồm các chuỗi chữ và số, hãy trả về <em><strong>giá trị lớn nhất</strong> của bất kỳ chuỗi nào trong </em><code>strs</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;alic3&quot;,&quot;bob&quot;,&quot;3&quot;,&quot;4&quot;,&quot;00000&quot;]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
- &quot;alic3&quot; gồm cả chữ cái và chữ số, nên giá trị của nó bằng độ dài, tức là 5.
- &quot;bob&quot; chỉ gồm các chữ cái, nên giá trị của nó cũng bằng độ dài, tức là 3.
- &quot;3&quot; chỉ gồm các chữ số, nên giá trị của nó bằng giá trị số tương ứng, tức là 3.
- &quot;4&quot; cũng chỉ gồm các chữ số, nên giá trị của nó là 4.
- &quot;00000&quot; chỉ gồm các chữ số, nên giá trị của nó là 0.
Do đó, giá trị lớn nhất là 5, thuộc về &quot;alic3&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;1&quot;,&quot;01&quot;,&quot;001&quot;,&quot;0001&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Mỗi chuỗi trong mảng đều có giá trị bằng 1. Vì vậy, ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= strs.length &lt;= 100</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 9</code></li>
	<li><code>strs[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường và chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi hoặc có giá trị thập phân của nó, hoặc, nếu chứa chữ cái, có giá trị bằng độ dài. Độ dài tối đa là $9$. Kiểm tra $\textit{isdigit}$ rồi lấy $\textit{int}$ hoặc $\textit{len}$, đồng thời giữ lại giá trị lớn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumValue(self, strs: List[str]) -> int:
        def f(s: str) -> int:
            return int(s) if all(c.isdigit() for c in s) else len(s)

        return max(f(s) for s in strs)
```

#### Java

```java
class Solution {
    public int maximumValue(String[] strs) {
        int ans = 0;
        for (var s : strs) {
            ans = Math.max(ans, f(s));
        }
        return ans;
    }

    private int f(String s) {
        int x = 0;
        for (int i = 0, n = s.length(); i < n; ++i) {
            char c = s.charAt(i);
            if (Character.isLetter(c)) {
                return n;
            }
            x = x * 10 + (c - '0');
        }
        return x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumValue(vector<string>& strs) {
        auto f = [](string& s) {
            int x = 0;
            for (char& c : s) {
                if (!isdigit(c)) {
                    return (int) s.size();
                }
                x = x * 10 + c - '0';
            }
            return x;
        };
        int ans = 0;
        for (auto& s : strs) {
            ans = max(ans, f(s));
        }
        return ans;
    }
};
```

#### Go

```go
func maximumValue(strs []string) (ans int) {
	f := func(s string) (x int) {
		for _, c := range s {
			if c >= 'a' && c <= 'z' {
				return len(s)
			}
			x = x*10 + int(c-'0')
		}
		return
	}
	for _, s := range strs {
		if x := f(s); ans < x {
			ans = x
		}
	}
	return
}
```

#### TypeScript

```ts
function maximumValue(strs: string[]): number {
    const f = (s: string) => (Number.isNaN(Number(s)) ? s.length : Number(s));
    return Math.max(...strs.map(f));
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_value(strs: Vec<String>) -> i32 {
        let mut ans = 0;
        for s in strs.iter() {
            let num = s.parse().unwrap_or(s.len());
            ans = ans.max(num);
        }
        ans as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int MaximumValue(string[] strs) {
        return strs.Max(f);
    }

    private int f(string s) {
        int x = 0;
        foreach (var c in s) {
            if (c >= 'a') {
                return s.Length;
            }
            x = x * 10 + (c - '0');
        }
        return x;
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int parseInt(char* s) {
    int n = strlen(s);
    int res = 0;
    for (int i = 0; i < n; i++) {
        if (!isdigit(s[i])) {
            return n;
        }
        res = res * 10 + s[i] - '0';
    }
    return res;
}

int maximumValue(char** strs, int strsSize) {
    int ans = 0;
    for (int i = 0; i < strsSize; i++) {
        int num = parseInt(strs[i]);
        ans = max(ans, num);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 phân tích toàn bộ chuỗi sau khi kiểm tra chuỗi chỉ gồm chữ số. Việc tích lũy từng chữ số sẽ trả về độ dài ngay khi gặp chữ cái, còn nếu không thì xây dựng số nguyên, không cần quét riêng một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumValue(self, strs: List[str]) -> int:
        def f(s: str) -> int:
            x = 0
            for c in s:
                if c.isalpha():
                    return len(s)
                x = x * 10 + ord(c) - ord("0")
            return x

        return max(f(s) for s in strs)
```

#### Rust

```rust
impl Solution {
    pub fn maximum_value(strs: Vec<String>) -> i32 {
        let parse = |s: String| -> i32 {
            let mut x = 0;

            for c in s.chars() {
                if c >= 'a' && c <= 'z' {
                    x = s.len();
                    break;
                }

                x = x * 10 + (((c as u8) - b'0') as usize);
            }

            x as i32
        };

        let mut ans = 0;
        for s in strs {
            let v = parse(s);
            if v > ans {
                ans = v;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc giống phương pháp 1 thông qua hàm parse có sẵn: nếu thành công thì dùng số, nếu thất bại thì dùng độ dài. Nhánh lỗi chính là trường hợp chuỗi chứa ký tự không phải chữ số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
use std::cmp::max;

impl Solution {
    pub fn maximum_value(strs: Vec<String>) -> i32 {
        let mut ans = 0;

        for s in strs {
            match s.parse::<i32>() {
                Ok(v) => {
                    ans = max(ans, v);
                }
                Err(_) => {
                    ans = max(ans, s.len() as i32);
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
