---
comments: true
difficulty: Medium
rating: 1391
source: Weekly Contest 513 Q2
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Divide and Conquer
    - Prefix Sum
    - Merge Sort
---

<!-- problem:start -->

# [4011. Count Subarrays With Even Odd Ratio I](https://leetcode.com/problems/count-subarrays-with-even-odd-ratio-i)

[中文文档](/solution/4000-4099/4011.Count%20Subarrays%20With%20Even%20Odd%20Ratio%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>a</code> và <code>b</code>.</p>

<p>Với một <span data-keyword="subarray-nonempty">mảng con</span>, gọi:</p>

<ul>
	<li><code>x</code> là số phần tử chẵn.</li>
	<li><code>y</code> là số phần tử lẻ.</li>
</ul>

<p>Tỷ lệ giữa số phần tử chẵn và số phần tử lẻ trong một mảng con được định nghĩa là <code>x / y</code>, trong đó các tỷ lệ được so sánh theo giá trị hữu tỉ chính xác.</p>

<p>Một mảng con được xem là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li><code>y &gt; 0</code>, và</li>
	<li><code>x / y &lt;= a / b</code>.</li>
</ul>

<p>Trả về số lượng mảng con hợp lệ trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2], a = 3, b = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Giá trị</th>
			<th style="border: 1px solid black;">Số phần tử chẵn</th>
			<th style="border: 1px solid black;">Số phần tử lẻ</th>
			<th style="border: 1px solid black;">Tỷ lệ</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..0]</code></td>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>0 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..1]</code></td>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..2]</code></td>
			<td style="border: 1px solid black;"><code>[1, 2, 1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>1 / 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..3]</code></td>
			<td style="border: 1px solid black;"><code>[1, 2, 1, 2]</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>2 / 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[1..2]</code></td>
			<td style="border: 1px solid black;"><code>[2, 1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[2..2]</code></td>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>0 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[2..3]</code></td>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 / 1</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, số lượng mảng con hợp lệ là 7.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,1], a = 2, b = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Giá trị</th>
			<th style="border: 1px solid black;">Số phần tử chẵn</th>
			<th style="border: 1px solid black;">Số phần tử lẻ</th>
			<th style="border: 1px solid black;">Tỷ lệ</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[0..2]</code></td>
			<td style="border: 1px solid black;"><code>[2, 2, 1]</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>2 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[1..2]</code></td>
			<td style="border: 1px solid black;"><code>[2, 1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 / 1</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>nums[2..2]</code></td>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>0 / 1</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, số lượng mảng con hợp lệ là 3.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,2], a = 1, b = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi mảng con đều chứa 0 số lẻ, nên không có mảng con nào hợp lệ.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= a, b &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê các mảng con

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 1000$, có $O(n^2)$ mảng con, nên việc liệt kê cả hai đầu mảng là khả thi.
>
> Khi cố định đầu trái, ta mở rộng đầu phải và đếm số phần tử lẻ $y$; số phần tử chẵn bằng độ dài trừ đi $y$. Điều kiện kiểm tra $\frac{x}{y}\le\frac{a}{b}$ chỉ có ý nghĩa khi $y>0$, tương ứng với phép so sánh số thực trong code, còn các mảng con có $y=0$ sẽ được bỏ qua.
>
> Không cần dùng prefix sum hay Fenwick tree; vòng lặp kép đã đếm được mọi mảng con hợp lệ.

<!-- thinking:end -->

Ta liệt kê chỉ số đầu trái $i$ của mảng con, sau đó mở rộng chỉ số đầu phải $j$ sang phải và duy trì số lượng số lẻ $y$ trong mảng con. Khi đó, số lượng số chẵn là $x = j - i + 1 - y$.

Nếu $y > 0$ và $\frac{x}{y} \le \frac{a}{b}$, mảng con là hợp lệ. Để tránh vấn đề sai số do phép tính số thực, ta có thể biến đổi điều kiện thành phép so sánh số nguyên tương đương $x \times b \le y \times a$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countRatioSubarrays(self, nums: list[int], a: int, b: int) -> int:
        ans = 0
        n = len(nums)
        for i in range(n):
            y = 0
            for j in range(i, n):
                y += nums[j] % 2
                x = j - i + 1 - y
                if y and (x / y) <= (a / b):
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countRatioSubarrays(int[] nums, int a, int b) {
        int n = nums.length;
        long ans = 0;

        for (int i = 0; i < n; i++) {
            int y = 0;

            for (int j = i; j < n; j++) {
                y += nums[j] % 2;
                int x = j - i + 1 - y;

                if (y > 0 && (long) x * b <= (long) y * a) {
                    ans++;
                }
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
    int countRatioSubarrays(vector<int>& nums, int a, int b) {
        int n = nums.size();
        long long ans = 0;

        for (int i = 0; i < n; i++) {
            int y = 0;

            for (int j = i; j < n; j++) {
                y += nums[j] % 2;
                int x = j - i + 1 - y;

                if (y > 0 && 1LL * x * b <= 1LL * y * a) {
                    ans++;
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func countRatioSubarrays(nums []int, a int, b int) int {
	n := len(nums)
	var ans int64 = 0

	for i := 0; i < n; i++ {
		y := 0

		for j := i; j < n; j++ {
			y += nums[j] % 2
			x := j - i + 1 - y

			if y > 0 && int64(x)*int64(b) <= int64(y)*int64(a) {
				ans++
			}
		}
	}

	return int(ans)
}
```

#### TypeScript

```ts
function countRatioSubarrays(nums: number[], a: number, b: number): number {
    const n = nums.length;
    let ans = 0;

    for (let i = 0; i < n; i++) {
        let y = 0;

        for (let j = i; j < n; j++) {
            y += nums[j] % 2;
            const x = j - i + 1 - y;

            if (y > 0 && x * b <= y * a) {
                ans++;
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
