---
comments: true
difficulty: Easy
rating: 1206
source: Weekly Contest 337 Q1
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [2595. Number of Even and Odd Bits](https://leetcode.com/problems/number-of-even-and-odd-bits)

[中文文档](/solution/2500-2599/2595.Number%20of%20Even%20and%20Odd%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <strong>dương</strong> <code>n</code>.</p>

<p>Gọi <code>even</code> là số lượng chỉ số chẵn trong biểu diễn nhị phân của <code>n</code> có giá trị bằng 1.</p>

<p>Gọi <code>odd</code> là số lượng chỉ số lẻ trong biểu diễn nhị phân của <code>n</code> có giá trị bằng 1.</p>

<p>Lưu ý rằng các bit được đánh chỉ số từ <strong>phải sang trái</strong> trong biểu diễn nhị phân của một số.</p>

<p>Trả về mảng <code>[even, odd]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 50</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của 50 là <code>110010</code>.</p>

<p>Nó có các bit 1 tại các chỉ số 1, 4 và 5.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của 2 là <code>10</code>.</p>

<p>Nó chỉ có bit 1 tại chỉ số 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ không vượt quá $10^3$, ta chỉ cần duyệt các bit từ thấp đến cao. Đảo chỉ số bằng $i\oplus 1$ và tăng bộ đếm tương ứng khi bit đang xét được bật.

<!-- thinking:end -->

Theo đề bài, ta liệt kê biểu diễn nhị phân của $n$ từ bit thấp nhất đến bit cao nhất. Nếu bit bằng $1$, ta tăng $1$ vào bộ đếm tương ứng tùy theo chỉ số của bit là chẵn hay lẻ.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là số nguyên được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def evenOddBit(self, n: int) -> List[int]:
        ans = [0, 0]
        i = 0
        while n:
            ans[i] += n & 1
            i ^= 1
            n >>= 1
        return ans
```

#### Java

```java
class Solution {
    public int[] evenOddBit(int n) {
        int[] ans = new int[2];
        for (int i = 0; n > 0; n >>= 1, i ^= 1) {
            ans[i] += n & 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> evenOddBit(int n) {
        vector<int> ans(2);
        for (int i = 0; n > 0; n >>= 1, i ^= 1) {
            ans[i] += n & 1;
        }
        return ans;
    }
};
```

#### Go

```go
func evenOddBit(n int) []int {
	ans := make([]int, 2)
	for i := 0; n != 0; n, i = n>>1, i^1 {
		ans[i] += n & 1
	}
	return ans
}
```

#### TypeScript

```ts
function evenOddBit(n: number): number[] {
    const ans = Array(2).fill(0);
    for (let i = 0; n > 0; n >>= 1, i ^= 1) {
        ans[i] += n & 1;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn even_odd_bit(mut n: i32) -> Vec<i32> {
        let mut ans = vec![0; 2];

        let mut i = 0;
        while n != 0 {
            ans[i] += n & 1;

            n >>= 1;
            i ^= 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 có độ phức tạp tuyến tính theo số lượng bit. Các chỉ số chẵn chính là mask $0\texttt{x}5555$; $\textit{bit\_count}(n\&\textit{mask})$ là số lượng bit chẵn, còn mask phần bù cho số lượng bit lẻ.

<!-- thinking:end -->

Ta có thể định nghĩa một mask $\textit{mask} = \text{0x5555}$, được biểu diễn dưới dạng nhị phân là $\text{0101 0101 0101 0101}_2$. Khi thực hiện phép AND bit giữa $n$ và $\textit{mask}$, ta nhận được các bit tại chỉ số chẵn trong biểu diễn nhị phân của $n$. Khi thực hiện phép AND bit giữa $n$ và phần bù của $\textit{mask}$, ta nhận được các bit tại chỉ số lẻ trong biểu diễn nhị phân của $n$. Sau đó, ta đếm số bit 1 trong hai kết quả này.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là số nguyên được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def evenOddBit(self, n: int) -> List[int]:
        mask = 0x5555
        even = (n & mask).bit_count()
        odd = (n & ~mask).bit_count()
        return [even, odd]
```

#### Java

```java
class Solution {
    public int[] evenOddBit(int n) {
        int mask = 0x5555;
        int even = Integer.bitCount(n & mask);
        int odd = Integer.bitCount(n & ~mask);
        return new int[] {even, odd};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> evenOddBit(int n) {
        int mask = 0x5555;
        int even = __builtin_popcount(n & mask);
        int odd = __builtin_popcount(n & ~mask);
        return {even, odd};
    }
};
```

#### Go

```go
func evenOddBit(n int) []int {
	mask := 0x5555
	even := bits.OnesCount32(uint32(n & mask))
	odd := bits.OnesCount32(uint32(n & ^mask))
	return []int{even, odd}
}
```

#### TypeScript

```ts
function evenOddBit(n: number): number[] {
    const mask = 0x5555;
    const even = bitCount(n & mask);
    const odd = bitCount(n & ~mask);
    return [even, odd];
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

#### Rust

```rust
impl Solution {
    pub fn even_odd_bit(n: i32) -> Vec<i32> {
        let mask: i32 = 0x5555;
        let even = (n & mask).count_ones() as i32;
        let odd = (n & !mask).count_ones() as i32;
        vec![even, odd]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
