---
comments: true
difficulty: Easy
rating: 1245
source: Weekly Contest 198 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [1518. Water Bottles](https://leetcode.com/problems/water-bottles)

[中文文档](/solution/1500-1599/1518.Water%20Bottles/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>numBottles</code> chai nước ban đầu đều đầy. Bạn có thể đổi <code>numExchange</code> chai rỗng lấy một chai nước đầy từ cửa hàng.</p>

<p>Uống một chai nước đầy sẽ biến nó thành một chai rỗng.</p>

<p>Với hai số nguyên <code>numBottles</code> và <code>numExchange</code>, hãy trả về <em><strong>số lượng lớn nhất</strong> chai nước bạn có thể uống</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1518.Water%20Bottles/images/sample_1_1875.png" style="width: 500px; height: 245px;" />
<pre>
<strong>Input:</strong> numBottles = 9, numExchange = 3
<strong>Output:</strong> 13
<strong>Explanation:</strong> Bạn có thể đổi 3 chai rỗng lấy 1 chai nước đầy.
Số chai nước có thể uống: 9 + 3 + 1 = 13.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1518.Water%20Bottles/images/sample_2_1875.png" style="width: 500px; height: 183px;" />
<pre>
<strong>Input:</strong> numBottles = 15, numExchange = 4
<strong>Output:</strong> 19
<strong>Explanation:</strong> Bạn có thể đổi 4 chai rỗng lấy 1 chai nước đầy. 
Số chai nước có thể uống: 15 + 3 + 1 = 19.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= numBottles &lt;= 100</code></li>
	<li><code>2 &lt;= numExchange &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cứ mỗi $numExchange$ chai rỗng có thể đổi lấy một chai đầy; ta cần tổng số chai đã uống. Các giá trị đủ nhỏ để mô phỏng từng lần đổi thay vì tìm công thức đóng.
>
> Trước hết uống $numBottles$ chai ban đầu. Khi số chai rỗng ít nhất bằng mức đổi, dùng $numExchange$ chai rỗng đổi một chai đầy; sau khi uống chai đó, số chai rỗng giảm $numExchange-1$ và đáp án tăng một. Dừng khi không thể đổi thêm.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numWaterBottles(self, numBottles: int, numExchange: int) -> int:
        ans = numBottles
        while numBottles >= numExchange:
            numBottles -= numExchange - 1
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {
        int ans = numBottles;
        for (; numBottles >= numExchange; ++ans) {
            numBottles -= (numExchange - 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numWaterBottles(int numBottles, int numExchange) {
        int ans = numBottles;
        for (; numBottles >= numExchange; ++ans) {
            numBottles -= (numExchange - 1);
        }
        return ans;
    }
};
```

#### Go

```go
func numWaterBottles(numBottles int, numExchange int) int {
	ans := numBottles
	for ; numBottles >= numExchange; ans++ {
		numBottles -= (numExchange - 1)
	}
	return ans
}
```

#### TypeScript

```ts
function numWaterBottles(numBottles: number, numExchange: number): number {
    let ans = numBottles;
    for (; numBottles >= numExchange; ++ans) {
        numBottles -= numExchange - 1;
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number} numBottles
 * @param {number} numExchange
 * @return {number}
 */
var numWaterBottles = function (numBottles, numExchange) {
    let ans = numBottles;
    for (; numBottles >= numExchange; ++ans) {
        numBottles -= numExchange - 1;
    }
    return ans;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $numBottles
     * @param Integer $numExchange
     * @return Integer
     */
    function numWaterBottles($numBottles, $numExchange) {
        $ans = $numBottles;
        while ($numBottles >= $numExchange) {
            $numBottles = $numBottles - $numExchange + 1;
            $ans++;
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
