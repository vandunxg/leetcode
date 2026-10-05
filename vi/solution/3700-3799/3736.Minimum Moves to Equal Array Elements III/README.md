---
comments: true
difficulty: Easy
rating: 1251
source: Biweekly Contest 169 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3736. Minimum Moves to Equal Array Elements III](https://leetcode.com/problems/minimum-moves-to-equal-array-elements-iii)

[中文文档](/solution/3700-3799/3736.Minimum%20Moves%20to%20Equal%20Array%20Elements%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Trong một bước, bạn có thể <strong>tăng</strong> giá trị của bất kỳ phần tử đơn lẻ nào <code>nums[i]</code> thêm 1.</p>

<p>Hãy trả về <strong>tổng số</strong> <strong>bước</strong> tối thiểu cần thiết để tất cả phần tử trong <code>nums</code> trở nên <strong>bằng nhau</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Để làm cho tất cả phần tử bằng nhau:</p>

<ul>
	<li>Tăng <code>nums[0] = 2</code> thêm 1 để được 3.</li>
	<li>Tăng <code>nums[1] = 1</code> thêm 1 để được 2.</li>
	<li>Tăng <code>nums[1] = 2</code> thêm 1 để được 3.</li>
</ul>

<p>Khi đó, tất cả phần tử của <code>nums</code> đều bằng 3. Tổng số bước tối thiểu là <code>3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Để làm cho tất cả phần tử bằng nhau:</p>

<ul>
	<li>Tăng <code>nums[0] = 4</code> thêm 1 để được 5.</li>
	<li>Tăng <code>nums[1] = 4</code> thêm 1 để được 5.</li>
</ul>

<p>Khi đó, tất cả phần tử của <code>nums</code> đều bằng 5. Tổng số bước tối thiểu là <code>2</code>.</p>
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

### Lời giải 1: Tính tổng và giá trị lớn nhất

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước tăng một phần tử thêm $1$, nên giá trị chung cuối cùng không thể nhỏ hơn giá trị lớn nhất hiện tại. Vì vậy, đích đến là giá trị lớn nhất, và tổng số lần tăng là $\textit{mx}\cdot n-s$.

<!-- thinking:end -->

Bài toán yêu cầu làm cho tất cả phần tử trong mảng bằng nhau, trong đó mỗi thao tác chỉ có thể tăng một phần tử lên 1. Để số thao tác là ít nhất, ta nên đưa tất cả phần tử về giá trị lớn nhất trong mảng.

Do đó, trước tiên ta có thể tính giá trị lớn nhất $\textit{mx}$ và tổng các phần tử trong mảng $\textit{s}$. Số thao tác cần thiết để đưa tất cả phần tử về $\textit{mx}$ là $\textit{mx} \times n - \textit{s}$, trong đó $n$ là độ dài của mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, nums: List[int]) -> int:
        n = len(nums)
        mx = max(nums)
        s = sum(nums)
        return mx * n - s
```

#### Java

```java
class Solution {
    public int minMoves(int[] nums) {
        int n = nums.length;
        int mx = 0;
        int s = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
            s += x;
        }
        return mx * n - s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(vector<int>& nums) {
        int n = nums.size();
        int mx = 0;
        int s = 0;
        for (int x : nums) {
            mx = max(mx, x);
            s += x;
        }
        return mx * n - s;
    }
};
```

#### Go

```go
func minMoves(nums []int) int {
	mx, s := 0, 0
	for _, x := range nums {
		mx = max(mx, x)
		s += x
	}
	return mx*len(nums) - s
}
```

#### TypeScript

```ts
function minMoves(nums: number[]): number {
    const n = nums.length;
    const mx = Math.max(...nums);
    const s = nums.reduce((a, b) => a + b, 0);
    return mx * n - s;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
