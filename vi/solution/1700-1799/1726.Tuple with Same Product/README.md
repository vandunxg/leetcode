---
comments: true
difficulty: Medium
rating: 1530
source: Weekly Contest 224 Q2
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [1726. Tuple with Same Product](https://leetcode.com/problems/tuple-with-same-product)

[中文文档](/solution/1700-1799/1726.Tuple%20with%20Same%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code> gồm các số nguyên dương <strong>phân biệt</strong>, trả về <em>số bộ </em><code>(a, b, c, d)</code><em> sao cho </em><code>a * b = c * d</code><em>, trong đó </em><code>a</code><em>, </em><code>b</code><em>, </em><code>c</code><em> và </em><code>d</code><em> là các phần tử của </em><code>nums</code><em> và </em><code>a != b != c != d</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,4,6]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Có 8 bộ hợp lệ:
(2,6,3,4) , (2,6,4,3) , (6,2,3,4) , (6,2,4,3)
(3,4,2,6) , (4,3,2,6) , (3,4,6,2) , (4,3,6,2)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,4,5,10]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Có 16 bộ hợp lệ:
(1,10,2,5) , (1,10,5,2) , (10,1,2,5) , (10,1,5,2)
(2,5,1,10) , (2,5,10,1) , (5,2,1,10) , (5,2,10,1)
(2,10,4,5) , (2,10,5,4) , (10,2,4,5) , (10,2,5,4)
(4,5,2,10) , (4,5,10,2) , (5,4,2,10) , (5,4,10,2)
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li>Tất cả phần tử trong <code>nums</code> đều <strong>phân biệt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổ hợp + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Với các bộ phân biệt thỏa mãn $a\cdot b=c\cdot d$: nếu một tích xuất hiện từ $v$ cặp, các cặp này tạo thành $\binom{v}{2}$ cặp-của-cặp, mỗi cặp cho $8$ bộ có thứ tự. $n\le 1000$ cho phép liệt kê mọi cặp không thứ tự.
>
> Đếm số cặp theo từng tích trong hash map rồi cộng $v(v-1)/2$ dịch trái ba bit.

<!-- thinking:end -->

Giả sử có $n$ cặp số, với bất kỳ hai cặp số $a, b$ và $c, d$ thỏa mãn điều kiện $a \times b = c \times d$, tổng số tổ hợp như vậy là $\mathrm{C}_n^2 = \frac{n \times (n-1)}{2}$.

Theo mô tả bài toán, mỗi tổ hợp thỏa mãn điều kiện trên có thể tạo thành $8$ bộ thỏa mãn yêu cầu. Vì vậy, ta nhân số tổ hợp có cùng tích với $8$ (tương đương dịch trái $3$ bit) rồi cộng lại để thu được kết quả.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tupleSameProduct(self, nums: List[int]) -> int:
        cnt = defaultdict(int)
        for i in range(1, len(nums)):
            for j in range(i):
                x = nums[i] * nums[j]
                cnt[x] += 1
        return sum(v * (v - 1) // 2 for v in cnt.values()) << 3
```

#### Java

```java
class Solution {
    public int tupleSameProduct(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int i = 1; i < nums.length; ++i) {
            for (int j = 0; j < i; ++j) {
                int x = nums[i] * nums[j];
                cnt.merge(x, 1, Integer::sum);
            }
        }
        int ans = 0;
        for (int v : cnt.values()) {
            ans += v * (v - 1) / 2;
        }
        return ans << 3;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int tupleSameProduct(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int i = 1; i < nums.size(); ++i) {
            for (int j = 0; j < i; ++j) {
                int x = nums[i] * nums[j];
                ++cnt[x];
            }
        }
        int ans = 0;
        for (auto& [_, v] : cnt) {
            ans += v * (v - 1) / 2;
        }
        return ans << 3;
    }
};
```

#### Go

```go
func tupleSameProduct(nums []int) int {
	cnt := map[int]int{}
	for i := 1; i < len(nums); i++ {
		for j := 0; j < i; j++ {
			x := nums[i] * nums[j]
			cnt[x]++
		}
	}
	ans := 0
	for _, v := range cnt {
		ans += v * (v - 1) / 2
	}
	return ans << 3
}
```

#### TypeScript

```ts
function tupleSameProduct(nums: number[]): number {
    const cnt: Map<number, number> = new Map();
    for (let i = 1; i < nums.length; ++i) {
        for (let j = 0; j < i; ++j) {
            const x = nums[i] * nums[j];
            cnt.set(x, (cnt.get(x) ?? 0) + 1);
        }
    }
    let ans = 0;
    for (const [_, v] of cnt) {
        ans += (v * (v - 1)) / 2;
    }
    return ans << 3;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn tuple_same_product(nums: Vec<i32>) -> i32 {
        let mut cnt: HashMap<i32, i32> = HashMap::new();
        let mut ans = 0;

        for i in 1..nums.len() {
            for j in 0..i {
                let x = nums[i] * nums[j];
                *cnt.entry(x).or_insert(0) += 1;
            }
        }

        for v in cnt.values() {
            ans += (v * (v - 1)) / 2;
        }

        ans << 3
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
