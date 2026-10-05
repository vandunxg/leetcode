---
comments: true
difficulty: Easy
rating: 1228
source: Weekly Contest 470 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3701. Compute Alternating Sum](https://leetcode.com/problems/compute-alternating-sum)

[中文文档](/solution/3700-3799/3701.Compute%20Alternating%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p><strong>Tổng xen kẽ</strong> của <code>nums</code> là giá trị thu được bằng cách <strong>cộng</strong> các phần tử ở chỉ số chẵn và <strong>trừ</strong> các phần tử ở chỉ số lẻ. Cụ thể, <code>nums[0] - nums[1] + nums[2] - nums[3]...</code></p>

<p>Trả về một số nguyên biểu thị tổng xen kẽ của <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,5,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các phần tử ở chỉ số chẵn là <code>nums[0] = 1</code> và <code>nums[2] = 5</code> vì 0 và 2 là các số chẵn.</li>
	<li>Các phần tử ở chỉ số lẻ là <code>nums[1] = 3</code> và <code>nums[3] = 7</code> vì 1 và 3 là các số lẻ.</li>
	<li>Tổng xen kẽ là <code>nums[0] - nums[1] + nums[2] - nums[3] = 1 - 3 + 5 - 7 = -4</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">100</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phần tử duy nhất ở chỉ số chẵn là <code>nums[0] = 100</code> vì 0 là một số chẵn.</li>
	<li>Không có phần tử nào ở chỉ số lẻ.</li>
	<li>Tổng xen kẽ là <code>nums[0] = 100</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mảng có độ dài tối đa là $100$, nên chỉ cần duyệt một lần theo đúng định nghĩa. Tổng xen kẽ là tổng các phần tử ở chỉ số chẵn trừ đi tổng các phần tử ở chỉ số lẻ; hai lát cắt này tính trực tiếp hai tổng đó.

<!-- thinking:end -->

Ta có thể duyệt trực tiếp qua mảng $\textit{nums}$. Với mỗi chỉ số $i$, nếu $i$ là số chẵn, ta cộng $\textit{nums}[i]$ vào đáp án; ngược lại, ta trừ $\textit{nums}[i]$ khỏi đáp án.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def alternatingSum(self, nums: List[int]) -> int:
        return sum(nums[0::2]) - sum(nums[1::2])
```

#### Java

```java
class Solution {
    public int alternatingSum(int[] nums) {
        int ans = 0;
        for (int i = 0; i < nums.length; ++i) {
            ans += (i % 2 == 0 ? nums[i] : -nums[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int alternatingSum(vector<int>& nums) {
        int ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            ans += (i % 2 == 0 ? nums[i] : -nums[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func alternatingSum(nums []int) (ans int) {
	for i, x := range nums {
		if i%2 == 0 {
			ans += x
		} else {
			ans -= x
		}
	}
	return
}
```

#### TypeScript

```ts
function alternatingSum(nums: number[]): number {
    let ans: number = 0;
    for (let i = 0; i < nums.length; ++i) {
        ans += i % 2 === 0 ? nums[i] : -nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
