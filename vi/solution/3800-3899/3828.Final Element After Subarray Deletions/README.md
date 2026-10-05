---
comments: true
difficulty: Medium
rating: 1591
source: Weekly Contest 487 Q2
tags:
    - Brainteaser
    - Array
    - Math
    - Game Theory
---

<!-- problem:start -->

# [3828. Final Element After Subarray Deletions](https://leetcode.com/problems/final-element-after-subarray-deletions)

[中文文档](/solution/3800-3899/3828.Final%20Element%20After%20Subarray%20Deletions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Alice và Bob chơi một trò chơi theo lượt, trong đó Alice đi trước.</p>

<ul>
	<li>Trong mỗi lượt, người chơi hiện tại chọn một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> bất kỳ <code>nums[l..r]</code> sao cho <code>r - l + 1 &lt; m</code>, trong đó <code>m</code> là <strong>độ dài hiện tại</strong> của mảng.</li>
	<li><strong>Mảng con được chọn sẽ bị xóa</strong>, các phần tử còn lại được <strong>nối lại</strong> để tạo thành mảng mới.</li>
	<li>Trò chơi tiếp tục cho đến khi <strong>chỉ còn lại một</strong> phần tử.</li>
</ul>

<p>Alice muốn <strong>tối đa hóa</strong> phần tử cuối cùng, còn Bob muốn <strong>tối thiểu hóa</strong> nó. Nếu cả hai đều chơi tối ưu, hãy trả về giá trị của phần tử còn lại cuối cùng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,5,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chiến lược tối ưu hợp lệ:</p>

<ul>
	<li>Alice xóa <code>[1]</code>, mảng trở thành <code>[5, 2]</code>.</li>
	<li>Bob xóa <code>[5]</code>, mảng trở thành <code>[2]</code>​​​​​​​. Vì vậy, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Alice xóa <code>[3]</code>, để lại mảng <code>[7]</code>. Vì Bob không thể đi lượt tiếp theo, đáp án là 7.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố mẹo

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt xóa một mảng con không phải toàn bộ mảng. Alice muốn tối đa hóa còn Bob muốn tối thiểu hóa giá trị cuối cùng. Với $n \le 10^5$, không thể duyệt cây trò chơi.
>
> Alice có thể xóa toàn bộ phần giữa ngay ở lượt đầu tiên và giữ lại một trong hai đầu mảng, nên đáp án ít nhất là giá trị lớn hơn trong hai đầu mảng.
>
> Bất kỳ giá trị nào ở giữa mà chưa phải phần tử cuối cùng đều có thể bị Bob xóa ở một lượt sau, nên Alice không thể đảm bảo giữ lại giá trị đó.
>
> Khi cả hai chơi tối ưu, kết quả chính xác là $\max(nums[0],nums[n-1])$.

<!-- thinking:end -->

Vì Alice đi trước, Alice có thể chọn xóa tất cả các phần tử ngoại trừ phần tử đầu tiên và phần tử cuối cùng, nên đáp án ít nhất là $\max(nums[0], nums[n - 1])$.

Đối với các phần tử ở các chỉ số $1, 2, ..., n-2$ (các phần tử ở giữa), ngay cả khi Alice muốn giữ lại một trong các phần tử này, Bob vẫn có thể chọn xóa nó, nên đáp án nhiều nhất là $\max(nums[0], nums[n - 1])$.

Do đó, đáp án chính xác là $\max(nums[0], nums[n - 1])$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def finalElement(self, nums: List[int]) -> int:
        return max(nums[0], nums[-1])
```

#### Java

```java
class Solution {
    public int finalElement(int[] nums) {
        return Math.max(nums[0], nums[nums.length - 1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int finalElement(vector<int>& nums) {
        return max(nums[0], nums.back());
    }
};
```

#### Go

```go
func finalElement(nums []int) int {
	return max(nums[0], nums[len(nums)-1])
}
```

#### TypeScript

```ts
function finalElement(nums: number[]): number {
    return Math.max(nums.at(0)!, nums.at(-1)!);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
