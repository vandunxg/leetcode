---
comments: true
difficulty: Easy
rating: 1200
source: Weekly Contest 509 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3982. Sum of Integers with Maximum Digit Range](https://leetcode.com/problems/sum-of-integers-with-maximum-digit-range)

[中文文档](/solution/3900-3999/3982.Sum%20of%20Integers%20with%20Maximum%20Digit%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p><strong>Khoảng chữ số</strong> của một số nguyên được định nghĩa là hiệu giữa chữ số <strong>lớn nhất</strong> và chữ số <strong>nhỏ nhất</strong> của nó.</p>

<p>Ví dụ, khoảng chữ số của 5724 là <code>7 - 2 = 5</code>.</p>

<p>Trả về tổng tất cả các số nguyên trong <code>nums</code> có <strong>khoảng chữ số</strong> bằng <strong>khoảng chữ số lớn nhất</strong> trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5724,111,350]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6074</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<tbody>
		<tr>
			<th style="text-align:center;"><code>i</code></th>
			<th style="text-align:center;"><code>nums[i]</code></th>
			<th style="text-align:center;">Lớn nhất</th>
			<th style="text-align:center;">Nhỏ nhất</th>
			<th style="text-align:center;">Khoảng chữ số</th>
		</tr>
		<tr>
			<td style="text-align:center;">0</td>
			<td style="text-align:center;">5724</td>
			<td style="text-align:center;">7</td>
			<td style="text-align:center;">2</td>
			<td style="text-align:center;">5</td>
		</tr>
		<tr>
			<td style="text-align:center;">1</td>
			<td style="text-align:center;">111</td>
			<td style="text-align:center;">1</td>
			<td style="text-align:center;">1</td>
			<td style="text-align:center;">0</td>
		</tr>
		<tr>
			<td style="text-align:center;">2</td>
			<td style="text-align:center;">350</td>
			<td style="text-align:center;">5</td>
			<td style="text-align:center;">0</td>
			<td style="text-align:center;">5</td>
		</tr>
	</tbody>
</table>

<p>Khoảng chữ số lớn nhất là 5. Các số nguyên có khoảng chữ số này là 5724 và 350, nên đáp án là <code>5724 + 350 = 6074</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [90,900]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">990</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<tbody>
		<tr>
			<th style="text-align:center;"><code>i</code></th>
			<th style="text-align:center;"><code>nums[i]</code></th>
			<th style="text-align:center;">Lớn nhất</th>
			<th style="text-align:center;">Nhỏ nhất</th>
			<th style="text-align:center;">Khoảng chữ số</th>
		</tr>
		<tr>
			<td style="text-align:center;">0</td>
			<td style="text-align:center;">90</td>
			<td style="text-align:center;">9</td>
			<td style="text-align:center;">0</td>
			<td style="text-align:center;">9</td>
		</tr>
		<tr>
			<td style="text-align:center;">1</td>
			<td style="text-align:center;">900</td>
			<td style="text-align:center;">9</td>
			<td style="text-align:center;">0</td>
			<td style="text-align:center;">9</td>
		</tr>
	</tbody>
</table>

<p>Khoảng chữ số lớn nhất là 9. Cả hai số nguyên đều có khoảng chữ số bằng 9, nên đáp án là <code>90 + 900 = 990</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>10 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng chữ số bằng chữ số lớn nhất trừ chữ số nhỏ nhất. Vì $n\le 100$, ta lần lượt tách từng chữ số, tính $b-a$ và duy trì khoảng tốt nhất $\textit{mx}$: nếu khoảng mới lớn hơn thì đặt lại đáp án, nếu bằng thì cộng thêm số đó.
>
> Chỉ cần duyệt một lần.

<!-- thinking:end -->

Ta duyệt mảng $\textit{nums}$. Với mỗi số nguyên $x$, ta tách các chữ số để tìm chữ số lớn nhất $b$ và chữ số nhỏ nhất $a$, sau đó tính khoảng chữ số $r = b - a$. Nếu $r$ lớn hơn khoảng chữ số lớn nhất hiện tại $\textit{mx}$, ta cập nhật $\textit{mx} = r$ và đặt lại đáp án bằng $x$; nếu $r$ bằng $\textit{mx}$, ta cộng $x$ vào đáp án.

Độ phức tạp thời gian là $O(n \log M)$, độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $\textit{nums}$ và $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDigitRange(self, nums: list[int]) -> int:
        ans = mx = 0
        for x in nums:
            a, b = 10, 0
            y = x
            while y:
                v = y % 10
                y //= 10
                a = min(a, v)
                b = max(b, v)
            r = b - a
            if mx < r:
                mx = r
                ans = x
            elif mx == r:
                ans += x
        return ans
```

#### Java

```java
class Solution {
    public int maxDigitRange(int[] nums) {
        int ans = 0, mx = 0;
        for (int x : nums) {
            int a = 10, b = 0;
            for (int y = x; y > 0; y /= 10) {
                int v = y % 10;
                a = Math.min(a, v);
                b = Math.max(b, v);
            }
            int r = b - a;
            if (mx < r) {
                mx = r;
                ans = x;
            } else if (mx == r) {
                ans += x;
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
    int maxDigitRange(vector<int>& nums) {
        int ans = 0, mx = 0;
        for (int x : nums) {
            int a = 10, b = 0;
            for (int y = x; y > 0; y /= 10) {
                int v = y % 10;
                a = min(a, v);
                b = max(b, v);
            }
            int r = b - a;
            if (mx < r) {
                mx = r;
                ans = x;
            } else if (mx == r) {
                ans += x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxDigitRange(nums []int) (ans int) {
	mx := 0
	for _, x := range nums {
		a, b := 10, 0
		for y := x; y > 0; y /= 10 {
			v := y % 10
			a = min(a, v)
			b = max(b, v)
		}
		r := b - a
		if mx < r {
			mx = r
			ans = x
		} else if mx == r {
			ans += x
		}
	}
	return
}
```

#### TypeScript

```ts
function maxDigitRange(nums: number[]): number {
    let [ans, mx] = [0, 0];
    for (const x of nums) {
        let [a, b] = [10, 0];
        for (let y = x; y; y = (y / 10) | 0) {
            const v = y % 10;
            a = Math.min(a, v);
            b = Math.max(b, v);
        }
        const r = b - a;
        if (mx < r) {
            mx = r;
            ans = x;
        } else if (mx == r) {
            ans += x;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
