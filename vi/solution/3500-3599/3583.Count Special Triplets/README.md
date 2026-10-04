---
comments: true
difficulty: Medium
rating: 1509
source: Weekly Contest 454 Q2
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3583. Count Special Triplets](https://leetcode.com/problems/count-special-triplets)

[中文文档](/solution/3500-3599/3583.Count%20Special%20Triplets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong>bộ ba đặc biệt</strong> được định nghĩa là một bộ ba chỉ số <code>(i, j, k)</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; k &lt; n</code>, trong đó <code>n = nums.length</code></li>
	<li><code>nums[i] == nums[j] * 2</code></li>
	<li><code>nums[k] == nums[j] * 2</code></li>
</ul>

<p>Trả về tổng số <strong>bộ ba đặc biệt</strong> trong mảng.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,3,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bộ ba đặc biệt duy nhất là <code>(i, j, k) = (0, 1, 2)</code>, trong đó:</p>

<ul>
	<li><code>nums[0] = 6</code>, <code>nums[1] = 3</code>, <code>nums[2] = 6</code></li>
	<li><code>nums[0] = nums[1] * 2 = 3 * 2 = 6</code></li>
	<li><code>nums[2] = nums[1] * 2 = 3 * 2 = 6</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bộ ba đặc biệt duy nhất là <code>(i, j, k) = (0, 2, 3)</code>, trong đó:</p>

<ul>
	<li><code>nums[0] = 0</code>, <code>nums[2] = 0</code>, <code>nums[3] = 0</code></li>
	<li><code>nums[0] = nums[2] * 2 = 0 * 2 = 0</code></li>
	<li><code>nums[3] = nums[2] * 2 = 0 * 2 = 0</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,4,2,8,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có đúng hai bộ ba đặc biệt:</p>

<ul>
	<li><code>(i, j, k) = (0, 1, 3)</code>

    <ul>
        <li><code>nums[0] = 8</code>, <code>nums[1] = 4</code>, <code>nums[3] = 8</code></li>
        <li><code>nums[0] = nums[1] * 2 = 4 * 2 = 8</code></li>
        <li><code>nums[3] = nums[1] * 2 = 4 * 2 = 8</code></li>
    </ul>
    </li>
    <li><code>(i, j, k) = (1, 2, 4)</code>
    <ul>
        <li><code>nums[1] = 4</code>, <code>nums[2] = 2</code>, <code>nums[4] = 4</code></li>
        <li><code>nums[1] = nums[2] * 2 = 2 * 2 = 4</code></li>
        <li><code>nums[4] = nums[2] * 2 = 2 * 2 = 4</code></li>
    </ul>
    </li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt số ở giữa + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một bộ ba đặc biệt có $nums[i]=nums[k]=2\cdot nums[j]$. Việc liệt kê hai đầu sẽ có độ phức tạp bậc hai. Ta cố định vị trí giữa $j$ và nhân số lần xuất hiện của $2x$ ở mỗi phía.
>
> Đưa mọi giá trị vào $\textit{right}$. Duyệt $x$ từ trái sang phải: giảm số lần xuất hiện của nó ở bên phải, cộng $\textit{left}[2x]\cdot\textit{right}[2x]$, sau đó tăng số lần xuất hiện ở bên trái. Lấy kết quả theo modulo $10^9+7$.

<!-- thinking:end -->

Ta có thể duyệt qua số ở giữa $\textit{nums}[j]$, đồng thời dùng hai hash table, $\textit{left}$ và $\textit{right}$, để lần lượt ghi lại số lần xuất hiện của các số ở bên trái và bên phải của $\textit{nums}[j]$.

Đầu tiên, ta thêm tất cả các số vào $\textit{right}$. Sau đó, ta duyệt từng số $\textit{nums}[j]$ từ trái sang phải. Trong quá trình duyệt:

1. Xóa $\textit{nums}[j]$ khỏi $\textit{right}$.
2. Đếm số lần xuất hiện của số $\textit{nums}[i] = \textit{nums}[j] * 2$ ở bên trái của $\textit{nums}[j]$, ký hiệu là $\textit{left}[\textit{nums}[j] * 2]$.
3. Đếm số lần xuất hiện của số $\textit{nums}[k] = \textit{nums}[j] * 2$ ở bên phải của $\textit{nums}[j]$, ký hiệu là $\textit{right}[\textit{nums}[j] * 2]$.
4. Nhân $\textit{left}[\textit{nums}[j] * 2]$ và $\textit{right}[\textit{nums}[j] * 2]$ để thu được số bộ ba đặc biệt có $\textit{nums}[j]$ là số ở giữa, rồi cộng kết quả vào đáp án.
5. Thêm $\textit{nums}[j]$ vào $\textit{left}$.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def specialTriplets(self, nums: List[int]) -> int:
        left = Counter()
        right = Counter(nums)
        ans = 0
        mod = 10**9 + 7
        for x in nums:
            right[x] -= 1
            ans = (ans + left[x * 2] * right[x * 2] % mod) % mod
            left[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public int specialTriplets(int[] nums) {
        Map<Integer, Integer> left = new HashMap<>();
        Map<Integer, Integer> right = new HashMap<>();
        for (int x : nums) {
            right.merge(x, 1, Integer::sum);
        }
        long ans = 0;
        final int mod = (int) 1e9 + 7;
        for (int x : nums) {
            right.merge(x, -1, Integer::sum);
            ans = (ans + 1L * left.getOrDefault(x * 2, 0) * right.getOrDefault(x * 2, 0) % mod)
                % mod;
            left.merge(x, 1, Integer::sum);
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int specialTriplets(vector<int>& nums) {
        unordered_map<int, int> left, right;
        for (int x : nums) {
            right[x]++;
        }
        long long ans = 0;
        const int mod = 1e9 + 7;
        for (int x : nums) {
            right[x]--;
            ans = (ans + 1LL * left[x * 2] * right[x * 2] % mod) % mod;
            left[x]++;
        }
        return (int) ans;
    }
};
```

#### Go

```go
func specialTriplets(nums []int) int {
	left := make(map[int]int)
	right := make(map[int]int)
	for _, x := range nums {
		right[x]++
	}
	ans := int64(0)
	mod := int64(1e9 + 7)
	for _, x := range nums {
		right[x]--
		ans = (ans + int64(left[x*2])*int64(right[x*2])%mod) % mod
		left[x]++
	}
	return int(ans)
}
```

#### TypeScript

```ts
function specialTriplets(nums: number[]): number {
    const left = new Map<number, number>();
    const right = new Map<number, number>();
    for (const x of nums) {
        right.set(x, (right.get(x) || 0) + 1);
    }
    let ans = 0;
    const mod = 1e9 + 7;
    for (const x of nums) {
        right.set(x, (right.get(x) || 0) - 1);
        const lx = left.get(x * 2) || 0;
        const rx = right.get(x * 2) || 0;
        ans = (ans + ((lx * rx) % mod)) % mod;
        left.set(x, (left.get(x) || 0) + 1);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn special_triplets(nums: Vec<i32>) -> i32 {
        let mut left: HashMap<i32, i64> = HashMap::new();
        let mut right: HashMap<i32, i64> = HashMap::new();

        for &x in &nums {
            *right.entry(x).or_insert(0) += 1;
        }

        let modulo: i64 = 1_000_000_007;
        let mut ans: i64 = 0;

        for &x in &nums {
            if let Some(v) = right.get_mut(&x) {
                *v -= 1;
            }

            let t = x * 2;

            let l = *left.get(&t).unwrap_or(&0);
            let r = *right.get(&t).unwrap_or(&0);

            ans = (ans + (l * r) % modulo) % modulo;

            *left.entry(x).or_insert(0) += 1;
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
