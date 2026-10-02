---
comments: true
difficulty: Hard
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [471. Encode String with Shortest Length 🔒](https://leetcode.com/problems/encode-string-with-shortest-length)

[中文文档](/solution/0400-0499/0471.Encode%20String%20with%20Shortest%20Length/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy mã hóa chuỗi sao cho độ dài sau khi mã hóa là ngắn nhất.</p>

<p>Quy tắc mã hóa là: <code>k[encoded_string]</code>, trong đó <code>encoded_string</code> nằm trong ngoặc vuông được lặp lại đúng <code>k</code> lần. <code>k</code> phải là số nguyên dương.</p>

<p>Nếu mã hóa không làm chuỗi ngắn hơn thì không mã hóa. Nếu có nhiều lời giải, trả về <strong>một lời giải bất kỳ</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaa&quot;
<strong>Đầu ra:</strong> &quot;aaa&quot;
<strong>Giải thích:</strong> Không có cách mã hóa nào tạo ra chuỗi ngắn hơn chuỗi đầu vào, nên ta giữ nguyên chuỗi.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaaa&quot;
<strong>Đầu ra:</strong> &quot;5[a]&quot;
<strong>Giải thích:</strong> &quot;5[a]&quot; ngắn hơn &quot;aaaaa&quot; một ký tự.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaaaaaaaa&quot;
<strong>Đầu ra:</strong> &quot;10[a]&quot;
<strong>Giải thích:</strong> &quot;a9[a]&quot; hoặc &quot;9[a]a&quot; cũng là lời giải hợp lệ; cả hai đều có độ dài bằng 5, giống như &quot;10[a]&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 150</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Mã hóa ngắn nhất có thể gói cả chuỗi con theo dạng $k[t]$ hoặc chia chuỗi con thành các phần. Với $n\le 150$, interval DP phù hợp.
>
> $f[i][j]$ là cách mã hóa ngắn nhất của $s[i..j]$. Trước tiên, thử nén toàn bộ đoạn bằng cách kiểm tra chu kỳ trên chuỗi được nối đôi (bỏ qua các đoạn có độ dài $<5$), sau đó thử mọi vị trí chia $f[i][k]+f[k+1][j]$. Duyệt $i$ giảm dần và $j$ tăng dần để các đoạn con cần thiết đã được tính trước.
>
> Phép kiểm tra chu kỳ giống với cách làm ở bài 459. Phần trong ngoặc vuông chứa cách mã hóa ngắn nhất của chu kỳ, không phải văn bản thô.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def encode(self, s: str) -> str:
        def g(i: int, j: int) -> str:
            t = s[i : j + 1]
            if len(t) < 5:
                return t
            k = (t + t).index(t, 1)
            if k < len(t):
                cnt = len(t) // k
                return f"{cnt}[{f[i][i + k - 1]}]"
            return t

        n = len(s)
        f = [[None] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            for j in range(i, n):
                f[i][j] = g(i, j)
                if j - i + 1 > 4:
                    for k in range(i, j):
                        t = f[i][k] + f[k + 1][j]
                        if len(f[i][j]) > len(t):
                            f[i][j] = t
        return f[0][-1]
```

#### Java

```java
class Solution {
    private String s;
    private String[][] f;

    public String encode(String s) {
        this.s = s;
        int n = s.length();
        f = new String[n][n];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                f[i][j] = g(i, j);
                if (j - i + 1 > 4) {
                    for (int k = i; k < j; ++k) {
                        String t = f[i][k] + f[k + 1][j];
                        if (f[i][j].length() > t.length()) {
                            f[i][j] = t;
                        }
                    }
                }
            }
        }
        return f[0][n - 1];
    }

    private String g(int i, int j) {
        String t = s.substring(i, j + 1);
        if (t.length() < 5) {
            return t;
        }
        int k = (t + t).indexOf(t, 1);
        if (k < t.length()) {
            int cnt = t.length() / k;
            return String.format("%d[%s]", cnt, f[i][i + k - 1]);
        }
        return t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string encode(string s) {
        int n = s.size();
        vector<vector<string>> f(n, vector<string>(n));

        auto g = [&](int i, int j) {
            string t = s.substr(i, j - i + 1);
            if (t.size() < 5) {
                return t;
            }
            int k = (t + t).find(t, 1);
            if (k < t.size()) {
                int cnt = t.size() / k;
                return to_string(cnt) + "[" + f[i][i + k - 1] + "]";
            }
            return t;
        };

        for (int i = n - 1; ~i; --i) {
            for (int j = i; j < n; ++j) {
                f[i][j] = g(i, j);
                if (j - i + 1 > 4) {
                    for (int k = i; k < j; ++k) {
                        string t = f[i][k] + f[k + 1][j];
                        if (t.size() < f[i][j].size()) {
                            f[i][j] = t;
                        }
                    }
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func encode(s string) string {
	n := len(s)
	f := make([][]string, n)
	for i := range f {
		f[i] = make([]string, n)
	}
	g := func(i, j int) string {
		t := s[i : j+1]
		if len(t) < 5 {
			return t
		}
		k := strings.Index((t + t)[1:], t) + 1
		if k < len(t) {
			cnt := len(t) / k
			return strconv.Itoa(cnt) + "[" + f[i][i+k-1] + "]"
		}
		return t
	}
	for i := n - 1; i >= 0; i-- {
		for j := i; j < n; j++ {
			f[i][j] = g(i, j)
			if j-i+1 > 4 {
				for k := i; k < j; k++ {
					t := f[i][k] + f[k+1][j]
					if len(t) < len(f[i][j]) {
						f[i][j] = t
					}
				}
			}
		}
	}
	return f[0][n-1]
}
```

#### TypeScript

```ts
function encode(s: string): string {
    const n = s.length;
    const f: string[][] = new Array(n).fill(0).map(() => new Array(n).fill(''));
    const g = (i: number, j: number): string => {
        const t = s.slice(i, j + 1);
        if (t.length < 5) {
            return t;
        }
        const k = t.repeat(2).indexOf(t, 1);
        if (k < t.length) {
            const cnt = Math.floor(t.length / k);
            return cnt + '[' + f[i][i + k - 1] + ']';
        }
        return t;
    };
    for (let i = n - 1; i >= 0; --i) {
        for (let j = i; j < n; ++j) {
            f[i][j] = g(i, j);
            if (j - i + 1 > 4) {
                for (let k = i; k < j; ++k) {
                    const t = f[i][k] + f[k + 1][j];
                    if (t.length < f[i][j].length) {
                        f[i][j] = t;
                    }
                }
            }
        }
    }
    return f[0][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
