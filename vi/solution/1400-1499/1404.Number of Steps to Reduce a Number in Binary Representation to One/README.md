---
comments: true
difficulty: Medium
rating: 1396
source: Weekly Contest 183 Q2
tags:
    - Bit Manipulation
    - String
    - Simulation
---

<!-- problem:start -->

# [1404. Number of Steps to Reduce a Number in Binary Representation to One](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-in-binary-representation-to-one)

[中文文档](/solution/1400-1499/1404.Number%20of%20Steps%20to%20Reduce%20a%20Number%20in%20Binary%20Representation%20to%20One/README.md)

## Mô tả

<!-- description:start -->

<p>Cho biểu diễn nhị phân của một số nguyên dưới dạng chuỗi <code>s</code>, hãy trả về <em>số bước cần thiết để giảm nó về </em><code>1</code><em> theo các quy tắc sau</em>:</p>

<ul>
	<li>
	<p>Nếu số hiện tại là số chẵn, bạn phải chia nó cho <code>2</code>.</p>
	</li>
	<li>
	<p>Nếu số hiện tại là số lẻ, bạn phải cộng thêm <code>1</code> vào nó.</p>
	</li>
</ul>

<p>Đề bài đảm bảo rằng với mọi test case, ta luôn có thể đưa số về 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1101&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> &quot;1101&quot; tương ứng với số 13 trong hệ thập phân.
Bước 1) 13 là số lẻ, cộng 1 và được 14.&nbsp;
Bước 2) 14 là số chẵn, chia cho 2 và được 7.
Bước 3) 7 là số lẻ, cộng 1 và được 8.
Bước 4) 8 là số chẵn, chia cho 2 và được 4.&nbsp;
Bước 5) 4 là số chẵn, chia cho 2 và được 2.&nbsp;
Bước 6) 2 là số chẵn, chia cho 2 và được 1.&nbsp;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> &quot;10&quot; tương ứng với số 2 trong hệ thập phân.
Bước 1) 2 là số chẵn, chia cho 2 và được 1.&nbsp;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1&quot;
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length&nbsp;&lt;= 500</code></li>
	<li><code>s</code> chỉ chứa các ký tự &#39;0&#39; hoặc &#39;1&#39;</li>
	<li><code>s[0] == &#39;1&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mô phỏng phép cộng 1 và dịch phải trên chuỗi 500 bit là đúng, nhưng nếu cộng một cách ngây thơ thì mỗi lần xử lý carry có thể phải quét lại toàn bộ chuỗi.
>
> Xét từ cuối chuỗi: một bit `0` tốn một lần dịch; một bit `1` phải được tăng lên để trở thành số chẵn rồi dịch, và carry có thể lan truyền. Một biến Boolean $\textit{carry}$ ghi lại phép cộng đang chờ xử lý mà không cần ghi đè `s`.
>
> Duyệt `s` từ phải sang trái (bỏ qua bit đầu tiên). Một bit `1` tốn hai bước và bật carry; một bit `0` tốn một bước. Nếu sau vòng lặp vẫn còn carry thì cần thêm một bước cho bit đầu tiên.

<!-- thinking:end -->

Ta mô phỏng các thao tác $1$ và $2$, đồng thời duy trì một carry $\textit{carry}$ để cho biết hiện có carry hay không. Ban đầu, $\textit{carry} = \text{false}$.

Ta duyệt chuỗi `s` từ cuối về đầu:

- Nếu $\textit{carry}$ là $\text{true}$, bit hiện tại `c` cần được tăng thêm `1`. Nếu `c` là `0`, sau khi cộng `1` nó trở thành `1` và $\textit{carry}$ trở thành $\text{false}$; nếu `c` là `1`, sau khi cộng `1` nó trở thành `0` và $\textit{carry}$ vẫn là $\text{true}$.
- Nếu `c` là `1`, ta cần thực hiện thao tác `1`, tức là cộng `1`, đồng thời đặt $\textit{carry}$ thành $\text{true}$.
- Lúc này `c` là `0`, nên ta cần thực hiện thao tác `2`, tức là chia cho `2`.

Sau khi duyệt xong, nếu $\textit{carry}$ vẫn là $\text{true}$, ta cần thực hiện thêm thao tác `1` một lần nữa.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi `s`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSteps(self, s: str) -> int:
        carry = False
        ans = 0
        for c in s[:0:-1]:
            if carry:
                if c == '0':
                    c = '1'
                    carry = False
                else:
                    c = '0'
            if c == '1':
                ans += 1
                carry = True
            ans += 1
        if carry:
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int numSteps(String s) {
        boolean carry = false;
        int ans = 0;
        for (int i = s.length() - 1; i > 0; --i) {
            char c = s.charAt(i);
            if (carry) {
                if (c == '0') {
                    c = '1';
                    carry = false;
                } else {
                    c = '0';
                }
            }
            if (c == '1') {
                ++ans;
                carry = true;
            }
            ++ans;
        }
        if (carry) {
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numSteps(string s) {
        int ans = 0;
        bool carry = false;
        for (int i = s.size() - 1; i; --i) {
            char c = s[i];
            if (carry) {
                if (c == '0') {
                    c = '1';
                    carry = false;
                } else
                    c = '0';
            }
            if (c == '1') {
                ++ans;
                carry = true;
            }
            ++ans;
        }
        if (carry) ++ans;
        return ans;
    }
};
```

#### Go

```go
func numSteps(s string) int {
	ans := 0
	carry := false
	for i := len(s) - 1; i > 0; i-- {
		c := s[i]
		if carry {
			if c == '0' {
				c = '1'
				carry = false
			} else {
				c = '0'
			}
		}
		if c == '1' {
			ans++
			carry = true
		}
		ans++
	}
	if carry {
		ans++
	}
	return ans
}
```

#### TypeScript

```ts
function numSteps(s: string): number {
    let ans = 0;
    let carry = false;

    for (let i = s.length - 1; i > 0; i--) {
        let c = s[i];

        if (carry) {
            if (c === '0') {
                c = '1';
                carry = false;
            } else {
                c = '0';
            }
        }

        if (c === '1') {
            ans++;
            carry = true;
        }

        ans++;
    }

    if (carry) {
        ans++;
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_steps(s: String) -> i32 {
        let bytes = s.as_bytes();
        let mut ans: i32 = 0;
        let mut carry = false;

        for i in (1..bytes.len()).rev() {
            let mut c = bytes[i];

            if carry {
                if c == b'0' {
                    c = b'1';
                    carry = false;
                } else {
                    c = b'0';
                }
            }

            if c == b'1' {
                ans += 1;
                carry = true;
            }

            ans += 1;
        }

        if carry {
            ans += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
