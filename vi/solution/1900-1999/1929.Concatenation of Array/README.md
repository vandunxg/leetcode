---
comments: true
difficulty: Easy
rating: 1132
source: Weekly Contest 249 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1929. Concatenation of Array](https://leetcode.com/problems/concatenation-of-array)

[中文文档](/solution/1900-1999/1929.Concatenation%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, bạn muốn tạo một mảng <code>ans</code> có độ dài <code>2n</code> sao cho <code>ans[i] == nums[i]</code> và <code>ans[i + n] == nums[i]</code> với <code>0 &lt;= i &lt; n</code> (<strong>đánh chỉ số từ 0</strong>).</p>

<p>Cụ thể, <code>ans</code> là phép <strong>nối</strong> của hai mảng <code>nums</code>.</p>

<p>Trả về <em>mảng </em><code>ans</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1]
<strong>Đầu ra:</strong> [1,2,1,1,2,1]
<strong>Giải thích:</strong> Mảng ans được tạo như sau:
- ans = [nums[0],nums[1],nums[2],nums[0],nums[1],nums[2]]
- ans = [1,2,1,1,2,1]</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2,1]
<strong>Đầu ra:</strong> [1,3,2,1,1,3,2,1]
<strong>Giải thích:</strong> Mảng ans được tạo như sau:
- ans = [nums[0],nums[1],nums[2],nums[3],nums[0],nums[1],nums[2],nums[3]]
- ans = [1,3,2,1,1,3,2,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Kết quả là hai bản sao của $\textit{nums}$ được nối với nhau. Nối mảng với chính nó sẽ tạo ra ánh xạ chỉ số theo yêu cầu.

<!-- thinking:end -->

Ta mô phỏng trực tiếp theo mô tả bài toán bằng cách lần lượt thêm các phần tử của $\textit{nums}$ vào mảng kết quả, sau đó thêm các phần tử của $\textit{nums}$ vào mảng kết quả một lần nữa.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getConcatenation(self, nums: List[int]) -> List[int]:
        return nums + nums
```

#### Java

```java
class Solution {
    public int[] getConcatenation(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n << 1];
        for (int i = 0; i < n << 1; ++i) {
            ans[i] = nums[i % n];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getConcatenation(vector<int>& nums) {
        for (int i = 0, n = nums.size(); i < n; ++i) {
            nums.push_back(nums[i]);
        }
        return nums;
    }
};
```

#### Go

```go
func getConcatenation(nums []int) []int {
	return append(nums, nums...)
}
```

#### TypeScript

```ts
function getConcatenation(nums: number[]): number[] {
    return [...nums, ...nums];
}
```

#### Rust

```rust
impl Solution {
    pub fn get_concatenation(nums: Vec<i32>) -> Vec<i32> {
        nums.repeat(2)
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var getConcatenation = function (nums) {
    return [...nums, ...nums];
};
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* getConcatenation(int* nums, int numsSize, int* returnSize) {
    int* ans = malloc(sizeof(int) * numsSize * 2);
    for (int i = 0; i < numsSize; i++) {
        ans[i] = ans[i + numsSize] = nums[i];
    }
    *returnSize = numsSize * 2;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
