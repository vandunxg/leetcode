---
comments: true
difficulty: Easy
rating: 1149
source: Biweekly Contest 125 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3065. Minimum Operations to Exceed Threshold Value I](https://leetcode.com/problems/minimum-operations-to-exceed-threshold-value-i)

[中文文档](/solution/3000-3099/3065.Minimum%20Operations%20to%20Exceed%20Threshold%20Value%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể xóa một lần xuất hiện của phần tử nhỏ nhất trong <code>nums</code>.</p>

<p>Trả về <em>số thao tác <strong>ít nhất</strong> cần thực hiện để mọi phần tử của mảng lớn hơn hoặc bằng</em> <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,11,10,1,3], k = 10
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Sau một thao tác, nums trở thành [2, 11, 10, 3].
Sau hai thao tác, nums trở thành [11, 10, 3].
Sau ba thao tác, nums trở thành [11, 10].
Ở thời điểm này, mọi phần tử của nums đều lớn hơn hoặc bằng 10 nên ta có thể dừng lại.
Có thể chứng minh rằng 3 là số thao tác ít nhất cần thực hiện để mọi phần tử của mảng lớn hơn hoặc bằng 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,4,9], k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mọi phần tử của mảng đều lớn hơn hoặc bằng 1 nên ta không cần thực hiện thao tác nào trên nums.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,4,9], k = 9
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> chỉ có một phần tử duy nhất của nums lớn hơn hoặc bằng 9 nên ta cần thực hiện thao tác 4 lần trên nums.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 50</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
    <li>Dữ liệu đầu vào được tạo sao cho tồn tại ít nhất một chỉ số <code>i</code> thỏa mãn <code>nums[i] &gt;= k</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa một giá trị nhỏ hơn $k$; ta muốn mọi giá trị còn lại $\ge k$. Thứ tự không làm thay đổi số lượng.
>
> Đáp án là số phần tử nhỏ hơn $k$, có thể tính được trong một lượt duyệt.

<!-- thinking:end -->

Chỉ cần duyệt qua mảng một lần và đếm số phần tử nhỏ hơn $k$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        return sum(x < k for x in nums)
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int k) {
        int ans = 0;
        for (int x : nums) {
            if (x < k) {
                ++ans;
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
    int minOperations(vector<int>& nums, int k) {
        int ans = 0;
        for (int x : nums) {
            if (x < k) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) (ans int) {
    for _, x := range nums {
        if x < k {
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function minOperations(nums: number[], k: number): number {
    return nums.filter(x => x < k).length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
