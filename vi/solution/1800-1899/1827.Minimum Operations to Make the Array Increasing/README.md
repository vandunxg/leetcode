---
comments: true
difficulty: Easy
rating: 1314
source: Biweekly Contest 50 Q1
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1827. Minimum Operations to Make the Array Increasing](https://leetcode.com/problems/minimum-operations-to-make-the-array-increasing)

[中文文档](/solution/1800-1899/1827.Minimum%20Operations%20to%20Make%20the%20Array%20Increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> (<strong>đánh chỉ số từ 0</strong>). Trong một thao tác, bạn có thể chọn một phần tử của mảng và tăng nó lên <code>1</code>.</p>

<ul>
<li>Ví dụ, nếu <code>nums = [1,2,3]</code>, bạn có thể tăng <code>nums[1]</code> để được <code>nums = [1,<u><b>3</b></u>,3]</code>.</li>
</ul>

<p>Trả về <em><strong>số thao tác ít nhất</strong> cần thực hiện để biến</em> <code>nums</code> <em>thành mảng <strong>tăng</strong> <strong>nghiêm ngặt</strong>.</em></p>

<p>Mảng <code>nums</code> được gọi là <strong>tăng nghiêm ngặt</strong> nếu <code>nums[i] &lt; nums[i+1]</code> với mọi <code>0 &lt;= i &lt; nums.length - 1</code>. Mảng có độ dài <code>1</code> hiển nhiên là tăng nghiêm ngặt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có thể thực hiện các thao tác sau:
1) Tăng nums[2], khi đó nums trở thành [1,1,<u><strong>2</strong></u>].
2) Tăng nums[1], khi đó nums trở thành [1,<u><strong>2</strong></u>,2].
3) Tăng nums[2], khi đó nums trở thành [1,2,<u><strong>3</strong></u>].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,2,4,1]
<strong>Đầu ra:</strong> 14
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ được tăng các phần tử, và mảng phải trở thành mảng tăng nghiêm ngặt với chi phí nhỏ nhất. Không cần tìm các giá trị cuối cùng tại từng chỉ số.
>
> Khi đi từ trái sang phải, giá trị lớn nhất của tiền tố đã xác định cận dưới tiếp theo. Nếu $v$ nhỏ hơn $mx+1$, ta trả phần chênh lệch và tăng nó lên; sau đó $mx$ trở thành giá trị lớn nhất mới của tiền tố. Tăng thêm chỉ làm các cận sau lớn hơn, nên một lần duyệt tham lam là tối ưu.

<!-- thinking:end -->

Ta dùng biến $mx$ để ghi nhận giá trị lớn nhất của mảng tăng nghiêm ngặt hiện tại, ban đầu $mx = 0$.

Duyệt mảng `nums` từ trái sang phải. Với phần tử hiện tại $v$, nếu $v \lt mx + 1$, ta cần tăng nó lên $mx + 1$ để đảm bảo mảng tăng nghiêm ngặt. Vì vậy, số thao tác lần này là $max(0, mx + 1 - v)$, được cộng vào đáp án, rồi cập nhật $mx=max(mx + 1, v)$. Tiếp tục với phần tử kế tiếp cho đến khi duyệt hết mảng.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng `nums`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        ans = mx = 0
        for v in nums:
            ans += max(0, mx + 1 - v)
            mx = max(mx + 1, v)
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int ans = 0, mx = 0;
        for (int v : nums) {
            ans += Math.max(0, mx + 1 - v);
            mx = Math.max(mx + 1, v);
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
        int ans = 0, mx = 0;
        for (int& v : nums) {
            ans += max(0, mx + 1 - v);
            mx = max(mx + 1, v);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) (ans int) {
	mx := 0
	for _, v := range nums {
		ans += max(0, mx+1-v)
		mx = max(mx+1, v)
	}
	return
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    let ans = 0;
    let max = 0;
    for (const v of nums) {
        ans += Math.max(0, max + 1 - v);
        max = Math.max(max + 1, v);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mut max = 0;
        for &v in nums.iter() {
            ans += (0).max(max + 1 - v);
            max = v.max(max + 1);
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MinOperations(int[] nums) {
        int ans = 0, mx = 0;
        foreach (int v in nums) {
            ans += Math.Max(0, mx + 1 - v);
            mx = Math.Max(mx + 1, v);
        }
        return ans;
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int minOperations(int* nums, int numsSize) {
    int ans = 0;
    int mx = 0;
    for (int i = 0; i < numsSize; i++) {
        ans += max(0, mx + 1 - nums[i]);
        mx = max(mx + 1, nums[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
