---
comments: true
difficulty: Medium
rating: 1697
source: Weekly Contest 130 Q2
tags:
    - Math
---

<!-- problem:start -->

# [1017. Convert to Base -2](https://leetcode.com/problems/convert-to-base-2)

[中文文档](/solution/1000-1099/1017.Convert%20to%20Base%20-2/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về <em>chuỗi nhị phân biểu diễn</em> <code>n</code> trong hệ cơ số <code>-2</code>.</p>

<p><strong>Lưu ý</strong> chuỗi trả về không được có các số 0 ở đầu, trừ khi chuỗi là <code>&quot;0&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> &quot;110&quot;
<strong>Giải thích:</strong> (-2)<sup>2</sup> + (-2)<sup>1</sup> = 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> &quot;111&quot;
<strong>Giải thích:</strong> (-2)<sup>2</sup> + (-2)<sup>1</sup> + (-2)<sup>0</sup> = 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> &quot;100&quot;
<strong>Giải thích:</strong> (-2)<sup>2</sup> = 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cách chuyển đổi thông thường sang cơ số dương sẽ cho số dư âm khi chia cho $-2$, nên không thể áp dụng nguyên xi. Vì $n\le 10^9$, biểu diễn chỉ có $O(\log n)$ bit, do đó mô phỏng từng chữ số là đủ.
>
> Bit thấp nhất là $n\bmod 2$. Nếu bit này bằng $1$, ta trừ đi giá trị vị trí hiện tại $k$ (là một lũy thừa của $-1$ ở các vị trí lẻ) để phần còn lại là số chẵn, sau đó chia cho $2$ và đổi dấu $k$.
>
> Ta thu thập các bit từ thấp đến cao rồi đảo ngược thứ tự. Số $0$ được biểu diễn thành $\texttt{0}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def baseNeg2(self, n: int) -> str:
        k = 1
        ans = []
        while n:
            if n % 2:
                ans.append('1')
                n -= k
            else:
                ans.append('0')
            n //= 2
            k *= -1
        return ''.join(ans[::-1]) or '0'
```

#### Java

```java
class Solution {
    public String baseNeg2(int n) {
        if (n == 0) {
            return "0";
        }
        int k = 1;
        StringBuilder ans = new StringBuilder();
        while (n != 0) {
            if (n % 2 != 0) {
                ans.append(1);
                n -= k;
            } else {
                ans.append(0);
            }
            k *= -1;
            n /= 2;
        }
        return ans.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string baseNeg2(int n) {
        if (n == 0) {
            return "0";
        }
        int k = 1;
        string ans;
        while (n) {
            if (n % 2) {
                ans.push_back('1');
                n -= k;
            } else {
                ans.push_back('0');
            }
            k *= -1;
            n /= 2;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func baseNeg2(n int) string {
	if n == 0 {
		return "0"
	}
	ans := []byte{}
	k := 1
	for n != 0 {
		if n%2 != 0 {
			ans = append(ans, '1')
			n -= k
		} else {
			ans = append(ans, '0')
		}
		k *= -1
		n /= 2
	}
	for i, j := 0, len(ans)-1; i < j; i, j = i+1, j-1 {
		ans[i], ans[j] = ans[j], ans[i]
	}
	return string(ans)
}
```

#### TypeScript

```ts
function baseNeg2(n: number): string {
    if (n === 0) {
        return '0';
    }
    let k = 1;
    const ans: string[] = [];
    while (n) {
        if (n % 2) {
            ans.push('1');
            n -= k;
        } else {
            ans.push('0');
        }
        k *= -1;
        n /= 2;
    }
    return ans.reverse().join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn base_neg2(n: i32) -> String {
        if n == 0 {
            return "0".to_string();
        }
        let mut k = 1;
        let mut ans = String::new();
        let mut num = n;
        while num != 0 {
            if num % 2 != 0 {
                ans.push('1');
                num -= k;
            } else {
                ans.push('0');
            }
            k *= -1;
            num /= 2;
        }
        ans.chars().rev().collect::<String>()
    }
}
```

#### C#

```cs
public class Solution {
    public string BaseNeg2(int n) {
        if (n == 0) {
            return "0";
        }
        int k = 1;
        StringBuilder ans = new StringBuilder();
        int num = n;
        while (num != 0) {
            if (num % 2 != 0) {
                ans.Append('1');
                num -= k;
            } else {
                ans.Append('0');
            }
            k *= -1;
            num /= 2;
        }
        char[] cs = ans.ToString().ToCharArray();
        Array.Reverse(cs);
        return new string(cs);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
