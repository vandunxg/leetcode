---
comments: true
difficulty: Easy
rating: 1199
source: Weekly Contest 494 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3875. Construct Uniform Parity Array I](https://leetcode.com/problems/construct-uniform-parity-array-i)

[中文文档](/solution/3800-3899/3875.Construct%20Uniform%20Parity%20Array%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng gồm <code>n</code> số nguyên <strong>phân biệt</strong> <code>nums1</code>.</p>

<p>Bạn muốn xây dựng một mảng khác <code>nums2</code> có độ dài <code>n</code> sao cho các phần tử trong <code>nums2</code> đều là <strong>số lẻ hoặc đều là số chẵn</strong>.</p>

<p>Với mỗi chỉ số <code>i</code>, bạn phải chọn <strong>chính xác một</strong> trong các lựa chọn sau (theo bất kỳ thứ tự nào):</p>

<ul>
	<li><code>nums2[i] = nums1[i]</code></li>
	<li><code>nums2[i] = nums1[i] - nums1[j]</code>, với chỉ số <code>j != i</code></li>
</ul>

<p>Trả về <code>true</code> nếu có thể xây dựng một mảng như vậy, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>nums2[0] = nums1[0] - nums1[1] = 2 - 3 = -1</code>.</li>
	<li>Chọn <code>nums2[1] = nums1[1] = 3</code>.</li>
	<li><code>nums2 = [-1, 3]</code>, và cả hai phần tử đều là số lẻ. Do đó, đáp án là <code>true</code>​​​​​​​.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Chọn <code>nums2[0] = nums1[0] = 4</code>.</li>
	<li>Chọn <code>nums2[1] = nums1[1] = 6</code>.</li>
	<li><code>nums2 = [4, 6]</code>, và tất cả phần tử đều là số chẵn. Do đó, đáp án là <code>true</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums1.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums1[i] &lt;= 100</code></li>
	<li>Các số nguyên trong <code>nums1</code> là phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Brain Teaser

<!-- thinking:start -->

> **Tư duy**
>
> $nums2[i]$ có thể là $nums1[i]$ hoặc $nums1[i]-nums1[j]$ mà không có ràng buộc về tính dương, và $nums2$ phải gồm toàn số lẻ hoặc toàn số chẵn.
>
> Nếu mọi phần tử đã có cùng parity, ta sao chép $nums1$. Nếu xuất hiện cả hai parity, ta trừ một giá trị có parity đối lập; hiệu luôn là số lẻ, tạo thành một mảng toàn số lẻ.
>
> Cả hai trường hợp đều có thể xây dựng được, nên đáp án luôn là true.
>
> Không cần kiểm tra các giá trị cụ thể.

<!-- thinking:end -->

Nếu tất cả phần tử trong $\textit{nums1}$ đều là số lẻ hoặc đều là số chẵn, ta có thể đặt $\textit{nums2}$ bằng $\textit{nums1}$, khi đó điều kiện được thỏa mãn.

Nếu $\textit{nums1}$ chứa cả số lẻ và số chẵn, ta có thể đặt mỗi phần tử của $\textit{nums2}$ bằng phần tử hiện tại của $\textit{nums1}$ trừ đi một phần tử khác parity trong $\textit{nums1}$. Vì số lẻ trừ số chẵn và số chẵn trừ số lẻ đều cho kết quả là số lẻ, tất cả phần tử của $\textit{nums2}$ sẽ là số lẻ, thỏa mãn điều kiện.

Do đó, bất kể các phần tử trong $\textit{nums1}$ đều là số lẻ, đều là số chẵn hay là hỗn hợp cả hai, ta luôn có thể xây dựng một $\textit{nums2}$ hợp lệ. Vì vậy, đáp án luôn là $\text{true}$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniformArray(self, nums1: list[int]) -> bool:
        return True
```

#### Java

```java
class Solution {
    public boolean uniformArray(int[] nums1) {
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool uniformArray(vector<int>& nums1) {
        return true;
    }
};
```

#### Go

```go
func uniformArray(nums1 []int) bool {
	return true
}
```

#### TypeScript

```ts
function uniformArray(nums1: number[]): boolean {
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
