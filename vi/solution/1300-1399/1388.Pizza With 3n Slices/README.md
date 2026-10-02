---
comments: true
difficulty: Hard
rating: 2409
source: Biweekly Contest 22 Q4
tags:
    - Greedy
    - Array
    - Dynamic Programming
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1388. Pizza With 3n Slices](https://leetcode.com/problems/pizza-with-3n-slices)

[中文文档](/solution/1300-1399/1388.Pizza%20With%203n%20Slices/README.md)

## Mô tả

<!-- description:start -->

<p>Có một chiếc pizza gồm <code>3n</code> miếng với kích thước khác nhau. Bạn và bạn bè sẽ lấy các miếng theo cách sau:</p>

<ul>
	<li>Bạn chọn <strong>bất kỳ</strong> miếng pizza nào.</li>
	<li>Người bạn Alice sẽ lấy miếng tiếp theo theo chiều ngược kim đồng hồ tính từ miếng bạn đã chọn.</li>
	<li>Người bạn Bob sẽ lấy miếng tiếp theo theo chiều kim đồng hồ tính từ miếng bạn đã chọn.</li>
	<li>Lặp lại cho đến khi không còn miếng pizza nào.</li>
</ul>

<p>Cho mảng số nguyên <code>slices</code> biểu diễn kích thước các miếng pizza theo chiều kim đồng hồ. Hãy trả về <em>tổng kích thước lớn nhất có thể của các miếng bạn lấy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1388.Pizza%20With%203n%20Slices/images/sample_3_1723.png" style="width: 500px; height: 266px;" />
<pre>
<strong>Đầu vào:</strong> slices = [1,2,3,4,5,6]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Chọn miếng pizza kích thước 4; Alice và Bob lần lượt lấy miếng kích thước 3 và 5. Sau đó, chọn miếng kích thước 6; cuối cùng Alice và Bob lần lượt lấy miếng kích thước 2 và 1. Tổng = 4 + 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1388.Pizza%20With%203n%20Slices/images/sample_4_1723.png" style="width: 500px; height: 299px;" />
<pre>
<strong>Đầu vào:</strong> slices = [8,9,8,6,1,1]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Mỗi lượt, hãy chọn một miếng pizza kích thước 8. Nếu bạn chọn miếng kích thước 9, những người bạn của bạn sẽ lấy các miếng kích thước 8.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 * n == slices.length</code></li>
	<li><code>1 &lt;= slices.length &lt;= 500</code></li>
	<li><code>1 &lt;= slices[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chọn $n$ miếng trong vòng tròn gồm $3n$ miếng sao cho không lấy hai miếng kề nhau và tổng kích thước lớn nhất. Bài toán trở thành chọn $n$ giá trị không kề nhau trên một vòng tròn. Miếng đầu và cuối xung đột, nên ta giải bài toán tuyến tính hai lần (bỏ miếng đầu hoặc bỏ miếng cuối). $f[i][j]$ là tổng lớn nhất khi chọn $j$ miếng trong $i$ miếng đầu tiên: bỏ miếng thứ $i$, hoặc chọn nó và cộng với $f[i-2][j-1]$. Đáp án là giá trị lớn hơn trong hai kết quả tuyến tính.

<!-- thinking:end -->

Ta có thể chuyển bài toán thành: Trong mảng vòng tròn độ dài $3n$, chọn $n$ số không kề nhau sao cho tổng của chúng lớn nhất.

Chứng minh như sau:

- Khi $n = 1$, ta có thể chọn bất kỳ số nào trong mảng.
- Khi $n > 1$, luôn tồn tại một số sao cho ở một phía của nó có hai số liên tiếp chưa được chọn, và ở phía còn lại có ít nhất một số chưa được chọn. Vì vậy, ta có thể loại số này cùng hai số ở hai bên khỏi mảng; $3(n - 1)$ số còn lại tạo thành một mảng vòng tròn mới. Bài toán được thu nhỏ thành chọn $n - 1$ số không kề nhau trong mảng vòng tròn độ dài $3(n - 1)$ sao cho tổng của chúng lớn nhất.

Do đó, bài toán cần giải có thể được chuyển thành: Trong mảng vòng tròn độ dài $3n$, chọn $n$ số không kề nhau sao cho tổng của chúng lớn nhất.

Trong mảng vòng tròn, nếu chọn số đầu tiên thì không thể chọn số cuối cùng, và ngược lại. Vì vậy, ta chia bài toán thành hai mảng: một mảng bỏ số đầu tiên, mảng còn lại bỏ số cuối cùng. Tìm giá trị lớn nhất riêng cho từng mảng rồi lấy giá trị lớn hơn.

Ta dùng hàm $g(nums)$ để tính tổng lớn nhất khi chọn $n$ số không kề nhau trong mảng $nums$. Khi đó, mục tiêu là tìm giá trị lớn hơn giữa $g(slices)$ và $g(slices[1:])$.

Hàm $g(nums)$ được giải như sau:

Gọi độ dài mảng $nums$ là $m$, và định nghĩa $f[i][j]$ là tổng lớn nhất khi chọn $j$ số không kề nhau trong $i$ số đầu tiên của mảng $nums$.

Xét $f[i][j]$: nếu không chọn số thứ $i$, thì $f[i][j] = f[i - 1][j]$. Nếu chọn số thứ $i$, thì $f[i][j] = f[i - 2][j - 1] + nums[i - 1]$. Do đó, ta có công thức chuyển trạng thái:

$$
f[i][j] = \max(f[i - 1][j], f[i - 2][j - 1] + nums[i - 1])
$$

Cuối cùng, trả về $f[m][n]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài mảng $slices$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSizeSlices(self, slices: List[int]) -> int:
        def g(nums: List[int]) -> int:
            m = len(nums)
            f = [[0] * (n + 1) for _ in range(m + 1)]
            for i in range(1, m + 1):
                for j in range(1, n + 1):
                    f[i][j] = max(
                        f[i - 1][j], (f[i - 2][j - 1] if i >= 2 else 0) + nums[i - 1]
                    )
            return f[m][n]

        n = len(slices) // 3
        a, b = g(slices[:-1]), g(slices[1:])
        return max(a, b)
```

#### Java

```java
class Solution {
    private int n;

    public int maxSizeSlices(int[] slices) {
        n = slices.length / 3;
        int[] nums = new int[slices.length - 1];
        System.arraycopy(slices, 1, nums, 0, nums.length);
        int a = g(nums);
        System.arraycopy(slices, 0, nums, 0, nums.length);
        int b = g(nums);
        return Math.max(a, b);
    }

    private int g(int[] nums) {
        int m = nums.length;
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                f[i][j] = Math.max(f[i - 1][j], (i >= 2 ? f[i - 2][j - 1] : 0) + nums[i - 1]);
            }
        }
        return f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSizeSlices(vector<int>& slices) {
        int n = slices.size() / 3;
        auto g = [&](vector<int>& nums) -> int {
            int m = nums.size();
            int f[m + 1][n + 1];
            memset(f, 0, sizeof f);
            for (int i = 1; i <= m; ++i) {
                for (int j = 1; j <= n; ++j) {
                    f[i][j] = max(f[i - 1][j], (i >= 2 ? f[i - 2][j - 1] : 0) + nums[i - 1]);
                }
            }
            return f[m][n];
        };
        vector<int> nums(slices.begin(), slices.end() - 1);
        int a = g(nums);
        nums = vector<int>(slices.begin() + 1, slices.end());
        int b = g(nums);
        return max(a, b);
    }
};
```

#### Go

```go
func maxSizeSlices(slices []int) int {
	n := len(slices) / 3
	g := func(nums []int) int {
		m := len(nums)
		f := make([][]int, m+1)
		for i := range f {
			f[i] = make([]int, n+1)
		}
		for i := 1; i <= m; i++ {
			for j := 1; j <= n; j++ {
				f[i][j] = max(f[i-1][j], nums[i-1])
				if i >= 2 {
					f[i][j] = max(f[i][j], f[i-2][j-1]+nums[i-1])
				}
			}
		}
		return f[m][n]
	}
	a, b := g(slices[:len(slices)-1]), g(slices[1:])
	return max(a, b)
}
```

#### TypeScript

```ts
function maxSizeSlices(slices: number[]): number {
    const n = Math.floor(slices.length / 3);
    const g = (nums: number[]): number => {
        const m = nums.length;
        const f: number[][] = Array(m + 1)
            .fill(0)
            .map(() => Array(n + 1).fill(0));
        for (let i = 1; i <= m; ++i) {
            for (let j = 1; j <= n; ++j) {
                f[i][j] = Math.max(f[i - 1][j], (i > 1 ? f[i - 2][j - 1] : 0) + nums[i - 1]);
            }
        }
        return f[m][n];
    };
    const a = g(slices.slice(0, -1));
    const b = g(slices.slice(1));
    return Math.max(a, b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
