---
comments: true
difficulty: Hard
rating: 2481
source: Biweekly Contest 18 Q4
tags:
    - Greedy
    - Array
    - Math
---

<!-- problem:start -->

# [1330. Reverse Subarray To Maximize Array Value](https://leetcode.com/problems/reverse-subarray-to-maximize-array-value)

[中文文档](/solution/1300-1399/1330.Reverse%20Subarray%20To%20Maximize%20Array%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. <em>Giá trị</em> của mảng được định nghĩa là tổng <code>|nums[i] - nums[i + 1]|</code> với mọi <code>0 &lt;= i &lt; nums.length - 1</code>.</p>

<p>Bạn được chọn một mảng con bất kỳ và đảo ngược nó. Bạn chỉ được thực hiện thao tác này <strong>một lần</strong>.</p>

<p>Hãy tìm giá trị lớn nhất có thể của mảng sau cùng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,1,5,4]
<strong>Đầu ra:</strong> 10
<b>Giải thích: </b>Đảo ngược mảng con [3,1,5] sẽ thu được mảng [2,5,1,3,4], có giá trị bằng 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,9,24,2,1,10]
<strong>Đầu ra:</strong> 68
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Đáp án được đảm bảo nằm trong phạm vi số nguyên 32-bit.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân loại trường hợp + liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị mảng là tổng các độ chênh lệch tuyệt đối giữa các phần tử kề nhau; ta được đảo ngược một mảng con. Vì $n \le 3 \times 10^4$, không thể liệt kê cả hai đầu mút. Phép đảo chỉ làm thay đổi các cạnh tại vị trí cắt.
>
> Gọi $s$ là tổng khi chưa đảo. Với các phép đảo chứa một trong hai đầu mảng, ta có thể liệt kê đầu còn lại trong $O(n)$. Với phép đảo nằm bên trong, xét các cặp $(x,y)$; mức tăng là khoảng cách lớn nhất trong bốn biểu thức tuyến tính, có thể tìm bằng cách theo dõi giá trị lớn nhất của $a-b$ và nhỏ nhất của $a+b$ trong một lượt duyệt.

<!-- thinking:end -->

Theo đề bài, ta cần tìm giá trị lớn nhất của mảng $\sum_{i=0}^{n-2} |a_i - a_{i+1}|$ sau khi đảo ngược một mảng con đúng một lần.

Ta xét các trường hợp sau:

1. Không đảo mảng con.
2. Đảo mảng con có chứa phần tử đầu tiên.
3. Đảo mảng con có chứa phần tử cuối cùng.
4. Đảo mảng con không chứa phần tử đầu tiên lẫn phần tử cuối cùng.

Gọi $s$ là giá trị mảng khi chưa đảo mảng con, tức $s = \sum_{i=0}^{n-2} |a_i - a_{i+1}|$. Ta có thể khởi tạo đáp án $ans = s$.

Nếu đảo mảng con có chứa phần tử đầu tiên, ta có thể liệt kê phần tử cuối $a_i$ của mảng con đã đảo, với $0 \leq i < n-1$. Khi đó, $ans = \max(ans, s + |a_0 - a_{i+1}| - |a_i - a_{i+1}|)$.

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1330.Reverse%20Subarray%20To%20Maximize%20Array%20Value/images/1-drawio.png" /></p>

Tương tự, nếu đảo mảng con có chứa phần tử cuối cùng, ta liệt kê phần tử đầu $a_{i+1}$ của mảng con đã đảo, với $0 \leq i < n-1$. Khi đó, $ans = \max(ans, s + |a_{n-1} - a_i| - |a_i - a_{i+1}|)$.

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1330.Reverse%20Subarray%20To%20Maximize%20Array%20Value/images/2-drawio.png" /></p>

Nếu đảo mảng con không chứa phần tử đầu tiên và cuối cùng, ta xem hai phần tử kề nhau bất kỳ trong mảng là một cặp điểm $(x, y)$. Gọi phần tử đầu của mảng con được đảo là $y_1$, phần tử liền trước nó là $x_1$; gọi phần tử cuối của mảng con là $x_2$, phần tử liền sau nó là $y_2$.

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1330.Reverse%20Subarray%20To%20Maximize%20Array%20Value/images/3-drawio.png" /></p>

