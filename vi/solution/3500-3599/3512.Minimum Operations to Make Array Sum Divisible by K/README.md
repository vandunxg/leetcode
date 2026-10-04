---
comments: true
difficulty: Easy
rating: 1228
source: Biweekly Contest 154 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3512. Minimum Operations to Make Array Sum Divisible by K](https://leetcode.com/problems/minimum-operations-to-make-array-sum-divisible-by-k)

[中文文档](/solution/3500-3599/3512.Minimum%20Operations%20to%20Make%20Array%20Sum%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Bạn có thể thực hiện thao tác sau bất kỳ số lần nào:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> và thay <code>nums[i]</code> bằng <code>nums[i] - 1</code>.</li>
</ul>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để tổng các phần tử trong mảng chia hết cho <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,9,7], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thực hiện 4 thao tác trên <code>nums[1] = 9</code>. Khi đó, <code>nums = [3, 5, 7]</code>.</li>
	<li>Tổng là 15, chia hết cho 5.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1,3], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng là 8, đã chia hết cho 4. Vì vậy, không cần thực hiện thao tác nào.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thực hiện 3 thao tác trên <code>nums[0] = 3</code> và 2 thao tác trên <code>nums[1] = 2</code>. Khi đó, <code>nums = [0, 0]</code>.</li>
	<li>Tổng là 0, chia hết cho 6.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng và Modulo

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác làm giảm một phần tử đi $1$, nên tổng cũng giảm $1$. Sau đúng $S \bmod k$ thao tác, tổng sẽ chia hết cho $k$.
>
> Chỉ cần duyệt một lượt để tính tổng và lấy modulo $k$; không cần mô phỏng từng lần giảm.

<!-- thinking:end -->

Bài toán thực chất yêu cầu phần dư của tổng các phần tử trong mảng khi chia cho $k$. Do đó, ta chỉ cần duyệt qua mảng, tính tổng tất cả các phần tử rồi lấy modulo $k$. Cuối cùng, trả về kết quả này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        return sum(nums) % k
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int k) {
        return Arrays.stream(nums).sum() % k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int k) {
        return reduce(nums.begin(), nums.end(), 0) % k;
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) (ans int) {
	for _, x := range nums {
		ans = (ans + x) % k
	}
	return
}
```

#### TypeScript

```ts
function minOperations(nums: number[], k: number): number {
    return nums.reduce((acc, x) => acc + x, 0) % k;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums: Vec<i32>, k: i32) -> i32 {
        nums.iter().sum::<i32>() % k
    }
}
```

#### C#

```cs
public class Solution {
    public int MinOperations(int[] nums, int k) {
        return nums.Sum() % k;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
