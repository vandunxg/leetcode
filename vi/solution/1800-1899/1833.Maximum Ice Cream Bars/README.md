---
comments: true
difficulty: Medium
rating: 1252
source: Weekly Contest 237 Q2
tags:
    - Greedy
    - Array
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [1833. Maximum Ice Cream Bars](https://leetcode.com/problems/maximum-ice-cream-bars)

[中文文档](/solution/1800-1899/1833.Maximum%20Ice%20Cream%20Bars/README.md)

## Mô tả

<!-- description:start -->

<p>Đó là một ngày hè oi bức, và một cậu bé muốn mua vài que kem.</p>

<p>Cửa hàng có <code>n</code> que kem. Cho một mảng <code>costs</code> có độ dài <code>n</code>, trong đó <code>costs[i]</code> là giá bằng xu của que kem thứ <code>i<sup>th</sup></code>. Ban đầu cậu bé có <code>coins</code> xu để chi tiêu và muốn mua được nhiều que kem nhất có thể.&nbsp;</p>

<p><strong>Lưu ý:</strong> Cậu bé có thể mua các que kem theo bất kỳ thứ tự nào.</p>

<p>Trả về <em><strong>số lượng que kem lớn nhất</strong> mà cậu bé có thể mua bằng </em><code>coins</code><em> xu.</em></p>

<p>Bạn phải giải bài toán bằng counting sort.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [1,3,2,4,1], coins = 7
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Cậu bé có thể mua các que kem ở các chỉ số 0,1,2,4 với tổng giá 1 + 3 + 2 + 1 = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [10,6,8,7,7,8], coins = 5
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Cậu bé không đủ tiền mua bất kỳ que kem nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [1,6,3,1,2,5], coins = 20
<strong>Đầu ra:</strong> 6
<strong>Giải thích: </strong>Cậu bé có thể mua tất cả các que kem với tổng giá 1 + 6 + 3 + 1 + 2 + 5 = 18.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>costs.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= costs[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= coins &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ngân sách là cố định và ta có thể mua theo bất kỳ thứ tự nào. Mua các que kem đắt trước không thể làm tăng số lượng mua được.
>
> Sắp xếp theo giá rồi mua từ rẻ nhất đến đắt nhất cho đến khi que tiếp theo có giá vượt quá số xu còn lại. Một lượt duyệt sau khi sắp xếp sẽ cho số lượng lớn nhất.

<!-- thinking:end -->

Để mua được nhiều que kem nhất có thể, vì có thể mua theo bất kỳ thứ tự nào, ta nên ưu tiên các que kem có giá thấp hơn.

Sắp xếp mảng $costs$, sau đó lần lượt mua từ que kem có giá thấp nhất cho đến khi không thể mua tiếp, rồi trả về số que kem đã mua được.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài mảng $costs$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxIceCream(self, costs: List[int], coins: int) -> int:
        costs.sort()
        for i, c in enumerate(costs):
            if coins < c:
                return i
            coins -= c
        return len(costs)
```

#### Java

```java
class Solution {
    public int maxIceCream(int[] costs, int coins) {
        Arrays.sort(costs);
        int n = costs.length;
        for (int i = 0; i < n; ++i) {
            if (coins < costs[i]) {
                return i;
            }
            coins -= costs[i];
        }
        return n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxIceCream(vector<int>& costs, int coins) {
        sort(costs.begin(), costs.end());
        int n = costs.size();
        for (int i = 0; i < n; ++i) {
            if (coins < costs[i]) return i;
            coins -= costs[i];
        }
        return n;
    }
};
```

#### Go

```go
func maxIceCream(costs []int, coins int) int {
	sort.Ints(costs)
	for i, c := range costs {
		if coins < c {
			return i
		}
		coins -= c
	}
	return len(costs)
}
```

#### TypeScript

```ts
function maxIceCream(costs: number[], coins: number): number {
    costs.sort((a, b) => a - b);
    const n = costs.length;
    for (let i = 0; i < n; ++i) {
        if (coins < costs[i]) {
            return i;
        }
        coins -= costs[i];
    }
    return n;
}
```

#### JavaScript

```js
/**
 * @param {number[]} costs
 * @param {number} coins
 * @return {number}
 */
var maxIceCream = function (costs, coins) {
    costs.sort((a, b) => a - b);
    const n = costs.length;
    for (let i = 0; i < n; ++i) {
        if (coins < costs[i]) {
            return i;
        }
        coins -= costs[i];
    }
    return n;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
