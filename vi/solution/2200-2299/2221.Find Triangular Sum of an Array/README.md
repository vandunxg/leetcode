---
comments: true
difficulty: Medium
rating: 1317
source: Biweekly Contest 75 Q2
tags:
    - Array
    - Math
    - Combinatorics
    - Number Theory
    - Simulation
---

<!-- problem:start -->

# [2221. Find Triangular Sum of an Array](https://leetcode.com/problems/find-triangular-sum-of-an-array)

[中文文档](/solution/2200-2299/2221.Find%20Triangular%20Sum%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>đánh chỉ số từ 0</strong>, trong đó <code>nums[i]</code> là một chữ số nằm trong khoảng từ <code>0</code> đến <code>9</code> (<strong>bao gồm cả hai đầu mút</strong>).</p>

<p><strong>Tổng tam giác</strong> của <code>nums</code> là giá trị của phần tử duy nhất còn lại trong <code>nums</code> sau khi kết thúc quy trình sau:</p>

<ol>
	<li>Gọi <code>nums</code> là mảng gồm <code>n</code> phần tử. Nếu <code>n == 1</code>, <strong>kết thúc</strong> quy trình. Nếu không, hãy <strong>tạo</strong> một mảng số nguyên <strong>đánh chỉ số từ 0</strong> mới là <code>newNums</code> có độ dài <code>n - 1</code>.</li>
	<li>Với mỗi chỉ số <code>i</code>, trong đó <code>0 &lt;= i &lt;&nbsp;n - 1</code>, <strong>gán</strong> giá trị của <code>newNums[i]</code> bằng <code>(nums[i] + nums[i+1]) % 10</code>, trong đó <code>%</code> là toán tử modulo.</li>
	<li><strong>Thay thế</strong> mảng <code>nums</code> bằng <code>newNums</code>.</li>
	<li><strong>Lặp lại</strong> toàn bộ quy trình bắt đầu từ bước 1.</li>
</ol>

<p>Trả về <em>tổng tam giác của</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2221.Find%20Triangular%20Sum%20of%20an%20Array/images/ex1drawio.png" style="width: 250px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
Sơ đồ trên mô tả quy trình để tính tổng tam giác của mảng.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Vì nums chỉ có một phần tử, tổng tam giác chính là giá trị của phần tử đó.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vòng lặp thay mảng bằng các tổng của hai phần tử kề nhau theo modulo $10$ cho đến khi chỉ còn lại một giá trị. Vì $n \le 10^3$, mô phỏng với độ phức tạp $O(n^2)$ là đủ. Không cần tìm công thức đóng.
>
> Với độ dài còn lại $k = n-1,\ldots,1$, ta gán $nums[i] = (nums[i]+nums[i+1]) \bmod 10$. Phần tử $nums[0]$ còn lại chính là tổng tam giác.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp các thao tác được mô tả trong đề bài. Thực hiện $n - 1$ vòng lặp trên mảng $\textit{nums}$, cập nhật mảng $\textit{nums}$ theo các quy tắc đã nêu trong đề bài ở mỗi vòng. Cuối cùng, trả về phần tử duy nhất còn lại trong mảng $\textit{nums}$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def triangularSum(self, nums: List[int]) -> int:
        for k in range(len(nums) - 1, 0, -1):
            for i in range(k):
                nums[i] = (nums[i] + nums[i + 1]) % 10
        return nums[0]
```

#### Java

```java
class Solution {
    public int triangularSum(int[] nums) {
        for (int k = nums.length - 1; k > 0; --k) {
            for (int i = 0; i < k; ++i) {
                nums[i] = (nums[i] + nums[i + 1]) % 10;
            }
        }
        return nums[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int triangularSum(vector<int>& nums) {
        for (int k = nums.size() - 1; k; --k) {
            for (int i = 0; i < k; ++i) {
                nums[i] = (nums[i] + nums[i + 1]) % 10;
            }
        }
        return nums[0];
    }
};
```

#### Go

```go
func triangularSum(nums []int) int {
	for k := len(nums) - 1; k > 0; k-- {
		for i := 0; i < k; i++ {
			nums[i] = (nums[i] + nums[i+1]) % 10
		}
	}
	return nums[0]
}
```

#### TypeScript

```ts
function triangularSum(nums: number[]): number {
    for (let k = nums.length - 1; k; --k) {
        for (let i = 0; i < k; ++i) {
            nums[i] = (nums[i] + nums[i + 1]) % 10;
        }
    }
    return nums[0];
}
```

#### Rust

```rust
impl Solution {
    pub fn triangular_sum(mut nums: Vec<i32>) -> i32 {
        let mut k = nums.len() as i32 - 1;
        while k > 0 {
            for i in 0..k as usize {
                nums[i] = (nums[i] + nums[i + 1]) % 10;
            }
            k -= 1;
        }
        nums[0]
    }
}
```

#### C#

```cs
public class Solution {
    public int TriangularSum(int[] nums) {
        for (int k = nums.Length - 1; k > 0; --k) {
            for (int i = 0; i < k; ++i) {
                nums[i] = (nums[i] + nums[i + 1]) % 10;
            }
        }
        return nums[0];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
