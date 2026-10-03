---
comments: true
difficulty: Hard
rating: 2092
source: Biweekly Contest 87 Q4
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2412. Minimum Money Required Before Transactions](https://leetcode.com/problems/minimum-money-required-before-transactions)

[中文文档](/solution/2400-2499/2412.Minimum%20Money%20Required%20Before%20Transactions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code><font face="monospace">transactions</font></code> được đánh chỉ số từ <strong>0</strong>, trong đó <code>transactions[i] = [cost<sub>i</sub>, cashback<sub>i</sub>]</code>.</p>

<p>Mảng này mô tả các giao dịch, trong đó mỗi giao dịch phải được thực hiện đúng một lần theo <strong>một thứ tự nào đó</strong>. Tại mỗi thời điểm, bạn có một lượng <code>money</code> nhất định. Để thực hiện giao dịch thứ <code>i</code>, điều kiện <code>money &gt;= cost<sub>i</sub></code> phải được thỏa mãn. Sau khi thực hiện một giao dịch, <code>money</code> trở thành <code>money - cost<sub>i</sub> + cashback<sub>i</sub></code>.</p>

<p>Hãy trả về <em>lượng </em><code>money</code><em> tối thiểu cần có trước khi thực hiện bất kỳ giao dịch nào để tất cả các giao dịch đều có thể được hoàn thành <strong>bất kể thứ tự</strong> của chúng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> transactions = [[2,1],[5,0],[4,2]]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:
</strong>Bắt đầu với money = 10, các giao dịch có thể được thực hiện theo bất kỳ thứ tự nào.
Có thể chứng minh rằng nếu bắt đầu với money &lt; 10 thì sẽ không thể hoàn thành tất cả giao dịch theo một thứ tự nào đó.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> transactions = [[3,0],[0,3]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Nếu các giao dịch được thực hiện theo thứ tự [[3,0],[0,3]], lượng money tối thiểu cần có để hoàn thành các giao dịch là 3.
- Nếu các giao dịch được thực hiện theo thứ tự [[0,3],[3,0]], lượng money tối thiểu cần có là 0.
Do đó, bắt đầu với money = 3 thì các giao dịch có thể được thực hiện theo bất kỳ thứ tự nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= transactions.length &lt;= 10<sup>5</sup></code></li>
	<li><code>transactions[i].length == 2</code></li>
	<li><code>0 &lt;= cost<sub>i</sub>, cashback<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Không thể thử mọi hoán vị giao dịch khi $n\le 10^5$. Các giao dịch bị lỗ đóng góp một tổng thiệt hại cố định $s$; lượng tiền tối thiểu xảy ra sau khi thanh toán một khoản chi phí và trước khi nhận cashback tương ứng.
>
> Hãy thử từng giao dịch là giao dịch tạo ra mức tối thiểu: một giao dịch bị lỗ $[a,b]$ cần $s+b$, vì $a-b$ đã nằm trong $s$; một giao dịch có lãi vẫn cần toàn bộ $s$ cộng với chi phí $a$. Đáp án là giá trị lớn nhất trong các trường hợp này.

<!-- thinking:end -->

Trước hết, ta cộng dồn toàn bộ lợi nhuận âm, ký hiệu là $s$. Sau đó, ta duyệt từng giao dịch $\text{transactions}[i] = [a, b]$ và coi đó là giao dịch cuối cùng. Nếu $a > b$, nghĩa là giao dịch hiện tại bị lỗ, và giao dịch này đã được tính khi ta cộng dồn các khoản lợi nhuận âm trước đó. Vì vậy, ta cập nhật đáp án bằng $s + b$. Ngược lại, ta cập nhật đáp án bằng $s + a$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số giao dịch. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumMoney(self, transactions: List[List[int]]) -> int:
        s = sum(max(0, a - b) for a, b in transactions)
        ans = 0
        for a, b in transactions:
            if a > b:
                ans = max(ans, s + b)
            else:
                ans = max(ans, s + a)
        return ans
```

#### Java

```java
class Solution {
    public long minimumMoney(int[][] transactions) {
        long s = 0;
        for (var e : transactions) {
            s += Math.max(0, e[0] - e[1]);
        }
        long ans = 0;
        for (var e : transactions) {
            if (e[0] > e[1]) {
                ans = Math.max(ans, s + e[1]);
            } else {
                ans = Math.max(ans, s + e[0]);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumMoney(vector<vector<int>>& transactions) {
        long long s = 0, ans = 0;
        for (auto& e : transactions) {
            s += max(0, e[0] - e[1]);
        }
        for (auto& e : transactions) {
            if (e[0] > e[1]) {
                ans = max(ans, s + e[1]);
            } else {
                ans = max(ans, s + e[0]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumMoney(transactions [][]int) int64 {
	s, ans := 0, 0
	for _, e := range transactions {
		s += max(0, e[0]-e[1])
	}
	for _, e := range transactions {
		if e[0] > e[1] {
			ans = max(ans, s+e[1])
		} else {
			ans = max(ans, s+e[0])
		}
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function minimumMoney(transactions: number[][]): number {
    const s = transactions.reduce((acc, [a, b]) => acc + Math.max(0, a - b), 0);
    let ans = 0;
    for (const [a, b] of transactions) {
        if (a > b) {
            ans = Math.max(ans, s + b);
        } else {
            ans = Math.max(ans, s + a);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_money(transactions: Vec<Vec<i32>>) -> i64 {
        let mut s: i64 = 0;
        for transaction in &transactions {
            let (a, b) = (transaction[0], transaction[1]);
            s += (a - b).max(0) as i64;
        }
        let mut ans = 0;
        for transaction in &transactions {
            let (a, b) = (transaction[0], transaction[1]);
            if a > b {
                ans = ans.max(s + b as i64);
            } else {
                ans = ans.max(s + a as i64);
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} transactions
 * @return {number}
 */
var minimumMoney = function (transactions) {
    const s = transactions.reduce((acc, [a, b]) => acc + Math.max(0, a - b), 0);
    let ans = 0;
    for (const [a, b] of transactions) {
        if (a > b) {
            ans = Math.max(ans, s + b);
        } else {
            ans = Math.max(ans, s + a);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