Khi đó, so với trường hợp không đảo mảng con, giá trị mảng thay đổi một lượng $|x_1 - x_2| + |y_1 - y_2| - |x_1 - y_1| - |x_2 - y_2|$. Hai số hạng đầu có thể được biểu diễn như sau:

$$
\left | x_1 - x_2 \right |  + \left | y_1 - y_2 \right | = \max \begin{cases} (x_1 + y_1) - (x_2 + y_2) \\ (x_1 - y_1) - (x_2 - y_2) \\ (-x_1 + y_1) - (-x_2 + y_2) \\ (-x_1 - y_1) - (-x_2 - y_2) \end{cases}
$$

Do đó, mức thay đổi của giá trị mảng là:

$$
\left | x_1 - x_2 \right |  + \left | y_1 - y_2 \right | - \left | x_1 - y_1 \right | - \left | x_2 - y_2 \right |  = \max \begin{cases} (x_1 + y_1) - \left |x_1 - y_1 \right | - \left ( (x_2 + y_2) + \left |x_2 - y_2 \right | \right ) \\ (x_1 - y_1) - \left |x_1 - y_1 \right | - \left ( (x_2 - y_2) + \left |x_2 - y_2 \right | \right ) \\ (-x_1 + y_1) - \left |x_1 - y_1 \right | - \left ( (-x_2 + y_2) + \left |x_2 - y_2 \right | \right ) \\ (-x_1 - y_1) - \left |x_1 - y_1 \right | - \left ( (-x_2 - y_2) + \left |x_2 - y_2 \right | \right ) \end{cases}
$$

Vì vậy, ta chỉ cần tìm giá trị lớn nhất $mx$ của $k_1 \times x + k_2 \times y$, với $k_1, k_2 \in \{-1, 1\}$, cùng giá trị nhỏ nhất tương ứng $mi$ của $|x - y|$. Mức tăng lớn nhất của giá trị mảng là $mx - mi$. Đáp án là $ans = \max(ans, s + \max(0, mx - mi))$.

Trong phần cài đặt, ta định nghĩa mảng độ dài 5 là $dirs=[1, -1, -1, 1, 1]$. Mỗi lần, ta lấy hai phần tử kề nhau làm giá trị của $k_1$ và $k_2$; cách này bao quát mọi trường hợp $k_1, k_2 \in \{-1, 1\}$.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

Bài toán tương tự:

