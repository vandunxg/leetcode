---
comments: true
difficulty: Medium
rating: 1381
source: Weekly Contest 243 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1881. Maximum Value after Insertion](https://leetcode.com/problems/maximum-value-after-insertion)

[中文文档](/solution/1800-1899/1881.Maximum%20Value%20after%20Insertion/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên rất lớn <code>n</code>, được biểu diễn dưới dạng chuỗi,​​​​​​ và một chữ số nguyên <code>x</code>. Các chữ số trong <code>n</code> và chữ số <code>x</code> nằm trong đoạn <strong>bao gồm cả hai đầu mút</strong> <code>[1, 9]</code>, và <code>n</code> có thể là một số <b>âm</b>.</p>

<p>Mục tiêu là <strong>tối đa hóa </strong><code>n</code><strong> về mặt giá trị số</strong> bằng cách chèn <code>x</code> vào bất kỳ vị trí nào trong biểu diễn thập phân của <code>n</code>​​​​​​. <strong>Không được</strong> chèn <code>x</code> vào bên trái dấu âm.</p>

<ul>
	<li>Ví dụ, nếu <code>n = 73</code> và <code>x = 6</code>, vị trí tốt nhất là giữa <code>7</code> và <code>3</code>, tạo thành <code>n = 763</code>.</li>
	<li>Nếu <code>n = -55</code> và <code>x = 2</code>, vị trí tốt nhất là trước chữ số <code>5</code> đầu tiên, tạo thành <code>n = -255</code>.</li>
</ul>

<p>Trả về <em>một chuỗi biểu diễn <strong>giá trị lớn nhất</strong> của </em><code>n</code><em>​​​​​​ sau khi chèn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = &quot;99&quot;, x = 9
<strong>Đầu ra:</strong> &quot;999&quot;
<strong>Giải thích:</strong> Kết quả giống nhau bất kể chèn 9 ở đâu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = &quot;-13&quot;, x = 2
<strong>Đầu ra:</strong> &quot;-123&quot;
<strong>Giải thích:</strong> Ta có thể tạo ra một trong các số {-213, -123, -132}, và số lớn nhất trong ba số đó là -123.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= x &lt;= 9</code></li>
	<li>Các chữ số trong <code>n</code>​​​ nằm trong đoạn <code>[1, 9]</code>.</li>
	<li><code>n</code> là biểu diễn hợp lệ của một số nguyên.</li>
	<li>Trong trường hợp <code>n</code> âm,​​​​​​ chuỗi sẽ bắt đầu bằng <code>&#39;-&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Chèn chữ số $x$ vào chuỗi thập phân $n$ để tối đa hóa giá trị của nó. Với số dương, ta muốn chữ số lớn hơn xuất hiện càng sớm càng tốt; với số âm thì ngược lại.
>
> Với $n$ dương, chèn trước chữ số đầu tiên nhỏ hơn $x$; với $n$ âm, bỏ qua dấu rồi chèn trước chữ số đầu tiên lớn hơn $x$.

<!-- thinking:end -->

Nếu $n$ âm, ta cần tìm vị trí đầu tiên có chữ số lớn hơn $x$ rồi chèn $x$ vào vị trí đó. Nếu $n$ dương, ta cần tìm vị trí đầu tiên có chữ số nhỏ hơn $x$ rồi chèn $x$ vào vị trí đó.

Độ phức tạp thời gian là $O(m)$, trong đó $m$ là độ dài của $n$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValue(self, n: str, x: int) -> str:
        i = 0
        if n[0] == "-":
            i += 1
            while i < len(n) and int(n[i]) <= x:
                i += 1
        else:
            while i < len(n) and int(n[i]) >= x:
                i += 1
        return n[:i] + str(x) + n[i:]
```

#### Java

```java
class Solution {
    public String maxValue(String n, int x) {
        int i = 0;
        if (n.charAt(0) == '-') {
            ++i;
            while (i < n.length() && n.charAt(i) - '0' <= x) {
                ++i;
            }
        } else {
            while (i < n.length() && n.charAt(i) - '0' >= x) {
                ++i;
            }
        }
        return n.substring(0, i) + x + n.substring(i);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string maxValue(string n, int x) {
        int i = 0;
        if (n[0] == '-') {
            ++i;
            while (i < n.size() && n[i] - '0' <= x) {
                ++i;
            }
        } else {
            while (i < n.size() && n[i] - '0' >= x) {
                ++i;
            }
        }
        n.insert(i, 1, x + '0');
        return n;
    }
};
```

#### Go

```go
func maxValue(n string, x int) string {
	i := 0
	y := byte('0' + x)
	if n[0] == '-' {
		i++
		for i < len(n) && n[i] <= y {
			i++
		}
	} else {
		for i < len(n) && n[i] >= y {
			i++
		}
	}
	return n[:i] + string(y) + n[i:]
}
```

#### TypeScript

```ts
function maxValue(n: string, x: number): string {
    let i = 0;
    if (n[0] === '-') {
        i++;
        while (i < n.length && +n[i] <= x) {
            i++;
        }
    } else {
        while (i < n.length && +n[i] >= x) {
            i++;
        }
    }
    return n.slice(0, i) + x + n.slice(i);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_value(n: String, x: i32) -> String {
        let s = n.as_bytes();
        let mut i = 0;
        if n.starts_with('-') {
            i += 1;
            while i < s.len() && (s[i] - b'0') as i32 <= x {
                i += 1;
            }
        } else {
            while i < s.len() && (s[i] - b'0') as i32 >= x {
                i += 1;
            }
        }
        let mut ans = String::new();
        ans.push_str(&n[0..i]);
        ans.push_str(&x.to_string());
        ans.push_str(&n[i..]);
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} n
 * @param {number} x
 * @return {string}
 */
var maxValue = function (n, x) {
    let i = 0;
    if (n[0] === '-') {
        i++;
        while (i < n.length && +n[i] <= x) {
            i++;
        }
    } else {
        while (i < n.length && +n[i] >= x) {
            i++;
        }
    }
    return n.slice(0, i) + x + n.slice(i);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
