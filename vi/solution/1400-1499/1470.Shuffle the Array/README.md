---
comments: true
difficulty: Easy
rating: 1120
source: Weekly Contest 192 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1470. Shuffle the Array](https://leetcode.com/problems/shuffle-the-array)

[中文文档](/solution/1400-1499/1470.Shuffle%20the%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code> gồm <code>2n</code> phần tử có dạng <code>[x<sub>1</sub>,x<sub>2</sub>,...,x<sub>n</sub>,y<sub>1</sub>,y<sub>2</sub>,...,y<sub>n</sub>]</code>.</p>

<p><em>Trả về mảng có dạng</em> <code>[x<sub>1</sub>,y<sub>1</sub>,x<sub>2</sub>,y<sub>2</sub>,...,x<sub>n</sub>,y<sub>n</sub>]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,5,1,3,4,7], n = 3
<strong>Đầu ra:</strong> [2,3,5,4,1,7]
<strong>Giải thích:</strong> Vì x<sub>1</sub>=2, x<sub>2</sub>=5, x<sub>3</sub>=1, y<sub>1</sub>=3, y<sub>2</sub>=4, y<sub>3</sub>=7 nên đáp án là [2,3,5,4,1,7].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,4,3,2,1], n = 4
<strong>Đầu ra:</strong> [1,4,2,3,3,2,4,1]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,2], n = 2
<strong>Đầu ra:</strong> [1,2,1,2]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>nums.length == 2n</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10^3</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Đan xen $[x_1,\ldots,x_n]$ với $[y_1,\ldots,y_n]$. Ghép hai nửa tương ứng rồi làm phẳng kết quả.

<!-- thinking:end -->

Ta duyệt các chỉ số $i$ trong khoảng $[0, n)$. Mỗi lần, ta lấy $\textit{nums}[i]$ và $\textit{nums}[i+n]$ rồi lần lượt đưa chúng vào mảng kết quả.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shuffle(self, nums: List[int], n: int) -> List[int]:
        return [x for pair in zip(nums[:n], nums[n:]) for x in pair]
```

#### Java

```java
class Solution {
    public int[] shuffle(int[] nums, int n) {
        int[] ans = new int[n << 1];
        for (int i = 0, j = 0; i < n; ++i) {
            ans[j++] = nums[i];
            ans[j++] = nums[i + n];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            ans.push_back(nums[i]);
            ans.push_back(nums[i + n]);
        }
        return ans;
    }
};
```

#### Go

```go
func shuffle(nums []int, n int) (ans []int) {
	for i := 0; i < n; i++ {
		ans = append(ans, nums[i])
		ans = append(ans, nums[i+n])
	}
	return
}
```

#### TypeScript

```ts
function shuffle(nums: number[], n: number): number[] {
    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        ans.push(nums[i], nums[i + n]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn shuffle(nums: Vec<i32>, n: i32) -> Vec<i32> {
        let n = n as usize;
        let mut ans = Vec::new();
        for i in 0..n {
            ans.push(nums[i]);
            ans.push(nums[i + n]);
        }
        ans
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* shuffle(int* nums, int numsSize, int n, int* returnSize) {
    int* ans = (int*) malloc(sizeof(int) * n * 2);
    for (int i = 0; i < n; i++) {
        ans[2 * i] = nums[i];
        ans[2 * i + 1] = nums[i + n];
    }
    *returnSize = n * 2;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