- [1131. Maximum of Absolute Value Expression](https://github.com/doocs/leetcode/blob/main/solution/1100-1199/1131.Maximum%20of%20Absolute%20Value%20Expression/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValueAfterReverse(self, nums: List[int]) -> int:
        ans = s = sum(abs(x - y) for x, y in pairwise(nums))
        for x, y in pairwise(nums):
            ans = max(ans, s + abs(nums[0] - y) - abs(x - y))
            ans = max(ans, s + abs(nums[-1] - x) - abs(x - y))
        for k1, k2 in pairwise((1, -1, -1, 1, 1)):
            mx, mi = -inf, inf
            for x, y in pairwise(nums):
                a = k1 * x + k2 * y
                b = abs(x - y)
                mx = max(mx, a - b)
                mi = min(mi, a + b)
            ans = max(ans, s + max(mx - mi, 0))
        return ans
```

#### Java

```java
class Solution {
    public int maxValueAfterReverse(int[] nums) {
        int n = nums.length;
        int s = 0;
        for (int i = 0; i < n - 1; ++i) {
            s += Math.abs(nums[i] - nums[i + 1]);
        }
        int ans = s;
        for (int i = 0; i < n - 1; ++i) {
            ans = Math.max(
                ans, s + Math.abs(nums[0] - nums[i + 1]) - Math.abs(nums[i] - nums[i + 1]));
            ans = Math.max(
                ans, s + Math.abs(nums[n - 1] - nums[i]) - Math.abs(nums[i] - nums[i + 1]));
        }
        int[] dirs = {1, -1, -1, 1, 1};
        final int inf = 1 << 30;
        for (int k = 0; k < 4; ++k) {
            int k1 = dirs[k], k2 = dirs[k + 1];
            int mx = -inf, mi = inf;
            for (int i = 0; i < n - 1; ++i) {
                int a = k1 * nums[i] + k2 * nums[i + 1];
                int b = Math.abs(nums[i] - nums[i + 1]);
                mx = Math.max(mx, a - b);
                mi = Math.min(mi, a + b);
            }
            ans = Math.max(ans, s + Math.max(0, mx - mi));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxValueAfterReverse(vector<int>& nums) {
        int n = nums.size();
        int s = 0;
        for (int i = 0; i < n - 1; ++i) {
            s += abs(nums[i] - nums[i + 1]);
        }
        int ans = s;
        for (int i = 0; i < n - 1; ++i) {
            ans = max(ans, s + abs(nums[0] - nums[i + 1]) - abs(nums[i] - nums[i + 1]));
            ans = max(ans, s + abs(nums[n - 1] - nums[i]) - abs(nums[i] - nums[i + 1]));
        }
        int dirs[5] = {1, -1, -1, 1, 1};
        const int inf = 1 << 30;
        for (int k = 0; k < 4; ++k) {
            int k1 = dirs[k], k2 = dirs[k + 1];
            int mx = -inf, mi = inf;
            for (int i = 0; i < n - 1; ++i) {
                int a = k1 * nums[i] + k2 * nums[i + 1];
                int b = abs(nums[i] - nums[i + 1]);
                mx = max(mx, a - b);
                mi = min(mi, a + b);
            }
            ans = max(ans, s + max(0, mx - mi));
        }
        return ans;
    }
};
```

#### Go

```go
func maxValueAfterReverse(nums []int) int {
	s, n := 0, len(nums)
	for i, x := range nums[:n-1] {
		y := nums[i+1]
		s += abs(x - y)
	}
	ans := s
	for i, x := range nums[:n-1] {
		y := nums[i+1]
		ans = max(ans, s+abs(nums[0]-y)-abs(x-y))
		ans = max(ans, s+abs(nums[n-1]-x)-abs(x-y))
	}
	dirs := [5]int{1, -1, -1, 1, 1}
	const inf = 1 << 30
	for k := 0; k < 4; k++ {
		k1, k2 := dirs[k], dirs[k+1]
		mx, mi := -inf, inf
		for i, x := range nums[:n-1] {
			y := nums[i+1]
			a := k1*x + k2*y
			b := abs(x - y)
			mx = max(mx, a-b)
			mi = min(mi, a+b)
		}
		ans = max(ans, s+max(mx-mi, 0))
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function maxValueAfterReverse(nums: number[]): number {
    const n = nums.length;
    let s = 0;
    for (let i = 0; i < n - 1; ++i) {
        s += Math.abs(nums[i] - nums[i + 1]);
    }
    let ans = s;
    for (let i = 0; i < n - 1; ++i) {
        const d = Math.abs(nums[i] - nums[i + 1]);
        ans = Math.max(ans, s + Math.abs(nums[0] - nums[i + 1]) - d);
        ans = Math.max(ans, s + Math.abs(nums[n - 1] - nums[i]) - d);
    }
    const dirs = [1, -1, -1, 1, 1];
    const inf = 1 << 30;
    for (let k = 0; k < 4; ++k) {
        let mx = -inf;
        let mi = inf;
        for (let i = 0; i < n - 1; ++i) {
            const a = dirs[k] * nums[i] + dirs[k + 1] * nums[i + 1];
            const b = Math.abs(nums[i] - nums[i + 1]);
            mx = Math.max(mx, a - b);
            mi = Math.min(mi, a + b);
        }
        ans = Math.max(ans, s + Math.max(0, mx - mi));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
