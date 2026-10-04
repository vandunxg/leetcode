---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2898. Maximum Linear Stock Score 🔒](https://leetcode.com/problems/maximum-linear-stock-score)

[中文文档](/solution/2800-2899/2898.Maximum%20Linear%20Stock%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 1</strong> <code>prices</code>, trong đó <code>prices[i]</code> là giá của một cổ phiếu cụ thể vào ngày thứ <code>i<sup>th</sup></code>, hãy chọn một số phần tử của <code>prices</code> sao cho lựa chọn đó là <strong>tuyến tính</strong>.</p>

<p>Một lựa chọn <code>indexes</code>, trong đó <code>indexes</code> là một mảng số nguyên <strong>đánh chỉ số từ 1</strong> có độ dài <code>k</code> và là một dãy con của mảng <code>[1, 2, ..., n]</code>, được gọi là <strong>tuyến tính</strong> nếu:</p>

<ul>
	<li>Với mọi <code>1 &lt; j &lt;= k</code>, <code>prices[indexes[j]] - prices[indexes[j - 1]] == indexes[j] - indexes[j - 1]</code>.</li>
</ul>

<p>Một <b>dãy con</b> là một mảng có thể được tạo ra từ một mảng khác bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p><strong>Điểm số</strong> của lựa chọn <code>indexes</code> bằng tổng của mảng sau: <code>[prices[indexes[1]], prices[indexes[2]], ..., prices[indexes[k]]</code>.</p>

<p>Trả về <em><strong>điểm số</strong> <strong>lớn nhất</strong> mà một lựa chọn tuyến tính có thể đạt được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,5,3,7,8]
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Ta có thể chọn các chỉ số [2,4,5]. Ta kiểm tra lựa chọn này là tuyến tính:
Với j = 2, ta có:
indexes[2] - indexes[1] = 4 - 2 = 2.
prices[4] - prices[2] = 7 - 5 = 2.
Với j = 3, ta có:
indexes[3] - indexes[2] = 5 - 4 = 1.
prices[5] - prices[4] = 8 - 7 = 1.
Tổng các phần tử là: prices[2] + prices[4] + prices[5] = 20.
Có thể chứng minh rằng tổng lớn nhất mà một lựa chọn tuyến tính có thể đạt được là 20.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [5,6,7,8,9]
<strong>Đầu ra:</strong> 35
<strong>Giải thích:</strong> Ta có thể chọn tất cả các chỉ số [1,2,3,4,5]. Vì mỗi phần tử chênh lệch đúng 1 so với phần tử trước đó, lựa chọn này là tuyến tính.
Tổng tất cả các phần tử là 35, đây là tổng lớn nhất có thể có từ mọi lựa chọn.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một lựa chọn tuyến tính có cùng giá trị $prices[i]-i$. Nhóm các giá trị prices theo khóa đó và tính tổng trong từng nhóm; tổng lớn nhất là đáp án.

<!-- thinking:end -->

Ta có thể biến đổi phương trình như sau:

$$
prices[i] - i = prices[j] - j
$$

Thực chất, bài toán là tìm tổng lớn nhất của tất cả các $prices[i]$ có cùng giá trị $prices[i] - i$.

Do đó, ta có thể dùng một hash table $cnt$ để lưu tổng của tất cả các $prices[i]$ có cùng giá trị $prices[i] - i$, rồi lấy giá trị lớn nhất trong hash table.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $prices$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, prices: List[int]) -> int:
        cnt = Counter()
        for i, x in enumerate(prices):
            cnt[x - i] += x
        return max(cnt.values())
```

#### Java

```java
class Solution {
    public long maxScore(int[] prices) {
        Map<Integer, Long> cnt = new HashMap<>();
        for (int i = 0; i < prices.length; ++i) {
            cnt.merge(prices[i] - i, (long) prices[i], Long::sum);
        }
        long ans = 0;
        for (long v : cnt.values()) {
            ans = Math.max(ans, v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxScore(vector<int>& prices) {
        unordered_map<int, long long> cnt;
        for (int i = 0; i < prices.size(); ++i) {
            cnt[prices[i] - i] += prices[i];
        }
        long long ans = 0;
        for (auto& [_, v] : cnt) {
            ans = max(ans, v);
        }
        return ans;
    }
};
```

#### Go

```go
func maxScore(prices []int) (ans int64) {
	cnt := map[int]int{}
	for i, x := range prices {
		cnt[x-i] += x
	}
	for _, v := range cnt {
		ans = max(ans, int64(v))
	}
	return
}
```

#### TypeScript

```ts
function maxScore(prices: number[]): number {
    const cnt: Map<number, number> = new Map();
    for (let i = 0; i < prices.length; ++i) {
        const j = prices[i] - i;
        cnt.set(j, (cnt.get(j) || 0) + prices[i]);
    }
    return Math.max(...cnt.values());
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn max_score(prices: Vec<i32>) -> i64 {
        let mut cnt: HashMap<i32, i64> = HashMap::new();

        for (i, x) in prices.iter().enumerate() {
            let key = (*x as i32) - (i as i32);
            let count = cnt.entry(key).or_insert(0);
            *count += *x as i64;
        }

        *cnt.values().max().unwrap_or(&0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
