---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k)

[中文文档](/solution/0700-0799/0713.Subarray%20Product%20Less%20Than%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về <em>số mảng con liên tiếp có tích của tất cả phần tử nhỏ hơn nghiêm ngặt </em><code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,5,2,6], k = 100
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Có 8 mảng con có tích nhỏ hơn 100:
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
Lưu ý, [10, 5, 2] không được tính vì tích bằng 100, không nhỏ hơn nghiêm ngặt k.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], k = 0
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các mảng con liên tiếp có tích nhỏ hơn nghiêm ngặt $k$. Vì $n$ có thể bằng $3\times 10^4$, duyệt cả hai đầu mảng sẽ tốn $O(n^2)$.
>
> Mọi giá trị đều dương nên tích của window không giảm khi tăng đầu phải. Khi tích đạt đến $k$, chỉ cần dịch đầu trái để window trở nên hợp lệ; mỗi pointer chỉ di chuyển tối đa một lần.
>
> Duy trì tích $p$ và đầu trái $l$. Sau khi nhân với $x$, chia cho $\textit{nums}[l]$ khi $p\ge k$. Khi đó có $r-l+1$ mảng con kết thúc tại $r$. Chỉ cần duyệt một lượt.

<!-- thinking:end -->

Ta có thể dùng two pointers để duy trì một sliding window sao cho tích của mọi phần tử trong window nhỏ hơn $k$.

Đặt hai pointer $l$ và $r$ ở hai đầu trái và phải của sliding window, ban đầu $l = r = 0$. Ta dùng biến $p$ để lưu tích các phần tử trong window, ban đầu $p = 1$.

Mỗi lần, ta dịch $r$ sang phải một bước, thêm phần tử $x$ tại vị trí đó vào window và cập nhật $p = p \times x$. Nếu $p \geq k$, ta liên tục dịch $l$ sang phải và cập nhật $p = p \div \text{nums}[l]$ cho đến khi $p < k$ hoặc $l \gt r$. Khi đó, có $r - l + 1$ mảng con kết thúc tại $r$ có tích nhỏ hơn $k$. Ta cộng số này vào đáp án rồi tiếp tục dịch $r$ cho đến cuối mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        ans = l = 0
        p = 1
        for r, x in enumerate(nums):
            p *= x
            while l <= r and p >= k:
                p //= nums[l]
                l += 1
            ans += r - l + 1
        return ans
```

#### Java

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int ans = 0, l = 0;
        int p = 1;
        for (int r = 0; r < nums.length; ++r) {
            p *= nums[r];
            while (l <= r && p >= k) {
                p /= nums[l++];
            }
            ans += r - l + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int ans = 0, l = 0;
        int p = 1;
        for (int r = 0; r < nums.size(); ++r) {
            p *= nums[r];
            while (l <= r && p >= k) {
                p /= nums[l++];
            }
            ans += r - l + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func numSubarrayProductLessThanK(nums []int, k int) (ans int) {
    l, p := 0, 1
    for r, x := range nums {
        p *= x
        for l <= r && p >= k {
            p /= nums[l]
            l++
        }
        ans += r - l + 1
    }
    return
}
```

#### TypeScript

```ts
function numSubarrayProductLessThanK(nums: number[], k: number): number {
    const n = nums.length;
    let [ans, l, p] = [0, 0, 1];
    for (let r = 0; r < n; ++r) {
        p *= nums[r];
        while (l <= r && p >= k) {
            p /= nums[l++];
        }
        ans += r - l + 1;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_subarray_product_less_than_k(nums: Vec<i32>, k: i32) -> i32 {
        let mut ans = 0;
        let mut l = 0;
        let mut p = 1;

        for (r, &x) in nums.iter().enumerate() {
            p *= x;
            while l <= r && p >= k {
                p /= nums[l];
                l += 1;
            }
            ans += (r - l + 1) as i32;
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var numSubarrayProductLessThanK = function (nums, k) {
    const n = nums.length;
    let [ans, l, p] = [0, 0, 1];
    for (let r = 0; r < n; ++r) {
        p *= nums[r];
        while (l <= r && p >= k) {
            p /= nums[l++];
        }
        ans += r - l + 1;
    }
    return ans;
};
```

#### Kotlin

```kotlin
class Solution {
    fun numSubarrayProductLessThanK(nums: IntArray, k: Int): Int {
        var ans = 0
        var l = 0
        var p = 1

        for (r in nums.indices) {
            p *= nums[r]
            while (l <= r && p >= k) {
                p /= nums[l]
                l++
            }
            ans += r - l + 1
        }

        return ans
    }
}
```

#### C#

```cs
public class Solution {
    public int NumSubarrayProductLessThanK(int[] nums, int k) {
        int ans = 0, l = 0;
        int p = 1;
        for (int r = 0; r < nums.Length; ++r) {
            p *= nums[r];
            while (l <= r && p >= k) {
                p /= nums[l++];
            }
            ans += r - l + 1;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
