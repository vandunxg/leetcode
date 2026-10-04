---
comments: true
difficulty: Hard
rating: 2978
source: Biweekly Contest 110 Q4
tags:
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [2809. Minimum Time to Make Array Sum At Most x](https://leetcode.com/problems/minimum-time-to-make-array-sum-at-most-x)

[中文文档](/solution/2800-2899/2809.Minimum%20Time%20to%20Make%20Array%20Sum%20At%20Most%20x/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong>, <code>nums1</code> và <code>nums2</code>, có cùng độ dài. Mỗi giây, với mọi chỉ số <code>0 &lt;= i &lt; nums1.length</code>, giá trị của <code>nums1[i]</code> được tăng thêm <code>nums2[i]</code>. <strong>Sau khi</strong> thực hiện việc này, bạn có thể thực hiện thao tác sau:</p>

<ul>
	<li>Chọn một chỉ số <code>0 &lt;= i &lt; nums1.length</code> và đặt <code>nums1[i] = 0</code>.</li>
</ul>

<p>Bạn cũng được cho một số nguyên <code>x</code>.</p>

<p>Hãy trả về <em>thời gian <strong>nhỏ nhất</strong> để tổng tất cả các phần tử của </em><code>nums1</code><em> <strong>nhỏ hơn hoặc bằng</strong> </em><code>x</code><em>, hoặc </em><code>-1</code><em> nếu không thể.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3], nums2 = [1,2,3], x = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ở giây thứ 1, ta thực hiện thao tác với i = 0. Khi đó nums1 = [0,2+2,3+3] = [0,4,6].
Ở giây thứ 2, ta thực hiện thao tác với i = 1. Khi đó nums1 = [0+1,0,6+3] = [1,0,9].
Ở giây thứ 3, ta thực hiện thao tác với i = 2. Khi đó nums1 = [1+1,0+2,0] = [2,2,0].
Lúc này tổng của nums1 = 4. Có thể chứng minh rằng các thao tác này là tối ưu, nên ta trả về 3.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3], nums2 = [3,3,3], x = 4
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng tổng của nums1 luôn lớn hơn x, bất kể thực hiện các thao tác như thế nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code><font face="monospace">1 &lt;= nums1.length &lt;= 10<sup>3</sup></font></code></li>
	<li><code>1 &lt;= nums1[i] &lt;= 10<sup>3</sup></code></li>
	<li><code>0 &lt;= nums2[i] &lt;= 10<sup>3</sup></code></li>
	<li><code>nums1.length == nums2.length</code></li>
	<li><code>0 &lt;= x &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Nếu thực hiện thao tác trên cùng một chỉ số nhiều hơn một lần thì chỉ lần cuối cùng có ý nghĩa, nên có nhiều nhất $n$ thao tác. Thao tác thứ $j$ trên chỉ số $i$ làm giảm tổng đi $nums_1[i]+nums_2[i]\cdot j$; để mức giảm lớn nhất, các giá trị $nums_2$ lớn hơn nên được thao tác muộn hơn. Sau khi sắp xếp theo $nums_2$, $f[i][j]$ là mức giảm lớn nhất khi dùng $j$ thao tác trên $i$ giá trị đầu tiên, và ta lấy $j$ nhỏ nhất sao cho tổng còn lại không vượt quá $x$.

<!-- thinking:end -->

Ta nhận thấy rằng nếu thực hiện thao tác trên cùng một số nhiều lần thì chỉ thao tác cuối cùng có ý nghĩa, còn các thao tác trước đó trên số này chỉ làm tăng các số khác. Vì vậy, ta thực hiện thao tác trên mỗi số nhiều nhất một lần, tức là số thao tác nằm trong $[0,..n]$.

Giả sử ta đã thực hiện $j$ thao tác, với các chỉ số của những số được thao tác là $i_1, i_2, \cdots, i_j$. Với $j$ thao tác này, lượng giảm tổng các phần tử mảng do mỗi thao tác tạo ra là:

$$
\begin{aligned}
& d_1 = nums_1[i_1] + nums_2[i_1] \times 1 \\
& d_2 = nums_1[i_2] + nums_2[i_2] \times 2 \\
& \cdots \\
& d_j = nums_1[i_j] + nums_2[i_j] \times j
\end{aligned}
$$

Theo góc nhìn tham lam, để tối đa hóa mức giảm của tổng các phần tử mảng, ta nên thực hiện thao tác trên các phần tử lớn hơn trong $nums_2$ càng muộn càng tốt. Vì vậy, ta có thể sắp xếp $nums_1$ và $nums_2$ theo thứ tự tăng dần của các giá trị phần tử trong $nums_2$.

