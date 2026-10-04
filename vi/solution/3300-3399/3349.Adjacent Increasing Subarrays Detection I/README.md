---
comments: true
difficulty: Easy
rating: 1298
source: Weekly Contest 423 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3349. Adjacent Increasing Subarrays Detection I](https://leetcode.com/problems/adjacent-increasing-subarrays-detection-i)

[中文文档](/solution/3300-3399/3349.Adjacent%20Increasing%20Subarrays%20Detection%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm <code>n</code> số nguyên và một số nguyên <code>k</code>, hãy xác định xem có tồn tại <strong>hai</strong> <strong>liền kề</strong> <span data-keyword="subarray-nonempty">mảng con</span> có độ dài <code>k</code> sao cho cả hai mảng con đều <strong>tăng</strong> <strong>nghiêm ngặt</strong> hay không. Cụ thể, hãy kiểm tra xem có hai mảng con bắt đầu tại các chỉ số <code>a</code> và <code>b</code> (<code>a &lt; b</code>) hay không, trong đó:</p>

<ul>
    <li>Cả hai mảng con <code>nums[a..a + k - 1]</code> và <code>nums[b..b + k - 1]</code> đều <strong>tăng</strong> <strong>nghiêm ngặt</strong>.</li>
    <li>Hai mảng con phải <strong>liền kề</strong>, nghĩa là <code>b = a + k</code>.</li>
</ul>

<p>Trả về <code>true</code> nếu <em>có thể</em> tìm được <strong>hai </strong>mảng con như vậy, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,7,8,9,2,3,4,3,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Mảng con bắt đầu tại chỉ số <code>2</code> là <code>[7, 8, 9]</code>, tăng nghiêm ngặt.</li>
    <li>Mảng con bắt đầu tại chỉ số <code>5</code> là <code>[2, 3, 4]</code>, cũng tăng nghiêm ngặt.</li>
    <li>Hai mảng con này liền kề, nên kết quả là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,4,4,4,5,6,7], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 100</code></li>
    <li><code>1 &lt; 2 * k &lt;= nums.length</code></li>
    <li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần kiểm tra xem có tồn tại hai đoạn liền kề, tăng nghiêm ngặt, có độ dài $k$ hay không. Với $n \le 100$, ta có thể thử mọi vị trí bắt đầu; tuy nhiên, một lần duyệt cũng cho phép ta tìm được $k$ lớn nhất có thể.
>
> Hai đoạn có thể cùng nằm trong một đoạn tăng dài (với độ dài $\lfloor \textit{cur}/2 \rfloor$), hoặc nằm trên hai đoạn tăng liền kề (với độ dài $\min(\textit{pre},\textit{cur})$).
>
> Ở mỗi vị trí ngắt, ta cập nhật cả hai khả năng và cuối cùng kiểm tra xem giá trị lớn nhất có ít nhất bằng $k$ hay không.

<!-- thinking:end -->

Theo mô tả bài toán, ta chỉ cần tìm độ dài lớn nhất của hai mảng con tăng liền kề $\textit{mx}$. Nếu $\textit{mx} \ge k$, thì tồn tại hai mảng con tăng nghiêm ngặt liền kề có độ dài $k$.

Ta có thể thực hiện một lần duyệt để tính $\textit{mx}$. Cụ thể, ta duy trì ba biến: $\textit{cur}$ và $\textit{pre}$ lần lượt biểu diễn độ dài của mảng con tăng hiện tại và mảng con tăng trước đó, còn $\textit{mx}$ biểu diễn độ dài lớn nhất của hai mảng con tăng liền kề.

Mỗi khi gặp một vị trí không tăng, ta cập nhật $\textit{mx}$, gán $\textit{cur}$ cho $\textit{pre}$ và đặt lại $\textit{cur}$ về $0$. Công thức cập nhật $\textit{mx}$ là $\textit{mx} = \max(\textit{mx}, \lfloor \frac{\textit{cur}}{2} \rfloor, \min(\textit{pre}, \textit{cur}))$, nghĩa là hai mảng con tăng liền kề có thể được lấy từ một nửa độ dài của mảng con tăng hiện tại, hoặc từ giá trị nhỏ hơn giữa mảng con tăng trước đó và mảng con tăng hiện tại.

Cuối cùng, ta chỉ cần kiểm tra xem $\textit{mx}$ có lớn hơn hoặc bằng $k$ hay không.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasIncreasingSubarrays(self, nums: List[int], k: int) -> bool:
        mx = pre = cur = 0
        for i, x in enumerate(nums):
            cur += 1
            if i == len(nums) - 1 or x >= nums[i + 1]:
                mx = max(mx, cur // 2, min(pre, cur))
                pre, cur = cur, 0
        return mx >= k
```

#### Java

```java
class Solution {
    public boolean hasIncreasingSubarrays(List<Integer> nums, int k) {
        int mx = 0, pre = 0, cur = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            ++cur;
            if (i == n - 1 || nums.get(i) >= nums.get(i + 1)) {
                mx = Math.max(mx, Math.max(cur / 2, Math.min(pre, cur)));
                pre = cur;
                cur = 0;
            }
        }
        return mx >= k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasIncreasingSubarrays(vector<int>& nums, int k) {
        int mx = 0, pre = 0, cur = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            ++cur;
            if (i == n - 1 || nums[i] >= nums[i + 1]) {
                mx = max({mx, cur / 2, min(pre, cur)});
                pre = cur;
                cur = 0;
            }
        }
        return mx >= k;
    }
};
```

#### Go

```go
func hasIncreasingSubarrays(nums []int, k int) bool {
    mx, pre, cur := 0, 0, 0
    for i, x := range nums {
        cur++
        if i == len(nums)-1 || x >= nums[i+1] {
            mx = max(mx, max(cur/2, min(pre, cur)))
            pre, cur = cur, 0
        }
    }
    return mx >= k
}
```

#### TypeScript

```ts
function hasIncreasingSubarrays(nums: number[], k: number): boolean {
    let [mx, pre, cur] = [0, 0, 0];
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        ++cur;
        if (i === n - 1 || nums[i] >= nums[i + 1]) {
            mx = Math.max(mx, (cur / 2) | 0, Math.min(pre, cur));
            [pre, cur] = [cur, 0];
        }
    }
    return mx >= k;
}
```

#### Rust

```rust
impl Solution {
    pub fn has_increasing_subarrays(nums: Vec<i32>, k: i32) -> bool {
        let n = nums.len();
        let (mut mx, mut pre, mut cur) = (0, 0, 0);

        for i in 0..n {
            cur += 1;
            if i == n - 1 || nums[i] >= nums[i + 1] {
                mx = mx.max(cur / 2).max(pre.min(cur));
                pre = cur;
                cur = 0;
            }
        }

        mx >= k
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {boolean}
 */
var hasIncreasingSubarrays = function (nums, k) {
    const n = nums.length;
    let [mx, pre, cur] = [0, 0, 0];
    for (let i = 0; i < n; ++i) {
        ++cur;
        if (i === n - 1 || nums[i] >= nums[i + 1]) {
            mx = Math.max(mx, cur >> 1, Math.min(pre, cur));
            pre = cur;
            cur = 0;
        }
    }
    return mx >= k;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
