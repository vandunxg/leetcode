---
comments: true
difficulty: Easy
rating: 1204
source: Weekly Contest 468 Q1
tags:
    - Bit Manipulation
    - Array
    - Simulation
---

<!-- problem:start -->

# [3688. Bitwise OR of Even Numbers in an Array](https://leetcode.com/problems/bitwise-or-of-even-numbers-in-an-array)

[中文文档](/solution/3600-3699/3688.Bitwise%20OR%20of%20Even%20Numbers%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Trả về kết quả <strong>OR bitwise</strong> của tất cả các số <strong>chẵn</strong> trong mảng.</p>

<p>Nếu trong <code>nums</code> không có số chẵn nào, hãy trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số chẵn là 2, 4 và 6. Phép OR bitwise của chúng bằng 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,9,11]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số chẵn nào, nên kết quả là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,8,16]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">24</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số chẵn là 8 và 16. Phép OR bitwise của chúng bằng 24.</p>
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
> Thực hiện phép OR bitwise trên các số chẵn. Nếu không có số chẵn nào, đáp án là $0$, phần tử đơn vị của phép OR.
>
> Lọc các số chẵn rồi thực hiện $\textit{reduce}$ với giá trị ban đầu là $0$. Vì $n\le 100$, chỉ cần duyệt một lần.

<!-- thinking:end -->

Ta định nghĩa một biến $\textit{ans}$ với giá trị ban đầu là 0. Sau đó, ta duyệt qua từng phần tử $x$ trong mảng $\textit{nums}$; nếu $x$ là số chẵn, ta cập nhật $\textit{ans}$ bằng phép OR bitwise của $\textit{ans}$ và $x$.

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def evenNumberBitwiseORs(self, nums: List[int]) -> int:
        return reduce(or_, (x for x in nums if x % 2 == 0), 0)
```

#### Java

```java
class Solution {
    public int evenNumberBitwiseORs(int[] nums) {
        int ans = 0;
        for (int x : nums) {
            if (x % 2 == 0) {
                ans |= x;
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
    int evenNumberBitwiseORs(vector<int>& nums) {
        int ans = 0;
        for (int x : nums) {
            if (x % 2 == 0) {
                ans |= x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func evenNumberBitwiseORs(nums []int) (ans int) {
	for _, x := range nums {
		if x%2 == 0 {
			ans |= x
		}
	}
	return
}
```

#### TypeScript

```ts
function evenNumberBitwiseORs(nums: number[]): number {
    return nums.reduce((ans, x) => (x % 2 === 0 ? ans | x : ans), 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
