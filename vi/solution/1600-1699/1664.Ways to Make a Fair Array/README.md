---
comments: true
difficulty: Medium
rating: 1590
source: Weekly Contest 216 Q3
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1664. Ways to Make a Fair Array](https://leetcode.com/problems/ways-to-make-a-fair-array)

[中文文档](/solution/1600-1699/1664.Ways%20to%20Make%20a%20Fair%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Bạn phải chọn <strong>chính xác một</strong> chỉ số (<strong>đánh chỉ số từ 0</strong>) và xóa phần tử tại đó. Lưu ý chỉ số các phần tử có thể thay đổi sau khi xóa.</p>

<p>For example, if <code>nums = [6,1,7,4,1]</code>:</p>

<ul>
	<li>Choosing to remove index <code>1</code> results in <code>nums = [6,7,4,1]</code>.</li>
	<li>Choosing to remove index <code>2</code> results in <code>nums = [6,1,4,1]</code>.</li>
	<li>Choosing to remove index <code>4</code> results in <code>nums = [6,1,7,4]</code>.</li>
</ul>

<p>Một mảng là <strong>cân bằng</strong> nếu tổng các giá trị ở chỉ số lẻ bằng tổng các giá trị ở chỉ số chẵn.</p>

<p>Trả về <em><strong>số lượng</strong> chỉ số có thể chọn sao cho sau khi xóa, </em><code>nums</code><em> </em><em>trở thành <strong>cân bằng</strong>. </em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,1,6,4]
<strong>Output:</strong> 1
<strong>Explanation:</strong>
Remove index 0: [1,6,4] -&gt; Even sum: 1 + 4 = 5. Odd sum: 6. Not fair.
Remove index 1: [2,6,4] -&gt; Even sum: 2 + 4 = 6. Odd sum: 6. Fair.
Remove index 2: [2,1,4] -&gt; Even sum: 2 + 4 = 6. Odd sum: 1. Not fair.
Remove index 3: [2,1,6] -&gt; Even sum: 2 + 6 = 8. Odd sum: 1. Not fair.
There is 1 index that you can remove to make nums fair.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,1,1]
<strong>Output:</strong> 3
<strong>Explanation:</strong>&nbsp;You can remove any index and the remaining array is fair.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> 0
<strong>Explanation:</strong>&nbsp;You cannot make a fair array after removing any index.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Enumeration + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chỉ số mà khi xóa sẽ làm tổng ở vị trí chẵn và lẻ bằng nhau. Vì $n$ có thể là $10^5$, việc xóa làm đảo parity ở phần bên phải, nên tổng chẵn/lẻ của prefix và suffix giúp quyết định mỗi chỉ số trong $O(1)$.
>
> Tính trước tổng chẵn $s_1$ và tổng lẻ $s_2$. Khi duyệt, $t_1,t_2$ là tổng chẵn/lẻ ở bên trái; dựa vào parity của chỉ số hiện tại để tính các tổng sau khi xóa.

<!-- thinking:end -->

Đầu tiên, ta tiền xử lý để lấy tổng $s_1$ của các phần tử ở chỉ số chẵn và tổng $s_2$ của các phần tử ở chỉ số lẻ trong mảng `nums`.

Sau đó, ta duyệt từng phần tử $v$ trong mảng `nums` từ đầu đến cuối, dùng $t_1$ và $t_2$ để lưu tổng các phần tử ở chỉ số chẵn và lẻ đã đi qua.

Ta nhận thấy nếu xóa phần tử hiện tại $v$, tổng các phần tử ở chỉ số chẵn và lẻ phía sau nó sẽ bị hoán đổi. Vì vậy, trước tiên ta xác định chỉ số hiện tại $i$ là chẵn hay lẻ.

- If it is an even index, after deleting the element, the sum of the elements at odd indices in the array is $t_2 + s_1 - t_1 - v$, and the sum of the elements at even indices is $t_1 + s_2 - t_2$. If these two sums are equal, it is a balanced array, and the answer is incremented by one.

- If it is an odd index, after deleting the element, the sum of the elements at even indices in the array is $t_1 + s_2 - t_2 - v$, and the sum of the elements at odd indices is $t_2 + s_1 - t_1$. If these two sums are equal, it is a balanced array, and the answer is incremented by one.

Sau đó, ta cập nhật $t_1$ và $t_2$ rồi tiếp tục duyệt phần tử kế tiếp. Duyệt hết mảng sẽ thu được đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToMakeFair(self, nums: List[int]) -> int:
        s1, s2 = sum(nums[::2]), sum(nums[1::2])
        ans = t1 = t2 = 0
        for i, v in enumerate(nums):
            ans += i % 2 == 0 and t2 + s1 - t1 - v == t1 + s2 - t2
            ans += i % 2 == 1 and t2 + s1 - t1 == t1 + s2 - t2 - v
            t1 += v if i % 2 == 0 else 0
            t2 += v if i % 2 == 1 else 0
        return ans
```

#### Java

```java
class Solution {
    public int waysToMakeFair(int[] nums) {
        int s1 = 0, s2 = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            s1 += i % 2 == 0 ? nums[i] : 0;
            s2 += i % 2 == 1 ? nums[i] : 0;
        }
        int t1 = 0, t2 = 0;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int v = nums[i];
            ans += i % 2 == 0 && t2 + s1 - t1 - v == t1 + s2 - t2 ? 1 : 0;
            ans += i % 2 == 1 && t2 + s1 - t1 == t1 + s2 - t2 - v ? 1 : 0;
            t1 += i % 2 == 0 ? v : 0;
            t2 += i % 2 == 1 ? v : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToMakeFair(vector<int>& nums) {
        int s1 = 0, s2 = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            s1 += i % 2 == 0 ? nums[i] : 0;
            s2 += i % 2 == 1 ? nums[i] : 0;
        }
        int t1 = 0, t2 = 0;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int v = nums[i];
            ans += i % 2 == 0 && t2 + s1 - t1 - v == t1 + s2 - t2;
            ans += i % 2 == 1 && t2 + s1 - t1 == t1 + s2 - t2 - v;
            t1 += i % 2 == 0 ? v : 0;
            t2 += i % 2 == 1 ? v : 0;
        }
        return ans;
    }
};
```

#### Go

```go
func waysToMakeFair(nums []int) (ans int) {
	var s1, s2, t1, t2 int
	for i, v := range nums {
		if i%2 == 0 {
			s1 += v
		} else {
			s2 += v
		}
	}
	for i, v := range nums {
		if i%2 == 0 && t2+s1-t1-v == t1+s2-t2 {
			ans++
		}
		if i%2 == 1 && t2+s1-t1 == t1+s2-t2-v {
			ans++
		}
		if i%2 == 0 {
			t1 += v
		} else {
			t2 += v
		}
	}
	return
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var waysToMakeFair = function (nums) {
    let [s1, s2, t1, t2] = [0, 0, 0, 0];
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        if (i % 2 == 0) {
            s1 += nums[i];
        } else {
            s2 += nums[i];
        }
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const v = nums[i];
        ans += i % 2 == 0 && t2 + s1 - t1 - v == t1 + s2 - t2;
        ans += i % 2 == 1 && t2 + s1 - t1 == t1 + s2 - t2 - v;
        t1 += i % 2 == 0 ? v : 0;
        t2 += i % 2 == 1 ? v : 0;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
