---
comments: true
difficulty: Easy
rating: 1229
source: Weekly Contest 233 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1800. Maximum Ascending Subarray Sum](https://leetcode.com/problems/maximum-ascending-subarray-sum)

[中文文档](/solution/1800-1899/1800.Maximum%20Ascending%20Subarray%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên dương <code>nums</code>, hãy trả về tổng <strong>lớn nhất</strong> có thể của một <span data-keyword="strictly-increasing-array">mảng con tăng nghiêm ngặt</span> trong<em> </em><code>nums</code>.</p>

<p>Mảng con được định nghĩa là một dãy số liên tiếp trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,20,30,5,10,50]
<strong>Đầu ra:</strong> 65
<strong>Giải thích: </strong>[5,10,50] là mảng con tăng có tổng lớn nhất, bằng 65.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,20,30,40,50]
<strong>Đầu ra:</strong> 150
<strong>Giải thích: </strong>[10,20,30,40,50] là mảng con tăng có tổng lớn nhất, bằng 150.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [12,17,15,13,10,11,12]
<strong>Đầu ra:</strong> 33
<strong>Giải thích: </strong>[10,11,12] là mảng con tăng có tổng lớn nhất, bằng 33.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng lớn nhất của một mảng con liên tiếp tăng nghiêm ngặt. Việc liệt kê mọi mảng con rồi kiểm tra điều kiện tăng tốn $O(n^2)$. Với $n \le 100$, cách này vẫn chạy được, nhưng phần lớn các khoảng sẽ bị loại ngay khi xuất hiện một lần giảm.
>
> Khi hai phần tử kề nhau phá vỡ thứ tự, không mảng con nào đi qua cặp phần tử đó có thể hợp lệ. Vì vậy, chỉ cần duyệt từ trái sang phải và duy trì tổng $t$ của đoạn tăng hiện tại: mở rộng và cập nhật đáp án khi phần tử tiếp theo lớn hơn, nếu không thì bắt đầu lại từ phần tử hiện tại. Một lượt duyệt là đủ để tìm cực đại trên toàn mảng.

<!-- thinking:end -->

Ta dùng biến $t$ để ghi nhận tổng hiện tại của mảng con tăng, và biến $ans$ để ghi nhận tổng lớn nhất của mảng con tăng.

Duyệt mảng $nums$:

Nếu phần tử hiện tại là phần tử đầu tiên của mảng, hoặc lớn hơn phần tử trước đó, ta cộng phần tử hiện tại vào tổng của mảng con tăng hiện tại, tức là $t = t + nums[i]$, rồi cập nhật tổng lớn nhất $ans = \max(ans, t)$. Ngược lại, phần tử hiện tại không thỏa mãn điều kiện của mảng con tăng, nên đặt lại tổng $t$ của mảng con tăng hiện tại bằng chính phần tử đó, tức là $t = nums[i]$.

Sau khi duyệt xong, trả về tổng lớn nhất $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAscendingSum(self, nums: List[int]) -> int:
        ans = t = 0
        for i, v in enumerate(nums):
            if i == 0 or v > nums[i - 1]:
                t += v
                ans = max(ans, t)
            else:
                t = v
        return ans
```

#### Java

```java
class Solution {
    public int maxAscendingSum(int[] nums) {
        int ans = 0, t = 0;
        for (int i = 0; i < nums.length; ++i) {
            if (i == 0 || nums[i] > nums[i - 1]) {
                t += nums[i];
                ans = Math.max(ans, t);
            } else {
                t = nums[i];
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
    int maxAscendingSum(vector<int>& nums) {
        int ans = 0, t = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (i == 0 || nums[i] > nums[i - 1]) {
                t += nums[i];
                ans = max(ans, t);
            } else {
                t = nums[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxAscendingSum(nums []int) int {
	ans, t := 0, 0
	for i, v := range nums {
		if i == 0 || v > nums[i-1] {
			t += v
			if ans < t {
				ans = t
			}
		} else {
			t = v
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxAscendingSum(nums: number[]): number {
    const n = nums.length;
    let res = nums[0];
    let sum = nums[0];
    for (let i = 1; i < n; i++) {
        if (nums[i] <= nums[i - 1]) {
            res = Math.max(res, sum);
            sum = 0;
        }
        sum += nums[i];
    }
    return Math.max(res, sum);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_ascending_sum(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut res = nums[0];
        let mut sum = nums[0];
        for i in 1..n {
            if nums[i - 1] >= nums[i] {
                res = res.max(sum);
                sum = 0;
            }
            sum += nums[i];
        }
        res.max(sum)
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int maxAscendingSum(int* nums, int numsSize) {
    int res = nums[0];
    int sum = nums[0];
    for (int i = 1; i < numsSize; i++) {
        if (nums[i - 1] >= nums[i]) {
            res = max(res, sum);
            sum = 0;
        }
        sum += nums[i];
    }
    return max(res, sum);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
