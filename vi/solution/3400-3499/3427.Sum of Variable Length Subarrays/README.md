---
comments: true
difficulty: Easy
rating: 1215
source: Weekly Contest 433 Q1
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3427. Sum of Variable Length Subarrays](https://leetcode.com/problems/sum-of-variable-length-subarrays)

[中文文档](/solution/3400-3499/3427.Sum%20of%20Variable%20Length%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code>. Với <strong>mỗi</strong> chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt; n</code>, hãy định nghĩa một <span data-keyword="subarray-nonempty">mảng con</span> <code>nums[start ... i]</code>, trong đó <code>start = max(0, i - nums[i])</code>.</p>

<p>Trả về tổng tất cả phần tử trong mảng con được định nghĩa cho mỗi chỉ số của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Chỉ số</th>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Tổng</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>nums[0] = [2]</code></td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>nums[0 ... 1] = [2, 3]</code></td>
			<td style="border: 1px solid black;">5</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>nums[1 ... 2] = [3, 1]</code></td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><strong>Tổng cộng</strong></td>
			<td style="border: 1px solid black;">&nbsp;</td>
			<td style="border: 1px solid black;">11</td>
		</tr>
	</tbody>
</table>

<p>Tổng là 11, nên kết quả là 11.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Chỉ số</th>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Tổng</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>nums[0] = [3]</code></td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>nums[0 ... 1] = [3, 1]</code></td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>nums[1 ... 2] = [1, 1]</code></td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;"><code>nums[1 ... 3] = [1, 1, 2]</code></td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><strong>Tổng cộng</strong></td>
			<td style="border: 1px solid black;">&nbsp;</td>
			<td style="border: 1px solid black;">13</td>
		</tr>
	</tbody>
</table>

<p>Tổng là 13, nên kết quả là 13.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với chỉ số $i$, phần đóng góp là mảng con $\textit{nums}[\max(0,i-\textit{nums}[i]):i]$. Vì $n\le 100$, ta có thể dùng hai vòng lặp lồng nhau.
>
> Mỗi truy vấn là hiệu của hai tổng tiền tố. Nếu cộng trực tiếp, ta sẽ tính lại cùng một tiền tố nhiều lần.
>
> Với $s[i]=\textit{nums}[0]+\cdots+\textit{nums}[i-1]$, phần tử thứ $i$ là $s[i+1]-s[\max(0,i-x)]$. Tổng các giá trị này chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarraySum(self, nums: List[int]) -> int:
        s = list(accumulate(nums, initial=0))
        return sum(s[i + 1] - s[max(0, i - x)] for i, x in enumerate(nums))
```

#### Java

```java
class Solution {
    public int subarraySum(int[] nums) {
        int n = nums.length;
        int[] s = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += s[i + 1] - s[Math.max(0, i - nums[i])];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += s[i + 1] - s[max(0, i - nums[i])];
        }
        return ans;
    }
};
```

#### Go

```go
func subarraySum(nums []int) (ans int) {
	s := make([]int, len(nums)+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	for i, x := range nums {
		ans += s[i+1] - s[max(0, i-x)]
	}
	return
}
```

#### TypeScript

```ts
function subarraySum(nums: number[]): number {
    const n = nums.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans += s[i + 1] - s[Math.max(0, i - nums[i])];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
