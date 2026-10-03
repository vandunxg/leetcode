---
comments: true
difficulty: Easy
rating: 1152
source: Weekly Contest 269 Q1
tags:
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2089. Find Target Indices After Sorting Array](https://leetcode.com/problems/find-target-indices-after-sorting-array)

[中文文档](/solution/2000-2099/2089.Find%20Target%20Indices%20After%20Sorting%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> và một phần tử đích <code>target</code>.</p>

<p><strong>Chỉ số đích</strong> là một chỉ số <code>i</code> sao cho <code>nums[i] == target</code>.</p>

<p>Trả về <em>danh sách các chỉ số đích của</em> <code>nums</code> sau khi <em>sắp xếp</em> <code>nums</code> <em>theo thứ tự <strong>không giảm</strong></em>. Nếu không có chỉ số đích nào, hãy trả về <em>một <strong>danh sách rỗng</strong></em>. Danh sách trả về phải được sắp xếp theo <strong>thứ tự tăng dần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,5,2,3], target = 2
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong> Sau khi sắp xếp, nums là [1,<u><strong>2</strong></u>,<u><strong>2</strong></u>,3,5].
Các chỉ số mà nums[i] == 2 là 1 và 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,5,2,3], target = 3
<strong>Đầu ra:</strong> [3]
<strong>Giải thích:</strong> Sau khi sắp xếp, nums là [1,2,2,<u><strong>3</strong></u>,5].
Chỉ số mà nums[i] == 3 là 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,5,2,3], target = 5
<strong>Đầu ra:</strong> [4]
<strong>Giải thích:</strong> Sau khi sắp xếp, nums là [1,2,2,3,<u><strong>5</strong></u>].
Chỉ số mà nums[i] == 5 là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i], target &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 100$, ta sắp xếp mảng rồi thu thập các chỉ số có giá trị bằng $target$; các chỉ số này vốn đã xuất hiện theo thứ tự tăng dần.
>
> Ta cũng có thể đếm tuyến tính các giá trị nhỏ hơn hoặc bằng target để tạo ra cùng một khoảng chỉ số; trong code, ta chỉ cần sắp xếp mảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def targetIndices(self, nums: List[int], target: int) -> List[int]:
        nums.sort()
        return [i for i, v in enumerate(nums) if v == target]
```

#### Java

```java
class Solution {
    public List<Integer> targetIndices(int[] nums, int target) {
        Arrays.sort(nums);
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] == target) {
                ans.add(i);
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
    vector<int> targetIndices(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        vector<int> ans;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == target) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func targetIndices(nums []int, target int) (ans []int) {
	sort.Ints(nums)
	for i, v := range nums {
		if v == target {
			ans = append(ans, i)
		}
	}
	return
}
```

#### TypeScript

```ts
function targetIndices(nums: number[], target: number): number[] {
    nums.sort((a, b) => a - b);
    let ans: number[] = [];
    for (let i = 0; i < nums.length; ++i) {
        if (nums[i] == target) {
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
