---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k)

[中文文档](/solution/0500-0599/0560.Subarray%20Sum%20Equals%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về <em>tổng số mảng con có tổng bằng</em> <code>k</code>.</p>

<p>Mảng con là một dãy phần tử <strong>không rỗng</strong> liên tiếp trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [1,1,1], k = 2
<strong>Đầu ra:</strong> 2
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [1,2,3], k = 3
<strong>Đầu ra:</strong> 2
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>-10<sup>7</sup> &lt;= k &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + Prefix sum

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các mảng con có tổng bằng $k$. Thử mọi cặp điểm đầu cuối sẽ tốn $O(n^2)$, không phù hợp khi $n \le 2 \times 10^4$.
>
> Với prefix sum $s$, cần đếm các prefix trước đó có tổng bằng $s-k$. Hash map lưu tần suất prefix sum và được khởi tạo với $\textit{cnt}[0]=1$, tương ứng prefix rỗng. Tra cứu trước rồi mới thêm prefix hiện tại để không dùng lại cùng chỉ số.

<!-- thinking:end -->

Ta dùng hash table `cnt` để lưu số lần xuất hiện của mỗi prefix sum trong mảng `nums`. Ban đầu, đặt `cnt[0]` bằng `1`, nghĩa là prefix sum `0` xuất hiện một lần.

Duyệt mảng `nums`, tính prefix sum `s`, cộng `cnt[s - k]` vào đáp án rồi tăng `cnt[s]` thêm `1`.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là `O(n)` và độ phức tạp không gian là `O(n)`, trong đó `n` là độ dài của mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        cnt = Counter({0: 1})
        ans = s = 0
        for x in nums:
            s += x
            ans += cnt[s - k]
            cnt[s] += 1
        return ans
```

#### Java

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        cnt.put(0, 1);
        int ans = 0, s = 0;
        for (int x : nums) {
            s += x;
            ans += cnt.getOrDefault(s - k, 0);
            cnt.merge(s, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> cnt{{0, 1}};
        int ans = 0, s = 0;
        for (int x : nums) {
            s += x;
            ans += cnt[s - k];
            ++cnt[s];
        }
        return ans;
    }
};
```

#### Go

```go
func subarraySum(nums []int, k int) (ans int) {
	cnt := map[int]int{0: 1}
	s := 0
	for _, x := range nums {
		s += x
		ans += cnt[s-k]
		cnt[s]++
	}
	return
}
```

#### TypeScript

```ts
function subarraySum(nums: number[], k: number): number {
    const cnt: Map<number, number> = new Map();
    cnt.set(0, 1);
    let [ans, s] = [0, 0];
    for (const x of nums) {
        s += x;
        ans += cnt.get(s - k) || 0;
        cnt.set(s, (cnt.get(s) || 0) + 1);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn subarray_sum(nums: Vec<i32>, k: i32) -> i32 {
        let mut cnt = HashMap::new();
        cnt.insert(0, 1);
        let mut ans = 0;
        let mut s = 0;
        for &x in &nums {
            s += x;
            if let Some(&v) = cnt.get(&(s - k)) {
                ans += v;
            }
            *cnt.entry(s).or_insert(0) += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
