---
comments: true
difficulty: Easy
rating: 1160
source: Weekly Contest 248 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1920. Build Array from Permutation](https://leetcode.com/problems/build-array-from-permutation)

[中文文档](/solution/1900-1999/1920.Build%20Array%20from%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>hoán vị zero-based</strong> <code>nums</code> (<strong>đánh chỉ số từ 0</strong>), hãy xây dựng một mảng <code>ans</code> có <strong>cùng độ dài</strong>, trong đó <code>ans[i] = nums[nums[i]]</code> với mỗi <code>0 &lt;= i &lt; nums.length</code>, rồi trả về mảng đó.</p>

<p>Một <strong>hoán vị zero-based</strong> <code>nums</code> là một mảng gồm các số nguyên <strong>phân biệt</strong> từ <code>0</code> đến <code>nums.length - 1</code> (<strong>bao gồm cả hai đầu</strong>).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,2,1,5,3,4]
<strong>Đầu ra:</strong> [0,1,2,4,5,3]<strong>
Giải thích:</strong> Mảng ans được xây dựng như sau:
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[0], nums[2], nums[1], nums[5], nums[3], nums[4]]
    = [0,1,2,4,5,3]</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,0,1,2,3,4]
<strong>Đầu ra:</strong> [4,5,0,1,2,3]
<strong>Giải thích:</strong> Mảng ans được xây dựng như sau:
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[5], nums[0], nums[1], nums[2], nums[3], nums[4]]
    = [4,5,0,1,2,3]</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt; nums.length</code></li>
	<li>Các phần tử trong <code>nums</code> là <strong>phân biệt</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này mà không sử dụng thêm không gian (tức là bộ nhớ <code>O(1)</code>) không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Định nghĩa $\textit{ans}[i]=\textit{nums}[\textit{nums}[i]]$ có thể cần thêm một mảng, nên chỉ cần dùng phép xây dựng mảng trực tiếp.
>
> Duyệt một lần để ghi lại mọi ánh xạ trong $O(n)$ thời gian.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quá trình được mô tả trong đề bài bằng cách xây dựng một mảng mới $\textit{ans}$. Với mỗi $i$, đặt $\textit{ans}[i] = \textit{nums}[\textit{nums}[i]]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Không tính phần không gian của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def buildArray(self, nums: List[int]) -> List[int]:
        return [nums[num] for num in nums]
```

#### Java

```java
class Solution {
    public int[] buildArray(int[] nums) {
        int[] ans = new int[nums.length];
        for (int i = 0; i < nums.length; ++i) {
            ans[i] = nums[nums[i]];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> buildArray(vector<int>& nums) {
        vector<int> ans;
        for (int& num : nums) {
            ans.push_back(nums[num]);
        }
        return ans;
    }
};
```

#### Go

```go
func buildArray(nums []int) []int {
	ans := make([]int, len(nums))
	for i, num := range nums {
		ans[i] = nums[num]
	}
	return ans
}
```

#### TypeScript

```ts
function buildArray(nums: number[]): number[] {
    return nums.map(x => nums[x]);
}
```

#### Rust

```rust
impl Solution {
    pub fn build_array(nums: Vec<i32>) -> Vec<i32> {
        nums.iter().map(|&v| nums[v as usize]).collect()
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var buildArray = function (nums) {
    return nums.map(x => nums[x]);
};
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* buildArray(int* nums, int numsSize, int* returnSize) {
    int* ans = malloc(sizeof(int) * numsSize);
    for (int i = 0; i < numsSize; i++) {
        ans[i] = nums[nums[i]];
    }
    *returnSize = numsSize;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
