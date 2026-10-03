---
comments: true
difficulty: Medium
rating: 1355
source: Weekly Contest 310 Q2
tags:
    - Greedy
    - Hash Table
    - String
---

<!-- problem:start -->

# [2405. Optimal Partition of String](https://leetcode.com/problems/optimal-partition-of-string)

[Tài liệu tiếng Trung](/solution/2400-2499/2405.Optimal%20Partition%20of%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, hãy phân chia chuỗi thành một hoặc nhiều <strong>chuỗi con</strong> sao cho các ký tự trong mỗi chuỗi con là <strong>duy nhất</strong>. Nghĩa là, không có chữ cái nào xuất hiện trong cùng một chuỗi con quá <strong>một lần</strong>.</p>

<p>Trả về <em>số lượng chuỗi con <strong>nhỏ nhất</strong> trong cách phân chia đó.</em></p>

<p>Lưu ý rằng mỗi ký tự phải thuộc đúng một chuỗi con trong cách phân chia.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abacaba&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:
</strong>Có hai cách phân chia khả dĩ là (&quot;a&quot;,&quot;ba&quot;,&quot;cab&quot;,&quot;a&quot;) và (&quot;ab&quot;,&quot;a&quot;,&quot;ca&quot;,&quot;ba&quot;).
Có thể chứng minh rằng 4 là số lượng chuỗi con nhỏ nhất cần thiết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ssssss&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:
</strong>Cách phân chia hợp lệ duy nhất là (&quot;s&quot;,&quot;s&quot;,&quot;s&quot;,&quot;s&quot;,&quot;s&quot;,&quot;s&quot;).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Số cách phân chia tăng theo cấp số mũ; $n\le 10^5$ khiến việc tìm kiếm không khả thi. Muốn số phần nhỏ nhất với các ký tự phân biệt trong mỗi phần, mỗi phần phải dài nhất có thể: chỉ cắt khi ký tự tiếp theo bị lặp.
>
> Hai mươi sáu chữ cái viết thường có thể được lưu trong một số nguyên $\textit{mask}$. Khi xảy ra xung đột, tăng số phần lên 1 và xóa mask, sau đó thêm ký tự hiện tại. Chỉ cần duyệt qua chuỗi một lần.

<!-- thinking:end -->

Theo mô tả bài toán, mỗi chuỗi con nên dài nhất có thể và chứa các ký tự duy nhất. Do đó, chúng ta có thể tham lam phân chia chuỗi.

Chúng ta định nghĩa một số nguyên nhị phân $\textit{mask}$ để ghi nhận các ký tự đã xuất hiện trong chuỗi con hiện tại. Bit thứ $i$ của $\textit{mask}$ bằng $1$ cho biết chữ cái thứ $i$ đã xuất hiện, còn bằng $0$ cho biết chữ cái đó chưa xuất hiện. Ngoài ra, chúng ta cần một biến $\textit{ans}$ để ghi nhận số lượng chuỗi con, ban đầu đặt $\textit{ans} = 1$.

Duyệt qua từng ký tự trong chuỗi $s$. Với mỗi ký tự $c$, chuyển nó thành một số nguyên $x$ trong khoảng từ $0$ đến $25$, sau đó kiểm tra xem bit thứ $x$ của $\textit{mask}$ có bằng $1$ hay không. Nếu bằng $1$, điều đó có nghĩa là ký tự hiện tại $c$ bị trùng trong chuỗi con hiện tại. Khi đó, tăng $\textit{ans}$ thêm $1$ và đặt lại $\textit{mask}$ về $0$. Ngược lại, đặt bit thứ $x$ của $\textit{mask}$ thành $1$. Sau đó, cập nhật $\textit{mask}$ thành kết quả OR bit của $\textit{mask}$ và $2^x$.

Cuối cùng, trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def partitionString(self, s: str) -> int:
        ans, mask = 1, 0
        for x in map(lambda c: ord(c) - ord("a"), s):
            if mask >> x & 1:
                ans += 1
                mask = 0
            mask |= 1 << x
        return ans
```

#### Java

```java
class Solution {
    public int partitionString(String s) {
        int ans = 1, mask = 0;
        for (int i = 0; i < s.length(); ++i) {
            int x = s.charAt(i) - 'a';
            if ((mask >> x & 1) == 1) {
                ++ans;
                mask = 0;
            }
            mask |= 1 << x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int partitionString(string s) {
        int ans = 1, mask = 0;
        for (char& c : s) {
            int x = c - 'a';
            if (mask >> x & 1) {
                ++ans;
                mask = 0;
            }
            mask |= 1 << x;
        }
        return ans;
    }
};
```

#### Go

```go
func partitionString(s string) int {
	ans, mask := 1, 0
	for _, c := range s {
		x := int(c - 'a')
		if mask>>x&1 == 1 {
			ans++
			mask = 0
		}
		mask |= 1 << x
	}
	return ans
}
```

#### TypeScript

```ts
function partitionString(s: string): number {
    let [ans, mask] = [1, 0];
    for (const c of s) {
        const x = c.charCodeAt(0) - 97;
        if ((mask >> x) & 1) {
            ++ans;
            mask = 0;
        }
        mask |= 1 << x;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn partition_string(s: String) -> i32 {
        let mut ans = 1;
        let mut mask = 0;
        for x in s.chars().map(|c| (c as u8 - b'a') as u32) {
            if mask >> x & 1 == 1 {
                ans += 1;
                mask = 0;
            }
            mask |= 1 << x;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
