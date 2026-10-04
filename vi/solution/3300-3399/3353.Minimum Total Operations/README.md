---
comments: true
difficulty: Easy
tags:
    - Array
---

<!-- problem:start -->

# [3353. Minimum Total Operations 🔒](https://leetcode.com/problems/minimum-total-operations)

[中文文档](/solution/3300-3399/3353.Minimum%20Total%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code><font face="monospace">nums</font></code>, bạn có thể thực hiện <em>bất kỳ</em> số lượng thao tác nào trên mảng này.</p>

<p>Trong mỗi <strong>thao tác</strong>, bạn có thể:</p>

<ul>
    <li>Chọn một <strong>tiền tố</strong> của mảng.</li>
    <li>Chọn một số nguyên <code><font face="monospace">k</font></code><font face="monospace"> </font>(có thể là số âm) và cộng <code><font face="monospace">k</font></code> vào mỗi phần tử trong tiền tố đã chọn.</li>
</ul>

<p>Một <strong>tiền tố</strong> của mảng là một mảng con bắt đầu từ đầu mảng và kéo dài đến một vị trí bất kỳ trong mảng.</p>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để làm cho tất cả phần tử trong <code>arr</code> bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Thao tác 1</strong>: Chọn tiền tố <code>[1, 4]</code> có độ dài 2 và cộng -2 vào mỗi phần tử của tiền tố. Mảng trở thành <code>[-1, 2, 2]</code>.</li>
    <li><strong>Thao tác 2</strong>: Chọn tiền tố <code>[-1]</code> có độ dài 1 và cộng 3 vào đó. Mảng trở thành <code>[2, 2, 2]</code>.</li>
    <li>Do đó, số thao tác tối thiểu cần thực hiện là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,10,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Tất cả phần tử đã bằng nhau nên không cần thực hiện thao tác nào.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác biến một tiền tố thành cùng một giá trị. Mục tiêu là làm cho toàn bộ mảng bằng nhau với ít thao tác nhất.
>
> Giá trị cuối cùng phải là $\textit{nums}[n-1]$. Mỗi cặp phần tử kề nhau khác nhau cần thêm một lần ghi đè tiền tố bên trái.
>
> Vì vậy, đáp án là số cặp phần tử kề nhau không bằng nhau, được đếm trong một lần duyệt.

<!-- thinking:end -->

Ta có thể duyệt qua mảng và với mỗi phần tử, nếu nó không bằng phần tử trước đó thì cần thực hiện một thao tác. Cuối cùng, trả về số thao tác.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        return sum(x != y for x, y in pairwise(nums))
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int ans = 0;
        for (int i = 1; i < nums.length; ++i) {
            ans += nums[i] != nums[i - 1] ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        int ans = 0;
        for (int i = 1; i < nums.size(); ++i) {
            ans += nums[i] != nums[i - 1];
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) (ans int) {
    for i, x := range nums[1:] {
        if x != nums[i] {
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    let ans = 0;
    for (let i = 1; i < nums.length; ++i) {
        ans += nums[i] !== nums[i - 1] ? 1 : 0;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        for i in 1..nums.len() {
            if nums[i] != nums[i - 1] {
                ans += 1;
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
var minOperations = function (nums) {
    let ans = 0;
    for (let i = 1; i < nums.length; ++i) {
        ans += nums[i] !== nums[i - 1] ? 1 : 0;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
