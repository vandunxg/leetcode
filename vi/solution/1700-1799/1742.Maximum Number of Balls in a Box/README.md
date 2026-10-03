---
comments: true
difficulty: Easy
rating: 1277
source: Weekly Contest 226 Q1
tags:
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [1742. Maximum Number of Balls in a Box](https://leetcode.com/problems/maximum-number-of-balls-in-a-box)

[中文文档](/solution/1700-1799/1742.Maximum%20Number%20of%20Balls%20in%20a%20Box/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn làm việc trong một nhà máy bóng, có <code>n</code> quả bóng được đánh số từ <code>lowLimit</code> đến <code>highLimit</code> <strong>bao gồm cả hai đầu</strong> (tức là <code>n == highLimit - lowLimit + 1</code>) và vô số hộp được đánh số từ <code>1</code> đến <code>infinity</code>.</p>

<p>Nhiệm vụ của bạn là đặt mỗi quả bóng vào hộp có số bằng tổng các chữ số trong số của quả bóng. Ví dụ, quả bóng số <code>321</code> được đặt vào hộp số <code>3 + 2 + 1 = 6</code>, còn quả bóng số <code>10</code> được đặt vào hộp số <code>1 + 0 = 1</code>.</p>

<p>Cho hai số nguyên <code>lowLimit</code> và <code>highLimit</code>, hãy trả về <em>số quả bóng trong hộp có nhiều bóng nhất</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> lowLimit = 1, highLimit = 10
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Số hộp:       1 2 3 4 5 6 7 8 9 10 11 ...
Số bóng:      2 1 1 1 1 1 1 1 1 0  0  ...
Hộp 1 có nhiều bóng nhất với 2 quả bóng.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> lowLimit = 5, highLimit = 15
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Số hộp:       1 2 3 4 5 6 7 8 9 10 11 ...
Số bóng:      1 1 1 1 2 2 1 1 1 0  0  ...
Hộp 5 và 6 có nhiều bóng nhất, mỗi hộp có 2 quả bóng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> lowLimit = 19, highLimit = 28
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Số hộp:       1 2 3 4 5 6 7 8 9 10 11 12 ...
Số bóng:      0 1 1 1 1 1 1 1 1 2  0  0  ...
Hộp 10 có nhiều bóng nhất với 2 quả bóng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= lowLimit &lt;= highLimit &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Hộp được xác định bởi tổng chữ số. Các số không vượt quá $10^5$, nên tổng chữ số luôn nhỏ hơn $50$ và vừa với một bộ đếm cố định.
>
> Duyệt $[\textit{lowLimit},\textit{highLimit}]$, cộng các chữ số, tăng $cnt[y]$, rồi trả về giá trị lớn nhất trong các ngăn.

<!-- thinking:end -->

Xét miền dữ liệu của bài toán, số bóng tối đa không vượt quá $10^5$, nên tổng chữ số lớn nhất của mỗi số nhỏ hơn $50$. Vì vậy, ta có thể tạo trực tiếp một mảng $\textit{cnt}$ có độ dài $50$ để đếm số lần xuất hiện của từng tổng chữ số.

Đáp án là giá trị lớn nhất trong mảng $\textit{cnt}$.

Độ phức tạp thời gian là $O(n \times \log_{10}m)$. Trong đó, $n = \textit{highLimit} - \textit{lowLimit} + 1$ và $m = \textit{highLimit}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBalls(self, lowLimit: int, highLimit: int) -> int:
        cnt = [0] * 50
        for x in range(lowLimit, highLimit + 1):
            y = 0
            while x:
                y += x % 10
                x //= 10
            cnt[y] += 1
        return max(cnt)
```

#### Java

```java
class Solution {
    public int countBalls(int lowLimit, int highLimit) {
        int[] cnt = new int[50];
        for (int i = lowLimit; i <= highLimit; ++i) {
            int y = 0;
            for (int x = i; x > 0; x /= 10) {
                y += x % 10;
            }
            ++cnt[y];
        }
        return Arrays.stream(cnt).max().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countBalls(int lowLimit, int highLimit) {
        int cnt[50] = {0};
        int ans = 0;
        for (int i = lowLimit; i <= highLimit; ++i) {
            int y = 0;
            for (int x = i; x; x /= 10) {
                y += x % 10;
            }
            ans = max(ans, ++cnt[y]);
        }
        return ans;
    }
};
```

#### Go

```go
func countBalls(lowLimit int, highLimit int) (ans int) {
	cnt := [50]int{}
	for i := lowLimit; i <= highLimit; i++ {
		y := 0
		for x := i; x > 0; x /= 10 {
			y += x % 10
		}
		cnt[y]++
		if ans < cnt[y] {
			ans = cnt[y]
		}
	}
	return
}
```

#### TypeScript

```ts
function countBalls(lowLimit: number, highLimit: number): number {
    const cnt: number[] = Array(50).fill(0);
    for (let i = lowLimit; i <= highLimit; ++i) {
        let y = 0;
        for (let x = i; x; x = Math.floor(x / 10)) {
            y += x % 10;
        }
        ++cnt[y];
    }
    return Math.max(...cnt);
}
```

#### Rust

```rust
impl Solution {
    pub fn count_balls(low_limit: i32, high_limit: i32) -> i32 {
        let mut cnt = vec![0; 50];
        for x in low_limit..=high_limit {
            let mut y = 0;
            let mut n = x;
            while n > 0 {
                y += n % 10;
                n /= 10;
            }
            cnt[y as usize] += 1;
        }
        *cnt.iter().max().unwrap()
    }
}
```

#### JavaScript

```js
/**
 * @param {number} lowLimit
 * @param {number} highLimit
 * @return {number}
 */
var countBalls = function (lowLimit, highLimit) {
    const cnt = Array(50).fill(0);
    for (let i = lowLimit; i <= highLimit; ++i) {
        let y = 0;
        for (let x = i; x; x = Math.floor(x / 10)) {
            y += x % 10;
        }
        ++cnt[y];
    }
    return Math.max(...cnt);
};
```

#### C#

```cs
public class Solution {
    public int CountBalls(int lowLimit, int highLimit) {
        int[] cnt = new int[50];
        for (int x = lowLimit; x <= highLimit; x++) {
            int y = 0;
            int n = x;
            while (n > 0) {
                y += n % 10;
                n /= 10;
            }
            cnt[y]++;
        }
        return cnt.Max();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
