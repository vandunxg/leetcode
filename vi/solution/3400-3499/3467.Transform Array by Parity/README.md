---
comments: true
difficulty: Easy
rating: 1165
source: Biweekly Contest 151 Q1
tags:
    - Array
    - Counting
    - Sorting
---

<!-- problem:start -->

# [3467. Transform Array by Parity](https://leetcode.com/problems/transform-array-by-parity)

[中文文档](/solution/3400-3499/3467.Transform%20Array%20by%20Parity/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code>. Hãy biến đổi <code>nums</code> bằng cách thực hiện các thao tác sau theo <strong>đúng</strong> thứ tự được chỉ định:</p>

<ol>
	<li>Thay mỗi số chẵn bằng 0.</li>
	<li>Thay mỗi số lẻ bằng 1.</li>
	<li>Sắp xếp mảng đã biến đổi theo thứ tự <strong>không giảm</strong>.</li>
</ol>

<p>Trả về mảng kết quả sau khi thực hiện các thao tác này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay các số chẵn (4 và 2) bằng 0 và các số lẻ (3 và 1) bằng 1. Khi đó, <code>nums = [0, 1, 0, 1]</code>.</li>
	<li>Sau khi sắp xếp <code>nums</code> theo thứ tự không giảm, <code>nums = [0, 0, 1, 1]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,5,1,4,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0,1,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay các số chẵn (4 và 2) bằng 0 và các số lẻ (1, 5 và 1) bằng 1. Khi đó, <code>nums = [1, 1, 1, 0, 0]</code>.</li>
	<li>Sau khi sắp xếp <code>nums</code> theo thứ tự không giảm, <code>nums = [0, 0, 1, 1, 1]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Các số chẵn trở thành $0$, các số lẻ trở thành $1$, rồi mảng được sắp xếp theo thứ tự không giảm. Điều đó có nghĩa là tất cả số 0 nằm ở phía trước. Vì $n\le 100$, chỉ cần đếm là đủ.
>
> Không cần sắp xếp: số lượng số chẵn $\textit{even}$ chính là độ dài của tiền tố gồm các số 0.
>
> Đếm số chẵn trong một lượt duyệt rồi ghi đè hai đoạn vào mảng.

<!-- thinking:end -->

Ta có thể duyệt qua mảng $\textit{nums}$ và đếm số phần tử chẵn $\textit{even}$. Sau đó, đặt $\textit{even}$ phần tử đầu tiên của mảng thành $0$ và các phần tử còn lại thành $1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def transformArray(self, nums: List[int]) -> List[int]:
        even = sum(x % 2 == 0 for x in nums)
        for i in range(even):
            nums[i] = 0
        for i in range(even, len(nums)):
            nums[i] = 1
        return nums
```

#### Java

```java
class Solution {
    public int[] transformArray(int[] nums) {
        int even = 0;
        for (int x : nums) {
            even += (x & 1 ^ 1);
        }
        for (int i = 0; i < even; ++i) {
            nums[i] = 0;
        }
        for (int i = even; i < nums.length; ++i) {
            nums[i] = 1;
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> transformArray(vector<int>& nums) {
        int even = 0;
        for (int x : nums) {
            even += (x & 1 ^ 1);
        }
        for (int i = 0; i < even; ++i) {
            nums[i] = 0;
        }
        for (int i = even; i < nums.size(); ++i) {
            nums[i] = 1;
        }
        return nums;
    }
};
```

#### Go

```go
func transformArray(nums []int) []int {
	even := 0
	for _, x := range nums {
		even += x&1 ^ 1
	}
	for i := 0; i < even; i++ {
		nums[i] = 0
	}
	for i := even; i < len(nums); i++ {
		nums[i] = 1
	}
	return nums
}
```

#### TypeScript

```ts
function transformArray(nums: number[]): number[] {
    const even = nums.filter(x => x % 2 === 0).length;
    for (let i = 0; i < even; ++i) {
        nums[i] = 0;
    }
    for (let i = even; i < nums.length; ++i) {
        nums[i] = 1;
    }
    return nums;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
