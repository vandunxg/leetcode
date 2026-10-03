---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Dynamic Programming
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2263. Make Array Non-decreasing or Non-increasing 🔒](https://leetcode.com/problems/make-array-non-decreasing-or-non-increasing)

[中文文档](/solution/2200-2299/2263.Make%20Array%20Non-decreasing%20or%20Non-increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Trong một thao tác, bạn có thể:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> trong phạm vi <code>0 &lt;= i &lt; nums.length</code></li>
	<li>Đặt <code>nums[i]</code> thành <code>nums[i] + 1</code> <strong>hoặc</strong> <code>nums[i] - 1</code></li>
</ul>

<p>Trả về <em><strong>số thao tác ít nhất</strong> để biến </em><code>nums</code><em> thành một mảng <strong>không giảm</strong> hoặc <strong>không tăng</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,4,5,0]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Một cách để biến nums thành mảng không tăng là:
- Cộng 1 vào nums[1] một lần để nó trở thành 3.
- Trừ 1 khỏi nums[2] một lần để nó trở thành 3.
- Trừ 1 khỏi nums[3] hai lần để nó trở thành 3.
Sau 4 thao tác, nums trở thành [3,3,3,3,0], là một mảng không tăng.
Lưu ý rằng cũng có thể biến nums thành [4,4,4,4,0] với 4 thao tác.
Có thể chứng minh rằng 4 là số thao tác ít nhất cần thực hiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,3,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> nums đã là mảng không giảm, nên không cần thực hiện thao tác nào và ta trả về 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> nums đã là mảng không giảm, nên không cần thực hiện thao tác nào và ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán với độ phức tạp thời gian <code>O(n*log(n))</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể tăng hoặc giảm một phần tử đi một đơn vị và muốn biến mảng thành không giảm hoặc không tăng với tổng chi phí nhỏ nhất. Vì cả $n$ và miền giá trị đều không vượt quá $10^3$, ta có thể dùng quy hoạch động theo giá trị cuối cùng của vị trí $i$. Mảng không tăng chính là mảng không giảm khi đảo ngược, nên ta xử lý mảng đảo.
>
> $f[i][j]$ là chi phí của $i$ phần tử đầu tiên khi phần tử thứ $i$ có giá trị bằng $j$. Khi đó $f[i][j] = \min_{k\le j} f[i-1][k] + |j-nums[i-1]|$, và ta duy trì giá trị nhỏ nhất trên đoạn đầu khi $j$ tăng dần. Chọn kết quả tốt hơn giữa mảng ban đầu và mảng đảo.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def convertArray(self, nums: List[int]) -> int:
        def solve(nums):
            n = len(nums)
            f = [[0] * 1001 for _ in range(n + 1)]
            for i, x in enumerate(nums, 1):
                mi = inf
                for j in range(1001):
                    if mi > f[i - 1][j]:
                        mi = f[i - 1][j]
                    f[i][j] = mi + abs(x - j)
            return min(f[n])

        return min(solve(nums), solve(nums[::-1]))
```

#### Java

```java
class Solution {
    public int convertArray(int[] nums) {
        return Math.min(solve(nums), solve(reverse(nums)));
    }

    private int solve(int[] nums) {
        int n = nums.length;
        int[][] f = new int[n + 1][1001];
        for (int i = 1; i <= n; ++i) {
            int mi = 1 << 30;
            for (int j = 0; j <= 1000; ++j) {
                mi = Math.min(mi, f[i - 1][j]);
                f[i][j] = mi + Math.abs(j - nums[i - 1]);
            }
        }
        int ans = 1 << 30;
        for (int x : f[n]) {
            ans = Math.min(ans, x);
        }
        return ans;
    }

    private int[] reverse(int[] nums) {
        for (int i = 0, j = nums.length - 1; i < j; ++i, --j) {
            int t = nums[i];
            nums[i] = nums[j];
            nums[j] = t;
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int convertArray(vector<int>& nums) {
        int a = solve(nums);
        reverse(nums.begin(), nums.end());
        int b = solve(nums);
        return min(a, b);
    }

    int solve(vector<int>& nums) {
        int n = nums.size();
        int f[n + 1][1001];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= n; ++i) {
            int mi = 1 << 30;
            for (int j = 0; j <= 1000; ++j) {
                mi = min(mi, f[i - 1][j]);
                f[i][j] = mi + abs(nums[i - 1] - j);
            }
        }
        return *min_element(f[n], f[n] + 1001);
    }
};
```

#### Go

```go
func convertArray(nums []int) int {
	return min(solve(nums), solve(reverse(nums)))
}

func solve(nums []int) int {
	n := len(nums)
	f := make([][1001]int, n+1)
	for i := 1; i <= n; i++ {
		mi := 1 << 30
		for j := 0; j <= 1000; j++ {
			mi = min(mi, f[i-1][j])
			f[i][j] = mi + abs(nums[i-1]-j)
		}
	}
	ans := 1 << 30
	for _, x := range f[n] {
		ans = min(ans, x)
	}
	return ans
}

func reverse(nums []int) []int {
	for i, j := 0, len(nums)-1; i < j; i, j = i+1, j-1 {
		nums[i], nums[j] = nums[j], nums[i]
	}
	return nums
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
