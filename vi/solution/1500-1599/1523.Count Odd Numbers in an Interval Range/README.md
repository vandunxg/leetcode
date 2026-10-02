---
comments: true
difficulty: Easy
rating: 1209
source: Biweekly Contest 31 Q1
tags:
    - Math
---

<!-- problem:start -->

# [1523. Count Odd Numbers in an Interval Range](https://leetcode.com/problems/count-odd-numbers-in-an-interval-range)

[中文文档](/solution/1500-1599/1523.Count%20Odd%20Numbers%20in%20an%20Interval%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên không âm <code>low</code> và <code><font face="monospace">high</font></code>. Hãy trả về <em>số lượng số lẻ nằm giữa </em><code>low</code><em> và </em><code><font face="monospace">high</font></code><em>&nbsp;(bao gồm cả hai đầu mút)</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> low = 3, high = 7
<strong>Output:</strong> 3
<b>Giải thích: </b>Các số lẻ giữa 3 và 7 là [3,5,7].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> low = 8, high = 10
<strong>Output:</strong> 1
<b>Giải thích: </b>Các số lẻ giữa 8 và 10 là [9].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= low &lt;= high&nbsp;&lt;= 10^9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ý tưởng tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các số lẻ trong đoạn đóng $[low,high]$. Độ dài đoạn có thể lên tới $10^9$, nên không thể kiểm tra từng số nguyên.
>
> Đoạn $[0,x]$ chứa $\lfloor(x+1)/2\rfloor$ số lẻ. Lấy số lượng trong $[0,low-1]$ trừ khỏi số lượng trong $[0,high]$ ta được $\lfloor(high+1)/2\rfloor-\lfloor low/2\rfloor$, có thể tính trong thời gian hằng số bằng phép dịch bit.

<!-- thinking:end -->

Ta biết số lượng số lẻ trong đoạn $[0, x]$ là $\lfloor\frac{x+1}{2}\rfloor$. Vì vậy, số lượng số lẻ trong đoạn $[low, high]$ là $\lfloor\frac{high+1}{2}\rfloor - \lfloor\frac{low}{2}\rfloor$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOdds(self, low: int, high: int) -> int:
        return ((high + 1) >> 1) - (low >> 1)
```

#### Java

```java
class Solution {
    public int countOdds(int low, int high) {
        return ((high + 1) >> 1) - (low >> 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countOdds(int low, int high) {
        return (high + 1 >> 1) - (low >> 1);
    }
};
```

#### Go

```go
func countOdds(low int, high int) int {
	return ((high + 1) >> 1) - (low >> 1)
}
```

#### TypeScript

```ts
function countOdds(low: number, high: number): number {
    return ((high + 1) >> 1) - (low >> 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn count_odds(low: i32, high: i32) -> i32 {
        ((high + 1) >> 1) - (low >> 1)
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $low
     * @param Integer $high
     * @return Integer
     */
    function countOdds($low, $high) {
        return (($high + 1) >> 1) - ($low >> 1);
    }
}
```

#### C

```c
int countOdds(int low, int high) {
    return ((high + 1) >> 1) - (low >> 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
