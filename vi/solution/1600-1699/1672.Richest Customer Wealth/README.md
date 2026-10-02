---
comments: true
difficulty: Easy
rating: 1182
source: Weekly Contest 217 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [1672. Richest Customer Wealth](https://leetcode.com/problems/richest-customer-wealth)

[中文文档](/solution/1600-1699/1672.Richest%20Customer%20Wealth/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới số nguyên <code>m x n</code> <code>accounts</code>, trong đó <code>accounts[i][j]</code> là số tiền khách hàng thứ <code>i​​​​​<sup>​​​​​​th</sup>​​​​</code> có trong ngân hàng thứ <code>j​​​​​<sup>​​​​​​th</sup></code>​​​​. Hãy trả về <em><strong>tài sản</strong> của khách hàng giàu nhất.</em></p>

<p><strong>Tài sản</strong> của một khách hàng là tổng số tiền người đó có trong tất cả tài khoản ngân hàng. Khách hàng giàu nhất là người có <strong>tài sản</strong> lớn nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> accounts = [[1,2,3],[3,2,1]]
<strong>Output:</strong> 6
<strong>Giải thích</strong><strong>:</strong>
<code>1st customer has wealth = 1 + 2 + 3 = 6
</code><code>2nd customer has wealth = 3 + 2 + 1 = 6
</code>Cả hai khách hàng đều giàu nhất với tài sản bằng 6, nên trả về 6.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> accounts = [[1,5],[7,3],[3,5]]
<strong>Output:</strong> 10
<strong>Giải thích</strong>:
1st customer has wealth = 6
2nd customer has wealth = 10
3rd customer has wealth = 8
 Khách hàng thứ 2 giàu nhất với tài sản bằng 10.</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> accounts = [[2,8,7],[7,1,3],[1,9,5]]
<strong>Output:</strong> 17
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m ==&nbsp;accounts.length</code></li>
	<li><code>n ==&nbsp;accounts[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>1 &lt;= accounts[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Tài sản là tổng của một hàng. Ma trận có kích thước tối đa $50\times 50$, nên ta tính tổng từng hàng rồi lấy giá trị lớn nhất.

<!-- thinking:end -->

Ta duyệt `accounts` và tìm tổng lớn nhất của các hàng.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của lưới. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumWealth(self, accounts: List[List[int]]) -> int:
        return max(sum(v) for v in accounts)
```

#### Java

```java
class Solution {
    public int maximumWealth(int[][] accounts) {
        int ans = 0;
        for (var e : accounts) {
            // int s = Arrays.stream(e).sum();
            int s = 0;
            for (int v : e) {
                s += v;
            }
            ans = Math.max(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumWealth(vector<vector<int>>& accounts) {
        int ans = 0;
        for (auto& v : accounts) {
            ans = max(ans, accumulate(v.begin(), v.end(), 0));
        }
        return ans;
    }
};
```

#### Go

```go
func maximumWealth(accounts [][]int) int {
	ans := 0
	for _, e := range accounts {
		s := 0
		for _, v := range e {
			s += v
		}
		if ans < s {
			ans = s
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maximumWealth(accounts: number[][]): number {
    return accounts.reduce(
        (r, v) =>
            Math.max(
                r,
                v.reduce((r, v) => r + v),
            ),
        0,
    );
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_wealth(accounts: Vec<Vec<i32>>) -> i32 {
        accounts.iter().map(|v| v.iter().sum()).max().unwrap()
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[][] $accounts
     * @return Integer
     */
    function maximumWealth($accounts) {
        $rs = 0;
        for ($i = 0; $i < count($accounts); $i++) {
            $sum = 0;
            for ($j = 0; $j < count($accounts[$i]); $j++) {
                $sum += $accounts[$i][$j];
            }
            if ($sum > $rs) {
                $rs = $sum;
            }
        }
        return $rs;
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int maximumWealth(int** accounts, int accountsSize, int* accountsColSize) {
    int ans = INT_MIN;
    for (int i = 0; i < accountsSize; i++) {
        int sum = 0;
        for (int j = 0; j < accountsColSize[i]; j++) {
            sum += accounts[i][j];
        }
        ans = max(ans, sum);
    }
    return ans;
}
```

#### Kotlin

```kotlin
class Solution {
    fun maximumWealth(accounts: Array<IntArray>): Int {
        var max = 0
        for (account in accounts) {
            val sum = account.sum()
            if (sum > max) {
                max = sum
            }
        }
        return max
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
