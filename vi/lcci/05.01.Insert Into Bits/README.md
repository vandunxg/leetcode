---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [05.01. Insert Into Bits](https://leetcode.cn/problems/insert-into-bits-lcci)

[Tài liệu tiếng Trung](/lcci/05.01.Insert%20Into%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số 32-bit, N và M, cùng hai vị trí bit i và j. Hãy viết một phương thức chèn M vào N sao cho M bắt đầu tại bit j và kết thúc tại bit i. Bạn có thể giả sử các bit từ j đến i có đủ chỗ để chứa toàn bộ M. Nghĩa là, nếu M = 10011, bạn có thể giả sử có ít nhất 5 bit nằm giữa bit j và bit i. Chẳng hạn, j không thể bằng 3 và i bằng 2, vì M không thể vừa đầy đủ giữa bit 3 và bit 2.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Đầu vào</strong>: N = 10000000000, M = 10011, i = 2, j = 6

<strong> Đầu ra</strong>: N = 10001001100

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Đầu vào</strong>: N = 0, M = 11111, i = 0, j = 4

<strong> Đầu ra</strong>: N = 11111

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> $M$ phải chiếm các bit từ $i$ đến $j$ của $N$. Phép OR trực tiếp sẽ để lại các bit $1$ trong đoạn đó, khiến chúng ghi đè lên các bit $0$ của $M$.
>
> Trước tiên xóa đoạn $[i,j]$, sau đó OR với $M$ đã dịch trái $i$ bit.
>
> Với mỗi $k\in[i,j]$, thực hiện $N \&= \sim(1\ll k)$, rồi trả về $N \mid (M \ll i)$. Đoạn này dài không quá một word.

<!-- thinking:end -->

Trước tiên, ta xóa các bit từ bit thứ $i$ đến bit thứ $j$ trong $N$, sau đó dịch trái $M$ đi $i$ bit, và cuối cùng thực hiện phép OR bit giữa $M$ và $N$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là kích thước của $N$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def insertBits(self, N: int, M: int, i: int, j: int) -> int:
        for k in range(i, j + 1):
            N &= ~(1 << k)
        return N | M << i
```

#### Java

```java
class Solution {
    public int insertBits(int N, int M, int i, int j) {
        for (int k = i; k <= j; ++k) {
            N &= ~(1 << k);
        }
        return N | M << i;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int insertBits(int N, int M, int i, int j) {
        for (int k = i; k <= j; ++k) {
            N &= ~(1 << k);
        }
        return N | M << i;
    }
};
```

#### Go

```go
func insertBits(N int, M int, i int, j int) int {
	for k := i; k <= j; k++ {
		N &= ^(1 << k)
	}
	return N | M<<i
}
```

#### TypeScript

```ts
function insertBits(N: number, M: number, i: number, j: number): number {
    for (let k = i; k <= j; ++k) {
        N &= ~(1 << k);
    }
    return N | (M << i);
}
```

#### Swift

```swift
class Solution {
    func insertBits(_ N: Int, _ M: Int, _ i: Int, _ j: Int) -> Int {
        var result = N

        for k in i...j {
            result &= ~(1 << k)
        }

        return result | (M << i)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
