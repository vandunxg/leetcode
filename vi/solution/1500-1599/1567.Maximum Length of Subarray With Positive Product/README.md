---
comments: true
difficulty: Medium
rating: 1710
source: Weekly Contest 204 Q2
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1567. Maximum Length of Subarray With Positive Product](https://leetcode.com/problems/maximum-length-of-subarray-with-positive-product)

[中文文档](/solution/1500-1599/1567.Maximum%20Length%20of%20Subarray%20With%20Positive%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy tìm độ dài lớn nhất của mảng con có tích của mọi phần tử dương.</p>

<p>Mảng con là một dãy liên tiếp gồm không hoặc nhiều giá trị được lấy từ mảng.</p>

<p>Trả về <em>độ dài lớn nhất của mảng con có tích dương</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,-2,-3,4]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Toàn bộ mảng nums đã có tích dương bằng 24.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [0,1,-2,-3,-4]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Mảng con dài nhất có tích dương là [1,-2,-3], có tích bằng 6.
Lưu ý không thể đưa 0 vào mảng con vì khi đó tích bằng 0, không phải số dương.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [-1,-2,-3,0,1]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Mảng con dài nhất có tích dương là [-1,-2] hoặc [-2,-3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tìm mảng con dài nhất có tích dương. Vì $n\le 10^5$, kiểm tra mọi cặp sẽ quá chậm. Số 0 chia mảng thành các đoạn và số âm chỉ đảo dấu, nên tại mỗi điểm cuối ta lưu độ dài lớn nhất của tích dương và âm.
>
> Gọi $f[i]$ và $g[i]$ là hai độ dài đó kết thúc tại $i$. Giá trị dương giữ nguyên dấu trước đó; giá trị âm hoán đổi hai trạng thái; số 0 phá vỡ cả hai. Đáp án là giá trị lớn nhất của $f[i]$.

<!-- thinking:end -->

Định nghĩa hai mảng $f$ và $g$ độ dài $n$, trong đó $f[i]$ là độ dài mảng con dài nhất kết thúc tại $\textit{nums}[i]$ có tích dương, còn $g[i]$ là độ dài mảng con dài nhất có tích âm.

Ban đầu, nếu $\textit{nums}[0] > 0$ thì $f[0] = 1$, ngược lại $f[0] = 0$; nếu $\textit{nums}[0] < 0$ thì $g[0] = 1$, ngược lại $g[0] = 0$. Khởi tạo đáp án $ans = f[0]$.

Tiếp theo, duyệt mảng $\textit{nums}$ từ $i = 1$. Với mỗi $i$, xét phần tử $\textit{nums}[i]$ và có các trường hợp sau:

- If $\textit{nums}[i] > 0$, then $f[i]$ can be transferred from $f[i - 1]$, i.e., $f[i] = f[i - 1] + 1$, and the value of $g[i]$ depends on whether $g[i - 1]$ is $0$. If $g[i - 1] = 0$, then $g[i] = 0$, otherwise $g[i] = g[i - 1] + 1$;
- If $\textit{nums}[i] < 0$, then the value of $f[i]$ depends on whether $g[i - 1]$ is $0$. If $g[i - 1] = 0$, then $f[i] = 0$, otherwise $f[i] = g[i - 1] + 1$, and $g[i]$ can be transferred from $f[i - 1]$, i.e., $g[i] = f[i - 1] + 1$.
- Then, we update the answer $ans = \max(ans, f[i])$.

Sau khi duyệt xong, trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        n = len(nums)
        f = [0] * n
        g = [0] * n
        f[0] = int(nums[0] > 0)
        g[0] = int(nums[0] < 0)
        ans = f[0]
        for i in range(1, n):
            if nums[i] > 0:
                f[i] = f[i - 1] + 1
                g[i] = 0 if g[i - 1] == 0 else g[i - 1] + 1
            elif nums[i] < 0:
                f[i] = 0 if g[i - 1] == 0 else g[i - 1] + 1
                g[i] = f[i - 1] + 1
            ans = max(ans, f[i])
        return ans
```

#### Java

```java
class Solution {
    public int getMaxLen(int[] nums) {
        int n = nums.length;
        int[] f = new int[n];
        int[] g = new int[n];
        f[0] = nums[0] > 0 ? 1 : 0;
        g[0] = nums[0] < 0 ? 1 : 0;
        int ans = f[0];
        for (int i = 1; i < n; ++i) {
            if (nums[i] > 0) {
                f[i] = f[i - 1] + 1;
                g[i] = g[i - 1] > 0 ? g[i - 1] + 1 : 0;
            } else if (nums[i] < 0) {
                f[i] = g[i - 1] > 0 ? g[i - 1] + 1 : 0;
                g[i] = f[i - 1] + 1;
            }
            ans = Math.max(ans, f[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getMaxLen(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n, 0), g(n, 0);
        f[0] = nums[0] > 0 ? 1 : 0;
        g[0] = nums[0] < 0 ? 1 : 0;
        int ans = f[0];

        for (int i = 1; i < n; ++i) {
            if (nums[i] > 0) {
                f[i] = f[i - 1] + 1;
                g[i] = g[i - 1] > 0 ? g[i - 1] + 1 : 0;
            } else if (nums[i] < 0) {
                f[i] = g[i - 1] > 0 ? g[i - 1] + 1 : 0;
                g[i] = f[i - 1] + 1;
            }
            ans = max(ans, f[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func getMaxLen(nums []int) int {
	n := len(nums)
	f := make([]int, n)
	g := make([]int, n)
	if nums[0] > 0 {
		f[0] = 1
	}
	if nums[0] < 0 {
		g[0] = 1
	}
	ans := f[0]

	for i := 1; i < n; i++ {
		if nums[i] > 0 {
			f[i] = f[i-1] + 1
			if g[i-1] > 0 {
				g[i] = g[i-1] + 1
			} else {
				g[i] = 0
			}
		} else if nums[i] < 0 {
			if g[i-1] > 0 {
				f[i] = g[i-1] + 1
			} else {
				f[i] = 0
			}
			g[i] = f[i-1] + 1
		}
		ans = max(ans, f[i])
	}
	return ans
}
```

#### TypeScript

```ts
function getMaxLen(nums: number[]): number {
    const n = nums.length;
    const f: number[] = Array(n).fill(0);
    const g: number[] = Array(n).fill(0);

    if (nums[0] > 0) {
        f[0] = 1;
    }
    if (nums[0] < 0) {
        g[0] = 1;
    }

    let ans = f[0];
    for (let i = 1; i < n; i++) {
        if (nums[i] > 0) {
            f[i] = f[i - 1] + 1;
            g[i] = g[i - 1] > 0 ? g[i - 1] + 1 : 0;
        } else if (nums[i] < 0) {
            f[i] = g[i - 1] > 0 ? g[i - 1] + 1 : 0;
            g[i] = f[i - 1] + 1;
        }

        ans = Math.max(ans, f[i]);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu không gian)

<!-- thinking:start -->

> **Thinking**
>
> Each state uses only the previous $f$ and $g$, so two scalars replace the arrays. Time stays linear and extra memory becomes constant.

<!-- thinking:end -->

Ta nhận thấy với mỗi $i$, các giá trị $f[i]$ và $g[i]$ chỉ phụ thuộc vào $f[i - 1]$ và $g[i - 1]$. Vì vậy, có thể dùng hai biến $f$ và $g$ lần lượt lưu giá trị của $f[i - 1]$ và $g[i - 1]$, tối ưu độ phức tạp không gian xuống $O(1)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        n = len(nums)
        f = int(nums[0] > 0)
        g = int(nums[0] < 0)
        ans = f
        for i in range(1, n):
            ff = gg = 0
            if nums[i] > 0:
                ff = f + 1
                gg = 0 if g == 0 else g + 1
            elif nums[i] < 0:
                ff = 0 if g == 0 else g + 1
                gg = f + 1
            f, g = ff, gg
            ans = max(ans, f)
        return ans
```

#### Java

```java
class Solution {
    public int getMaxLen(int[] nums) {
        int n = nums.length;
        int f = nums[0] > 0 ? 1 : 0;
        int g = nums[0] < 0 ? 1 : 0;
        int ans = f;

        for (int i = 1; i < n; i++) {
            int ff = 0, gg = 0;
            if (nums[i] > 0) {
                ff = f + 1;
                gg = g == 0 ? 0 : g + 1;
            } else if (nums[i] < 0) {
                ff = g == 0 ? 0 : g + 1;
                gg = f + 1;
            }
            f = ff;
            g = gg;
            ans = Math.max(ans, f);
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getMaxLen(vector<int>& nums) {
        int n = nums.size();
        int f = nums[0] > 0 ? 1 : 0;
        int g = nums[0] < 0 ? 1 : 0;
        int ans = f;

        for (int i = 1; i < n; i++) {
            int ff = 0, gg = 0;
            if (nums[i] > 0) {
                ff = f + 1;
                gg = g == 0 ? 0 : g + 1;
            } else if (nums[i] < 0) {
                ff = g == 0 ? 0 : g + 1;
                gg = f + 1;
            }
            f = ff;
            g = gg;
            ans = max(ans, f);
        }

        return ans;
    }
};
```

#### Go

```go
func getMaxLen(nums []int) int {
	n := len(nums)
	var f, g int
	if nums[0] > 0 {
		f = 1
	} else if nums[0] < 0 {
		g = 1
	}
	ans := f
	for i := 1; i < n; i++ {
		ff, gg := 0, 0
		if nums[i] > 0 {
			ff = f + 1
			gg = 0
			if g > 0 {
				gg = g + 1
			}
		} else if nums[i] < 0 {
			ff = 0
			if g > 0 {
				ff = g + 1
			}
			gg = f + 1
		}
		f, g = ff, gg
		ans = max(ans, f)
	}
	return ans
}
```

#### TypeScript

```ts
function getMaxLen(nums: number[]): number {
    const n = nums.length;
    let [f, g] = [0, 0];
    if (nums[0] > 0) {
        f = 1;
    } else if (nums[0] < 0) {
        g = 1;
    }
    let ans = f;
    for (let i = 1; i < n; i++) {
        let [ff, gg] = [0, 0];
        if (nums[i] > 0) {
            ff = f + 1;
            gg = g > 0 ? g + 1 : 0;
        } else if (nums[i] < 0) {
            ff = g > 0 ? g + 1 : 0;
            gg = f + 1;
        }
        [f, g] = [ff, gg];
        ans = Math.max(ans, f);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
