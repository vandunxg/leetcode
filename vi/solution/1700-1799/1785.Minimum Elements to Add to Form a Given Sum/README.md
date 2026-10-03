---
comments: true
difficulty: Medium
rating: 1432
source: Weekly Contest 231 Q2
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1785. Minimum Elements to Add to Form a Given Sum](https://leetcode.com/problems/minimum-elements-to-add-to-form-a-given-sum)

[中文文档](/solution/1700-1799/1785.Minimum%20Elements%20to%20Add%20to%20Form%20a%20Given%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và hai số nguyên <code>limit</code> và <code>goal</code>. Mảng <code>nums</code> có tính chất <code>abs(nums[i]) &lt;= limit</code>.</p>

<p>Trả về <em>số phần tử ít nhất cần thêm để tổng của mảng bằng </em><code>goal</code>. Mảng phải tiếp tục thỏa mãn <code>abs(nums[i]) &lt;= limit</code>.</p>

<p>Lưu ý rằng <code>abs(x)</code> bằng <code>x</code> nếu <code>x &gt;= 0</code>, và bằng <code>-x</code> trong trường hợp còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-1,1], limit = 3, goal = -4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể thêm -2 và -3, khi đó tổng mảng là 1 - 1 + 1 - 2 - 3 = -4.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-10,9,1], limit = 100, goal = 0
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= limit &lt;= 10<sup>6</sup></code></li>
	<li><code>-limit &lt;= nums[i] &lt;= limit</code></li>
	<li><code>-10<sup>9</sup> &lt;= goal &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể thêm các số nguyên có giá trị tuyệt đối không vượt quá $\textit{limit}$ để tổng trở thành $\textit{goal}$, với số lần thêm ít nhất.
>
> Khoảng cách $d=|\sum nums-\textit{goal}|$ giảm nhiều nhất $\textit{limit}$ sau mỗi lần thêm, nên số lần thêm ít nhất là $\lceil d/\textit{limit}\rceil$.

<!-- thinking:end -->

Trước hết, ta tính tổng $s$ của các phần tử trong mảng, sau đó tính độ chênh lệch $d$ giữa $s$ và $goal$.

Số phần tử cần thêm là giá trị tuyệt đối của $d$ chia cho $limit$ rồi làm tròn lên, tức là $\lceil \frac{|d|}{limit} \rceil$.

Lưu ý rằng trong bài này, miền giá trị của phần tử mảng là $[-10^6, 10^6]$, số phần tử tối đa là $10^5$, nên tổng $s$ và độ chênh lệch $d$ có thể vượt miền của số nguyên 32-bit; do đó cần dùng số nguyên 64-bit.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(1)$. Ở đây, $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minElements(self, nums: List[int], limit: int, goal: int) -> int:
        d = abs(sum(nums) - goal)
        return (d + limit - 1) // limit
```

#### Java

```java
class Solution {
    public int minElements(int[] nums, int limit, int goal) {
        // long s = Arrays.stream(nums).asLongStream().sum();
        long s = 0;
        for (int v : nums) {
            s += v;
        }
        long d = Math.abs(s - goal);
        return (int) ((d + limit - 1) / limit);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minElements(vector<int>& nums, int limit, int goal) {
        long long s = accumulate(nums.begin(), nums.end(), 0ll);
        long long d = abs(s - goal);
        return (d + limit - 1) / limit;
    }
};
```

#### Go

```go
func minElements(nums []int, limit int, goal int) int {
	s := 0
	for _, v := range nums {
		s += v
	}
	d := abs(s - goal)
	return (d + limit - 1) / limit
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minElements(nums: number[], limit: number, goal: number): number {
    const sum = nums.reduce((r, v) => r + v, 0);
    const diff = Math.abs(goal - sum);
    return Math.floor((diff + limit - 1) / limit);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_elements(nums: Vec<i32>, limit: i32, goal: i32) -> i32 {
        let limit = limit as i64;
        let goal = goal as i64;
        let mut sum = 0;
        for &num in nums.iter() {
            sum += num as i64;
        }
        let diff = (goal - sum).abs();
        ((diff + limit - 1) / limit) as i32
    }
}
```

#### C

```c
int minElements(int* nums, int numsSize, int limit, int goal) {
    long long sum = 0;
    for (int i = 0; i < numsSize; i++) {
        sum += nums[i];
    }
    long long diff = labs(goal - sum);
    return (diff + limit - 1) / limit;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
