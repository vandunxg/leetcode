---
comments: true
difficulty: Medium
rating: 1370
source: Weekly Contest 468 Q2
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3689. Maximum Total Subarray Value I](https://leetcode.com/problems/maximum-total-subarray-value-i)

[中文文档](/solution/3600-3699/3689.Maximum%20Total%20Subarray%20Value%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Bạn cần chọn <strong>chính xác</strong> <code>k</code> <span data-keyword="subarray-nonempty">mảng con</span> không rỗng <code>nums[l..r]</code> của <code>nums</code>. Các mảng con có thể chồng lấn, và có thể chọn cùng một mảng con (cùng <code>l</code> và <code>r</code>) <strong>nhiều hơn một lần</strong>.</p>

<p><strong>Giá trị</strong> của một mảng con <code>nums[l..r]</code> được định nghĩa là: <code>max(nums[l..r]) - min(nums[l..r])</code>.</p>

<p><strong>Tổng giá trị</strong> là tổng các <strong>giá trị</strong> của tất cả các mảng con đã chọn.</p>

<p>Trả về tổng giá trị <strong>lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chọn tối ưu là:</p>

<ul>
	<li>Chọn <code>nums[0..1] = [1, 3]</code>. Giá trị lớn nhất là 3 và giá trị nhỏ nhất là 1, nên giá trị của mảng con là <code>3 - 1 = 2</code>.</li>
	<li>Chọn <code>nums[0..2] = [1, 3, 2]</code>. Giá trị lớn nhất vẫn là 3 và giá trị nhỏ nhất vẫn là 1, nên giá trị cũng là <code>3 - 1 = 2</code>.</li>
</ul>

<p>Cộng hai giá trị này ta được <code>2 + 2 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,5,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chọn tối ưu là:</p>

<ul>
	<li>Chọn <code>nums[0..3] = [4, 2, 5, 1]</code>. Giá trị lớn nhất là 5 và giá trị nhỏ nhất là 1, nên giá trị của mảng con là <code>5 - 1 = 4</code>.</li>
	<li>Chọn <code>nums[0..3] = [4, 2, 5, 1]</code>. Giá trị lớn nhất là 5 và giá trị nhỏ nhất là 1, nên giá trị cũng là <code>4</code>.</li>
	<li>Chọn <code>nums[2..3] = [5, 1]</code>. Giá trị lớn nhất là 5 và giá trị nhỏ nhất là 1, nên giá trị một lần nữa là <code>4</code>.</li>
</ul>

<p>Cộng ba giá trị này ta được <code>4 + 4 + 4 = 12</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 5 * 10<sup>​​​​​​​4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quan sát đơn giản

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị của một mảng con là giá trị lớn nhất trừ đi giá trị nhỏ nhất. Ta chọn $k$ mảng con (có thể chồng lấn). Không mảng con nào có giá trị vượt quá $\max-\min$ trên toàn mảng, và mọi đoạn chứa cả hai cực trị đều đạt giới hạn đó.
>
> Vì vậy, chọn đoạn chứa cả hai cực trị $k$ lần sẽ tạo ra $k$ bản sao của $\max-\min$.
>
> Đáp án là $k\cdot(\max(\textit{nums})-\min(\textit{nums}))$.

<!-- thinking:end -->

Ta có thể nhận thấy rằng giá trị của một mảng con chỉ phụ thuộc vào giá trị lớn nhất và nhỏ nhất trên toàn mảng. Vì vậy, ta chỉ cần tìm giá trị lớn nhất và nhỏ nhất trên toàn mảng, sau đó nhân hiệu của chúng với $k$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTotalValue(self, nums: List[int], k: int) -> int:
        return k * (max(nums) - min(nums))
```

#### Java

```java
class Solution {
    public long maxTotalValue(int[] nums, int k) {
        int mx = 0, mn = 1 << 30;
        for (int x : nums) {
            mx = Math.max(mx, x);
            mn = Math.min(mn, x);
        }
        return 1L * k * (mx - mn);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxTotalValue(vector<int>& nums, int k) {
        auto [mn, mx] = minmax_element(nums.begin(), nums.end());
        return 1LL * k * (*mx - *mn);
    }
};
```

#### Go

```go
func maxTotalValue(nums []int, k int) int64 {
	return int64(k * (slices.Max(nums) - slices.Min(nums)))
}
```

#### TypeScript

```ts
function maxTotalValue(nums: number[], k: number): number {
    const mn = Math.min(...nums);
    const mx = Math.max(...nums);
    return k * (mx - mn);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
