---
comments: true
difficulty: Easy
rating: 1234
source: Weekly Contest 128 Q1
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [1009. Complement of Base 10 Integer](https://leetcode.com/problems/complement-of-base-10-integer)

[中文文档](/solution/1000-1099/1009.Complement%20of%20Base%2010%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số bù</strong> của một số nguyên là số thu được bằng cách đổi tất cả bit <code>0</code> thành <code>1</code> và tất cả bit <code>1</code> thành <code>0</code> trong biểu diễn nhị phân của số đó.</p>

<ul>
	<li>Ví dụ, số nguyên <code>5</code> có biểu diễn nhị phân là <code>&quot;101&quot;</code>; <strong>số bù</strong> của nó là <code>&quot;010&quot;</code>, tức số nguyên <code>2</code>.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số bù của nó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 5 có biểu diễn nhị phân là &quot;101&quot;; số bù của nó là &quot;010&quot; trong hệ nhị phân, tương ứng với 2 trong hệ thập phân.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> 7 có biểu diễn nhị phân là &quot;111&quot;; số bù của nó là &quot;000&quot; trong hệ nhị phân, tương ứng với 0 trong hệ thập phân.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> 10 có biểu diễn nhị phân là &quot;1010&quot;; số bù của nó là &quot;0101&quot; trong hệ nhị phân, tương ứng với 5 trong hệ thập phân.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt; 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống bài 476: <a href="https://leetcode.com/problems/number-complement/" target="_blank">https://leetcode.com/problems/number-complement/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Có thể viết biểu diễn nhị phân rồi đảo các bit để tìm số bù, nhưng $n$ có thể lên tới $10^9$, không được đảo các số 0 đứng đầu, và theo định nghĩa số bù của $n=0$ là $1$.
>
> Duyệt từ bit thấp lên cao chỉ đảo những bit thực sự xuất hiện trong $n$. Khi $n$ trở thành $0$ thì dừng, nhờ đó các bit 0 ở phía trên không bị thay đổi.
>
> Chỉ số $i$ đánh dấu bit hiện tại; ta OR bit thấp nhất sau khi đảo của $n$ vào $\textit{ans}$, rồi dịch phải $n$ cho đến khi nó bằng 0.

<!-- thinking:end -->

Trước tiên, kiểm tra xem $n$ có bằng $0$ không. Nếu có, trả về $1$.

Tiếp theo, khởi tạo hai biến $\textit{ans}$ và $i$ bằng $0$, rồi duyệt các bit của $n$. Ở mỗi lượt, đặt bit thứ $i$ của $\textit{ans}$ thành giá trị đảo của bit thứ $i$ trong $n$, tăng $i$ thêm $1$ và dịch phải $n$ một bit.

Cuối cùng, trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số thập phân được cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bitwiseComplement(self, n: int) -> int:
        if n == 0:
            return 1
        ans = i = 0
        while n:
            ans |= ((n & 1 ^ 1) << i)
            i += 1
            n >>= 1
        return ans
```

#### Java

```java
class Solution {
    public int bitwiseComplement(int n) {
        if (n == 0) {
            return 1;
        }
        int ans = 0, i = 0;
        while (n != 0) {
            ans |= (n & 1 ^ 1) << (i++);
            n >>= 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int bitwiseComplement(int n) {
        if (n == 0) {
            return 1;
        }
        int ans = 0, i = 0;
        while (n != 0) {
            ans |= (n & 1 ^ 1) << (i++);
            n >>= 1;
        }
        return ans;
    }
};
```

#### Go

```go
func bitwiseComplement(n int) (ans int) {
	if n == 0 {
		return 1
	}
	for i := 0; n != 0; n >>= 1 {
		ans |= (n&1 ^ 1) << i
		i++
	}
	return
}
```

#### TypeScript

```ts
function bitwiseComplement(n: number): number {
    if (n === 0) {
        return 1;
    }
    let ans = 0;
    for (let i = 0; n; n >>= 1) {
        ans |= ((n & 1) ^ 1) << i++;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn bitwise_complement(mut n: i32) -> i32 {
        if n == 0 {
            return 1;
        }
        let mut ans = 0;
        let mut i = 0;
        while n != 0 {
            ans |= ((n & 1) ^ 1) << i;
            n >>= 1;
            i += 1;
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int BitwiseComplement(int n) {
        if (n == 0) {
            return 1;
        }
        int ans = 0, i = 0;
        while (n != 0) {
            ans |= ((n & 1) ^ 1) << (i++);
            n >>= 1;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
