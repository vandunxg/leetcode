---
comments: true
difficulty: Medium
rating: 1846
source: Weekly Contest 403 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3196. Maximize Total Cost of Alternating Subarrays](https://leetcode.com/problems/maximize-total-cost-of-alternating-subarrays)

[中文文档](/solution/3100-3199/3196.Maximize%20Total%20Cost%20of%20Alternating%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p><strong>Chi phí</strong> của một <span data-keyword="subarray-nonempty">mảng con</span> <code>nums[l..r]</code>, với <code>0 &lt;= l &lt;= r &lt; n</code>, được định nghĩa như sau:</p>

<p><code>cost(l, r) = nums[l] - nums[l + 1] + ... + nums[r] * (&minus;1)<sup>r &minus; l</sup></code></p>

<p>Nhiệm vụ của bạn là <strong>chia</strong> <code>nums</code> thành các mảng con sao cho <strong>tổng</strong> <strong>chi phí</strong> của các mảng con là <strong>lớn nhất</strong>, đồng thời đảm bảo mỗi phần tử thuộc về <strong>chính xác một</strong> mảng con.</p>

<p>Cụ thể, nếu <code>nums</code> được chia thành <code>k</code> mảng con, với <code>k &gt; 1</code>, tại các chỉ số <code>i<sub>1</sub>, i<sub>2</sub>, ..., i<sub>k &minus; 1</sub></code>, trong đó <code>0 &lt;= i<sub>1</sub> &lt; i<sub>2</sub> &lt; ... &lt; i<sub>k - 1</sub> &lt; n - 1</code>, thì tổng chi phí sẽ là:</p>

<p><code>cost(0, i<sub>1</sub>) + cost(i<sub>1</sub> + 1, i<sub>2</sub>) + ... + cost(i<sub>k &minus; 1</sub> + 1, n &minus; 1)</code></p>

<p>Trả về một số nguyên biểu thị <em>tổng chi phí lớn nhất</em> của các mảng con sau khi chia mảng một cách tối ưu.</p>

<p><strong>Lưu ý:</strong> Nếu <code>nums</code> không được chia thành các mảng con, tức là <code>k = 1</code>, tổng chi phí đơn giản là <code>cost(0, n - 1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách để tối đa hóa tổng chi phí là chia <code>[1, -2, 3, 4]</code> thành các mảng con <code>[1, -2, 3]</code> và <code>[4]</code>. Tổng chi phí sẽ là <code>(1 + 2 + 3) + 4 = 10</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-1,1,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách để tối đa hóa tổng chi phí là chia <code>[1, -1, 1, -1]</code> thành các mảng con <code>[1, -1]</code> và <code>[1, -1]</code>. Tổng chi phí sẽ là <code>(1 + 1) + (1 + 1) = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0]</span></p>

<p><strong>Đầu ra:</strong> 0</p>

<p><strong>Giải thích:</strong></p>

<p>Ta không thể chia mảng thêm nữa, nên đáp án là 0.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn toàn bộ mảng cho tổng chi phí <code>1 + 1 = 2</code>, đây là giá trị lớn nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đoạn được tính điểm với các dấu xen kẽ và phần tử đầu tiên mang dấu dương. Số cách chia là hàm mũ.
>
> Việc có thể đổi dấu giá trị hiện tại hay không chỉ phụ thuộc vào việc giá trị trước đó đã được đổi dấu hay chưa: một lần đổi dấu sẽ buộc hạng tử tiếp theo mang dấu dương.
>
> Ghi nhớ $dfs(i,j)$ với $j=1$ nghĩa là được phép đổi dấu. Luôn thử $nums[i]+dfs(i+1,1)$, và nếu được phép thì thử thêm $-nums[i]+dfs(i+1,0)$.

<!-- thinking:end -->

Theo mô tả bài toán, nếu số hiện tại chưa bị đổi dấu thì số tiếp theo có thể bị đổi dấu hoặc không; nếu số hiện tại đã bị đổi dấu thì số tiếp theo chỉ có thể giữ nguyên dấu.

Do đó, ta định nghĩa hàm $\textit{dfs}(i, j)$, biểu diễn việc bắt đầu từ số thứ $i$ và cho biết số thứ $i$ có thể bị đổi dấu hay không, trong đó $j$ cho biết số thứ $i$ có bị đổi dấu hay không. Nếu $j = 0$, nghĩa là số thứ $i$ không thể bị đổi dấu; ngược lại, nó có thể bị đổi dấu. Đáp án là $\textit{dfs}(0, 0)$.

Quá trình thực thi hàm $dfs(i, j)$ như sau:

- Nếu $i \geq \textit{len}(nums)$, nghĩa là đã duyệt hết mảng, trả về $0$;
- Nếu không, số thứ $i$ có thể giữ nguyên dấu, khi đó đáp án là $nums[i] + \textit{dfs}(i + 1, 1)$; nếu $j = 1$, nghĩa là số thứ $i$ có thể bị đổi dấu, khi đó đáp án là $\max(\textit{dfs}(i + 1, 0) - nums[i])$. Ta lấy giá trị lớn hơn trong hai trường hợp.

Để tránh tính toán lặp lại, ta có thể dùng memoization để lưu các kết quả đã được tính.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTotalCost(self, nums: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= len(nums):
                return 0
            ans = nums[i] + dfs(i + 1, 1)
            if j == 1:
                ans = max(ans, -nums[i] + dfs(i + 1, 0))
            return ans

        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Long[][] f;
    private int[] nums;
    private int n;

    public long maximumTotalCost(int[] nums) {
        n = nums.length;
        this.nums = nums;
        f = new Long[n][2];
        return dfs(0, 0);
    }

    private long dfs(int i, int j) {
        if (i >= n) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        f[i][j] = nums[i] + dfs(i + 1, 1);
        if (j == 1) {
            f[i][j] = Math.max(f[i][j], -nums[i] + dfs(i + 1, 0));
        }
        return f[i][j];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumTotalCost(vector<int>& nums) {
        int n = nums.size();
        long long f[n][2];
        fill(f[0], f[n], LLONG_MIN);
        auto dfs = [&](this auto&& dfs, int i, int j) -> long long {
            if (i >= n) {
                return 0;
            }
            if (f[i][j] != LLONG_MIN) {
                return f[i][j];
            }
            f[i][j] = nums[i] + dfs(i + 1, 1);
            if (j) {
                f[i][j] = max(f[i][j], -nums[i] + dfs(i + 1, 0));
            }
            return f[i][j];
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func maximumTotalCost(nums []int) int64 {
    n := len(nums)
    f := make([][2]int64, n)
    for i := range f {
        f[i] = [2]int64{-1e18, -1e18}
    }
    var dfs func(int, int) int64
    dfs = func(i, j int) int64 {
        if i >= n {
            return 0
        }
        if f[i][j] != -1e18 {
            return f[i][j]
        }
        f[i][j] = int64(nums[i]) + dfs(i+1, 1)
        if j > 0 {
            f[i][j] = max(f[i][j], int64(-nums[i])+dfs(i+1, 0))
        }
        return f[i][j]
    }
    return dfs(0, 0)
}
```

#### TypeScript

```ts
function maximumTotalCost(nums: number[]): number {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array(2).fill(-Infinity));
    const dfs = (i: number, j: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i][j] !== -Infinity) {
            return f[i][j];
        }
        f[i][j] = nums[i] + dfs(i + 1, 1);
        if (j) {
            f[i][j] = Math.max(f[i][j], -nums[i] + dfs(i + 1, 0));
        }
        return f[i][j];
    };
    return dfs(0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 đã có số trạng thái tuyến tính nhưng vẫn dùng đệ quy. Hai lựa chọn có thể được rút gọn thành hai biến vô hướng.
>
> $f$ là điểm tốt nhất nếu giá trị hiện tại giữ dấu dương, còn $g$ là điểm tốt nhất nếu nó bị đổi dấu. Việc đổi dấu yêu cầu giá trị trước đó phải giữ dấu dương.
>
> Đặt $f=\max(f,g)+x$ và $g=f_{old}-x$. Đáp án là $\max(f,g)$ với không gian phụ hằng số.

<!-- thinking:end -->

Ta có thể chuyển phép tìm kiếm có memoization ở Lời giải 1 thành quy hoạch động.

Định nghĩa $f$ và $g$ là hai trạng thái, trong đó $f$ biểu diễn giá trị lớn nhất khi số hiện tại không bị đổi dấu, còn $g$ biểu diễn giá trị lớn nhất khi số hiện tại bị đổi dấu.

Duyệt qua mảng $nums$, với số thứ $i$, ta có thể cập nhật giá trị của $f$ và $g$ dựa trên các trạng thái của chúng:

- Nếu số hiện tại không bị đổi dấu, giá trị của $f$ là $\max(f, g) + x$, nghĩa là nếu số hiện tại không bị đổi dấu thì số trước đó có thể đã bị đổi dấu hoặc không;
- Nếu số hiện tại bị đổi dấu, giá trị của $g$ là $f - x$, nghĩa là nếu số hiện tại bị đổi dấu thì số trước đó không thể bị đổi dấu.

Đáp án cuối cùng là $\max(f, g)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTotalCost(self, nums: List[int]) -> int:
        f, g = -inf, 0
        for x in nums:
            f, g = max(f, g) + x, f - x
        return max(f, g)
```

#### Java

```java
class Solution {
    public long maximumTotalCost(int[] nums) {
        long f = Long.MIN_VALUE / 2, g = 0;
        for (int x : nums) {
            long ff = Math.max(f, g) + x;
            long gg = f - x;
            f = ff;
            g = gg;
        }
        return Math.max(f, g);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumTotalCost(vector<int>& nums) {
        long long f = LLONG_MIN / 2, g = 0;
        for (int x : nums) {
            long long ff = max(f, g) + x, gg = f - x;
            f = ff;
            g = gg;
        }
        return max(f, g);
    }
};
```

#### Go

```go
func maximumTotalCost(nums []int) int64 {
    f, g := math.MinInt64/2, 0
    for _, x := range nums {
        f, g = max(f, g)+x, f-x
    }
    return int64(max(f, g))
}
```

#### TypeScript

```ts
function maximumTotalCost(nums: number[]): number {
    let [f, g] = [-Infinity, 0];
    for (const x of nums) {
        [f, g] = [Math.max(f, g) + x, f - x];
    }
    return Math.max(f, g);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
