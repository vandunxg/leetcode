---
comments: true
difficulty: Easy
rating: 1209
source: Weekly Contest 236 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [1822. Sign of the Product of an Array](https://leetcode.com/problems/sign-of-the-product-of-an-array)

[中文文档](/solution/1800-1899/1822.Sign%20of%20the%20Product%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Triển khai hàm <code>signFunc(x)</code> trả về:</p>

<ul>
<li><code>1</code> nếu <code>x</code> dương.</li>
<li><code>-1</code> nếu <code>x</code> âm.</li>
<li><code>0</code> nếu <code>x</code> bằng <code>0</code>.</li>
</ul>

<p>Cho mảng số nguyên <code>nums</code>. Gọi <code>product</code> là tích của mọi giá trị trong mảng <code>nums</code>.</p>

<p>Trả về <code>signFunc(product)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,-2,-3,-4,3,2,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Tích của mọi giá trị trong mảng là 144, và signFunc(144) = 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,0,2,-3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tích của mọi giá trị trong mảng là 0, và signFunc(0) = 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,1,-1,1,-1]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Tích của mọi giá trị trong mảng là -1, và signFunc(-1) = -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Dấu của tích chỉ phụ thuộc vào các số 0 và số lượng số âm. Tính toàn bộ tích có thể gây tràn số.
>
> Duy trì dấu hiện tại $ans$, trả về $0$ khi gặp số 0 và đảo dấu $ans$ khi gặp số âm. Cách duyệt này tìm được dấu mà không cần nhân trực tiếp các giá trị.

<!-- thinking:end -->

Bài toán yêu cầu trả về dấu của tích các phần tử trong mảng, tức là trả về $1$ với số dương, $-1$ với số âm và $0$ nếu tích bằng $0$.

Ta định nghĩa biến kết quả `ans`, ban đầu bằng $1$.

Sau đó, ta duyệt từng phần tử $v$ trong mảng. Nếu $v$ là số âm, ta nhân `ans` với $-1$. Nếu $v$ bằng $0$, ta trả về $0$ ngay.

Sau khi duyệt xong, ta trả về `ans`.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arraySign(self, nums: List[int]) -> int:
        ans = 1
        for v in nums:
            if v == 0:
                return 0
            if v < 0:
                ans *= -1
        return ans
```

#### Java

```java
class Solution {
    public int arraySign(int[] nums) {
        int ans = 1;
        for (int v : nums) {
            if (v == 0) {
                return 0;
            }
            if (v < 0) {
                ans *= -1;
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
    int arraySign(vector<int>& nums) {
        int ans = 1;
        for (int v : nums) {
            if (!v) return 0;
            if (v < 0) ans *= -1;
        }
        return ans;
    }
};
```

#### Go

```go
func arraySign(nums []int) int {
	ans := 1
	for _, v := range nums {
		if v == 0 {
			return 0
		}
		if v < 0 {
			ans *= -1
		}
	}
	return ans
}
```

#### Rust

```rust
impl Solution {
    pub fn array_sign(nums: Vec<i32>) -> i32 {
        let mut ans = 1;
        for &num in nums.iter() {
            if num == 0 {
                return 0;
            }
            if num < 0 {
                ans *= -1;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var arraySign = function (nums) {
    let ans = 1;
    for (const v of nums) {
        if (!v) {
            return 0;
        }
        if (v < 0) {
            ans *= -1;
        }
    }
    return ans;
};
```

#### C

```c
int arraySign(int* nums, int numsSize) {
    int ans = 1;
    for (int i = 0; i < numsSize; i++) {
        if (nums[i] == 0) {
            return 0;
        }
        if (nums[i] < 0) {
            ans *= -1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
