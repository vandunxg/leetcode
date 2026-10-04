---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Binary Search
---

<!-- problem:start -->

# [3344. Maximum Sized Array 🔒](https://leetcode.com/problems/maximum-sized-array)

[中文文档](/solution/3300-3399/3344.Maximum%20Sized%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>s</code>, gọi <code>A</code> là một mảng 3D có kích thước<!-- notionvc: f8069282-c5f5-4da1-91b8-fa0c1c168ea1 --> <code>n &times; n &times; n</code>, trong đó mỗi phần tử <code>A[i][j][k]</code> được xác định như sau:</p>

<ul>
    <li><code>A[i][j][k] = i * (j OR k)</code>, trong đó <code>0 &lt;= i, j, k &lt; n</code>.</li>
</ul>

<p>Hãy trả về giá trị <strong>lớn nhất</strong> có thể của <code>n</code> sao cho <strong>tổng</strong> tất cả các phần tử trong mảng <code>A</code> không vượt quá <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các phần tử của mảng <code>A</code> với <code>n = 2</code><strong>:</strong>

    <ul>
        <li><code>A[0][0][0] = 0 * (0 OR 0) = 0</code></li>
        <li><code>A[0][0][1] = 0 * (0 OR 1) = 0</code></li>
        <li><code>A[0][1][0] = 0 * (1 OR 0) = 0</code></li>
        <li><code>A[0][1][1] = 0 * (1 OR 1) = 0</code></li>
        <li><code>A[1][0][0] = 1 * (0 OR 0) = 0</code></li>
        <li><code>A[1][0][1] = 1 * (0 OR 1) = 1</code></li>
        <li><code>A[1][1][0] = 1 * (1 OR 0) = 1</code></li>
        <li><code>A[1][1][1] = 1 * (1 OR 1) = 1</code></li>
    </ul>
    </li>
    <li>Tổng các phần tử trong mảng <code>A</code> là 3, không vượt quá 10, nên giá trị lớn nhất có thể của <code>n</code> là 2.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các phần tử của mảng <code>A</code> với <code>n = 1</code>:

    <ul>
        <li><code>A[0][0][0] = 0 * (0 OR 0) = 0</code></li>
    </ul>
    </li>
    <li>Tổng các phần tử trong mảng <code>A</code> là 0, không vượt quá 0, nên giá trị lớn nhất có thể của <code>n</code> là 1.</li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>0 &lt;= s &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> $f(n)=\sum_{i,j,k<n}(i\cdot(j\lor k))$ tăng theo $n$; ta cần tìm $n$ lớn nhất sao cho $f(n)\le s$. Việc duyệt ba vòng lặp không phù hợp với $s \le 10^{15}$, nhưng $n$ tối đa khoảng $1320$.
>
> Đưa $i$ ra ngoài giúp rút gọn tổng thành một prefix của các phép OR theo cặp. Ta tiền xử lý $f[i]$ bằng cách cộng $i$ và $2(i\lor j)$ với $j<i$.
>
> Sau đó, tìm kiếm nhị phân để tìm $m$ lớn nhất sao cho $f[m-1]\cdot(m-1)\cdot m/2 \le s$.

<!-- thinking:end -->

Ta có thể ước lượng sơ bộ giá trị lớn nhất của $n$. Với $j \lor k$, tổng các kết quả xấp xỉ $n^2 (n - 1) / 2$. Nhân giá trị này với từng $i \in [0, n)$, kết quả xấp xỉ $(n-1)^5 / 4$. Để đảm bảo $(n - 1)^5 / 4 \leq s$, ta có $n \leq 1320$.

Do đó, ta có thể tiền xử lý $f[n] = \sum_{i=0}^{n-1} \sum_{j=0}^{i} (i \lor j)$, sau đó dùng tìm kiếm nhị phân để tìm $n$ lớn nhất sao cho $f[n-1] \cdot (n-1) \cdot n / 2 \leq s$.

Về độ phức tạp thời gian, bước tiền xử lý có độ phức tạp $O(n^2)$, còn tìm kiếm nhị phân có độ phức tạp $O(\log n)$. Do đó, độ phức tạp thời gian tổng thể là $O(n^2 + \log n)$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
mx = 1330
f = [0] * mx
for i in range(1, mx):
    f[i] = f[i - 1] + i
    for j in range(i):
        f[i] += 2 * (i | j)


class Solution:
    def maxSizedArray(self, s: int) -> int:
        l, r = 1, mx
        while l < r:
            m = (l + r + 1) >> 1
            if f[m - 1] * (m - 1) * m // 2 <= s:
                l = m
            else:
                r = m - 1
        return l
```

#### Java

```java
class Solution {
    private static final int MX = 1330;
    private static final long[] f = new long[MX];
    static {
        for (int i = 1; i < MX; ++i) {
            f[i] = f[i - 1] + i;
            for (int j = 0; j < i; ++j) {
                f[i] += 2 * (i | j);
            }
        }
    }
    public int maxSizedArray(long s) {
        int l = 1, r = MX;
        while (l < r) {
            int m = (l + r + 1) >> 1;
            if (f[m - 1] * (m - 1) * m / 2 <= s) {
                l = m;
            } else {
                r = m - 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
const int MX = 1330;
long long f[MX];
auto init = [] {
    f[0] = 0;
    for (int i = 1; i < MX; ++i) {
        f[i] = f[i - 1] + i;
        for (int j = 0; j < i; ++j) {
            f[i] += 2 * (i | j);
        }
    }
    return 0;
}();

class Solution {
public:
    int maxSizedArray(long long s) {
        int l = 1, r = MX;
        while (l < r) {
            int m = (l + r + 1) >> 1;
            if (f[m - 1] * (m - 1) * m / 2 <= s) {
                l = m;
            } else {
                r = m - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
const MX = 1330

var f [MX]int64

func init() {
    f[0] = 0
    for i := 1; i < MX; i++ {
        f[i] = f[i-1] + int64(i)
        for j := 0; j < i; j++ {
            f[i] += 2 * int64(i|j)
        }
    }
}

func maxSizedArray(s int64) int {
    l, r := 1, MX
    for l < r {
        m := (l + r + 1) >> 1
        if f[m-1]*int64(m-1)*int64(m)/2 <= s {
            l = m
        } else {
            r = m - 1
        }
    }
    return l
}
```

#### TypeScript

```ts
const MX = 1330;
const f: bigint[] = Array(MX).fill(0n);
(() => {
    f[0] = 0n;
    for (let i = 1; i < MX; i++) {
        f[i] = f[i - 1] + BigInt(i);
        for (let j = 0; j < i; j++) {
            f[i] += BigInt(2) * BigInt(i | j);
        }
    }
})();

function maxSizedArray(s: number): number {
    let l = 1,
        r = MX;
    const target = BigInt(s);

    while (l < r) {
        const m = (l + r + 1) >> 1;
        if ((f[m - 1] * BigInt(m - 1) * BigInt(m)) / BigInt(2) <= target) {
            l = m;
        } else {
            r = m - 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
