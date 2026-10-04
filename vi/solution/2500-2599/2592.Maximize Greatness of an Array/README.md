---
comments: true
difficulty: Medium
rating: 1569
source: Biweekly Contest 100 Q2
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [2592. Maximize Greatness of an Array](https://leetcode.com/problems/maximize-greatness-of-an-array)

[中文文档](/solution/2500-2599/2592.Maximize%20Greatness%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ 0. Bạn được phép hoán vị <code>nums</code> thành một mảng mới <code>perm</code> tùy ý.</p>

<p>Ta định nghĩa <strong>greatness</strong> của <code>nums</code> là số lượng chỉ số <code>0 &lt;= i &lt; nums.length</code> mà tại đó <code>perm[i] &gt; nums[i]</code>.</p>

<p>Hãy trả về <em><strong>greatness</strong> lớn nhất có thể đạt được sau khi hoán vị</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,2,1,3,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một trong những cách sắp xếp tối ưu là perm = [2,5,1,3,3,1,1].
Tại các chỉ số = 0, 1, 3 và 4, perm[i] &gt; nums[i]. Vì vậy, ta trả về 4.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể chứng minh rằng perm tối ưu là [2,3,4,1].
Tại các chỉ số = 0, 1 và 2, perm[i] &gt; nums[i]. Vì vậy, ta trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Greatness là số lượng vị trí thỏa mãn $perm[i]>nums[i]$; $perm$ là một hoán vị của $nums$. Ta nên ghép các giá trị lớn hơn với càng nhiều giá trị nhỏ hơn càng tốt.
>
> Sau khi sắp xếp, ta duyệt các ứng viên $x$ từ trái sang phải và ghép $x$ với phần tử chưa ghép tiếp theo $nums[i]$ bất cứ khi nào $x$ lớn hơn phần tử đó. Số cặp ghép thành công chính là đáp án.

<!-- thinking:end -->

Trước tiên, ta có thể sắp xếp mảng $nums$.

Sau đó, ta định nghĩa một con trỏ $i$ trỏ đến phần tử đầu tiên của mảng $nums$. Ta duyệt qua mảng $nums$; với mỗi phần tử $x$ gặp được, nếu $x$ lớn hơn $nums[i]$ thì dịch con trỏ $i$ sang phải.

Cuối cùng, ta trả về giá trị của con trỏ $i$.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximizeGreatness(self, nums: List[int]) -> int:
        nums.sort()
        i = 0
        for x in nums:
            i += x > nums[i]
        return i
```

#### Java

```java
class Solution {
    public int maximizeGreatness(int[] nums) {
        Arrays.sort(nums);
        int i = 0;
        for (int x : nums) {
            if (x > nums[i]) {
                ++i;
            }
        }
        return i;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximizeGreatness(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int i = 0;
        for (int x : nums) {
            i += x > nums[i];
        }
        return i;
    }
};
```

#### Go

```go
func maximizeGreatness(nums []int) int {
	sort.Ints(nums)
	i := 0
	for _, x := range nums {
		if x > nums[i] {
			i++
		}
	}
	return i
}
```

#### TypeScript

```ts
function maximizeGreatness(nums: number[]): number {
    nums.sort((a, b) => a - b);
    let i = 0;
    for (const x of nums) {
        if (x > nums[i]) {
            i += 1;
        }
    }
    return i;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
