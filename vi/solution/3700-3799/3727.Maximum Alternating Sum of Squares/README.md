---
comments: true
difficulty: Medium
rating: 1454
source: Weekly Contest 473 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3727. Maximum Alternating Sum of Squares](https://leetcode.com/problems/maximum-alternating-sum-of-squares)

[中文文档](/solution/3700-3799/3727.Maximum%20Alternating%20Sum%20of%20Squares/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Bạn có thể <strong>sắp xếp lại các phần tử</strong> theo bất kỳ thứ tự nào.</p>

<p><strong>Điểm xen kẽ</strong> của một mảng <code>arr</code> được định nghĩa như sau:</p>

<ul>
	<li><code>score = arr[0]<sup>2</sup> - arr[1]<sup>2</sup> + arr[2]<sup>2</sup> - arr[3]<sup>2</sup> + ...</code></li>
</ul>

<p>Trả về một số nguyên biểu thị <strong>điểm xen kẽ lớn nhất có thể đạt được</strong> của <code>nums</code> sau khi sắp xếp lại các phần tử.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách sắp xếp lại <code>nums</code> là <code>[2,1,3]</code>, cho điểm xen kẽ lớn nhất trong tất cả các cách sắp xếp lại có thể.</p>

<p>Điểm xen kẽ được tính như sau:</p>

<p><code>score = 2<sup>2</sup> - 1<sup>2</sup> + 3<sup>2</sup> = 4 - 1 + 9 = 12</code></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-1,2,-2,3,-3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách sắp xếp lại <code>nums</code> là <code>[-3,-1,-2,1,3,2]</code>, cho điểm xen kẽ lớn nhất trong tất cả các cách sắp xếp lại có thể.</p>

<p>Điểm xen kẽ được tính như sau:</p>

<p><code>score = (-3)<sup>2</sup> - (-1)<sup>2</sup> + (-2)<sup>2</sup> - (1)<sup>2</sup> + (3)<sup>2</sup> - (2)<sup>2</sup> = 9 - 1 + 4 - 1 + 9 - 4 = 16</code></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-4 * 10<sup>4</sup> &lt;= nums[i] &lt;= 4 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Điểm xen kẽ chỉ phụ thuộc vào độ lớn bình phương và tính chẵn lẻ của chỉ số, không phụ thuộc vào dấu ban đầu. Các bình phương lớn hơn nên nằm ở những vị trí được cộng, còn các bình phương nhỏ hơn nằm ở những vị trí bị trừ; vì vậy, sau khi sắp xếp theo bình phương, hiệu giữa nửa sau và nửa đầu là tối ưu.

<!-- thinking:end -->

Ta có thể sắp xếp các phần tử của mảng theo giá trị bình phương, sau đó đặt các phần tử có bình phương lớn hơn ở các chỉ số chẵn và các phần tử có bình phương nhỏ hơn ở các chỉ số lẻ.

Điểm xen kẽ cuối cùng là tổng bình phương của các phần tử lớn hơn trừ đi tổng bình phương của các phần tử nhỏ hơn, tức là tổng bình phương của nửa sau trong mảng $\text{nums}$ đã sắp xếp trừ đi tổng bình phương của nửa đầu.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAlternatingSum(self, nums: List[int]) -> int:
        nums.sort(key=lambda x: x * x)
        n = len(nums)
        s1 = sum(x * x for x in nums[: n // 2])
        s2 = sum(x * x for x in nums[n // 2 :])
        return s2 - s1
```

#### Java

```java
class Solution {
    public long maxAlternatingSum(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            nums[i] *= nums[i];
        }
        Arrays.sort(nums);
        long ans = 0;
        int m = n / 2;
        for (int i = 0; i < m; ++i) {
            ans -= nums[i];
        }
        for (int i = m; i < n; ++i) {
            ans += nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxAlternatingSum(vector<int>& nums) {
        for (int& x : nums) {
            x = x * x;
        }
        ranges::sort(nums);
        long long ans = 0, m = nums.size() / 2;
        for (int i = 0; i < m; ++i) {
            ans -= nums[i];
        }
        for (int i = m; i < nums.size(); ++i) {
            ans += nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func maxAlternatingSum(nums []int) (ans int64) {
	for i, x := range nums {
		nums[i] *= x
	}
	slices.Sort(nums)
	m := len(nums) / 2
	for _, x := range nums[:m] {
		ans -= int64(x)
	}
	for _, x := range nums[m:] {
		ans += int64(x)
	}
	return
}
```

#### TypeScript

```ts
function maxAlternatingSum(nums: number[]): number {
    const n = nums.length;
    for (let i = 0; i < n; i++) {
        nums[i] = nums[i] ** 2;
    }
    nums.sort((a, b) => a - b);
    const m = Math.floor(n / 2);
    let ans = 0;
    for (let i = 0; i < m; i++) {
        ans -= nums[i];
    }
    for (let i = m; i < n; i++) {
        ans += nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
