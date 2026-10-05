---
comments: true
difficulty: Medium
rating: 2042
source: Weekly Contest 493 Q3
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3872. Longest Arithmetic Sequence After Changing At Most One Element](https://leetcode.com/problems/longest-arithmetic-sequence-after-changing-at-most-one-element)

[中文文档](/solution/3800-3899/3872.Longest%20Arithmetic%20Sequence%20After%20Changing%20At%20Most%20One%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <span data-keyword="subarray">mảng con</span> là <strong>cấp số cộng</strong> nếu hiệu giữa các phần tử liên tiếp trong mảng con là hằng số.</p>

<p>Bạn có thể thay thế <strong>nhiều nhất một</strong> phần tử trong <code>nums</code> bằng bất kỳ <strong>số nguyên</strong> nào. Sau đó, bạn chọn một mảng con cấp số cộng từ <code>nums</code>.</p>

<p>Trả về một số nguyên biểu thị <strong>độ dài lớn nhất</strong> của mảng con cấp số cộng mà bạn có thể chọn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,7,5,10,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay <code>nums[3] = 10</code> bằng 3. Mảng trở thành <code>[9, 7, 5, 3, 1]</code>.</li>
	<li>Chọn mảng con <code>[<u><strong>9, 7, 5, 3, 1</strong></u>]</code>. Đây là một cấp số cộng vì hiệu giữa hai phần tử liên tiếp là -2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,6,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay <code>nums[0] = 1</code> bằng -2. Mảng trở thành <code>[-2, 2, 6, 7]</code>.</li>
	<li>Chọn mảng con <code>[<u><strong>-2, 2, 6</strong></u>, 7]</code>. Đây là một cấp số cộng vì hiệu giữa hai phần tử liên tiếp là 4.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>4 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân rã tiền tố và hậu tố + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi thay đổi nhiều nhất một phần tử, ta cần tìm mảng con cấp số cộng liên tiếp dài nhất. Vì $n \le 10^5$, ta không thể mở rộng từ mọi vị trí thay đổi.
>
> Nếu không thay đổi phần tử nào, đoạn dài nhất là một chuỗi các hiệu giữa phần tử kề nhau bằng nhau. Khi thay đổi $i$, ta có thể mở rộng đoạn bên trái, đoạn bên phải hoặc nối cả hai đoạn bằng cùng một công sai.
>
> Tính trước độ dài cấp số cộng $f,g$ kết thúc hoặc bắt đầu tại $i$, sau đó thử từng vị trí thay đổi theo ba cách trên.
>
> Để nối cả hai phía, $nums[i+1]-nums[i-1]$ phải là số chẵn, và chỉ cộng thêm độ dài khi hiệu đó khớp với các đoạn kề bên.

<!-- thinking:end -->

Trước tiên, ta tính các hiệu giữa những phần tử kề nhau của mảng và lưu chúng trong mảng $d$, trong đó $d[i] = nums[i] - nums[i - 1]$.

Tiếp theo, ta định nghĩa hai mảng $f$ và $g$. $f[i]$ biểu diễn độ dài mảng con cấp số cộng dài nhất kết thúc tại phần tử thứ $i$, còn $g[i]$ biểu diễn độ dài mảng con cấp số cộng dài nhất bắt đầu tại phần tử thứ $i$. Ban đầu, $f[0] = 1$, $g[n - 1] = 1$, các phần tử còn lại được khởi tạo bằng $2$.

Ta có thể tính các giá trị của $f$ và $g$ trong một lần duyệt:

- Với $f$: nếu $d[i] == d[i - 1]$, thì $f[i] = f[i - 1] + 1$.
- Với $g$: nếu $d[i + 1] == d[i + 2]$, thì $g[i] = g[i + 1] + 1$.

Sau đó, ta khởi tạo đáp án bằng $3$, vì luôn có thể tạo một mảng con cấp số cộng độ dài $3$ bằng cách thay thế một phần tử. Ta liệt kê từng phần tử và thử thay thế nó bằng một giá trị phù hợp để tạo mảng con dài hơn:

- Với mỗi phần tử $i$, ta có thể dùng trực tiếp $f[i]$ hoặc $g[i]$ để cập nhật đáp án.
- Nếu $i > 0$, ta có thể thay thế $nums[i]$ bằng $nums[i - 1] + d[i - 1]$ để mở rộng mảng con cấp số cộng kết thúc tại $i - 1$, rồi cập nhật đáp án thành $f[i - 1] + 1$.
- Nếu $i + 1 < n$, ta có thể thay thế $nums[i]$ bằng $nums[i + 1] - d[i + 1]$ để mở rộng mảng con cấp số cộng bắt đầu tại $i + 1$, rồi cập nhật đáp án thành $g[i + 1] + 1$.
- Nếu $0 < i < n - 1$, ta có thể thay thế $nums[i]$ bằng $nums[i - 1] + \frac{nums[i + 1] - nums[i - 1]}{2}$ để thử nối $f[i - 1]$ và $g[i + 1]$. Nếu giá trị này là số nguyên và khớp với cả $d[i - 1]$ và $d[i + 1]$, ta cập nhật đáp án thành $3 + (f[i - 1] - 1) + (g[i + 1] - 1)$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestArithmetic(self, nums: List[int]) -> int:
        n = len(nums)
        d = [0] * n
        for i in range(1, n):
            d[i] = nums[i] - nums[i - 1]

        f = [2] * n
        g = [2] * n
        f[0] = g[n - 1] = 1
        for i in range(2, n):
            if d[i] == d[i - 1]:
                f[i] = f[i - 1] + 1
        for i in range(n - 3, -1, -1):
            if d[i + 1] == d[i + 2]:
                g[i] = g[i + 1] + 1

        ans = 3
        for i in range(n):
            ans = max(ans, f[i], g[i])
            if i > 0:
                ans = max(ans, f[i - 1] + 1)
            if i + 1 < n:
                ans = max(ans, g[i + 1] + 1)
            if 0 < i < n - 1:
                diff = nums[i + 1] - nums[i -1]
                if diff % 2 == 0:
                    diff //= 2
                    k = 3
                    if i > 1 and diff == d[i - 1]:
                        k += f[i - 1] - 1
                    if i < n - 2 and diff == d[i + 2]:
                        k += g[i + 1] - 1
                    ans = max(ans, k)
        return ans
```

#### Java

```java
class Solution {
    public int longestArithmetic(int[] nums) {
        int n = nums.length;
        int[] d = new int[n];
        for (int i = 1; i < n; i++) {
            d[i] = nums[i] - nums[i - 1];
        }

        int[] f = new int[n];
        int[] g = new int[n];
        Arrays.fill(f, 2);
        Arrays.fill(g, 2);
        f[0] = 1;
        g[n - 1] = 1;

        for (int i = 2; i < n; i++) {
            if (d[i] == d[i - 1]) {
                f[i] = f[i - 1] + 1;
            }
        }

        for (int i = n - 3; i >= 0; i--) {
            if (d[i + 1] == d[i + 2]) {
                g[i] = g[i + 1] + 1;
            }
        }

        int ans = 3;
        for (int i = 0; i < n; i++) {
            ans = Math.max(ans, Math.max(f[i], g[i]));
            if (i > 0) {
                ans = Math.max(ans, f[i - 1] + 1);
            }
            if (i + 1 < n) {
                ans = Math.max(ans, g[i + 1] + 1);
            }
            if (i > 0 && i < n - 1) {
                int diff = nums[i + 1] - nums[i - 1];
                if (diff % 2 == 0) {
                    diff /= 2;
                    int k = 3;
                    if (i > 1 && diff == d[i - 1]) {
                        k += f[i - 1] - 1;
                    }
                    if (i < n - 2 && diff == d[i + 2]) {
                        k += g[i + 1] - 1;
                    }
                    ans = Math.max(ans, k);
                }
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
    int longestArithmetic(vector<int>& nums) {
        int n = nums.size();
        vector<int> d(n);
        for (int i = 1; i < n; i++) {
            d[i] = nums[i] - nums[i - 1];
        }

        vector<int> f(n, 2), g(n, 2);
        f[0] = 1;
        g[n - 1] = 1;

        for (int i = 2; i < n; i++) {
            if (d[i] == d[i - 1]) {
                f[i] = f[i - 1] + 1;
            }
        }

        for (int i = n - 3; i >= 0; i--) {
            if (d[i + 1] == d[i + 2]) {
                g[i] = g[i + 1] + 1;
            }
        }

        int ans = 3;
        for (int i = 0; i < n; i++) {
            ans = max(ans, max(f[i], g[i]));
            if (i > 0) {
                ans = max(ans, f[i - 1] + 1);
            }
            if (i + 1 < n) {
                ans = max(ans, g[i + 1] + 1);
            }
            if (i > 0 && i < n - 1) {
                int diff = nums[i + 1] - nums[i - 1];
                if (diff % 2 == 0) {
                    diff /= 2;
                    int k = 3;
                    if (i > 1 && diff == d[i - 1]) {
                        k += f[i - 1] - 1;
                    }
                    if (i < n - 2 && diff == d[i + 2]) {
                        k += g[i + 1] - 1;
                    }
                    ans = max(ans, k);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestArithmetic(nums []int) int {
	n := len(nums)
	d := make([]int, n)
	for i := 1; i < n; i++ {
		d[i] = nums[i] - nums[i-1]
	}

	f := make([]int, n)
	g := make([]int, n)
	for i := range f {
		f[i], g[i] = 2, 2
	}
	f[0], g[n-1] = 1, 1

	for i := 2; i < n; i++ {
		if d[i] == d[i-1] {
			f[i] = f[i-1] + 1
		}
	}

	for i := n - 3; i >= 0; i-- {
		if d[i+1] == d[i+2] {
			g[i] = g[i+1] + 1
		}
	}

	ans := 3
	for i := 0; i < n; i++ {
		ans = max(ans, f[i], g[i])

		if i > 0 {
			ans = max(ans, f[i-1]+1)
		}
		if i+1 < n {
			ans = max(ans, g[i+1]+1)
		}

		if i > 0 && i < n-1 {
			diff := nums[i+1] - nums[i-1]
			if diff%2 == 0 {
				diff /= 2
				k := 3
				if i > 1 && diff == d[i-1] {
					k += f[i-1] - 1
				}
				if i < n-2 && diff == d[i+2] {
					k += g[i+1] - 1
				}
				ans = max(ans, k)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestArithmetic(nums: number[]): number {
    const n = nums.length;
    const d = new Array(n).fill(0);

    for (let i = 1; i < n; i++) {
        d[i] = nums[i] - nums[i - 1];
    }

    const f = new Array(n).fill(2);
    const g = new Array(n).fill(2);
    f[0] = 1;
    g[n - 1] = 1;

    for (let i = 2; i < n; i++) {
        if (d[i] === d[i - 1]) {
            f[i] = f[i - 1] + 1;
        }
    }

    for (let i = n - 3; i >= 0; i--) {
        if (d[i + 1] === d[i + 2]) {
            g[i] = g[i + 1] + 1;
        }
    }

    let ans = 3;
    for (let i = 0; i < n; i++) {
        ans = Math.max(ans, f[i], g[i]);
        if (i > 0) {
            ans = Math.max(ans, f[i - 1] + 1);
        }
        if (i + 1 < n) {
            ans = Math.max(ans, g[i + 1] + 1);
        }
        if (i > 0 && i < n - 1) {
            let diff = nums[i + 1] - nums[i - 1];
            if (diff % 2 === 0) {
                diff = Math.floor(diff / 2);
                let k = 3;
                if (i > 1 && diff === d[i - 1]) {
                    k += f[i - 1] - 1;
                }
                if (i < n - 2 && diff === d[i + 2]) {
                    k += g[i + 1] - 1;
                }
                ans = Math.max(ans, k);
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
