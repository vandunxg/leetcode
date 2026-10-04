---
comments: true
difficulty: Hard
rating: 2232
source: Weekly Contest 417 Q4
tags:
    - Bit Manipulation
    - Recursion
    - Math
---

<!-- problem:start -->

# [3307. Find the K-th Character in String Game II](https://leetcode.com/problems/find-the-k-th-character-in-string-game-ii)

[中文文档](/solution/3300-3399/3307.Find%20the%20K-th%20Character%20in%20String%20Game%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob đang chơi một trò chơi. Ban đầu, Alice có chuỗi <code>word = &quot;a&quot;</code>.</p>

<p>Bạn được cho một số nguyên <strong>dương</strong> <code>k</code>. Bạn cũng được cho một mảng số nguyên <code>operations</code>, trong đó <code>operations[i]</code> biểu thị <strong>loại</strong> của phép toán thứ <code>i<sup>th</sup></code>.</p>

<p>Sau đó Bob sẽ yêu cầu Alice thực hiện <strong>tất cả</strong> các phép toán theo thứ tự:</p>

<ul>
	<li>Nếu <code>operations[i] == 0</code>, <strong>nối thêm</strong> một bản sao của <code>word</code> vào chính nó.</li>
	<li>Nếu <code>operations[i] == 1</code>, tạo một chuỗi mới bằng cách <strong>đổi</strong> mỗi ký tự trong <code>word</code> thành ký tự <strong>tiếp theo</strong> trong bảng chữ cái tiếng Anh, rồi <strong>nối</strong> chuỗi đó vào <em>chuỗi</em> <code>word</code> ban đầu. Ví dụ, thực hiện phép toán trên <code>&quot;c&quot;</code> tạo ra <code>&quot;cd&quot;</code>, còn thực hiện trên <code>&quot;zb&quot;</code> tạo ra <code>&quot;zbac&quot;</code>.</li>
</ul>

<p>Trả về giá trị của ký tự thứ <code>k<sup>th</sup></code> trong <code>word</code> sau khi thực hiện tất cả các phép toán.</p>

<p><strong>Lưu ý</strong> rằng ký tự <code>&#39;z&#39;</code> có thể được đổi thành <code>&#39;a&#39;</code> trong phép toán loại thứ hai.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 5, operations = [0,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;a&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, <code>word == &quot;a&quot;</code>. Alice thực hiện ba phép toán như sau:</p>

<ul>
	<li>Nối thêm <code>&quot;a&quot;</code> vào <code>&quot;a&quot;</code>, <code>word</code> trở thành <code>&quot;aa&quot;</code>.</li>
	<li>Nối thêm <code>&quot;aa&quot;</code> vào <code>&quot;aa&quot;</code>, <code>word</code> trở thành <code>&quot;aaaa&quot;</code>.</li>
	<li>Nối thêm <code>&quot;aaaa&quot;</code> vào <code>&quot;aaaa&quot;</code>, <code>word</code> trở thành <code>&quot;aaaaaaaa&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 10, operations = [0,1,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;b&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, <code>word == &quot;a&quot;</code>. Alice thực hiện bốn phép toán như sau:</p>

<ul>
	<li>Nối thêm <code>&quot;a&quot;</code> vào <code>&quot;a&quot;</code>, <code>word</code> trở thành <code>&quot;aa&quot;</code>.</li>
	<li>Nối thêm <code>&quot;bb&quot;</code> vào <code>&quot;aa&quot;</code>, <code>word</code> trở thành <code>&quot;aabb&quot;</code>.</li>
	<li>Nối thêm <code>&quot;aabb&quot;</code> vào <code>&quot;aabb&quot;</code>, <code>word</code> trở thành <code>&quot;aabbaabb&quot;</code>.</li>
	<li>Nối thêm <code>&quot;bbccbbcc&quot;</code> vào <code>&quot;aabbaabb&quot;</code>, <code>word</code> trở thành <code>&quot;aabbaabbbbccbbcc&quot;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 10<sup>14</sup></code></li>
	<li><code>1 &lt;= operations.length &lt;= 100</code></li>
	<li><code>operations[i]</code> chỉ có thể là 0 hoặc 1.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>word</code> có <strong>ít nhất</strong> <code>k</code> ký tự sau khi thực hiện tất cả các phép toán.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Công thức truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> Với $k \le 10^{14}$, chúng ta không thể xây dựng chuỗi như ở phần I. Mỗi phép toán làm độ dài tăng gấp đôi, nên ký tự thứ $k$ được xác định bởi một chuỗi quyết định "dịch hoặc không dịch".
>
> Tìm độ dài đầu tiên $n=2^i$ không nhỏ hơn $k$, sau đó duyệt ngược các phép toán. Nếu $k$ nằm ở nửa sau, nó tương ứng với chỉ số ở nửa đầu; ta cộng $1$ khi $\textit{operations}[i-1]=1$, rồi đưa $k$ về nửa đầu.
>
> Khi độ dài còn $1$, phần dịch tích lũy modulo $26$ chính là chữ cái. Quá trình này thực hiện trong $O(\log k)$ bước.

<!-- thinking:end -->

Vì độ dài chuỗi tăng gấp đôi sau mỗi phép toán, nếu thực hiện $i$ phép toán thì độ dài chuỗi sẽ là $2^i$.

Chúng ta có thể mô phỏng quá trình này để tìm độ dài chuỗi đầu tiên $n$ lớn hơn hoặc bằng $k$.

Tiếp theo, chúng ta truy ngược và xét các trường hợp sau:

- Nếu $k \gt n / 2$, điều đó có nghĩa là $k$ nằm ở nửa sau. Nếu $\textit{operations}[i - 1] = 1$, ký tự ở vị trí $k$ thu được bằng cách cộng $1$ vào ký tự ở nửa đầu. Ta cộng $1$ vào ký tự đó, sau đó cập nhật $k$ thành $k - n / 2$.
- Nếu $k \le n / 2$, điều đó có nghĩa là $k$ nằm ở nửa đầu và không bị ảnh hưởng bởi $\textit{operations}[i - 1]$.
- Tiếp theo, cập nhật $n$ thành $n / 2$ và tiếp tục truy ngược cho đến khi $n = 1$.

Cuối cùng, lấy số thu được modulo $26$ rồi cộng mã ASCII của `'a'` để nhận được đáp án.

Độ phức tạp thời gian là $O(\log k)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthCharacter(self, k: int, operations: List[int]) -> str:
        n, i = 1, 0
        while n < k:
            n *= 2
            i += 1
        d = 0
        while n > 1:
            if k > n // 2:
                k -= n // 2
                d += operations[i - 1]
            n //= 2
            i -= 1
        return chr(d % 26 + ord("a"))
```

#### Java

```java
class Solution {
    public char kthCharacter(long k, int[] operations) {
        long n = 1;
        int i = 0;
        while (n < k) {
            n *= 2;
            ++i;
        }
        int d = 0;
        while (n > 1) {
            if (k > n / 2) {
                k -= n / 2;
                d += operations[i - 1];
            }
            n /= 2;
            --i;
        }
        return (char) ('a' + (d % 26));
    }
}
```

#### C++

```cpp
class Solution {
public:
    char kthCharacter(long long k, vector<int>& operations) {
        long long n = 1;
        int i = 0;
        while (n < k) {
            n *= 2;
            ++i;
        }
        int d = 0;
        while (n > 1) {
            if (k > n / 2) {
                k -= n / 2;
                d += operations[i - 1];
            }
            n /= 2;
            --i;
        }
        return 'a' + (d % 26);
    }
};
```

#### Go

```go
func kthCharacter(k int64, operations []int) byte {
	n := int64(1)
	i := 0
	for n < k {
		n *= 2
		i++
	}
	d := 0
	for n > 1 {
		if k > n/2 {
			k -= n / 2
			d += operations[i-1]
		}
		n /= 2
		i--
	}
	return byte('a' + (d % 26))
}
```

#### TypeScript

```ts
function kthCharacter(k: number, operations: number[]): string {
    let n = 1;
    let i = 0;
    while (n < k) {
        n *= 2;
        i++;
    }
    let d = 0;
    while (n > 1) {
        if (k > n / 2) {
            k -= n / 2;
            d += operations[i - 1];
        }
        n /= 2;
        i--;
    }
    return String.fromCharCode('a'.charCodeAt(0) + (d % 26));
}
```

#### Rust

```rust
impl Solution {
    pub fn kth_character(mut k: i64, operations: Vec<i32>) -> char {
        let mut n = 1i64;
        let mut i = 0;
        while n < k {
            n *= 2;
            i += 1;
        }
        let mut d = 0;
        while n > 1 {
            if k > n / 2 {
                k -= n / 2;
                d += operations[i - 1] as i64;
            }
            n /= 2;
            i -= 1;
        }
        ((b'a' + (d % 26) as u8) as char)
    }
}
```

#### C#

```cs
public class Solution {
    public char KthCharacter(long k, int[] operations) {
        long n = 1;
        int i = 0;
        while (n < k) {
            n *= 2;
            ++i;
        }
        int d = 0;
        while (n > 1) {
            if (k > n / 2) {
                k -= n / 2;
                d += operations[i - 1];
            }
            n /= 2;
            --i;
        }
        return (char)('a' + (d % 26));
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $k
     * @param Integer[] $operations
     * @return String
     */
    function kthCharacter($k, $operations) {
        $n = 1;
        $i = 0;
        while ($n < $k) {
            $n *= 2;
            ++$i;
        }
        $d = 0;
        while ($n > 1) {
            if ($k > $n / 2) {
                $k -= $n / 2;
                $d += $operations[$i - 1];
            }
            $n /= 2;
            --$i;
        }
        return chr(ord('a') + ($d % 26));
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
