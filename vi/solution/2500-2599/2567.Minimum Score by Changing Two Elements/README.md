---
comments: true
difficulty: Medium
rating: 1608
source: Biweekly Contest 98 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2567. Minimum Score by Changing Two Elements](https://leetcode.com/problems/minimum-score-by-changing-two-elements)

[中文文档](/solution/2500-2599/2567.Minimum%20Score%20by%20Changing%20Two%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<ul>
	<li>Điểm <strong>thấp</strong> của <code>nums</code> là hiệu tuyệt đối <strong>nhỏ nhất</strong> giữa hai số nguyên bất kỳ.</li>
	<li>Điểm <strong>cao</strong> của <code>nums</code> là hiệu tuyệt đối <strong>lớn nhất</strong> giữa hai số nguyên bất kỳ.</li>
	<li><strong>Điểm số</strong> của <code>nums</code> là tổng của điểm <strong>cao</strong> và điểm <strong>thấp</strong>.</li>
</ul>

<p>Trả về <strong>điểm số nhỏ nhất</strong> sau khi <strong>thay đổi hai phần tử</strong> của <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,7,8,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay đổi <code>nums[0]</code> và <code>nums[1]</code> thành 6 để <code>nums</code> trở thành [6,6,7,8,5].</li>
	<li>Điểm thấp là hiệu tuyệt đối nhỏ nhất: |6 - 6| = 0.</li>
	<li>Điểm cao là hiệu tuyệt đối lớn nhất: |8 - 5| = 3.</li>
	<li>Tổng của điểm cao và điểm thấp là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay đổi <code>nums[1]</code> và <code>nums[2]</code> thành 1 để <code>nums</code> trở thành [1,1,1].</li>
	<li>Tổng của hiệu tuyệt đối lớn nhất và hiệu tuyệt đối nhỏ nhất là 0.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số là khoảng cách nhỏ nhất giữa hai phần tử kề nhau cộng với độ rộng của mảng. Ta có thể thay đổi tối đa hai giá trị. Sao chép một giá trị hiện có sẽ đưa điểm thấp về $0$, khi đó chỉ còn lại độ rộng.
>
> Sau khi sắp xếp, hai lần thay đổi tương đương với việc loại bỏ các phần tử biên ở hai đầu. Ba khả năng — bỏ hai phần tử nhỏ nhất, bỏ một phần tử ở mỗi đầu, hoặc bỏ hai phần tử lớn nhất — bao quát đáp án tối ưu; ta chọn độ rộng nhỏ nhất còn lại.

<!-- thinking:end -->

Từ đề bài, ta biết rằng điểm thấp thực chất là hiệu nhỏ nhất giữa hai phần tử kề nhau trong mảng đã sắp xếp, còn điểm cao là hiệu giữa phần tử đầu tiên và phần tử cuối cùng của mảng đã sắp xếp. Điểm số của mảng $nums$ là tổng của điểm thấp và điểm cao.

Vì vậy, trước tiên ta có thể sắp xếp mảng. Do đề bài cho phép thay đổi giá trị của tối đa hai phần tử trong mảng, ta có thể thay đổi một số để nó giống với một số khác trong mảng, đưa điểm thấp về $0$. Khi đó, điểm số của mảng $nums$ thực chất là điểm cao. Ta có thể chọn một trong các thay đổi sau:

Thay đổi hai số nhỏ nhất thành $nums[2]$, khi đó điểm cao là $nums[n - 1] - nums[2]$;

Thay đổi số nhỏ nhất thành $nums[1]$ và số lớn nhất thành $nums[n - 2]$, khi đó điểm cao là $nums[n - 2] - nums[1]$;

Thay đổi hai số lớn nhất thành $nums[n - 3]$, khi đó điểm cao là $nums[n - 3] - nums[0]$.

Cuối cùng, ta trả về điểm số nhỏ nhất trong ba thay đổi trên.

Độ phức tạp thời gian là $O(n \log n)$, độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $nums$.

Các bài tương tự:

-[1509. Chênh lệch nhỏ nhất giữa giá trị lớn nhất và nhỏ nhất trong ba lần di chuyển](https://github.com/doocs/leetcode/blob/main/solution/1500-1599/1509.Minimum%20Difference%20Between%20Largest%20and%20Smallest%20Value%20in%20Three%20Moves/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeSum(self, nums: List[int]) -> int:
        nums.sort()
        return min(nums[-1] - nums[2], nums[-2] - nums[1], nums[-3] - nums[0])
```

#### Java

```java
class Solution {
    public int minimizeSum(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int a = nums[n - 1] - nums[2];
        int b = nums[n - 2] - nums[1];
        int c = nums[n - 3] - nums[0];
        return Math.min(a, Math.min(b, c));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimizeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        return min({nums[n - 1] - nums[2], nums[n - 2] - nums[1], nums[n - 3] - nums[0]});
    }
};
```

#### Go

```go
func minimizeSum(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	return min(nums[n-1]-nums[2], min(nums[n-2]-nums[1], nums[n-3]-nums[0]))
}
```

#### TypeScript

```ts
function minimizeSum(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    return Math.min(nums[n - 3] - nums[0], nums[n - 2] - nums[1], nums[n - 1] - nums[2]);
}
```

#### Rust

```rust
impl Solution {
    pub fn minimize_sum(mut nums: Vec<i32>) -> i32 {
        nums.sort();
        let n = nums.len();
        (nums[n - 1] - nums[2])
            .min(nums[n - 2] - nums[1])
            .min(nums[n - 3] - nums[0])
    }
}
```

#### C

```c
#define min(a, b) (((a) < (b)) ? (a) : (b))

int cmp(const void* a, const void* b) {
    return *(int*) a - *(int*) b;
}

int minimizeSum(int* nums, int numsSize) {
    qsort(nums, numsSize, sizeof(int), cmp);
    return min(nums[numsSize - 1] - nums[2], min(nums[numsSize - 2] - nums[1], nums[numsSize - 3] - nums[0]));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
