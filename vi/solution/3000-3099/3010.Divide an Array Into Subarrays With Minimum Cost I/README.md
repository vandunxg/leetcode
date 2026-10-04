---
comments: true
difficulty: Easy
rating: 1291
source: Biweekly Contest 122 Q1
tags:
    - Array
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3010. Divide an Array Into Subarrays With Minimum Cost I](https://leetcode.com/problems/divide-an-array-into-subarrays-with-minimum-cost-i)

[中文文档](/solution/3000-3099/3010.Divide%20an%20Array%20Into%20Subarrays%20With%20Minimum%20Cost%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p><strong>Chi phí</strong> của một mảng là giá trị của <strong>phần tử đầu tiên</strong> trong mảng đó. Ví dụ, chi phí của <code>[1,2,3]</code> là <code>1</code>, còn chi phí của <code>[3,4,1]</code> là <code>3</code>.</p>

<p>Bạn cần chia <code>nums</code> thành <code>3</code> <span data-keyword="subarray-nonempty">mảng con</span> <strong>liên tiếp, không giao nhau</strong>.</p>

<p>Hãy trả về <em><strong>tổng nhỏ nhất</strong> có thể có của <strong>chi phí</strong> các mảng con này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,12]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Cách tốt nhất để tạo thành 3 mảng con là: [1], [2] và [3,12], với tổng chi phí là 1 + 2 + 3 = 6.
Các cách khác để tạo thành 3 mảng con là:
- [1], [2,3] và [12], với tổng chi phí là 1 + 2 + 12 = 15.
- [1,2], [3] và [12], với tổng chi phí là 1 + 3 + 12 = 16.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,3]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Cách tốt nhất để tạo thành 3 mảng con là: [5], [4] và [3], với tổng chi phí là 5 + 4 + 3 = 12.
Có thể chứng minh rằng 12 là chi phí nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,3,1,1]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Cách tốt nhất để tạo thành 3 mảng con là: [10,3], [1] và [1], với tổng chi phí là 10 + 1 + 1 = 12.
Có thể chứng minh rằng 12 là chi phí nhỏ nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt để tìm giá trị nhỏ nhất và nhỏ thứ hai

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 50$ và ta chia mảng thành ba mảng con, trong đó chi phí là tổng các phần tử đầu tiên. Phần tử đầu tiên của mảng con thứ nhất luôn là $\textit{nums}[0]$.
>
> Hai mảng con còn lại bắt đầu tại hai phần tử khác nhau có chỉ số thuộc $[1,n)$. Để tổng nhỏ nhất, ta chọn giá trị nhỏ nhất và nhỏ thứ hai trong phần còn lại của mảng.
>
> Chỉ cần một lần quét để theo dõi hai giá trị này; ta không cần tạo ra các vị trí chia.

<!-- thinking:end -->

Ta đặt phần tử đầu tiên của mảng $nums$ là $a$, phần tử nhỏ nhất trong các phần tử còn lại là $b$, và phần tử nhỏ thứ hai là $c$. Đáp án là $a+b+c$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, nums: List[int]) -> int:
        a, b, c = nums[0], inf, inf
        for x in nums[1:]:
            if x < b:
                c, b = b, x
            elif x < c:
                c = x
        return a + b + c
```

#### Java

```java
class Solution {
    public int minimumCost(int[] nums) {
        int a = nums[0], b = 100, c = 100;
        for (int i = 1; i < nums.length; ++i) {
            if (nums[i] < b) {
                c = b;
                b = nums[i];
            } else if (nums[i] < c) {
                c = nums[i];
            }
        }
        return a + b + c;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCost(vector<int>& nums) {
        int a = nums[0], b = 100, c = 100;
        for (int i = 1; i < nums.size(); ++i) {
            if (nums[i] < b) {
                c = b;
                b = nums[i];
            } else if (nums[i] < c) {
                c = nums[i];
            }
        }
        return a + b + c;
    }
};
```

#### Go

```go
func minimumCost(nums []int) int {
	a, b, c := nums[0], 100, 100
	for _, x := range nums[1:] {
		if x < b {
			b, c = x, b
		} else if x < c {
			c = x
		}
	}
	return a + b + c
}
```

#### TypeScript

```ts
function minimumCost(nums: number[]): number {
    let [a, b, c] = [nums[0], 100, 100];
    for (const x of nums.slice(1)) {
        if (x < b) {
            [b, c] = [x, b];
        } else if (x < c) {
            c = x;
        }
    }
    return a + b + c;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_cost(nums: Vec<i32>) -> i32 {
        let a: i32 = nums[0];
        let mut b: i32 = i32::MAX;
        let mut c: i32 = i32::MAX;

        for &x in nums.iter().skip(1) {
            if x < b {
                c = b;
                b = x;
            } else if x < c {
                c = x;
            }
        }

        a + b + c
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
