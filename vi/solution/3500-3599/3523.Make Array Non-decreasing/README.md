---
comments: true
difficulty: Medium
rating: 1435
source: Weekly Contest 446 Q2
tags:
    - Stack
    - Greedy
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [3523. Make Array Non-decreasing](https://leetcode.com/problems/make-array-non-decreasing)

[中文文档](/solution/3500-3599/3523.Make%20Array%20Non-decreasing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Trong một thao tác, bạn có thể chọn một <span data-keyword="subarray-nonempty">mảng con</span> và thay thế nó bằng một phần tử duy nhất có giá trị bằng giá trị <strong>lớn nhất</strong> của mảng con.</p>

<p>Trả về <strong>kích thước lớn nhất có thể</strong> của mảng sau khi thực hiện không hoặc nhiều thao tác sao cho mảng kết quả là <strong>không giảm</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,5,3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách để đạt được kích thước lớn nhất là:</p>

<ol>
    <li>Thay thế mảng con <code>nums[1..2] = [2, 5]</code> bằng <code>5</code> &rarr; <code>[4, 5, 3, 5]</code>.</li>
    <li>Thay thế mảng con <code>nums[2..3] = [3, 5]</code> bằng <code>5</code> &rarr; <code>[4, 5, 5]</code>.</li>
</ol>

<p>Mảng cuối cùng <code>[4, 5, 5]</code> là mảng không giảm và có kích thước <font face="monospace">3.</font></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không cần thực hiện thao tác nào vì mảng <code>[1,2,3]</code> đã không giảm.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 2 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác gộp một mảng con thành giá trị lớn nhất của nó, vì vậy dãy cuối cùng là một chuỗi không giảm được chọn từ trái sang phải. Với $n \le 2 \cdot 10^5$, không thể thử mọi cách gộp.
>
> Giữ lại mọi giá trị lớn hơn hoặc bằng giá trị lớn nhất hiện tại rồi cập nhật giá trị lớn nhất đó. Các giá trị bị bỏ qua có thể được gộp vào một đoạn lớn hơn (hoặc bằng) ở phía sau, nên số lượng giá trị được giữ lại là tối ưu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumPossibleSize(self, nums: List[int]) -> int:
        ans = mx = 0
        for x in nums:
            if mx <= x:
                ans += 1
                mx = x
        return ans
```

#### Java

```java
class Solution {
    public int maximumPossibleSize(int[] nums) {
        int ans = 0, mx = 0;
        for (int x : nums) {
            if (mx <= x) {
                ++ans;
                mx = x;
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
    int maximumPossibleSize(vector<int>& nums) {
        int ans = 0, mx = 0;
        for (int x : nums) {
            if (mx <= x) {
                ++ans;
                mx = x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumPossibleSize(nums []int) int {
    ans, mx := 0, 0
    for _, x := range nums {
        if mx <= x {
            ans++
            mx = x
        }
    }
    return ans
}
```

#### TypeScript

```ts
function maximumPossibleSize(nums: number[]): number {
    let [ans, mx] = [0, 0];
    for (const x of nums) {
        if (mx <= x) {
            ++ans;
            mx = x;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
