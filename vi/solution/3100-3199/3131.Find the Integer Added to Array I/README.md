---
comments: true
difficulty: Easy
rating: 1160
source: Weekly Contest 395 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3131. Find the Integer Added to Array I](https://leetcode.com/problems/find-the-integer-added-to-array-i)

[Tài liệu tiếng Trung](/solution/3100-3199/3131.Find%20the%20Integer%20Added%20to%20Array%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng có cùng độ dài là <code>nums1</code> và <code>nums2</code>.</p>

<p>Mỗi phần tử trong <code>nums1</code> được tăng (hoặc giảm trong trường hợp số âm) một lượng nguyên, được biểu diễn bởi biến <code>x</code>.</p>

<p>Kết quả là <code>nums1</code> trở thành <strong>bằng</strong> <code>nums2</code>. Hai mảng được xem là <strong>bằng nhau</strong> khi chúng chứa cùng các số nguyên với cùng số lần xuất hiện.</p>

<p>Trả về số nguyên <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">nums1 = [2,6,4], nums2 = [9,7,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số nguyên được cộng vào mỗi phần tử của <code>nums1</code> là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">nums1 = [10], nums2 = [5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">-5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số nguyên được cộng vào mỗi phần tử của <code>nums1</code> là -5.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">nums1 = [1,1,1,1], nums2 = [1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số nguyên được cộng vào mỗi phần tử của <code>nums1</code> là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length == nums2.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 1000</code></li>
	<li>Các test case được tạo sao cho tồn tại một số nguyên <code>x</code> để <code>nums1</code> có thể trở thành <code>nums2</code> bằng cách cộng <code>x</code> vào mỗi phần tử của <code>nums1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính hiệu nhỏ nhất

<!-- thinking:start -->

> **Tư duy**
>
> $nums2$ là $nums1$ sau khi cộng cùng một số nguyên vào mọi phần tử. Để ghép các phần tử tương ứng và tìm ra độ dịch, ta cần sắp xếp hoặc dùng map.
>
> Phép cộng đồng nhất không làm thay đổi phần tử nào là nhỏ nhất, vì vậy độ dịch chính là hiệu giữa hai giá trị nhỏ nhất.
>
> Trả về $\min(nums2)-\min(nums1)$ sau khi duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể tìm giá trị nhỏ nhất của mỗi mảng, sau đó trả về hiệu giữa hai giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def addedInteger(self, nums1: List[int], nums2: List[int]) -> int:
        return min(nums2) - min(nums1)
```

#### Java

```java
class Solution {
    public int addedInteger(int[] nums1, int[] nums2) {
        return Arrays.stream(nums2).min().getAsInt() - Arrays.stream(nums1).min().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int addedInteger(vector<int>& nums1, vector<int>& nums2) {
        return *min_element(nums2.begin(), nums2.end()) - *min_element(nums1.begin(), nums1.end());
    }
};
```

#### Go

```go
func addedInteger(nums1 []int, nums2 []int) int {
	return slices.Min(nums2) - slices.Min(nums1)
}
```

#### TypeScript

```ts
function addedInteger(nums1: number[], nums2: number[]): number {
    return Math.min(...nums2) - Math.min(...nums1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
