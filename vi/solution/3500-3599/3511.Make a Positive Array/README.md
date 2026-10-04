---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3511. Make a Positive Array 🔒](https://leetcode.com/problems/make-a-positive-array)

[中文文档](/solution/3500-3599/3511.Make%20a%20Positive%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code>. Một mảng được xem là <strong>dương</strong> nếu tổng của tất cả các phần tử trong mỗi <strong><span data-keyword="subarray">mảng con</span></strong> có <strong>nhiều hơn hai</strong> phần tử là số dương.</p>

<p>Bạn có thể thực hiện thao tác sau một số lần bất kỳ:</p>

<ul>
	<li>Thay thế <strong>một</strong> phần tử trong <code>nums</code> bằng một số nguyên bất kỳ trong khoảng từ -10<sup>18</sup> đến 10<sup>18</sup>.</li>
</ul>

<p>Hãy tìm số thao tác <strong>tối thiểu</strong> cần thực hiện để biến <code>nums</code> thành một mảng <strong>dương</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-10,15,-12]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một mảng con có nhiều hơn 2 phần tử, đó là chính mảng ban đầu. Tổng tất cả phần tử là <code>(-10) + 15 + (-12) = -7</code>. Bằng cách thay <code>nums[0]</code> bằng 0, tổng mới trở thành <code>0 + 15 + (-12) = 3</code>. Vì vậy, mảng hiện đã là mảng dương.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,-2,3,-1,2,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con duy nhất có nhiều hơn 2 phần tử và tổng không dương là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Chỉ số mảng con</th>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Tổng</th>
			<th style="border: 1px solid black;">Mảng con sau khi thay thế (Đặt nums[1] = 1)</th>
			<th style="border: 1px solid black;">Tổng mới</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">nums[0...2]</td>
			<td style="border: 1px solid black;">[-1, -2, 3]</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[-1, 1, 3]</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">nums[0...3]</td>
			<td style="border: 1px solid black;">[-1, -2, 3, -1]</td>
			<td style="border: 1px solid black;">-1</td>
			<td style="border: 1px solid black;">[-1, 1, 3, -1]</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">nums[1...3]</td>
			<td style="border: 1px solid black;">[-2, 3, -1]</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[1, 3, -1]</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>nums</code> trở thành mảng dương sau một thao tác.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng đã là mảng dương, nên không cần thực hiện thao tác nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra mọi mảng con có độ dài ít nhất $3$ sẽ tốn $O(n^2)$, trong khi $n \le 10^5$. Việc thay một giá trị bằng một số nguyên bất kỳ sẽ ngắt ràng buộc trên các prefix tại vị trí đó.
>
> Duyệt các prefix sum từ trái sang phải, đồng thời giữ vị trí bắt đầu cửa sổ và $\textit{pre\_mx}$, là prefix sum lớn nhất của một đoạn có độ dài ít nhất $2$ trong cửa sổ. Nếu prefix hiện tại không lớn hơn $\textit{pre\_mx}$, một mảng con có tổng không dương đã xuất hiện: tăng số lần thay thế lên một và đặt lại cửa sổ. Cắt tại vị trí xung đột giúp số thao tác là nhỏ nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeArrayPositive(self, nums: List[int]) -> int:
        l = -1
        ans = pre_mx = s = 0
        for r, x in enumerate(nums):
            s += x
            if r - l > 2 and s <= pre_mx:
                ans += 1
                l = r
                pre_mx = s = 0
            elif r - l >= 2:
                pre_mx = max(pre_mx, s - x - nums[r - 1])
        return ans
```

#### Java

```java
class Solution {
    public int makeArrayPositive(int[] nums) {
        int ans = 0;
        long preMx = 0, s = 0;
        for (int l = -1, r = 0; r < nums.length; r++) {
            int x = nums[r];
            s += x;
            if (r - l > 2 && s <= preMx) {
                ans++;
                l = r;
                preMx = s = 0;
            } else if (r - l >= 2) {
                preMx = Math.max(preMx, s - x - nums[r - 1]);
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
    int makeArrayPositive(vector<int>& nums) {
        int ans = 0;
        long long preMx = 0, s = 0;
        for (int l = -1, r = 0; r < nums.size(); r++) {
            int x = nums[r];
            s += x;
            if (r - l > 2 && s <= preMx) {
                ans++;
                l = r;
                preMx = s = 0;
            } else if (r - l >= 2) {
                preMx = max(preMx, s - x - nums[r - 1]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func makeArrayPositive(nums []int) (ans int) {
	l := -1
	preMx := 0
	s := 0
	for r, x := range nums {
		s += x
		if r-l > 2 && s <= preMx {
			ans++
			l = r
			preMx = 0
			s = 0
		} else if r-l >= 2 {
			preMx = max(preMx, s-x-nums[r-1])
		}
	}
	return
}
```

#### TypeScript

```ts
function makeArrayPositive(nums: number[]): number {
    let l = -1;
    let [ans, preMx, s] = [0, 0, 0];
    for (let r = 0; r < nums.length; r++) {
        const x = nums[r];
        s += x;
        if (r - l > 2 && s <= preMx) {
            ans++;
            l = r;
            preMx = 0;
            s = 0;
        } else if (r - l >= 2) {
            preMx = Math.max(preMx, s - x - nums[r - 1]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
