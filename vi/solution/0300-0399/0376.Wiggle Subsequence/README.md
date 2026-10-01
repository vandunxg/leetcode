---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence)

[中文文档](/solution/0300-0399/0376.Wiggle%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Wiggle sequence</strong> là dãy mà hiệu giữa các số liên tiếp luân phiên nghiêm ngặt giữa số dương và số âm. Hiệu đầu tiên (nếu có) có thể dương hoặc âm. Dãy chỉ có một phần tử và dãy có hai phần tử khác nhau hiển nhiên đều là wiggle sequence.</p>

<ul>
	<li>Ví dụ, <code>[1, 7, 4, 9, 2, 5]</code> là một <strong>wiggle sequence</strong> vì các hiệu <code>(6, -3, 5, -7, 3)</code> luân phiên giữa số dương và số âm.</li>
	<li>Ngược lại, <code>[1, 4, 7, 2, 5]</code> và <code>[1, 7, 4, 5, 5]</code> không phải wiggle sequence. Dãy thứ nhất không thỏa mãn vì hai hiệu đầu đều dương; dãy thứ hai không thỏa mãn vì hiệu cuối bằng 0.</li>
</ul>

<p><strong>Dãy con</strong> được tạo bằng cách xóa một số phần tử (có thể không xóa phần tử nào) khỏi dãy ban đầu, sao cho các phần tử còn lại vẫn giữ nguyên thứ tự.</p>

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>độ dài của <strong>wiggle subsequence</strong> dài nhất của </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,7,4,9,2,5]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Toàn bộ dãy là một wiggle sequence với các hiệu (6, -3, 5, -7, 3).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,17,5,10,13,15,10,5,16,8]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có một số dãy con đạt được độ dài này.
Một trong số đó là [1, 17, 10, 13, 10, 16, 8], với các hiệu (16, -7, 3, -3, 6, -8).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6,7,8,9]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này trong thời gian <code>O(n)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Wiggle subsequence luân phiên tăng và giảm. Có quá nhiều dãy con để liệt kê hết. Với mỗi vị trí kết thúc, ta chỉ cần phân biệt hiệu cuối là tăng hay giảm.
>
> $f[i]$ biểu thị dãy kết thúc bằng hiệu tăng, còn $g[i]$ biểu thị dãy kết thúc bằng hiệu giảm. Nếu $nums[j]$ nhỏ hơn $nums[i]$, ta có thể nối một dãy kết thúc bằng hiệu giảm để tạo dãy thuộc $f$; nếu lớn hơn, ta nối một dãy kết thúc bằng hiệu tăng để tạo dãy thuộc $g$. Đáp án là giá trị lớn nhất trong các $f$ và $g$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là độ dài wiggle sequence kết thúc tại phần tử thứ $i$ với xu hướng tăng, và $g[i]$ là độ dài wiggle sequence kết thúc tại phần tử thứ $i$ với xu hướng giảm. Ban đầu, $f[0] = g[0] = 1$, vì dãy chỉ có một phần tử thì độ dài wiggle sequence là $1$. Khởi tạo đáp án bằng $1$.

Với $f[i]$, trong đó $i \geq 1$, ta duyệt $j$ trong khoảng $[0, i)$. Nếu $nums[j] < nums[i]$, ta có thể thêm phần tử tại $i$ sau phần tử tại $j$ để tạo wiggle sequence có xu hướng tăng, khi đó $f[i] = \max(f[i], g[j] + 1)$. Nếu $nums[j] > nums[i]$, ta có thể thêm phần tử tại $i$ sau phần tử tại $j$ để tạo wiggle sequence có xu hướng giảm, khi đó $g[i] = \max(g[i], f[j] + 1)$. Sau đó, cập nhật đáp án thành $\max(f[i], g[i])$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 1
        f = [1] * n
        g = [1] * n
        for i in range(1, n):
            for j in range(i):
                if nums[j] < nums[i]:
                    f[i] = max(f[i], g[j] + 1)
                elif nums[j] > nums[i]:
                    g[i] = max(g[i], f[j] + 1)
            ans = max(ans, f[i], g[i])
        return ans
```

#### Java

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int n = nums.length;
        int ans = 1;
        int[] f = new int[n];
        int[] g = new int[n];
        f[0] = 1;
        g[0] = 1;
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    f[i] = Math.max(f[i], g[j] + 1);
                } else if (nums[j] > nums[i]) {
                    g[i] = Math.max(g[i], f[j] + 1);
                }
            }
            ans = Math.max(ans, Math.max(f[i], g[i]));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int n = nums.size();
        int ans = 1;
        vector<int> f(n, 1);
        vector<int> g(n, 1);
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    f[i] = max(f[i], g[j] + 1);
                } else if (nums[j] > nums[i]) {
                    g[i] = max(g[i], f[j] + 1);
                }
            }
            ans = max({ans, f[i], g[i]});
        }
        return ans;
    }
};
```

#### Go

```go
func wiggleMaxLength(nums []int) int {
	n := len(nums)
	f := make([]int, n)
	g := make([]int, n)
	f[0], g[0] = 1, 1
	ans := 1
	for i := 1; i < n; i++ {
		for j := 0; j < i; j++ {
			if nums[j] < nums[i] {
				f[i] = max(f[i], g[j]+1)
			} else if nums[j] > nums[i] {
				g[i] = max(g[i], f[j]+1)
			}
		}
		ans = max(ans, max(f[i], g[i]))
	}
	return ans
}
```

#### TypeScript

```ts
function wiggleMaxLength(nums: number[]): number {
    const n = nums.length;
    const f: number[] = Array(n).fill(1);
    const g: number[] = Array(n).fill(1);
    let ans = 1;
    for (let i = 1; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (nums[i] > nums[j]) {
                f[i] = Math.max(f[i], g[j] + 1);
            } else if (nums[i] < nums[j]) {
                g[i] = Math.max(g[i], f[j] + 1);
            }
        }
        ans = Math.max(ans, f[i], g[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