Tiếp theo, ta xét cách cài đặt quy hoạch động. Ta dùng $f[i][j]$ để biểu diễn giá trị lớn nhất có thể giảm khỏi tổng các phần tử mảng khi xét $i$ phần tử đầu tiên của mảng $nums_1$ và thực hiện $j$ thao tác. Ta có công thức chuyển trạng thái:

$$
f[i][j] = \max \{f[i-1][j], f[i-1][j-1] + nums_1[i] + nums_2[i] \times j\}
$$

Cuối cùng, ta duyệt $j$ và tìm giá trị $j$ nhỏ nhất thỏa mãn $s_1 + s_2 \times j - f[n][j] \le x$.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, nums1: List[int], nums2: List[int], x: int) -> int:
        n = len(nums1)
        f = [[0] * (n + 1) for _ in range(n + 1)]
        for i, (a, b) in enumerate(sorted(zip(nums1, nums2), key=lambda z: z[1]), 1):
            for j in range(n + 1):
                f[i][j] = f[i - 1][j]
                if j > 0:
                    f[i][j] = max(f[i][j], f[i - 1][j - 1] + a + b * j)
        s1 = sum(nums1)
        s2 = sum(nums2)
        for j in range(n + 1):
            if s1 + s2 * j - f[n][j] <= x:
                return j
        return -1
