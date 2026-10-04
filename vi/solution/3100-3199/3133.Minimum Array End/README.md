---
comments: true
difficulty: Medium
rating: 1934
source: Weekly Contest 395 Q3
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [3133. Minimum Array End](https://leetcode.com/problems/minimum-array-end)

[中文文档](/solution/3100-3199/3133.Minimum%20Array%20End/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>n</code> và <code>x</code>. Hãy xây dựng một mảng gồm các số nguyên <strong>dương</strong> <code>nums</code> có kích thước <code>n</code> sao cho với mọi <code>0 &lt;= i &lt; n - 1</code>, <code>nums[i + 1]</code> <strong>lớn hơn</strong> <code>nums[i]</code>, và kết quả của phép <code>AND</code> bit giữa tất cả phần tử của <code>nums</code> bằng <code>x</code>.</p>

<p>Trả về giá trị <strong>nhỏ nhất</strong> có thể có của <code>nums[n - 1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, x = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> có thể là <code>[4,5,6]</code> và phần tử cuối cùng của nó là 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, x = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> có thể là <code>[7,15]</code> và phần tử cuối cùng của nó là 15.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, x &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Xây dựng một mảng tăng nghiêm ngặt có độ dài-$n$ với AND bằng $x$ và giá trị cuối nhỏ nhất. Việc thử lần lượt các ứng viên ngay sau $x$ sẽ quá chậm khi $n$ lớn.
>
> Giá trị đầu tiên phải là $x$. Các giá trị sau chỉ có thể bật những bit đang là 0 của $x$, nếu không phép AND sẽ làm mất các bit. Những bit tự do đó, khi đọc như một bộ đếm nhị phân, chính là $0,1,\ldots,n-1$.
>
> Viết các bit của $n-1$ vào những vị trí bit 0 của $x$ từ thấp đến cao, rồi đặt phần còn lại vào bit $31$ trở lên. Kết quả là phần tử cuối nhỏ nhất.

<!-- thinking:end -->

Theo mô tả đề bài, để phần tử cuối cùng của mảng nhỏ nhất và kết quả phép AND bit của các phần tử trong mảng bằng $x$, phần tử đầu tiên của mảng phải là $x$.

Giả sử biểu diễn nhị phân của $x$ là $\underline{1}00\underline{1}00$, khi đó dãy mảng là $\underline{1}00\underline{1}00$, $\underline{1}00\underline{1}01$, $\underline{1}00\underline{1}10$, $\underline{1}00\underline{1}11$...

Nếu bỏ qua phần được gạch chân, dãy mảng là $0000$, $0001$, $0010$, $0011$..., phần tử đầu tiên là $0$, còn phần tử thứ $n$ là $n-1$.

Do đó, đáp án là điền từng bit trong biểu diễn nhị phân của $n-1$ vào các bit $0$ trong biểu diễn nhị phân của $x$ dựa trên $x$.

Độ phức tạp thời gian là $O(\log x)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minEnd(self, n: int, x: int) -> int:
        n -= 1
        ans = x
        for i in range(31):
            if x >> i & 1 ^ 1:
                ans |= (n & 1) << i
                n >>= 1
        ans |= n << 31
        return ans
```

#### Java

```java
class Solution {
    public long minEnd(int n, int x) {
        --n;
        long ans = x;
        for (int i = 0; i < 31; ++i) {
            if ((x >> i & 1) == 0) {
                ans |= (n & 1) << i;
                n >>= 1;
            }
        }
        ans |= (long) n << 31;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minEnd(int n, int x) {
        --n;
        long long ans = x;
        for (int i = 0; i < 31; ++i) {
            if (x >> i & 1 ^ 1) {
                ans |= (n & 1) << i;
                n >>= 1;
            }
        }
        ans |= (1LL * n) << 31;
        return ans;
    }
};
```

#### Go

```go
func minEnd(n int, x int) (ans int64) {
	n--
	ans = int64(x)
	for i := 0; i < 31; i++ {
		if x>>i&1 == 0 {
			ans |= int64((n & 1) << i)
			n >>= 1
		}
	}
	ans |= int64(n) << 31
	return
}
```

#### TypeScript

```ts
function minEnd(n: number, x: number): number {
    --n;
    let ans: bigint = BigInt(x);
    for (let i = 0; i < 31; ++i) {
        if (((x >> i) & 1) ^ 1) {
            ans |= BigInt(n & 1) << BigInt(i);
            n >>= 1;
        }
    }
    ans |= BigInt(n) << BigInt(31);
    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
