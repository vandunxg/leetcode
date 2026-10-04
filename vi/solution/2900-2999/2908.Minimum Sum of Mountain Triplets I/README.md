---
comments: true
difficulty: Easy
rating: 1253
source: Weekly Contest 368 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2908. Minimum Sum of Mountain Triplets I](https://leetcode.com/problems/minimum-sum-of-mountain-triplets-i)

[中文文档](/solution/2900-2999/2908.Minimum%20Sum%20of%20Mountain%20Triplets%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code>.</p>

<p>Một bộ ba chỉ số <code>(i, j, k)</code> là một <strong>bộ ba dạng núi</strong> nếu:</p>

<ul>
	<li><code>i &lt; j &lt; k</code></li>
	<li><code>nums[i] &lt; nums[j]</code> và <code>nums[k] &lt; nums[j]</code></li>
</ul>

<p>Hãy trả về <em><strong>tổng nhỏ nhất có thể</strong> của một bộ ba dạng núi của</em> <code>nums</code>. <em>Nếu không tồn tại bộ ba như vậy, trả về</em> <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8,6,1,5,3]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Bộ ba (2, 3, 4) là một bộ ba dạng núi có tổng bằng 9 vì:
- 2 &lt; 3 &lt; 4
- nums[2] &lt; nums[3] và nums[4] &lt; nums[3]
Tổng của bộ ba này là nums[2] + nums[3] + nums[4] = 9. Có thể chứng minh rằng không tồn tại bộ ba dạng núi nào có tổng nhỏ hơn 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,8,7,10,2]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Bộ ba (1, 3, 5) là một bộ ba dạng núi có tổng bằng 13 vì:
- 1 &lt; 3 &lt; 5
- nums[1] &lt; nums[3] và nums[5] &lt; nums[3]
Tổng của bộ ba này là nums[1] + nums[3] + nums[5] = 13. Có thể chứng minh rằng không tồn tại bộ ba dạng núi nào có tổng nhỏ hơn 13.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,5,4,3,4,5]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không tồn tại bộ ba dạng núi nào trong nums.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một bộ ba dạng núi cần $i<j<k$ và $nums[i]<nums[j]>nums[k]$, đồng thời tổng cần được tối thiểu hóa. Vì $n \le 50$, ta có thể dùng ba vòng lặp lồng nhau. Sau khi cố định đỉnh $j$, chỉ cần quan tâm đến giá trị nhỏ nhất ở bên trái và bên phải, đồng thời cả hai phải nhỏ hơn nghiêm ngặt so với $nums[j]$.
>
> Ta tiền xử lý các giá trị nhỏ nhất ở hậu tố trong $right$, sau đó duyệt từ trái sang phải và theo dõi $left$. Với mỗi đỉnh, kiểm tra điều kiện rồi cập nhật tổng bộ ba, hoặc trả về $-1$ nếu không tìm thấy bộ ba nào.

<!-- thinking:end -->

Ta có thể tiền xử lý giá trị nhỏ nhất ở bên phải của mỗi vị trí và lưu vào mảng $right[i]$, trong đó $right[i]$ biểu diễn giá trị nhỏ nhất trong $nums[i+1..n-1]$.

Tiếp theo, ta liệt kê phần tử giữa $nums[i]$ của bộ ba dạng núi từ trái sang phải, đồng thời dùng biến $left$ để biểu diễn giá trị nhỏ nhất trong $ums[0..i-1]$, và biến $ans$ để biểu diễn tổng nhỏ nhất hiện tại. Với mỗi $i$, ta cần tìm phần tử $nums[i]$ thỏa mãn $left < nums[i]$ và $right[i+1] < nums[i]$, sau đó cập nhật $ans$.

Cuối cùng, nếu $ans$ vẫn giữ giá trị khởi tạo, điều đó có nghĩa là không tồn tại bộ ba dạng núi nào, và ta trả về $-1$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSum(self, nums: List[int]) -> int:
        n = len(nums)
        right = [inf] * (n + 1)
        for i in range(n - 1, -1, -1):
            right[i] = min(right[i + 1], nums[i])
        ans = left = inf
        for i, x in enumerate(nums):
            if left < x and right[i + 1] < x:
                ans = min(ans, left + x + right[i + 1])
            left = min(left, x)
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int minimumSum(int[] nums) {
        int n = nums.length;
        int[] right = new int[n + 1];
        final int inf = 1 << 30;
        right[n] = inf;
        for (int i = n - 1; i >= 0; --i) {
            right[i] = Math.min(right[i + 1], nums[i]);
        }
        int ans = inf, left = inf;
        for (int i = 0; i < n; ++i) {
            if (left < nums[i] && right[i + 1] < nums[i]) {
                ans = Math.min(ans, left + nums[i] + right[i + 1]);
            }
            left = Math.min(left, nums[i]);
        }
        return ans == inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSum(vector<int>& nums) {
        int n = nums.size();
        const int inf = 1 << 30;
        int right[n + 1];
        right[n] = inf;
        for (int i = n - 1; ~i; --i) {
            right[i] = min(right[i + 1], nums[i]);
        }
        int ans = inf, left = inf;
        for (int i = 0; i < n; ++i) {
            if (left < nums[i] && right[i + 1] < nums[i]) {
                ans = min(ans, left + nums[i] + right[i + 1]);
            }
            left = min(left, nums[i]);
        }
        return ans == inf ? -1 : ans;
    }
};
```

#### Go

```go
func minimumSum(nums []int) int {
	n := len(nums)
	const inf = 1 << 30
	right := make([]int, n+1)
	right[n] = inf
	for i := n - 1; i >= 0; i-- {
		right[i] = min(right[i+1], nums[i])
	}
	ans, left := inf, inf
	for i, x := range nums {
		if left < x && right[i+1] < x {
			ans = min(ans, left+x+right[i+1])
		}
		left = min(left, x)
	}
	if ans == inf {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumSum(nums: number[]): number {
    const n = nums.length;
    const right: number[] = Array(n + 1).fill(Infinity);
    for (let i = n - 1; ~i; --i) {
        right[i] = Math.min(right[i + 1], nums[i]);
    }
    let [ans, left] = [Infinity, Infinity];
    for (let i = 0; i < n; ++i) {
        if (left < nums[i] && right[i + 1] < nums[i]) {
            ans = Math.min(ans, left + nums[i] + right[i + 1]);
        }
        left = Math.min(left, nums[i]);
    }
    return ans === Infinity ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
