---
comments: true
difficulty: Medium
rating: 1687
source: Weekly Contest 429 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3397. Maximum Number of Distinct Elements After Operations](https://leetcode.com/problems/maximum-number-of-distinct-elements-after-operations)

[中文文档](/solution/3300-3399/3397.Maximum%20Number%20of%20Distinct%20Elements%20After%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Bạn được phép thực hiện <strong>thao tác</strong> sau đây trên mỗi phần tử của mảng <strong>không quá</strong> <em>một lần</em>:</p>

<ul>
	<li>Cộng vào phần tử một số nguyên trong khoảng <code>[-k, k]</code>.</li>
</ul>

<p>Trả về số lượng phần tử <strong>phân biệt</strong> <strong>lớn nhất</strong> có thể có trong <code>nums</code> sau khi thực hiện các <strong>thao tác</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,3,3,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> trở thành <code>[-1, 0, 1, 2, 3, 4]</code> sau khi thực hiện các thao tác trên bốn phần tử đầu tiên.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4,4,4], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bằng cách cộng -1 vào <code>nums[0]</code> và 1 vào <code>nums[1]</code>, <code>nums</code> trở thành <code>[3, 5, 4, 4]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi $x$ có thể trở thành một số nguyên bất kỳ trong $[x-k,x+k]$; mục tiêu là tạo ra nhiều giá trị phân biệt nhất có thể. Vì $n \le 10^5$, ta sắp xếp mảng rồi lần lượt chọn số nguyên nhỏ nhất vẫn còn có thể sử dụng.
>
> $\textit{pre}$ là giá trị cuối cùng đã dùng. Ta đưa $x$ về $\min(x+k,\max(x-k,\textit{pre}+1))$ và đếm nếu giá trị đó vẫn lớn hơn $\textit{pre}$.
>
> Việc ưu tiên các số nhỏ hơn sẽ để lại nhiều khoảng trống hơn cho các khoảng lớn hơn về sau, đó là lý do ta sắp xếp mảng.

<!-- thinking:end -->

Ta có thể sắp xếp mảng $\textit{nums}$, sau đó xét từng phần tử $x$ từ trái sang phải.

Với phần tử đầu tiên, ta có thể đổi nó thành $x - k$ một cách tham lam, đưa $x$ về giá trị nhỏ nhất để dành nhiều khoảng trống hơn cho các phần tử tiếp theo. Ta dùng biến $\textit{pre}$ để theo dõi giá trị lớn nhất trong các phần tử đã sử dụng, ban đầu là âm vô cùng.

Với mỗi phần tử $x$ tiếp theo, ta có thể đổi nó thành $\min(x + k, \max(x - k, \textit{pre} + 1))$. Ở đây, $\max(x - k, \textit{pre} + 1)$ nghĩa là ta cố gắng đưa $x$ về giá trị nhỏ nhất nhưng không nhỏ hơn $\textit{pre} + 1$. Nếu giá trị này tồn tại và nhỏ hơn $x + k$, ta có thể đổi $x$ thành giá trị đó, tăng số lượng phần tử phân biệt lên một, đồng thời cập nhật $\textit{pre}$ thành giá trị đó.

Sau khi duyệt qua toàn bộ mảng, ta thu được số lượng phần tử phân biệt lớn nhất.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistinctElements(self, nums: List[int], k: int) -> int:
        nums.sort()
        ans = 0
        pre = -inf
        for x in nums:
            cur = min(x + k, max(x - k, pre + 1))
            if cur > pre:
                ans += 1
                pre = cur
        return ans
```

#### Java

```java
class Solution {
    public int maxDistinctElements(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int ans = 0, pre = Integer.MIN_VALUE;
        for (int x : nums) {
            int cur = Math.min(x + k, Math.max(x - k, pre + 1));
            if (cur > pre) {
                ++ans;
                pre = cur;
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
    int maxDistinctElements(vector<int>& nums, int k) {
        ranges::sort(nums);
        int ans = 0, pre = INT_MIN;
        for (int x : nums) {
            int cur = min(x + k, max(x - k, pre + 1));
            if (cur > pre) {
                ++ans;
                pre = cur;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxDistinctElements(nums []int, k int) (ans int) {
	sort.Ints(nums)
	pre := math.MinInt32
	for _, x := range nums {
		cur := min(x+k, max(x-k, pre+1))
		if cur > pre {
			ans++
			pre = cur
		}
	}
	return
}
```

#### TypeScript

```ts
function maxDistinctElements(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    let [ans, pre] = [0, -Infinity];
    for (const x of nums) {
        const cur = Math.min(x + k, Math.max(x - k, pre + 1));
        if (cur > pre) {
            ++ans;
            pre = cur;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_distinct_elements(mut nums: Vec<i32>, k: i32) -> i32 {
        nums.sort();
        let mut ans = 0;
        let mut pre = i32::MIN;

        for &x in &nums {
            let cur = (x + k).min((x - k).max(pre + 1));
            if cur > pre {
                ans += 1;
                pre = cur;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