```

#### Java

```java
class Solution {
    public int minimumTime(List<Integer> nums1, List<Integer> nums2, int x) {
        int n = nums1.size();
        int[][] f = new int[n + 1][n + 1];
        int[][] nums = new int[n][0];
        for (int i = 0; i < n; ++i) {
            nums[i] = new int[] {nums1.get(i), nums2.get(i)};
        }
        Arrays.sort(nums, Comparator.comparingInt(a -> a[1]));
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j > 0) {
                    int a = nums[i - 1][0], b = nums[i - 1][1];
                    f[i][j] = Math.max(f[i][j], f[i - 1][j - 1] + a + b * j);
                }
            }
        }
        int s1 = 0, s2 = 0;
        for (int v : nums1) {
            s1 += v;
        }
        for (int v : nums2) {
            s2 += v;
        }

        for (int j = 0; j <= n; ++j) {
            if (s1 + s2 * j - f[n][j] <= x) {
                return j;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTime(vector<int>& nums1, vector<int>& nums2, int x) {
        int n = nums1.size();
        vector<pair<int, int>> nums;
        for (int i = 0; i < n; ++i) {
            nums.emplace_back(nums2[i], nums1[i]);
        }
        sort(nums.begin(), nums.end());
        int f[n + 1][n + 1];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j) {
                    auto [b, a] = nums[i - 1];
                    f[i][j] = max(f[i][j], f[i - 1][j - 1] + a + b * j);
                }
            }
        }
        int s1 = accumulate(nums1.begin(), nums1.end(), 0);
        int s2 = accumulate(nums2.begin(), nums2.end(), 0);
        for (int j = 0; j <= n; ++j) {
            if (s1 + s2 * j - f[n][j] <= x) {
                return j;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minimumTime(nums1 []int, nums2 []int, x int) int {
	n := len(nums1)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	type pair struct{ a, b int }
	nums := make([]pair, n)
	var s1, s2 int
	for i := range nums {
		s1 += nums1[i]
		s2 += nums2[i]
		nums[i] = pair{nums1[i], nums2[i]}
	}
	sort.Slice(nums, func(i, j int) bool { return nums[i].b < nums[j].b })
	for i := 1; i <= n; i++ {
		for j := 0; j <= n; j++ {
			f[i][j] = f[i-1][j]
			if j > 0 {
				a, b := nums[i-1].a, nums[i-1].b
				f[i][j] = max(f[i][j], f[i-1][j-1]+a+b*j)
			}
		}
	}
	for j := 0; j <= n; j++ {
		if s1+s2*j-f[n][j] <= x {
			return j
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minimumTime(nums1: number[], nums2: number[], x: number): number {
    const n = nums1.length;
    const f: number[][] = Array(n + 1)
        .fill(0)
        .map(() => Array(n + 1).fill(0));
    const nums: number[][] = [];
    for (let i = 0; i < n; ++i) {
        nums.push([nums1[i], nums2[i]]);
    }
    nums.sort((a, b) => a[1] - b[1]);
    for (let i = 1; i <= n; ++i) {
        for (let j = 0; j <= n; ++j) {
            f[i][j] = f[i - 1][j];
            if (j > 0) {
                const [a, b] = nums[i - 1];
                f[i][j] = Math.max(f[i][j], f[i - 1][j - 1] + a + b * j);
            }
        }
    }
    const s1 = nums1.reduce((a, b) => a + b, 0);
    const s2 = nums2.reduce((a, b) => a + b, 0);
    for (let j = 0; j <= n; ++j) {
        if (s1 + s2 * j - f[n][j] <= x) {
            return j;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> $f[i][j]$ chỉ phụ thuộc vào hàng trước đó tại $j$ và $j-1$. Dùng mảng rolling và duyệt $j$ theo thứ tự giảm dần giúp giảm không gian xuống $O(n)$ mà không thay đổi các phép chuyển trạng thái.

<!-- thinking:end -->

$f[i][j]$ chỉ phụ thuộc vào $f[i-1][j]$ và $f[i-1][j-1]$, nên ta có thể bỏ chiều thứ nhất và duyệt $j$ từ lớn đến nhỏ, giảm độ phức tạp không gian xuống $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, nums1: List[int], nums2: List[int], x: int) -> int:
        n = len(nums1)
        f = [0] * (n + 1)
        for a, b in sorted(zip(nums1, nums2), key=lambda z: z[1]):
            for j in range(n, 0, -1):
                f[j] = max(f[j], f[j - 1] + a + b * j)
        s1 = sum(nums1)
        s2 = sum(nums2)
        for j in range(n + 1):
            if s1 + s2 * j - f[j] <= x:
                return j
        return -1
```

#### Java

```java
class Solution {
    public int minimumTime(List<Integer> nums1, List<Integer> nums2, int x) {
        int n = nums1.size();
        int[] f = new int[n + 1];
        int[][] nums = new int[n][0];
        for (int i = 0; i < n; ++i) {
            nums[i] = new int[] {nums1.get(i), nums2.get(i)};
        }
        Arrays.sort(nums, Comparator.comparingInt(a -> a[1]));
        for (int[] e : nums) {
            int a = e[0], b = e[1];
            for (int j = n; j > 0; --j) {
                f[j] = Math.max(f[j], f[j - 1] + a + b * j);
            }
        }
        int s1 = 0, s2 = 0;
        for (int v : nums1) {
            s1 += v;
        }
        for (int v : nums2) {
            s2 += v;
        }

        for (int j = 0; j <= n; ++j) {
            if (s1 + s2 * j - f[j] <= x) {
                return j;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTime(vector<int>& nums1, vector<int>& nums2, int x) {
        int n = nums1.size();
        vector<pair<int, int>> nums;
        for (int i = 0; i < n; ++i) {
            nums.emplace_back(nums2[i], nums1[i]);
        }
        sort(nums.begin(), nums.end());
        int f[n + 1];
        memset(f, 0, sizeof(f));
        for (auto [b, a] : nums) {
            for (int j = n; j; --j) {
                f[j] = max(f[j], f[j - 1] + a + b * j);
            }
        }
        int s1 = accumulate(nums1.begin(), nums1.end(), 0);
        int s2 = accumulate(nums2.begin(), nums2.end(), 0);
        for (int j = 0; j <= n; ++j) {
            if (s1 + s2 * j - f[j] <= x) {
                return j;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minimumTime(nums1 []int, nums2 []int, x int) int {
	n := len(nums1)
	f := make([]int, n+1)
	type pair struct{ a, b int }
	nums := make([]pair, n)
	var s1, s2 int
	for i := range nums {
		s1 += nums1[i]
		s2 += nums2[i]
		nums[i] = pair{nums1[i], nums2[i]}
	}
	sort.Slice(nums, func(i, j int) bool { return nums[i].b < nums[j].b })
	for _, e := range nums {
		a, b := e.a, e.b
		for j := n; j > 0; j-- {
			f[j] = max(f[j], f[j-1]+a+b*j)
		}
	}
	for j := 0; j <= n; j++ {
		if s1+s2*j-f[j] <= x {
			return j
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minimumTime(nums1: number[], nums2: number[], x: number): number {
    const n = nums1.length;
    const f: number[] = new Array(n + 1).fill(0);
    const nums: number[][] = [];
    for (let i = 0; i < n; ++i) {
        nums.push([nums1[i], nums2[i]]);
    }
    nums.sort((a, b) => a[1] - b[1]);
    for (const [a, b] of nums) {
        for (let j = n; j > 0; --j) {
            f[j] = Math.max(f[j], f[j - 1] + a + b * j);
        }
    }
    const s1 = nums1.reduce((a, b) => a + b, 0);
    const s2 = nums2.reduce((a, b) => a + b, 0);
    for (let j = 0; j <= n; ++j) {
        if (s1 + s2 * j - f[j] <= x) {
            return j;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
