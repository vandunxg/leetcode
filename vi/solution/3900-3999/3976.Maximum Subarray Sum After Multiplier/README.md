---
comments: true
difficulty: Medium
rating: 1981
source: Weekly Contest 508 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3976. Maximum Subarray Sum After Multiplier](https://leetcode.com/problems/maximum-subarray-sum-after-multiplier)

[中文文档](/solution/3900-3999/3976.Maximum%20Subarray%20Sum%20After%20Multiplier/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên dương <code>k</code>.</p>

<p>Bạn phải chọn <strong>chính xác</strong> một <span data-keyword="subarray-nonempty">mảng con</span> của <code>nums</code> và thực hiện <strong>chính xác</strong> một trong các thao tác sau:</p>

<ol>
	<li>Nhân mỗi số trong mảng con được chọn với <code>k</code>.</li>
	<li>Chia mỗi số trong mảng con được chọn cho <code>k</code>.
	<ul>
		<li>Khi chia một số dương cho <code>k</code>, dùng giá trị <strong>làm tròn xuống</strong> của kết quả phép chia.</li>
		<li>Khi chia một số âm cho <code>k</code>, dùng giá trị <strong>làm tròn lên</strong> của kết quả phép chia.</li>
	</ul>
	</li>
</ol>

<p>Trả về tổng <strong>lớn nhất</strong> có thể của một mảng con <strong>không rỗng</strong> trong mảng kết quả.</p>

<p>Lưu ý rằng mảng con được chọn để thực hiện thao tác và mảng con được chọn để tính tổng có thể <strong>khác nhau</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-2,3,4,-5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Nhân mỗi số trong mảng con <code>[3, 4]</code> với 2.</li>
	<li>Kết quả là <code>nums = [1, -2, 6, 8, -5]</code>.</li>
	<li>Mảng con có tổng lớn nhất là <code>[6, 8]</code>, nên đầu ra là <code>6 + 8 = 14</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-5,-4,-3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia mỗi số trong mảng con <code>[-3]</code> cho 2.</li>
	<li>Kết quả là <code>nums = [-5, -4, -1]</code>.</li>
	<li>Mảng con có tổng lớn nhất là <code>[-1]</code>, nên đầu ra là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể nhân một đoạn liên tiếp với $k$, chia một đoạn liên tiếp cho $k$, hoặc không làm gì. Với $n\le 10^5$, cần quyết định tuyến tính giữa bốn trạng thái: “chưa bắt đầu / đang nhân / đang chia / đã kết thúc”.
>
> $f[i][j]$ là tổng tốt nhất của mảng con kết thúc tại $i$ ở trạng thái $j$. Các chuyển trạng thái bắt đầu, tiếp tục hoặc kết thúc đoạn được biến đổi, và một mảng con mới có thể bắt đầu từ $0$. Đáp án là giá trị lớn nhất trên mọi trạng thái.
>
> Sau khi dùng mảng cuộn, không gian giảm xuống hằng số và thời gian là $O(n)$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là tổng lớn nhất của mảng con kết thúc tại $nums[i]$ với trạng thái hiện tại là $j$. Có $4$ trạng thái của $j$:

- Trạng thái $0$: mảng con hiện tại chưa thực hiện thao tác nào;
- Trạng thái $1$: mảng con hiện tại đang được nhân với $k$;
- Trạng thái $2$: mảng con hiện tại đang được chia cho $k$;
- Trạng thái $3$: thao tác trên mảng con hiện tại đã hoàn tất.

Ban đầu, $f[0][0] = 0$ và với mọi trạng thái khác, $f[i][j] = -\infty$.

Tiếp theo, ta xét các chuyển trạng thái. Với số thứ $i$ là $nums[i]$, ta có thể không thực hiện thao tác, nhân với $k$, chia cho $k$, hoặc tiếp tục sau khi thao tác đã hoàn tất:

- Nếu không thực hiện thao tác, thì $f[i][0] = \max(f[i-1][0], 0) + nums[i]$;
- Nếu nhân với $k$, thì $f[i][1] = \max(f[i-1][0], f[i-1][1], 0) + nums[i] \times k$;
- Nếu chia cho $k$, thì $f[i][2] = \max(f[i-1][0], f[i-1][2], 0) + \lfloor \frac{nums[i]}{k} \rfloor$;
- Nếu thao tác đã hoàn tất, thì $f[i][3] = \max(f[i-1][1], f[i-1][2], f[i-1][3]) + nums[i]$.

Ta lấy giá trị lớn nhất trong mọi trạng thái làm đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubarraySum(self, nums: List[int], k: int) -> int:
        n = len(nums)
        f = [[-inf] * 4 for _ in range(n + 1)]
        f[0][0] = 0
        ans = -inf
        for i, x in enumerate(nums, 1):
            f[i][0] = max(f[i - 1][0], 0) + x
            f[i][1] = max(f[i - 1][0], f[i - 1][1], 0) + x * k
            f[i][2] = max(f[i - 1][0], f[i - 1][2], 0) + int(x / k)
            f[i][3] = max(f[i - 1][1], f[i - 1][2], f[i - 1][3]) + x
            ans = max(ans, max(f[i]))
        return ans
```

#### Java

```java
class Solution {
    public long maxSubarraySum(int[] nums, int k) {
        int n = nums.length;
        long inf = Long.MIN_VALUE / 4;

        long[][] f = new long[n + 1][4];

        for (int i = 0; i <= n; i++) {
            Arrays.fill(f[i], inf);
        }

        f[0][0] = 0;
        long ans = inf;

        for (int i = 1; i <= n; i++) {
            long x = nums[i - 1];

            f[i][0] = Math.max(f[i - 1][0], 0) + x;
            f[i][1] = Math.max(Math.max(f[i - 1][0], f[i - 1][1]), 0) + x * k;
            f[i][2] = Math.max(Math.max(f[i - 1][0], f[i - 1][2]), 0) + (x / k);
            f[i][3] = Math.max(Math.max(f[i - 1][1], f[i - 1][2]), f[i - 1][3]) + x;

            ans = Math.max(ans, Math.max(Math.max(f[i][0], f[i][1]), Math.max(f[i][2], f[i][3])));
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxSubarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        long long inf = numeric_limits<long long>::min() / 4;

        vector<array<long long, 4>> f(n + 1);

        for (int i = 0; i <= n; i++) {
            f[i].fill(inf);
        }

        f[0][0] = 0;
        long long ans = inf;

        for (int i = 1; i <= n; i++) {
            long long x = nums[i - 1];

            f[i][0] = max(f[i - 1][0], 0LL) + x;
            f[i][1] = max({f[i - 1][0], f[i - 1][1], 0LL}) + x * k;
            f[i][2] = max({f[i - 1][0], f[i - 1][2], 0LL}) + (x / k);
            f[i][3] = max({f[i - 1][1], f[i - 1][2], f[i - 1][3]}) + x;

            ans = max(ans, *max_element(f[i].begin(), f[i].end()));
        }

        return ans;
    }
};
```

#### Go

```go
func maxSubarraySum(nums []int, k int) int64 {
	n := len(nums)
	inf := int64(math.MinInt64 / 4)

	f := make([][4]int64, n+1)
	for i := range f {
		for j := 0; j < 4; j++ {
			f[i][j] = inf
		}
	}

	f[0][0] = 0
	ans := inf

	for i := 1; i <= n; i++ {
		x := int64(nums[i-1])

		f[i][0] = max(f[i-1][0], 0) + x
		f[i][1] = max(max(f[i-1][0], f[i-1][1]), 0) + x*int64(k)
		f[i][2] = max(max(f[i-1][0], f[i-1][2]), 0) + x/int64(k)
		f[i][3] = max(max(f[i-1][1], f[i-1][2]), f[i-1][3]) + x

		ans = max(ans, max(max(f[i][0], f[i][1]), max(f[i][2], f[i][3])))
	}

	return ans
}
```

#### TypeScript

```ts
function maxSubarraySum(nums: number[], k: number): number {
    const n = nums.length;
    const inf = -1e18;

    const f: number[][] = Array.from({ length: n + 1 }, () => {
        const arr = new Array(4).fill(inf);
        return arr;
    });

    f[0][0] = 0;
    let ans = inf;

    for (let i = 1; i <= n; i++) {
        const x = nums[i - 1];

        f[i][0] = Math.max(f[i - 1][0], 0) + x;
        f[i][1] = Math.max(Math.max(f[i - 1][0], f[i - 1][1]), 0) + x * k;
        f[i][2] = Math.max(Math.max(f[i - 1][0], f[i - 1][2]), 0) + Math.trunc(x / k);
        f[i][3] = Math.max(Math.max(f[i - 1][1], f[i - 1][2]), f[i - 1][3]) + x;

        ans = Math.max(ans, Math.max(...f[i]));
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
