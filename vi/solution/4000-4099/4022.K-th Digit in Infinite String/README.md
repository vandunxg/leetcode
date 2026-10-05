---
comments: true
difficulty: Medium
rating: 1914
source: Biweekly Contest 189 Q3
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [4022. K-th Digit in Infinite String](https://leetcode.com/problems/k-th-digit-in-infinite-string)

[中文文档](/solution/4000-4099/4022.K-th%20Digit%20in%20Infinite%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>k</code>.</p>

<p>Một chuỗi <strong>vô hạn</strong> được tạo thành bằng cách <strong>nối</strong> biểu diễn <strong>thập phân</strong> của các số nguyên <strong>dương</strong>, không có dấu phân cách.</p>

<p>Với mọi số nguyên không âm <code>b</code>, block <code>b</code> chứa các số nguyên <strong>dương</strong> từ <code>10 * b</code> đến <code>10 * b + 9</code>. Các số nguyên trong mỗi block được nối như sau:</p>

<ul>
	<li>Nếu <code>b</code> là số chẵn, nối các số nguyên theo thứ tự <strong>tăng dần</strong>.</li>
	<li>Nếu <code>b</code> là số lẻ, nối các số nguyên theo thứ tự <strong>giảm dần</strong>.</li>
</ul>

<p>Do đó, chuỗi bắt đầu bằng các số nguyên từ 1 đến 9, tiếp theo là từ 19 đến 10, rồi từ 20 đến 29, sau đó từ 39 đến 30, và cứ tiếp tục như vậy.</p>

<p>Trả về chữ số thứ <code>k<sup>th</sup></code> (đánh số từ 1) của chuỗi này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi bắt đầu bằng <code>&quot;123<u>4</u>56789..&quot;</code>. Chữ số thứ 4<sup>th</sup> là <code>&#39;4&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 15</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi bắt đầu bằng <code>&quot;12345678919181<u>7</u>..&quot;</code>. Chữ số thứ 15<sup>th</sup> là <code>&#39;7&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 11</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi bắt đầu bằng <code>&quot;1234567891<u>9</u>..&quot;</code>. Chữ số thứ 11<sup>th</sup> là <code>&#39;9&#39;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> $k$ có thể rất lớn, vì vậy ta không thể tạo toàn bộ chuỗi vô hạn. Các số được nhóm theo số chữ số, và độ dài của mỗi nhóm có công thức đóng, nên ta trừ lần lượt các nhóm có độ dài nhỏ hơn cho đến khi $k$ rơi vào một độ dài và một block cụ thể.
>
> Block $b$ chứa mười số nguyên có $d$ chữ số, được sắp xếp tăng dần khi $b$ chẵn và giảm dần khi $b$ lẻ. Offset bên trong block giúp xác định số nguyên, sau đó ta đọc chữ số cần tìm.
>
> Toàn bộ vị trí được xác định chỉ bằng phép chia và phép lấy phần dư của $k$, với thời gian $O(\log k)$.

<!-- thinking:end -->

Chuỗi vô hạn được tạo thành bằng cách nối các block: block $b$ chứa các số nguyên dương từ $10b$ đến $10b+9$ (block $0$ bắt đầu từ $1$). Các block chẵn được nối theo thứ tự tăng dần, còn các block lẻ theo thứ tự giảm dần.

Trước tiên, ta xử lý các số từ $1$ đến $9$ (tổng cộng $9$ chữ số). Sau đó, ta nhóm theo số chữ số $d = 2, 3, \ldots$: các số có $d$ chữ số tương ứng với các block $b \in [10^{d-2}, 10^{d-1} - 1]$, tức là có $9 \times 10^{d-2}$ block. Mỗi block có $10$ số gồm $d$ chữ số, nên mỗi block đóng góp $10d$ chữ số.

Ta trừ tổng số chữ số của từng nhóm cho đến khi xác định được nhóm chứa chữ số thứ $k$. Sau đó, từ offset còn lại, ta tính chỉ số block $b$ và vị trí bên trong block, xác định số nguyên tương ứng theo tính chẵn lẻ của $b$, rồi lấy chữ số cần tìm.

Độ phức tạp thời gian là $O(\log k)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthDigit(self, k: int) -> int:
        if k <= 9:
            return k

        k -= 9
        d = 2
        start = 1

        while True:
            cnt = 9 * 10 ** (d - 2)
            size = 10 * d

            if k <= cnt * size:
                break

            k -= cnt * size
            d += 1
            start *= 10

        b = start + (k - 1) // size
        pos = (k - 1) % size

        i = pos // d
        num = 10 * b + i if b % 2 == 0 else 10 * b + 9 - i

        return int(str(num)[pos % d])
```

#### Java

```java
class Solution {
    public int kthDigit(long k) {
        if (k <= 9) {
            return (int) k;
        }

        k -= 9;
        long d = 2;
        long start = 1;
        long size = 0;

        while (true) {
            long cnt = 9 * (long) Math.pow(10, d - 2);
            size = 10 * d;

            if (k <= cnt * size) {
                break;
            }

            k -= cnt * size;
            d++;
            start *= 10;
        }

        long b = start + (k - 1) / size;
        long pos = (k - 1) % size;

        long i = pos / d;

        long num = (b % 2 == 0) ? 10 * b + i : 10 * b + 9 - i;

        return String.valueOf(num).charAt((int) (pos % d)) - '0';
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kthDigit(long long k) {
        if (k <= 9) {
            return (int) k;
        }

        k -= 9;
        long long d = 2;
        long long start = 1;
        long long size = 0;

        while (true) {
            long long cnt = 9 * (long long) pow(10, d - 2);
            size = 10 * d;

            if (k <= cnt * size) {
                break;
            }

            k -= cnt * size;
            d++;
            start *= 10;
        }

        long long b = start + (k - 1) / size;
        long long pos = (k - 1) % size;

        long long i = pos / d;

        long long num;
        if (b % 2 == 0) {
            num = 10 * b + i;
        } else {
            num = 10 * b + 9 - i;
        }

        return to_string(num)[pos % d] - '0';
    }
};
```

#### Go

```go
import (
	"math"
	"strconv"
)

func kthDigit(k int64) int {
	if k <= 9 {
		return int(k)
	}

	k -= 9
	var d int64 = 2
	var start int64 = 1
	var size int64

	for {
		cnt := int64(9) * int64(math.Pow10(int(d-2)))
		size = 10 * d

		if k <= cnt*size {
			break
		}

		k -= cnt * size
		d++
		start *= 10
	}

	b := start + (k-1)/size
	pos := (k - 1) % size

	i := pos / d

	var num int64
	if b%2 == 0 {
		num = 10*b + i
	} else {
		num = 10*b + 9 - i
	}

	s := strconv.FormatInt(num, 10)

	return int(s[pos%d] - '0')
}
```

#### TypeScript

```ts
function kthDigit(k: number): number {
    if (k <= 9) {
        return k;
    }

    k -= 9;
    let d = 2;
    let start = 1;
    let size = 0;

    while (true) {
        const cnt = 9 * Math.pow(10, d - 2);
        size = 10 * d;

        if (k <= cnt * size) {
            break;
        }

        k -= cnt * size;
        d++;
        start *= 10;
    }

    const b = start + Math.floor((k - 1) / size);
    const pos = (k - 1) % size;

    const i = Math.floor(pos / d);

    let num: number;
    if (b % 2 === 0) {
        num = 10 * b + i;
    } else {
        num = 10 * b + 9 - i;
    }

    return Number(String(num)[pos % d]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
