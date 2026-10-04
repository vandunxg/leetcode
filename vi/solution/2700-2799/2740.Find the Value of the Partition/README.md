---
comments: true
difficulty: Medium
rating: 1301
source: Weekly Contest 350 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [2740. Find the Value of the Partition](https://leetcode.com/problems/find-the-value-of-the-partition)

[中文文档](/solution/2700-2799/2740.Find%20the%20Value%20of%20the%20Partition/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>dương</strong> <code>nums</code>.</p>

<p>Chia <code>nums</code> thành hai mảng, <code>nums1</code> và <code>nums2</code>, sao cho:</p>

<ul>
	<li>Mỗi phần tử của mảng <code>nums</code> thuộc về một trong hai mảng <code>nums1</code> hoặc <code>nums2</code>.</li>
	<li>Cả hai mảng đều <strong>không rỗng</strong>.</li>
	<li>Giá trị của cách chia là <strong>nhỏ nhất</strong>.</li>
</ul>

<p>Giá trị của cách chia là <code>|max(nums1) - min(nums2)|</code>.</p>

<p>Trong đó, <code>max(nums1)</code> là phần tử lớn nhất của mảng <code>nums1</code>, còn <code>min(nums2)</code> là phần tử nhỏ nhất của mảng <code>nums2</code>.</p>

<p>Trả về <em>số nguyên biểu thị giá trị của cách chia như vậy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2,4]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể chia mảng nums thành nums1 = [1,2] và nums2 = [3,4].
- Phần tử lớn nhất của mảng nums1 bằng 2.
- Phần tử nhỏ nhất của mảng nums2 bằng 3.
Giá trị của cách chia là |2 - 3| = 1.
Có thể chứng minh rằng 1 là giá trị nhỏ nhất trong tất cả các cách chia.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [100,1,10]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Ta có thể chia mảng nums thành nums1 = [10] và nums2 = [100,1].
- Phần tử lớn nhất của mảng nums1 bằng 10.
- Phần tử nhỏ nhất của mảng nums2 bằng 1.
Giá trị của cách chia là |10 - 1| = 9.
Có thể chứng minh rằng 9 là giá trị nhỏ nhất trong tất cả các cách chia.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Chia mảng thành hai nhóm không rỗng và tối thiểu hóa chênh lệch giữa giá trị lớn nhất của nhóm thứ nhất và giá trị nhỏ nhất của nhóm thứ hai. Thứ tự các phần tử trong mỗi nhóm không quan trọng; điều quan trọng là cách tách các phần tử cực trị toàn cục.
>
> Sau khi sắp xếp, mỗi cặp phần tử kề nhau đều có thể là vị trí chia giữa giá trị lớn nhất bên trái và giá trị nhỏ nhất bên phải, còn giá trị của phép chia là chênh lệch giữa chúng. Đáp án là khoảng cách nhỏ nhất giữa hai phần tử kề nhau.

<!-- thinking:end -->

Bài toán yêu cầu ta tối thiểu hóa giá trị của cách chia. Vì vậy, ta có thể sắp xếp mảng rồi lấy chênh lệch nhỏ nhất giữa hai số kề nhau.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findValueOfPartition(self, nums: List[int]) -> int:
        nums.sort()
        return min(b - a for a, b in pairwise(nums))
```

#### Java

```java
class Solution {
    public int findValueOfPartition(int[] nums) {
        Arrays.sort(nums);
        int ans = 1 << 30;
        for (int i = 1; i < nums.length; ++i) {
            ans = Math.min(ans, nums[i] - nums[i - 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findValueOfPartition(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int ans = 1 << 30;
        for (int i = 1; i < nums.size(); ++i) {
            ans = min(ans, nums[i] - nums[i - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func findValueOfPartition(nums []int) int {
	sort.Ints(nums)
	ans := 1 << 30
	for i, x := range nums[1:] {
		ans = min(ans, x-nums[i])
	}
	return ans
}
```

#### TypeScript

```ts
function findValueOfPartition(nums: number[]): number {
    nums.sort((a, b) => a - b);
    let ans = Infinity;
    for (let i = 1; i < nums.length; ++i) {
        ans = Math.min(ans, Math.abs(nums[i] - nums[i - 1]));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_value_of_partition(mut nums: Vec<i32>) -> i32 {
        nums.sort();
        let mut ans = i32::MAX;
        for i in 1..nums.len() {
            ans = ans.min(nums[i] - nums[i - 1]);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
