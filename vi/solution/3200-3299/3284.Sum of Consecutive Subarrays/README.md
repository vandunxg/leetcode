---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Dynamic Programming
---

<!-- problem:start -->

# [3284. Sum of Consecutive Subarrays 🔒](https://leetcode.com/problems/sum-of-consecutive-subarrays)

[中文文档](/solution/3200-3299/3284.Sum%20of%20Consecutive%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Ta gọi một mảng <code>arr</code> có độ dài <code>n</code> là <strong>liên tiếp</strong> nếu thỏa mãn một trong các điều kiện sau:</p>

<ul>
	<li><code>arr[i] - arr[i - 1] == 1</code> với <em>mọi</em> <code>1 &lt;= i &lt; n</code>.</li>
	<li><code>arr[i] - arr[i - 1] == -1</code> với <em>mọi</em> <code>1 &lt;= i &lt; n</code>.</li>
</ul>

<p><strong>Giá trị</strong> của một mảng là tổng các phần tử của nó.</p>

<p>Ví dụ, <code>[3, 4, 5]</code> là một mảng liên tiếp có giá trị bằng 12 và <code>[9, 8]</code> cũng là một mảng có giá trị bằng 17. Trong khi đó, <code>[3, 4, 3]</code> và <code>[8, 6]</code> không phải là mảng liên tiếp.</p>

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <em>tổng</em> <strong>giá trị</strong> của tất cả <strong>những </strong><span data-keyword="subarray-nonempty">mảng con liên tiếp</span>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9 </sup>+ 7.</code></p>

<p><strong>Lưu ý</strong> rằng một mảng có độ dài 1 cũng được xem là liên tiếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con liên tiếp là: <code>[1]</code>, <code>[2]</code>, <code>[3]</code>, <code>[1, 2]</code>, <code>[2, 3]</code>, <code>[1, 2, 3]</code>.<br />
Tổng giá trị của chúng là: <code>1 + 2 + 3 + 3 + 5 + 6 = 20</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,5,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con liên tiếp là: <code>[1]</code>, <code>[3]</code>, <code>[5]</code>, <code>[7]</code>.<br />
Tổng giá trị của chúng là: <code>1 + 3 + 5 + 7 = 16</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,6,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">32</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con liên tiếp là: <code>[7]</code>, <code>[6]</code>, <code>[1]</code>, <code>[2]</code>, <code>[7, 6]</code>, <code>[1, 2]</code>.<br />
Tổng giá trị của chúng là: <code>7 + 6 + 1 + 2 + 13 + 3 = 32</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Công thức truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con liên tiếp là một đoạn mà hiệu giữa hai phần tử kề nhau là $\pm 1$; ta cần tính tổng của tất cả các đoạn như vậy, bao gồm cả các mảng chỉ có một phần tử. Với $n$ lớn, việc liệt kê các đoạn là không thể. Độ dài và tổng của đoạn tăng (tương ứng giảm) kết thúc tại $i$ có thể được tính theo công thức truy hồi.
>
> Hiệu bằng $1$ sẽ kéo dài đoạn tăng; hiệu bằng $-1$ sẽ kéo dài đoạn giảm; nếu không thì chỉ cộng riêng phần tử hiện tại. Khi hiệu là $\pm 1$, mảng chỉ có một phần tử đã nằm trong tổng của đoạn tương ứng. Chỉ cần bốn biến được cập nhật tuần tự.

<!-- thinking:end -->

Ta định nghĩa hai biến $f$ và $g$, lần lượt biểu diễn độ dài của mảng tăng và mảng giảm kết thúc tại phần tử hiện tại. Hai biến khác là $s$ và $t$, lần lượt biểu diễn tổng của mảng tăng và mảng giảm kết thúc tại phần tử hiện tại. Ban đầu, $f = g = 1$ và $s = t = \textit{nums}[0]$.

Tiếp theo, ta duyệt mảng bắt đầu từ phần tử thứ hai. Với phần tử hiện tại $\textit{nums}[i]$, ta xét mảng tăng và mảng giảm kết thúc tại $\textit{nums}[i]$.

Nếu $\textit{nums}[i] - \textit{nums}[i - 1] = 1$, thì $\textit{nums}[i]$ có thể được thêm vào mảng tăng kết thúc tại $\textit{nums}[i - 1]$. Khi đó, ta cập nhật $f$ và $s$, rồi cộng $s$ vào đáp án.

Nếu $\textit{nums}[i] - \textit{nums}[i - 1] = -1$, thì $\textit{nums}[i]$ có thể được thêm vào mảng giảm kết thúc tại $\textit{nums}[i - 1]$. Khi đó, ta cập nhật $g$ và $t$, rồi cộng $t$ vào đáp án.

Nếu không, $\textit{nums}[i]$ không thể được thêm vào mảng tăng hay mảng giảm kết thúc tại $\textit{nums}[i - 1]$. Ta cộng $\textit{nums}[i]$ vào đáp án.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSum(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        f = g = 1
        s = t = nums[0]
        ans = nums[0]
        for x, y in pairwise(nums):
            if y - x == 1:
                f += 1
                s += f * y
                ans = (ans + s) % mod
            else:
                f = 1
                s = y
            if y - x == -1:
                g += 1
                t += g * y
                ans = (ans + t) % mod
            else:
                g = 1
                t = y
            if abs(y - x) != 1:
                ans = (ans + y) % mod
        return ans
```

#### Java

```java
class Solution {
    public int getSum(int[] nums) {
        final int mod = (int) 1e9 + 7;
        long s = nums[0], t = nums[0], ans = nums[0];
        int f = 1, g = 1;
        for (int i = 1; i < nums.length; ++i) {
            int x = nums[i - 1], y = nums[i];
            if (y - x == 1) {
                ++f;
                s += 1L * f * y;
                ans = (ans + s) % mod;
            } else {
                f = 1;
                s = y;
            }
            if (y - x == -1) {
                ++g;
                t += 1L * g * y;
                ans = (ans + t) % mod;
            } else {
                g = 1;
                t = y;
            }
            if (Math.abs(y - x) != 1) {
                ans = (ans + y) % mod;
            }
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getSum(vector<int>& nums) {
        const int mod = 1e9 + 7;
        long long s = nums[0], t = nums[0], ans = nums[0];
        int f = 1, g = 1;
        for (int i = 1; i < nums.size(); ++i) {
            int x = nums[i - 1], y = nums[i];
            if (y - x == 1) {
                ++f;
                s += 1LL * f * y;
                ans = (ans + s) % mod;
            } else {
                f = 1;
                s = y;
            }
            if (y - x == -1) {
                ++g;
                t += 1LL * g * y;
                ans = (ans + t) % mod;
            } else {
                g = 1;
                t = y;
            }
            if (abs(y - x) != 1) {
                ans = (ans + y) % mod;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getSum(nums []int) int {
	const mod int = 1e9 + 7
	f, g := 1, 1
	s, t := nums[0], nums[0]
	ans := nums[0]

	for i := 1; i < len(nums); i++ {
		x, y := nums[i-1], nums[i]

		if y-x == 1 {
			f++
			s += f * y
			ans = (ans + s) % mod
		} else {
			f = 1
			s = y
		}

		if y-x == -1 {
			g++
			t += g * y
			ans = (ans + t) % mod
		} else {
			g = 1
			t = y
		}

		if abs(y-x) != 1 {
			ans = (ans + y) % mod
		}
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
function getSum(nums: number[]): number {
    const mod = 10 ** 9 + 7;
    let f = 1,
        g = 1;
    let s = nums[0],
        t = nums[0];
    let ans = nums[0];

    for (let i = 1; i < nums.length; i++) {
        const x = nums[i - 1];
        const y = nums[i];

        if (y - x === 1) {
            f++;
            s += f * y;
            ans = (ans + s) % mod;
        } else {
            f = 1;
            s = y;
        }

        if (y - x === -1) {
            g++;
            t += g * y;
            ans = (ans + t) % mod;
        } else {
            g = 1;
            t = y;
        }

        if (Math.abs(y - x) !== 1) {
            ans = (ans + y) % mod;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
