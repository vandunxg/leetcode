---
comments: true
difficulty: Medium
rating: 1622
source: Biweekly Contest 84 Q2
tags:
    - Array
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [2364. Count Number of Bad Pairs](https://leetcode.com/problems/count-number-of-bad-pairs)

[中文文档](/solution/2300-2399/2364.Count%20Number%20of%20Bad%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code>. Một cặp chỉ số <code>(i, j)</code> là một <strong>cặp xấu</strong> nếu <code>i &lt; j</code> và <code>j - i != nums[j] - nums[i]</code>.</p>

<p>Trả về <em>tổng số <strong>cặp xấu</strong> trong </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [4,1,3,3]
<strong>Output:</strong> 5
<strong>Explanation:</strong> Cặp (0, 1) là một cặp xấu vì 1 - 0 != 1 - 4.
Cặp (0, 2) là một cặp xấu vì 2 - 0 != 3 - 4, 2 != -1.
Cặp (0, 3) là một cặp xấu vì 3 - 0 != 3 - 4, 3 != -1.
Cặp (1, 2) là một cặp xấu vì 2 - 1 != 3 - 1, 1 != 2.
Cặp (2, 3) là một cặp xấu vì 3 - 2 != 3 - 3, 1 != 0.
Có tổng cộng 5 cặp xấu, nên ta trả về 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,4,5]
<strong>Output:</strong> 0
<strong>Explanation:</strong> Không có cặp xấu nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Biến đổi phương trình + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp xấu có $j-i \ne nums[j]-nums[i]$, tức là $i-nums[i] \ne j-nums[j]$. Vì $n \le 10^5$, ta đếm thông qua các cặp tốt bổ sung.
>
> Quét từ trái sang phải: chỉ số $i$ đóng góp $i$ trừ đi số lượng trước đó của $i-nums[i]$ cặp xấu, sau đó tăng key đó lên. Việc đếm trên prefix giúp tránh vòng lặp kép.

<!-- thinking:end -->

Theo mô tả đề bài, với mọi $i \lt j$, nếu $j - i \neq \textit{nums}[j] - \textit{nums}[i]$, thì $(i, j)$ là một cặp xấu.

Ta có thể biến đổi phương trình thành $i - \textit{nums}[i] \neq j - \textit{nums}[j]$. Điều này gợi ý sử dụng một hash table $cnt$ để đếm số lần xuất hiện của $i - \textit{nums}[i]$.

Trong khi duyệt mảng, với phần tử hiện tại $\textit{nums}[i]$, ta cộng $i - cnt[i - \textit{nums}[i]]$ vào đáp án, sau đó tăng số lượng của $i - \textit{nums}[i]$ lên $1$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBadPairs(self, nums: List[int]) -> int:
        cnt = Counter()
        ans = 0
        for i, x in enumerate(nums):
            ans += i - cnt[i - x]
            cnt[i - x] += 1
        return ans
```

#### Java

```java
class Solution {
    public long countBadPairs(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        long ans = 0;
        for (int i = 0; i < nums.length; ++i) {
            int x = i - nums[i];
            ans += i - cnt.merge(x, 1, Integer::sum) + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countBadPairs(vector<int>& nums) {
        unordered_map<int, int> cnt;
        long long ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            int x = i - nums[i];
            ans += i - cnt[x]++;
        }
        return ans;
    }
};
```

#### Go

```go
func countBadPairs(nums []int) (ans int64) {
	cnt := map[int]int{}
	for i, x := range nums {
		x = i - x
		ans += int64(i - cnt[x])
		cnt[x]++
	}
	return
}
```

#### TypeScript

```ts
function countBadPairs(nums: number[]): number {
    const cnt = new Map<number, number>();
    let ans = 0;
    for (let i = 0; i < nums.length; ++i) {
        const x = i - nums[i];
        ans += i - (cnt.get(x) ?? 0);
        cnt.set(x, (cnt.get(x) ?? 0) + 1);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn count_bad_pairs(nums: Vec<i32>) -> i64 {
        let mut cnt: HashMap<i32, i64> = HashMap::new();
        let mut ans: i64 = 0;
        for (i, &num) in nums.iter().enumerate() {
            let x = i as i32 - num;
            let count = *cnt.get(&x).unwrap_or(&0);
            ans += i as i64 - count;
            *cnt.entry(x).or_insert(0) += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
