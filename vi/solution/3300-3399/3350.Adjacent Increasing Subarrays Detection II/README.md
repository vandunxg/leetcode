---
comments: true
difficulty: Medium
rating: 1600
source: Weekly Contest 423 Q2
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3350. Adjacent Increasing Subarrays Detection II](https://leetcode.com/problems/adjacent-increasing-subarrays-detection-ii)

[中文文档](/solution/3300-3399/3350.Adjacent%20Increasing%20Subarrays%20Detection%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm <code>n</code> số nguyên, nhiệm vụ của bạn là tìm giá trị <strong>lớn nhất</strong> của <code>k</code> sao cho tồn tại <strong>hai</strong> <span data-keyword="subarray-nonempty">mảng con</span> liền kề, mỗi mảng có độ dài <code>k</code>, và cả hai mảng con đều <strong>tăng</strong> <strong>nghiêm ngặt</strong>. Cụ thể, hãy kiểm tra xem có <strong>hai</strong> mảng con có độ dài <code>k</code> bắt đầu tại các chỉ số <code>a</code> và <code>b</code> (<code>a &lt; b</code>) hay không, trong đó:</p>

<ul>
    <li>Cả hai mảng con <code>nums[a..a + k - 1]</code> và <code>nums[b..b + k - 1]</code> đều <strong>tăng nghiêm ngặt</strong>.</li>
    <li>Hai mảng con phải <strong>liền kề</strong>, nghĩa là <code>b = a + k</code>.</li>
</ul>

<p>Trả về giá trị <strong>lớn nhất</strong> <em>có thể</em> của <code>k</code>.</p>

<p>Một <strong>mảng con</strong> là một dãy phần tử liên tiếp <b>không rỗng</b> nằm trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,7,8,9,2,3,4,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Mảng con bắt đầu tại chỉ số 2 là <code>[7, 8, 9]</code>, đây là mảng tăng nghiêm ngặt.</li>
    <li>Mảng con bắt đầu tại chỉ số 5 là <code>[2, 3, 4]</code>, cũng là mảng tăng nghiêm ngặt.</li>
    <li>Hai mảng con này liền kề, và 3 là giá trị <strong>lớn nhất</strong> có thể của <code>k</code> để tồn tại hai mảng con liền kề tăng nghiêm ngặt như vậy.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,4,4,4,5,6,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Mảng con bắt đầu tại chỉ số 0 là <code>[1, 2]</code>, đây là mảng tăng nghiêm ngặt.</li>
    <li>Mảng con bắt đầu tại chỉ số 2 là <code>[3, 4]</code>, cũng là mảng tăng nghiêm ngặt.</li>
    <li>Hai mảng con này liền kề, và 2 là giá trị <strong>lớn nhất</strong> có thể của <code>k</code> để tồn tại hai mảng con liền kề tăng nghiêm ngặt như vậy.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Phần I chỉ kiểm tra một giá trị $k$ cho trước; ở đây ta cần giá trị $k$ lớn nhất. Với $n \le 2 \times 10^5$, ta vẫn cần giữ lại đáp án trong cùng một lượt duyệt các điểm ngắt.
>
> Các ứng viên vẫn là “chia đôi một đoạn” và “lấy giá trị nhỏ hơn trong hai đoạn liền kề”.
>
> Giá trị lớn nhất được cập nhật ở cuối chính là $k$ lớn nhất khả thi.

<!-- thinking:end -->

Ta có thể dùng một lượt duyệt để tính độ dài lớn nhất của hai mảng con tăng liền kề, lưu trong biến $\textit{ans}$. Cụ thể, ta duy trì ba biến: $\textit{cur}$ và $\textit{pre}$ lần lượt biểu diễn độ dài của mảng con tăng hiện tại và mảng con tăng trước đó, còn $\textit{ans}$ biểu diễn độ dài lớn nhất của hai mảng con tăng liền kề.

Mỗi khi gặp một vị trí không tăng, ta cập nhật $\textit{ans}$, gán $\textit{cur}$ cho $\textit{pre}$ và đặt lại $\textit{cur}$ về $0$. Công thức cập nhật $\textit{ans}$ là $\textit{ans} = \max(\textit{ans}, \lfloor \frac{\textit{cur}}{2} \rfloor, \min(\textit{pre}, \textit{cur}))$, nghĩa là hai mảng con tăng liền kề có thể được lấy từ một nửa độ dài của mảng con tăng hiện tại, hoặc từ giá trị nhỏ hơn giữa mảng con tăng trước đó và mảng con tăng hiện tại.

Cuối cùng, ta chỉ cần trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxIncreasingSubarrays(self, nums: List[int]) -> int:
        ans = pre = cur = 0
        for i, x in enumerate(nums):
            cur += 1
            if i == len(nums) - 1 or x >= nums[i + 1]:
                ans = max(ans, cur // 2, min(pre, cur))
                pre, cur = cur, 0
        return ans
```

#### Java

```java
class Solution {
    public int maxIncreasingSubarrays(List<Integer> nums) {
        int ans = 0, pre = 0, cur = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            ++cur;
            if (i == n - 1 || nums.get(i) >= nums.get(i + 1)) {
                ans = Math.max(ans, Math.max(cur / 2, Math.min(pre, cur)));
                pre = cur;
                cur = 0;
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
    int maxIncreasingSubarrays(vector<int>& nums) {
        int ans = 0, pre = 0, cur = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            ++cur;
            if (i == n - 1 || nums[i] >= nums[i + 1]) {
                ans = max({ans, cur / 2, min(pre, cur)});
                pre = cur;
                cur = 0;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxIncreasingSubarrays(nums []int) (ans int) {
    pre, cur := 0, 0
    for i, x := range nums {
        cur++
        if i == len(nums)-1 || x >= nums[i+1] {
            ans = max(ans, max(cur/2, min(pre, cur)))
            pre, cur = cur, 0
        }
    }
    return
}
```

#### TypeScript

```ts
function maxIncreasingSubarrays(nums: number[]): number {
    let [ans, pre, cur] = [0, 0, 0];
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        ++cur;
        if (i === n - 1 || nums[i] >= nums[i + 1]) {
            ans = Math.max(ans, (cur / 2) | 0, Math.min(pre, cur));
            [pre, cur] = [cur, 0];
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_increasing_subarrays(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let (mut ans, mut pre, mut cur) = (0, 0, 0);

        for i in 0..n {
            cur += 1;
            if i == n - 1 || nums[i] >= nums[i + 1] {
                ans = ans.max(cur / 2).max(pre.min(cur));
                pre = cur;
                cur = 0;
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
var maxIncreasingSubarrays = function (nums) {
    let [ans, pre, cur] = [0, 0, 0];
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        ++cur;
        if (i === n - 1 || nums[i] >= nums[i + 1]) {
            ans = Math.max(ans, cur >> 1, Math.min(pre, cur));
            [pre, cur] = [cur, 0];
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
