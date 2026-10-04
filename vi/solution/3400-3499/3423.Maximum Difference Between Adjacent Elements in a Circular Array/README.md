---
comments: true
difficulty: Easy
rating: 1184
source: Biweekly Contest 148 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3423. Maximum Difference Between Adjacent Elements in a Circular Array](https://leetcode.com/problems/maximum-difference-between-adjacent-elements-in-a-circular-array)

[中文文档](/solution/3400-3499/3423.Maximum%20Difference%20Between%20Adjacent%20Elements%20in%20a%20Circular%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <strong>vòng</strong> <code>nums</code>, hãy tìm độ chênh lệch tuyệt đối <b>lớn nhất</b> giữa các phần tử kề nhau.</p>

<p><strong>Lưu ý</strong>: Trong một mảng vòng, phần tử đầu tiên và phần tử cuối cùng là hai phần tử kề nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>nums</code> là mảng vòng, <code>nums[0]</code> và <code>nums[2]</code> là hai phần tử kề nhau. Chúng có độ chênh lệch tuyệt đối lớn nhất là <code>|4 - 1| = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-5,-10,-5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai phần tử kề nhau <code>nums[0]</code> và <code>nums[1]</code> có độ chênh lệch tuyệt đối lớn nhất là <code>|-5 - (-10)| = 5</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mảng vòng cũng cần so sánh phần tử đầu tiên và phần tử cuối cùng. $n\le 100$, vì vậy chỉ cần duyệt một lần.
>
> Việc thêm $\textit{nums}[0]$ vào cuối biến cặp phần tử nối vòng thành một cặp phần tử kề nhau thông thường.
>
> Ta lấy độ chênh lệch tuyệt đối lớn nhất trong $\textit{pairwise}(\textit{nums}+[\textit{nums}[0]])$.

<!-- thinking:end -->

Ta duyệt mảng $\textit{nums}$, tính độ chênh lệch tuyệt đối giữa các phần tử kề nhau và duy trì độ chênh lệch tuyệt đối lớn nhất. Cuối cùng, ta so sánh giá trị này với độ chênh lệch tuyệt đối giữa phần tử đầu tiên và phần tử cuối cùng rồi lấy giá trị lớn hơn.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAdjacentDistance(self, nums: List[int]) -> int:
        return max(abs(a - b) for a, b in pairwise(nums + [nums[0]]))
```

#### Java

```java
class Solution {
    public int maxAdjacentDistance(int[] nums) {
        int n = nums.length;
        int ans = Math.abs(nums[0] - nums[n - 1]);
        for (int i = 1; i < n; ++i) {
            ans = Math.max(ans, Math.abs(nums[i] - nums[i - 1]));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxAdjacentDistance(vector<int>& nums) {
        int ans = abs(nums[0] - nums.back());
        for (int i = 1; i < nums.size(); ++i) {
            ans = max(ans, abs(nums[i] - nums[i - 1]));
        }
        return ans;
    }
};
```

#### Go

```go
func maxAdjacentDistance(nums []int) int {
	ans := abs(nums[0] - nums[len(nums)-1])
	for i := 1; i < len(nums); i++ {
		ans = max(ans, abs(nums[i]-nums[i-1]))
	}
	return ans
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
function maxAdjacentDistance(nums: number[]): number {
    const n = nums.length;
    let ans = Math.abs(nums[0] - nums[n - 1]);
    for (let i = 1; i < n; ++i) {
        ans = Math.max(ans, Math.abs(nums[i] - nums[i - 1]));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_adjacent_distance(nums: Vec<i32>) -> i32 {
        nums.iter()
            .zip(nums.iter().cycle().skip(1))
            .take(nums.len())
            .map(|(a, b)| (*a - *b).abs())
            .max()
            .unwrap_or(0)
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxAdjacentDistance(int[] nums) {
        int n = nums.Length;
        int ans = Math.Abs(nums[0] - nums[n - 1]);
        for (int i = 1; i < n; ++i) {
            ans = Math.Max(ans, Math.Abs(nums[i] - nums[i - 1]));
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
