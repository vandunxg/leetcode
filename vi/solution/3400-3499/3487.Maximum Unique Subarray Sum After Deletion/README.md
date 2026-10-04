---
comments: true
difficulty: Easy
rating: 1399
source: Weekly Contest 441 Q1
tags:
    - Greedy
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3487. Maximum Unique Subarray Sum After Deletion](https://leetcode.com/problems/maximum-unique-subarray-sum-after-deletion)

[中文文档](/solution/3400-3499/3487.Maximum%20Unique%20Subarray%20Sum%20After%20Deletion/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn được phép xóa bất kỳ số phần tử nào khỏi <code>nums</code> nhưng không được để mảng trở thành <strong>rỗng</strong>. Sau khi thực hiện thao tác xóa, hãy chọn một <span data-keyword="subarray-nonempty">mảng con</span> của <code>nums</code> sao cho:</p>

<ol>
    <li>Mọi phần tử trong mảng con đều <strong>khác nhau</strong>.</li>
    <li>Tổng các phần tử trong mảng con là <strong>lớn nhất</strong>.</li>
</ol>

<p>Trả về <strong>tổng lớn nhất</strong> của một mảng con như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn toàn bộ mảng mà không xóa phần tử nào để đạt được tổng lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,0,1,1]</span></p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Xóa phần tử <code>nums[0] == 1</code>, <code>nums[1] == 1</code>, <code>nums[2] == 0</code> và <code>nums[3] == 1</code>. Chọn toàn bộ mảng <code>[1]</code> để đạt được tổng lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,-1,-2,1,0,-1]</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p>Xóa các phần tử <code>nums[2] == -1</code> và <code>nums[3] == -2</code>, sau đó chọn mảng con <code>[2, 1]</code> từ <code>[1, 2, 1, 0, -1]</code> để đạt được tổng lớn nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 100</code></li>
    <li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể xóa bất kỳ phần tử nào; các phần tử còn lại phải có giá trị đôi một khác nhau và tổng lớn nhất. Nếu tất cả đều không dương, lựa chọn tốt nhất là một phần tử có giá trị lớn nhất.
>
> Khi có ít nhất một số dương, các số âm và các số dương trùng lặp không bao giờ có lợi: số âm làm giảm tổng, còn phần tử trùng lặp là không hợp lệ.
>
> Nếu giá trị lớn nhất toàn cục không dương, trả về giá trị đó. Ngược lại, dùng một set để cộng các số dương phân biệt.

<!-- thinking:end -->

Trước hết, ta tìm giá trị lớn nhất $\textit{mx}$ trong mảng. Nếu $\textit{mx} \leq 0$, mọi phần tử trong mảng đều nhỏ hơn hoặc bằng 0. Vì cần chọn một mảng con không rỗng có tổng lớn nhất, tổng lớn nhất sẽ là $\textit{mx}$.

Nếu $\textit{mx} > 0$, ta cần tìm tất cả các số nguyên dương phân biệt trong mảng sao cho tổng của chúng là lớn nhất. Ta có thể dùng một hash table $\textit{s}$ để ghi nhận tất cả các số nguyên dương phân biệt, sau đó duyệt qua mảng và cộng các số nguyên dương phân biệt.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, nums: List[int]) -> int:
        mx = max(nums)
        if mx <= 0:
            return mx
        ans = 0
        s = set()
        for x in nums:
            if x < 0 or x in s:
                continue
            ans += x
            s.add(x)
        return ans
```

#### Java

```java
class Solution {
    public int maxSum(int[] nums) {
        int mx = Arrays.stream(nums).max().getAsInt();
        if (mx <= 0) {
            return mx;
        }
        boolean[] s = new boolean[201];
        int ans = 0;
        for (int x : nums) {
            if (x < 0 || s[x]) {
                continue;
            }
            ans += x;
            s[x] = true;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSum(vector<int>& nums) {
        int mx = ranges::max(nums);
        if (mx <= 0) {
            return mx;
        }
        unordered_set<int> s;
        int ans = 0;
        for (int x : nums) {
            if (x < 0 || s.contains(x)) {
                continue;
            }
            ans += x;
            s.insert(x);
        }
        return ans;
    }
};
```

#### Go

```go
func maxSum(nums []int) (ans int) {
    mx := slices.Max(nums)
    if mx <= 0 {
        return mx
    }
    s := make(map[int]bool)
    for _, x := range nums {
        if x < 0 || s[x] {
            continue
        }
        ans += x
        s[x] = true
    }
    return
}
```

#### TypeScript

```ts
function maxSum(nums: number[]): number {
    const mx = Math.max(...nums);
    if (mx <= 0) {
        return mx;
    }
    const s = new Set<number>();
    let ans: number = 0;
    for (const x of nums) {
        if (x < 0 || s.has(x)) {
            continue;
        }
        ans += x;
        s.add(x);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn max_sum(nums: Vec<i32>) -> i32 {
        let mx = *nums.iter().max().unwrap_or(&0);
        if mx <= 0 {
            return mx;
        }

        let mut s = HashSet::new();
        let mut ans = 0;

        for &x in &nums {
            if x < 0 || s.contains(&x) {
                continue;
            }
            ans += x;
            s.insert(x);
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxSum(int[] nums) {
        int mx = nums.Max();
        if (mx <= 0) {
            return mx;
        }

        HashSet<int> s = new HashSet<int>();
        int ans = 0;

        foreach (int x in nums) {
            if (x < 0 || s.Contains(x)) {
                continue;
            }
            ans += x;
            s.Add(x);
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
