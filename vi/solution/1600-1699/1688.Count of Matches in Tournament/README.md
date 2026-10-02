---
comments: true
difficulty: Easy
rating: 1203
source: Weekly Contest 219 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [1688. Count of Matches in Tournament](https://leetcode.com/problems/count-of-matches-in-tournament)

[中文文档](/solution/1600-1699/1688.Count%20of%20Matches%20in%20Tournament/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, là số đội trong một giải đấu có luật đặc biệt:</p>

<ul>
	<li>Nếu số đội hiện tại là <strong>chẵn</strong>, mỗi đội được ghép với một đội khác. Có tổng cộng <code>n / 2</code> trận đấu và <code>n / 2</code> đội đi tiếp.</li>
	<li>Nếu số đội hiện tại là <strong>lẻ</strong>, một đội ngẫu nhiên đi tiếp, các đội còn lại được ghép cặp. Có tổng cộng <code>(n - 1) / 2</code> trận đấu và <code>(n - 1) / 2 + 1</code> đội đi tiếp.</li>
</ul>

<p>Hãy trả về <em>số trận đấu được tổ chức cho đến khi xác định được người thắng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 7
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Chi tiết giải đấu:
- Vòng 1: Số đội = 7, số trận = 3 và 4 đội đi tiếp.
- Vòng 2: Số đội = 4, số trận = 2 và 2 đội đi tiếp.
- Vòng 3: Số đội = 2, số trận = 1 và 1 đội được tuyên bố là người thắng.
Tổng số trận = 3 + 2 + 1 = 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 14
<strong>Output:</strong> 13
<strong>Giải thích:</strong> Chi tiết giải đấu:
- Vòng 1: Số đội = 14, số trận = 7 và 7 đội đi tiếp.
- Vòng 2: Số đội = 7, số trận = 3 và 4 đội đi tiếp.
- Vòng 3: Số đội = 4, số trận = 2 và 2 đội đi tiếp.
- Vòng 4: Số đội = 2, số trận = 1 và 1 đội được tuyên bố là người thắng.
Tổng số trận = 7 + 3 + 2 + 1 = 13.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhận xét nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi trận đấu loại một đội. Để có một nhà vô địch, cần loại $n-1$ đội, nên số trận luôn là $n-1$; không cần mô phỏng các vòng chẵn/lẻ.

<!-- thinking:end -->

Từ đề bài, có tổng cộng $n$ đội. Mỗi lần ghép cặp sẽ loại một đội. Vì vậy, số trận bằng số đội bị loại, tức là $n - 1$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfMatches(self, n: int) -> int:
        return n - 1
```

#### Java

```java
class Solution {
    public int numberOfMatches(int n) {
        return n - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfMatches(int n) {
        return n - 1;
    }
};
```

#### Go

```go
func numberOfMatches(n int) int {
	return n - 1
}
```

#### TypeScript

```ts
function numberOfMatches(n: number): number {
    return n - 1;
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var numberOfMatches = function (n) {
    return n - 1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
