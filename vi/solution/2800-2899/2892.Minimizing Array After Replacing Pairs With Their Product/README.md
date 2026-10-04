---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2892. Minimizing Array After Replacing Pairs With Their Product 🔒](https://leetcode.com/problems/minimizing-array-after-replacing-pairs-with-their-product)

[中文文档](/solution/2800-2899/2892.Minimizing%20Array%20After%20Replacing%20Pairs%20With%20Their%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, bạn có thể thực hiện thao tác sau trên mảng bao nhiêu lần tùy ý:</p>

<ul>
	<li>Chọn hai phần tử <strong>kề nhau</strong> của mảng, chẳng hạn <code>x</code> và <code>y</code>, sao cho <code>x * y &lt;= k</code>, rồi thay thế cả hai bằng một <strong>phần tử duy nhất</strong> có giá trị <code>x * y</code> (ví dụ, trong một thao tác, mảng <code>[1, 2, 2, 3]</code> với <code>k = 5</code> có thể trở thành <code>[1, 4, 3]</code> hoặc <code>[2, 2, 3]</code>, nhưng không thể trở thành <code>[1, 2, 6]</code>).</li>
</ul>

<p>Trả về <em>độ dài <strong>nhỏ nhất</strong> có thể có của </em><code>nums</code><em> sau khi thực hiện thao tác trên bao nhiêu lần tùy ý</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,3,7,3,5], k = 20
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta thực hiện các thao tác sau:
1. [<u>2,3</u>,3,7,3,5] -&gt; [<u>6</u>,3,7,3,5]
2. [<u>6,3</u>,7,3,5] -&gt; [<u>18</u>,7,3,5]
3. [18,7,<u>3,5</u>] -&gt; [18,7,<u>15</u>]
Có thể chứng minh rằng 3 là độ dài nhỏ nhất có thể đạt được với thao tác đã cho.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3,3], k = 6
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta không thể thực hiện thao tác nào vì tích của mọi cặp phần tử kề nhau đều lớn hơn 6.
Do đó, đáp án là 4.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị kề nhau có tích không vượt quá $k$ có thể được gộp. Nếu có số 0, tích của toàn bộ mảng sẽ là $0$, nên mảng có thể thu gọn còn độ dài $1$. Nếu không, ta mở rộng tích hiện tại khi tích vẫn $\le k$ và bắt đầu một đoạn mới khi không thể, nhờ đó số đoạn là nhỏ nhất.

<!-- thinking:end -->

Ta dùng một biến $ans$ để ghi nhận độ dài hiện tại của mảng và một biến $y$ để ghi nhận tích hiện tại của mảng. Ban đầu, $ans = 1$ và $y = nums[0]$.

Ta bắt đầu duyệt từ phần tử thứ hai của mảng. Gọi phần tử hiện tại là $x$:

- Nếu $x = 0$, tích của toàn bộ mảng là $0 \le k$, nên độ dài nhỏ nhất của mảng kết quả là $1$, và ta có thể trả về ngay.
- Nếu $x \times y \le k$, ta có thể gộp $x$ và $y$, tức là $y = x \times y$.
- Nếu $x \times y \gt k$, ta không thể gộp $x$ và $y$, nên cần xem $x$ là một phần tử riêng, tức là $ans = ans + 1$ và $y = x$.

Đáp án cuối cùng là $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó n là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minArrayLength(self, nums: List[int], k: int) -> int:
        ans, y = 1, nums[0]
        for x in nums[1:]:
            if x == 0:
                return 1
            if x * y <= k:
                y *= x
            else:
                y = x
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minArrayLength(int[] nums, int k) {
        int ans = 1;
        long y = nums[0];
        for (int i = 1; i < nums.length; ++i) {
            int x = nums[i];
            if (x == 0) {
                return 1;
            }
            if (x * y <= k) {
                y *= x;
            } else {
                y = x;
                ++ans;
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
    int minArrayLength(vector<int>& nums, int k) {
        int ans = 1;
        long long y = nums[0];
        for (int i = 1; i < nums.size(); ++i) {
            int x = nums[i];
            if (x == 0) {
                return 1;
            }
            if (x * y <= k) {
                y *= x;
            } else {
                y = x;
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minArrayLength(nums []int, k int) int {
	ans, y := 1, nums[0]
	for _, x := range nums[1:] {
		if x == 0 {
			return 1
		}
		if x*y <= k {
			y *= x
		} else {
			y = x
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minArrayLength(nums: number[], k: number): number {
    let [ans, y] = [1, nums[0]];
    for (const x of nums.slice(1)) {
        if (x === 0) {
            return 1;
        }
        if (x * y <= k) {
            y *= x;
        } else {
            y = x;
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
