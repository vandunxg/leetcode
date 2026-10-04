---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [3173. Bitwise OR of Adjacent Elements 🔒](https://leetcode.com/problems/bitwise-or-of-adjacent-elements)

[中文文档](/solution/3100-3199/3173.Bitwise%20OR%20of%20Adjacent%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> có độ dài <code>n</code>, hãy trả về một mảng <code>answer</code> có độ dài <code>n - 1</code> sao cho <code>answer[i] = nums[i] | nums[i + 1]</code>, trong đó <code>|</code> là phép <code>OR</code> bitwise.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,7,15]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,7,15]</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,4,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[12,6]</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,4,9,11]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,13,11]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i]&nbsp;&lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Phần tử ở chỉ số $i$ là $nums[i]\lor nums[i+1]$ và không phụ thuộc vào các phần tử ở xa hơn.
>
> Mỗi cặp phần tử liền kề có thể được tính độc lập.
>
> Áp dụng phép OR cho từng cặp trong `pairwise(nums)` để thu được một mảng có độ dài $n-1$.

<!-- thinking:end -->

Ta duyệt qua $n - 1$ phần tử đầu tiên của mảng. Với mỗi phần tử, ta tính giá trị OR bitwise của nó và phần tử tiếp theo, rồi lưu kết quả vào mảng answer.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Không tính phần không gian của mảng answer, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def orArray(self, nums: List[int]) -> List[int]:
        return [a | b for a, b in pairwise(nums)]
```

#### Java

```java
class Solution {
    public int[] orArray(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n - 1];
        for (int i = 0; i < n - 1; ++i) {
            ans[i] = nums[i] | nums[i + 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> orArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n - 1);
        for (int i = 0; i < n - 1; ++i) {
            ans[i] = nums[i] | nums[i + 1];
        }
        return ans;
    }
};
```

#### Go

```go
func orArray(nums []int) (ans []int) {
	for i, x := range nums[1:] {
		ans = append(ans, x|nums[i])
	}
	return
}
```

#### TypeScript

```ts
function orArray(nums: number[]): number[] {
    return nums.slice(0, -1).map((v, i) => v | nums[i + 1]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
