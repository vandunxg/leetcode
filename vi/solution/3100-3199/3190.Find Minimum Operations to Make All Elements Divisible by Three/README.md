---
comments: true
difficulty: Easy
rating: 1139
source: Biweekly Contest 133 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3190. Find Minimum Operations to Make All Elements Divisible by Three](https://leetcode.com/problems/find-minimum-operations-to-make-all-elements-divisible-by-three)

[中文文档](/solution/3100-3199/3190.Find%20Minimum%20Operations%20to%20Make%20All%20Elements%20Divisible%20by%20Three/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Trong một thao tác, bạn có thể cộng hoặc trừ 1 vào <strong>bất kỳ</strong> phần tử nào của <code>nums</code>.</p>

<p>Hãy trả về số thao tác <strong>ít nhất</strong> để tất cả phần tử của <code>nums</code> đều chia hết cho 3.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể làm cho tất cả phần tử của mảng chia hết cho 3 bằng 3 thao tác:</p>

<ul>
    <li>Trừ 1 khỏi 1.</li>
    <li>Cộng 1 vào 2.</li>
    <li>Trừ 1 khỏi 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,6,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 50</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác cộng hoặc trừ một đơn vị từ một giá trị. Phần dư $1$ cần giảm đi một đơn vị, phần dư $2$ cần tăng thêm một đơn vị, còn phần dư $0$ thì không cần làm gì.
>
> Các phần tử không ảnh hưởng lẫn nhau, nên đáp án là số phần tử không phải bội của $3$.
>
> Tính tổng của điều kiện $x\bmod 3\neq 0$.

<!-- thinking:end -->

Ta duyệt trực tiếp qua mảng $\textit{nums}$. Với mỗi phần tử $x$, nếu $x \bmod 3 \neq 0$, có hai trường hợp:

- Nếu $x \bmod 3 = 1$, ta có thể giảm $x$ đi $1$ để được $x - 1$, là số chia hết cho $3$.
- Nếu $x \bmod 3 = 2$, ta có thể tăng $x$ thêm $1$ để được $x + 1$, là số chia hết cho $3$.

Do đó, chỉ cần đếm số phần tử trong mảng không chia hết cho $3$ là có được số thao tác ít nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        return sum(x % 3 != 0 for x in nums)
```

#### Java

```java
class Solution {
    public int minimumOperations(int[] nums) {
        int ans = 0;
        for (int x : nums) {
            ans += x % 3 != 0 ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<int>& nums) {
        int ans = 0;
        for (int x : nums) {
            ans += x % 3 != 0 ? 1 : 0;
        }
        return ans;
    }
};
```

#### Go

```go
func minimumOperations(nums []int) (ans int) {
    for _, x := range nums {
        if x%3 != 0 {
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function minimumOperations(nums: number[]): number {
    return nums.reduce((acc, x) => acc + (x % 3 !== 0 ? 1 : 0), 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_operations(nums: Vec<i32>) -> i32 {
        nums.iter().filter(|&&x| x % 3 != 0).count() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
