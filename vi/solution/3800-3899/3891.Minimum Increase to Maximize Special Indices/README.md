---
comments: true
difficulty: Medium
rating: 1952
source: Weekly Contest 496 Q3
tags:
    - Greedy
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3891. Minimum Increase to Maximize Special Indices](https://leetcode.com/problems/minimum-increase-to-maximize-special-indices)

[中文文档](/solution/3800-3899/3891.Minimum%20Increase%20to%20Maximize%20Special%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Một chỉ số <code>i</code> (<code>0 &lt; i &lt; n - 1</code>) là <strong>đặc biệt</strong> nếu <code>nums[i] &gt; nums[i - 1]</code> và <code>nums[i] &gt; nums[i + 1]</code>.</p>

<p>Bạn có thể thực hiện các phép toán bằng cách chọn <strong>bất kỳ</strong> chỉ số <code>i</code> nào và <strong>tăng</strong> <code>nums[i]</code> thêm 1.</p>

<p>Mục tiêu của bạn là:</p>

<ul>
	<li><strong>Tối đa hóa</strong> số lượng chỉ số <strong>đặc biệt</strong>.</li>
	<li><strong>Tối thiểu hóa</strong> tổng số <strong>phép toán</strong> cần thực hiện để đạt được số lượng <strong>tối đa</strong> đó.</li>
</ul>

<p>Hãy trả về một số nguyên biểu thị tổng số phép toán <strong>nhỏ nhất</strong> cần thực hiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Ban đầu, <code>nums = [1, 2, 2]</code>.</li>
	<li>Tăng <code>nums[1]</code> thêm 1, mảng trở thành <code>[1, 3, 2]</code>.</li>
	<li>Mảng cuối cùng <code>[1, 3, 2]</code> có 1 chỉ số đặc biệt, đây là số lượng lớn nhất có thể đạt được.</li>
	<li>Không thể đạt được số lượng chỉ số đặc biệt này với ít phép toán hơn. Vì vậy, đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Ban đầu, <code>nums = [2, 1, 1, 3]</code>.</li>
	<li>Thực hiện 2 phép toán tại chỉ số 1, mảng trở thành <code>[2, 3, 1, 3]</code>.</li>
	<li>Mảng cuối cùng <code>[2, 3, 1, 3]</code> có 1 chỉ số đặc biệt, đây là số lượng lớn nhất có thể đạt được. Vì vậy, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,2,1,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong>​​​​​​​​​​​​​​​​​​​​​</p>

<ul>
	<li>Ban đầu, <code>nums = [5, 2, 1, 4, 3]</code>.</li>
	<li>Thực hiện 4 phép toán tại chỉ số 1, mảng trở thành <code>[5, 6, 1, 4, 3]</code>.</li>
	<li>Mảng cuối cùng <code>[5, 6, 1, 4, 3]</code> có 2 chỉ số đặc biệt, đây là số lượng lớn nhất có thể đạt được. Vì vậy, đáp án là 4.​​​​​​​</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số đặc biệt là một đỉnh nghiêm ngặt. Ta chỉ có thể tăng thêm $1$, trước tiên tối đa hóa số lượng đỉnh rồi mới tối thiểu hóa tổng số lần tăng. $n \le 10^5$.
>
> Các đỉnh không thể kề nhau. Mảng có độ dài lẻ có thể chọn mọi chỉ số lẻ; mảng có độ dài chẵn phải bỏ qua một chỉ số trong $[1,n-2]$.
>
> Chi phí để tăng $i$ vượt qua cả hai phần tử lân cận là $\max(0,\max(nums[i-1],nums[i+1])+1-nums[i])$. Hàm memoized $\mathrm{dfs}(i,j)$ bắt đầu từ $i$, với $j$ lần bỏ qua còn lại.
>
> Sau khi chọn một chỉ số, ta chuyển đến $i+2$; nếu còn một lần bỏ qua, ta có thể chuyển đến $i+1$ và sử dụng lần bỏ qua đó. Việc tìm kiếm bắt đầu tại $1$ với $j$ bằng giá trị đối nghịch với $n \bmod 2$.

<!-- thinking:end -->

Ta nhận thấy rằng nếu độ dài mảng là lẻ, việc tăng tất cả các phần tử ở chỉ số lẻ sao cho mỗi phần tử lớn hơn $1$ đơn vị so với cả hai phần tử kề bên sẽ tạo ra số lượng chỉ số đặc biệt lớn nhất có thể. Nếu độ dài mảng là chẵn, trong các chỉ số thuộc đoạn $[1, n - 2]$, ta bỏ qua chính xác một chỉ số, còn với các chỉ số còn lại, ta tăng cách một phần tử sao cho mỗi phần tử lớn hơn $1$ đơn vị so với cả hai phần tử kề bên; cách này cũng tạo ra số lượng chỉ số đặc biệt lớn nhất có thể.

Do đó, ta thiết kế hàm $\text{dfs}(i, j)$, biểu thị số phép toán nhỏ nhất cần thực hiện để đạt được số lượng chỉ số đặc biệt lớn nhất có thể khi bắt đầu từ chỉ số $i$, với $j$ lần bỏ qua còn lại. Với mỗi chỉ số $i$, ta có thể tăng nó để lớn hơn $1$ đơn vị so với cả hai phần tử kề bên, hoặc bỏ qua nó. Ta sử dụng tìm kiếm có ghi nhớ để tránh tính toán lặp lại.

Cách triển khai $\text{dfs}(i, j)$ như sau:

- Nếu $i \geq n - 1$, trả về $0$.
- Tính số phép toán cần thực hiện để tăng $nums[i]$ sao cho nó lớn hơn $1$ đơn vị so với cả hai phần tử kề bên, gọi là $cost$.
- Tính tổng chi phí khi chọn tăng $nums[i]$: $cost + \text{dfs}(i + 2, j)$.
- Nếu $j > 0$, tính tổng chi phí khi chọn bỏ qua $nums[i]$: $\text{dfs}(i + 1, 0)$, rồi cập nhật $ans$ thành giá trị nhỏ hơn trong hai lựa chọn.

Cuối cùng, trả về $\text{dfs}(1, (n \bmod 2) \oplus 1)$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minIncrease(self, nums: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= len(nums) - 1:
                return 0
            cost = max(0, max(nums[i - 1], nums[i + 1]) + 1 - nums[i])
            ans = cost + dfs(i + 2, j)
            if j:
                ans = min(ans, dfs(i + 1, 0))
            return ans

        return dfs(1, len(nums) & 1 ^ 1)
```

#### Java

```java
class Solution {
    private Long[][] f;
    private int[] nums;
    private int n;

    public long minIncrease(int[] nums) {
        n = nums.length;
        this.nums = nums;
        f = new Long[n][2];
        return dfs(1, n & 1 ^ 1);
    }

    private long dfs(int i, int j) {
        if (i >= n - 1) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int cost = Math.max(0, Math.max(nums[i - 1], nums[i + 1]) + 1 - nums[i]);
        long ans = cost + dfs(i + 2, j);
        if (j > 0) {
            ans = Math.min(ans, dfs(i + 1, 0));
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
private:
    vector<vector<long long>> f;
    vector<int> nums;
    int n;

public:
    long long minIncrease(vector<int>& nums) {
        this->nums = nums;
        n = nums.size();
        f.assign(n, vector<long long>(2, -1));
        return dfs(1, (n & 1) ^ 1);
    }

    long long dfs(int i, int j) {
        if (i >= n - 1) {
            return 0;
        }
        if (f[i][j] != -1) {
            return f[i][j];
        }
        int cost = max(0, max(nums[i - 1], nums[i + 1]) + 1 - nums[i]);
        long long ans = cost + dfs(i + 2, j);
        if (j > 0) {
            ans = min(ans, dfs(i + 1, 0));
        }
        return f[i][j] = ans;
    }
};
```

#### Go

```go
func minIncrease(nums []int) int64 {
	n := len(nums)

	f := make([][]int64, n)
	for i := range f {
		f[i] = []int64{-1, -1}
	}

	var dfs func(i, j int) int64
	dfs = func(i, j int) int64 {
		if i >= n-1 {
			return 0
		}
		if f[i][j] != -1 {
			return f[i][j]
		}

		cost := max(0, max(nums[i-1], nums[i+1])+1-nums[i])
		ans := int64(cost) + dfs(i+2, j)

		if j > 0 {
			if t := dfs(i+1, 0); t < ans {
				ans = t
			}
		}

		f[i][j] = ans
		return ans
	}

	return dfs(1, (n&1)^1)
}
```

#### TypeScript

```ts
function minIncrease(nums: number[]): number {
    const n = nums.length;

    const f: number[][] = Array.from({ length: n }, () => Array(2).fill(-1));

    const dfs = (i: number, j: number): number => {
        if (i >= n - 1) {
            return 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }

        const cost = Math.max(0, Math.max(nums[i - 1], nums[i + 1]) + 1 - nums[i]);
        let ans = cost + dfs(i + 2, j);

        if (j > 0) {
            ans = Math.min(ans, dfs(i + 1, 0));
        }

        f[i][j] = ans;
        return ans;
    };

    return dfs(1, (n & 1) ^ 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
