---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [868. Binary Gap](https://leetcode.com/problems/binary-gap)

[中文文档](/solution/0800-0899/0868.Binary%20Gap/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương <code>n</code>, hãy tìm và trả về <em><strong>khoảng cách lớn nhất</strong> giữa hai bit <code>1</code> <strong>liên tiếp theo thứ tự xuất hiện</strong> trong biểu diễn nhị phân của </em><code>n</code><em>. Nếu không có hai bit <code>1</code> liền kề, trả về </em><code>0</code><em>.</em></p>

<p>Hai bit <code>1</code> được xem là <strong>liên tiếp theo thứ tự xuất hiện</strong> nếu giữa chúng chỉ có các bit <code>0</code> (có thể không có bit <code>0</code> nào). <b>Khoảng cách</b> giữa hai bit <code>1</code> là độ chênh lệch tuyệt đối giữa vị trí bit của chúng. Ví dụ, hai bit <code>1</code> trong <code>&quot;1001&quot;</code> cách nhau 3.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 22
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 22 ở dạng nhị phân là &quot;10110&quot;.
Cặp bit 1 liên tiếp theo thứ tự xuất hiện đầu tiên là &quot;<u>1</u>0<u>1</u>10&quot;, cách nhau 2.
Cặp bit 1 liên tiếp theo thứ tự xuất hiện thứ hai là &quot;10<u>11</u>0&quot;, cách nhau 1.
Đáp án là khoảng cách lớn hơn trong hai khoảng cách này, tức 2.
Lưu ý, &quot;<u>1</u>01<u>1</u>0&quot; không phải cặp hợp lệ vì có một bit 1 nằm giữa hai bit 1 được gạch dưới.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 8
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> 8 ở dạng nhị phân là &quot;1000&quot;.
Không có cặp bit 1 liên tiếp theo thứ tự xuất hiện nào trong biểu diễn nhị phân của 8, nên ta trả về 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 5 ở dạng nhị phân là &quot;101&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm khoảng cách lớn nhất giữa hai bit 1 liên tiếp theo thứ tự xuất hiện trong biểu diễn nhị phân. Vì $n\le 10^9$, chỉ cần duyệt các bit, không cần chuyển sang chuỗi.
>
> Lưu vị trí của bit 1 trước đó và cập nhật khoảng cách khi gặp bit 1 tiếp theo. Nếu có ít hơn hai bit 1 thì đáp án vẫn là $0$.

<!-- thinking:end -->

Ta dùng hai biến $\textit{pre}$ và $\textit{cur}$ lần lượt biểu diễn vị trí bit 1 trước đó và vị trí hiện tại. Ban đầu, $\textit{pre} = 100$ và $\textit{cur} = 0$. Sau đó, duyệt biểu diễn nhị phân của $n$. Mỗi khi gặp bit 1, tính khoảng cách giữa vị trí hiện tại và vị trí bit 1 trước đó rồi cập nhật đáp án.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số nguyên đầu vào. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def binaryGap(self, n: int) -> int:
        ans = 0
        pre, cur = inf, 0
        while n:
            if n & 1:
                ans = max(ans, cur - pre)
                pre = cur
            cur += 1
            n >>= 1
        return ans
```

#### Java

```java
class Solution {
    public int binaryGap(int n) {
        int ans = 0;
        for (int pre = 100, cur = 0; n != 0; n >>= 1) {
            if (n % 2 == 1) {
                ans = Math.max(ans, cur - pre);
                pre = cur;
            }
            ++cur;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int binaryGap(int n) {
        int ans = 0;
        for (int pre = 100, cur = 0; n != 0; n >>= 1) {
            if (n & 1) {
                ans = max(ans, cur - pre);
                pre = cur;
            }
            ++cur;
        }
        return ans;
    }
};
```

#### Go

```go
func binaryGap(n int) (ans int) {
	for pre, cur := 100, 0; n != 0; n >>= 1 {
		if n&1 == 1 {
			ans = max(ans, cur-pre)
			pre = cur
		}
		cur++
	}
	return
}
```

#### TypeScript

```ts
function binaryGap(n: number): number {
    let ans = 0;
    for (let pre = 100, cur = 0; n; n >>= 1) {
        if (n & 1) {
            ans = Math.max(ans, cur - pre);
            pre = cur;
        }
        ++cur;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn binary_gap(mut n: i32) -> i32 {
        let mut ans = 0;
        let mut pre = 100;
        let mut cur = 0;
        while n != 0 {
            if n % 2 == 1 {
                ans = ans.max(cur - pre);
                pre = cur;
            }
            cur += 1;
            n >>= 1;
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var binaryGap = function (n) {
    let ans = 0;
    for (let pre = 100, cur = 0; n; n >>= 1) {
        if (n & 1) {
            ans = Math.max(ans, cur - pre);
            pre = cur;
        }
        ++cur;
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int BinaryGap(int n) {
        int ans = 0;
        for (int pre = 100, cur = 0; n != 0; n >>= 1) {
            if (n % 2 == 1) {
                ans = Math.Max(ans, cur - pre);
                pre = cur;
            }
            ++cur;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
