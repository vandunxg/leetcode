---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [442. Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array)

[中文文档](/solution/0400-0499/0442.Find%20All%20Duplicates%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó mọi số nguyên đều nằm trong khoảng <code>[1, n]</code> và mỗi số xuất hiện <strong>nhiều nhất</strong> <strong>hai lần</strong>. Hãy trả về <em>mảng chứa tất cả các số xuất hiện <strong>hai lần</strong></em>.</p>

<p>Bạn cần viết thuật toán chạy trong thời gian <code>O(n)</code> và chỉ dùng bộ nhớ phụ <em>hằng số</em>, không tính bộ nhớ cần để lưu kết quả.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [4,3,2,7,8,2,3,1]
<strong>Đầu ra:</strong> [2,3]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [1,1,2]
<strong>Đầu ra:</strong> [1]
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [1]
<strong>Đầu ra:</strong> []
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= n</code></li>
	<li>Mỗi phần tử trong <code>nums</code> xuất hiện <strong>một</strong> hoặc <strong>hai</strong> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị nằm trong $[1,n]$ và xuất hiện nhiều nhất hai lần; yêu cầu là thời gian tuyến tính và bộ nhớ phụ hằng số. Hash set có thể tìm các giá trị trùng lặp nhưng cần $O(n)$ bộ nhớ.
>
> Đổi chỗ để đưa mỗi giá trị $v$ về chỉ số $v-1$ (cycle sort). Sau đó, nếu giá trị tại chỉ số $i$ khác $i+1$ thì đó là một giá trị trùng lặp còn lại.
>
> Vòng lặp đổi chỗ dừng khi $nums[i]=nums[nums[i]-1]$. Vì vậy, khi gặp giá trị trùng với giá trị đã đúng vị trí, quá trình sẽ dừng thay vì lặp vô hạn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDuplicates(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)):
            while nums[i] != nums[nums[i] - 1]:
                nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1]
        return [v for i, v in enumerate(nums) if v != i + 1]
```

#### Java

```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            while (nums[i] != nums[nums[i] - 1]) {
                swap(nums, i, nums[i] - 1);
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (nums[i] != i + 1) {
                ans.add(nums[i]);
            }
        }
        return ans;
    }

    void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            while (nums[i] != nums[nums[i] - 1]) {
                swap(nums[i], nums[nums[i] - 1]);
            }
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (nums[i] != i + 1) {
                ans.push_back(nums[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findDuplicates(nums []int) []int {
	for i := range nums {
		for nums[i] != nums[nums[i]-1] {
			nums[i], nums[nums[i]-1] = nums[nums[i]-1], nums[i]
		}
	}
	var ans []int
	for i, v := range nums {
		if v != i+1 {
			ans = append(ans, v)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findDuplicates(nums: number[]): number[] {
    for (let i = 0; i < nums.length; i++) {
        while (nums[i] !== nums[nums[i] - 1]) {
            const temp = nums[i];
            nums[i] = nums[temp - 1];
            nums[temp - 1] = temp;
        }
    }
    const ans: number[] = [];
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== i + 1) {
            ans.push(nums[i]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
