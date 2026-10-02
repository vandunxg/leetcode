---
comments: true
difficulty: Easy
rating: 1212
source: Biweekly Contest 24 Q1
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1413. Minimum Value to Get Positive Step by Step Sum](https://leetcode.com/problems/minimum-value-to-get-positive-step-by-step-sum)

[中文文档](/solution/1400-1499/1413.Minimum%20Value%20to%20Get%20Positive%20Step%20by%20Step%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên&nbsp;<code>nums</code>, ban đầu bạn có một giá trị <strong>dương</strong> <em>startValue</em><em>.</em></p>

<p>Ở mỗi bước, bạn tính tổng tuần tự của <em>startValue</em>&nbsp;cộng với các phần tử trong <code>nums</code>&nbsp;(từ trái sang phải).</p>

<p>Trả về giá trị <strong>dương</strong> nhỏ nhất của&nbsp;<em>startValue</em>&nbsp;sao cho tổng tuần tự không bao giờ nhỏ hơn 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [-3,2,-3,4,2]
<strong>Output:</strong> 5
<strong>Explanation: </strong>Nếu chọn startValue = 4, ở bước thứ ba tổng tuần tự sẽ nhỏ hơn 1.
<strong>step by step sum</strong>
<strong>startValue = 4 | startValue = 5 | nums</strong>
  (4 <strong>-3</strong> ) = 1  | (5 <strong>-3</strong> ) = 2    |  -3
  (1 <strong>+2</strong> ) = 3  | (2 <strong>+2</strong> ) = 4    |   2
  (3 <strong>-3</strong> ) = 0  | (4 <strong>-3</strong> ) = 1    |  -3
  (0 <strong>+4</strong> ) = 4  | (1 <strong>+4</strong> ) = 5    |   4
  (4 <strong>+2</strong> ) = 6  | (5 <strong>+2</strong> ) = 7    |   2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2]
<strong>Output:</strong> 1
<strong>Explanation:</strong> Giá trị bắt đầu nhỏ nhất phải là số dương.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,-2,-3]
<strong>Output:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mọi tổng hiện tại đều phải không nhỏ hơn $1$. Nếu giá trị bắt đầu là $x$, thì $x$ cộng với mọi tổng tiền tố phải $\ge 1$, do đó $x\ge 1-\min\textit{prefix}$. Đồng thời $x\ge 1$.
>
> $n\le 100$. Duyệt một lần để theo dõi tổng tiền tố và giá trị nhỏ nhất $t$ của nó; đáp án là $\max(1,1-t)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minStartValue(self, nums: List[int]) -> int:
        s, t = 0, inf
        for num in nums:
            s += num
            t = min(t, s)
        return max(1, 1 - t)
```

#### Java

```java
class Solution {
    public int minStartValue(int[] nums) {
        int s = 0;
        int t = Integer.MAX_VALUE;
        for (int num : nums) {
            s += num;
            t = Math.min(t, s);
        }
        return Math.max(1, 1 - t);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minStartValue(vector<int>& nums) {
        int s = 0, t = INT_MAX;
        for (int num : nums) {
            s += num;
            t = min(t, s);
        }
        return max(1, 1 - t);
    }
};
```

#### Go

```go
func minStartValue(nums []int) int {
	s, t := 0, 10000
	for _, num := range nums {
		s += num
		if s < t {
			t = s
		}
	}
	if t < 0 {
		return 1 - t
	}
	return 1
}
```

#### TypeScript

```ts
function minStartValue(nums: number[]): number {
    let sum = 0;
    let min = Infinity;
    for (const num of nums) {
        sum += num;
        min = Math.min(min, sum);
    }
    return Math.max(1, 1 - min);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_start_value(nums: Vec<i32>) -> i32 {
        let mut sum = 0;
        let mut min = i32::MAX;
        for num in nums.iter() {
            sum += num;
            min = min.min(sum);
        }
        (1).max(1 - min)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 theo dõi giá trị nhỏ nhất trong quá trình duyệt. Việc xây dựng toàn bộ các tổng tiền tố bằng `accumulate` rồi lấy $\min$ sử dụng cùng một công thức, chỉ khác là có một mảng tổng tiền tố rõ ràng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minStartValue(self, nums: List[int]) -> int:
        s = list(accumulate(nums))
        return 1 if min(s) >= 0 else abs(min(s)) + 1
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
