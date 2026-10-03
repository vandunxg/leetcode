---
comments: true
difficulty: Easy
rating: 1184
source: Weekly Contest 255 Q1
tags:
    - Array
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [1979. Find Greatest Common Divisor of Array](https://leetcode.com/problems/find-greatest-common-divisor-of-array)

[中文文档](/solution/1900-1999/1979.Find%20Greatest%20Common%20Divisor%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về<strong> </strong><em><strong>ước chung lớn nhất</strong> của số nhỏ nhất và số lớn nhất trong </em><code>nums</code>.</p>

<p><strong>Ước chung lớn nhất</strong> của hai số là số nguyên dương lớn nhất có thể chia hết cả hai số đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,5,6,9,10]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Số nhỏ nhất trong nums là 2.
Số lớn nhất trong nums là 10.
Ước chung lớn nhất của 2 và 10 là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,5,6,8,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Số nhỏ nhất trong nums là 3.
Số lớn nhất trong nums là 8.
Ước chung lớn nhất của 3 và 8 là 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Số nhỏ nhất trong nums là 3.
Số lớn nhất trong nums là 3.
Ước chung lớn nhất của 3 và 3 là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ước chung lớn nhất của toàn bộ mảng bằng ước chung lớn nhất của số lớn nhất và số nhỏ nhất. Duyệt một lượt để tìm hai đầu mút, sau đó dùng $\gcd$ để hoàn tất.

<!-- thinking:end -->

Ta có thể mô phỏng theo mô tả bài toán. Trước tiên, tìm giá trị lớn nhất và nhỏ nhất trong mảng $\textit{nums}$, sau đó tìm ước chung lớn nhất của hai giá trị này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findGCD(self, nums: List[int]) -> int:
        return gcd(max(nums), min(nums))
```

#### Java

```java
class Solution {
    public int findGCD(int[] nums) {
        int a = 1, b = 1000;
        for (int x : nums) {
            a = Math.max(a, x);
            b = Math.min(b, x);
        }
        return gcd(a, b);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findGCD(vector<int>& nums) {
        auto [min, max] = ranges::minmax_element(nums);
        return gcd(*min, *max);
    }
};
```

#### Go

```go
func findGCD(nums []int) int {
	a, b := slices.Max(nums), slices.Min(nums)
	return gcd(a, b)
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function findGCD(nums: number[]): number {
    const min = Math.min(...nums);
    const max = Math.max(...nums);
    return gcd(min, max);
}

function gcd(a: number, b: number): number {
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}
```

#### Rust

```rust
impl Solution {
    pub fn find_gcd(nums: Vec<i32>) -> i32 {
        let min_val = *nums.iter().min().unwrap();
        let max_val = *nums.iter().max().unwrap();
        gcd(min_val, max_val)
    }
}

fn gcd(mut a: i32, mut b: i32) -> i32 {
    while b != 0 {
        let temp = b;
        b = a % b;
        a = temp;
    }
    a
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
