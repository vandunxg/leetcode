---
comments: true
difficulty: Easy
rating: 1388
source: Weekly Contest 369 Q1
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [2917. Find the K-or of an Array](https://leetcode.com/problems/find-the-k-or-of-an-array)

[中文文档](/solution/2900-2999/2917.Find%20the%20K-or%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Hãy định nghĩa phép toán <strong>K-or</strong> bằng cách mở rộng phép OR theo bit tiêu chuẩn. Trong K-or, một vị trí bit trong kết quả được đặt thành <code>1</code> nếu có ít nhất <code>k</code> số trong <code>nums</code> có bit đó bằng <code>1</code>.</p>

<p>Trả về <em>K-or của</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào:</strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> nums = [7,12,9,8,9,15], k = 4 </span></p>

<p><strong>Đầu ra:</strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 9 </span></p>

<p><strong>Giải thích: </strong></p>

<p>Biểu diễn các số ở dạng nhị phân:</p>

<table style="text-indent:10px; margin-bottom=20px;">
	<tbody>
		<tr>
			<th><b>Số</b></th>
			<th>Bit 3</th>
			<th>Bit 2</th>
			<th>Bit 1</th>
			<th>Bit 0</th>
		</tr>
		<tr>
			<td><b>7</b></td>
			<td>0</td>
			<td>1</td>
			<td>1</td>
			<td>1</td>
		</tr>
		<tr>
			<td><b>12</b></td>
			<td>1</td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td><b>9</b></td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>1</td>
		</tr>
		<tr>
			<td><b>8</b></td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td><b>9</b></td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>1</td>
		</tr>
		<tr>
			<td><b>15</b></td>
			<td>1</td>
			<td>1</td>
			<td>1</td>
			<td>1</td>
		</tr>
		<tr>
			<td><b>Kết quả = 9</b></td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>1</td>
		</tr>
	</tbody>
</table>

<p>Bit 0 bằng 1 trong các số 7, 9, 9 và 15. Bit 3 bằng 1 trong các số 12, 9, 8, 9 và 15.<br />
Chỉ bit 0 và bit 3 thỏa mãn. Kết quả là <code>(1001)<sub>2</sub> = 9</code>.</p>
</div>

<p><strong class="example">Ví dụ 2: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào:</strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> nums = [2,12,1,11,4,5], k = 6 </span></p>

<p><strong>Đầu ra:</strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 0 </span></p>

<p><strong>Giải thích:&nbsp;</strong>Không có bit nào bằng 1 trong cả sáu số của mảng, như yêu cầu của K-or với <code>k = 6</code>. Do đó, kết quả là 0.</p>
</div>

<p><strong class="example">Ví dụ 3: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào:</strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> nums = [10,8,5,9,11,6,8], k = 1 </span></p>

<p><strong>Đầu ra:</strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 15 </span></p>

<p><strong>Giải thích: </strong> Vì <code>k == 1</code>, 1-or của mảng bằng phép OR theo bit của tất cả phần tử trong mảng. Do đó, đáp án là <code>10 OR 8 OR 5 OR 9 OR 11 OR 6 OR 8 = 15</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>0 &lt;= nums[i] &lt; 2<sup>31</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Bit $i$ của K-or bằng $1$ khi và chỉ khi có ít nhất $k$ số có bit đó bằng 1; các bit độc lập với nhau. Vì các giá trị nhỏ hơn $2^{31}$, chỉ cần xét $32$ bit.
>
> Đếm số bit 1 ở mỗi vị trí, rồi OR $2^i$ vào đáp án nếu số đếm ít nhất là $k$. Với $n \le 50$, không cần cấu trúc dữ liệu bổ sung.

<!-- thinking:end -->

Ta có thể duyệt từng bit $i$ trong phạm vi $[0, 32)$ và đếm số phần tử trong mảng $nums$ có bit thứ $i$ bằng $1$, gọi là $cnt$. Nếu $cnt \ge k$, ta cộng $2^i$ vào đáp án.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài của mảng $nums$ và giá trị lớn nhất trong $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKOr(self, nums: List[int], k: int) -> int:
        ans = 0
        for i in range(32):
            cnt = sum(x >> i & 1 for x in nums)
            if cnt >= k:
                ans |= 1 << i
        return ans
```

#### Java

```java
class Solution {
    public int findKOr(int[] nums, int k) {
        int ans = 0;
        for (int i = 0; i < 32; ++i) {
            int cnt = 0;
            for (int x : nums) {
                cnt += (x >> i & 1);
            }
            if (cnt >= k) {
                ans |= 1 << i;
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
    int findKOr(vector<int>& nums, int k) {
        int ans = 0;
        for (int i = 0; i < 32; ++i) {
            int cnt = 0;
            for (int x : nums) {
                cnt += (x >> i & 1);
            }
            if (cnt >= k) {
                ans |= 1 << i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findKOr(nums []int, k int) (ans int) {
	for i := 0; i < 32; i++ {
		cnt := 0
		for _, x := range nums {
			cnt += (x >> i & 1)
		}
		if cnt >= k {
			ans |= 1 << i
		}
	}
	return
}
```

#### TypeScript

```ts
function findKOr(nums: number[], k: number): number {
    let ans = 0;
    for (let i = 0; i < 32; ++i) {
        let cnt = 0;
        for (const x of nums) {
            cnt += (x >> i) & 1;
        }
        if (cnt >= k) {
            ans |= 1 << i;
        }
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public int FindKOr(int[] nums, int k) {
        int ans = 0;
        for (int i = 0; i < 32; ++i) {
            int cnt = 0;
            foreach (int x in nums) {
                cnt += (x >> i & 1);
            }
            if (cnt >= k) {
                ans |= 1 << i;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
