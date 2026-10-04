---
comments: true
difficulty: Easy
rating: 1369
source: Weekly Contest 466 Q1
tags:
    - Bit Manipulation
    - Brainteaser
    - Array
---

<!-- problem:start -->

# [3674. Minimum Operations to Equalize Array](https://leetcode.com/problems/minimum-operations-to-equalize-array)

[中文文档](/solution/3600-3699/3674.Minimum%20Operations%20to%20Equalize%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Trong một phép toán, chọn một mảng con bất kỳ <code>nums[l...r]</code> (<code>0 &lt;= l &lt;= r &lt; n</code>) và <strong>thay thế</strong> mỗi phần tử trong mảng con đó bằng <strong>phép AND bit</strong> của tất cả các phần tử.</p>

<p>Trả về số phép toán <strong>ít nhất</strong> cần thực hiện để tất cả phần tử của <code>nums</code> bằng nhau.</p>
Một <strong>mảng con</strong> là một dãy phần tử liên tiếp <b>không rỗng</b> trong một mảng.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code>nums[0...1]</code>: <code>(1 AND 2) = 0</code>, nên mảng trở thành <code>[0, 0]</code> và tất cả phần tử bằng nhau sau 1 phép toán.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> là <code>[5, 5, 5]</code>, vốn đã có tất cả phần tử bằng nhau, nên cần 0 phép toán.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Một phép toán thay thế một mảng con bằng $\gcd$ của nó. Nếu mọi phần tử vốn đã bằng nhau thì không cần thực hiện phép toán nào; nếu không, thực hiện một phép toán trên toàn bộ mảng sẽ làm chúng bằng nhau.
>
> Vì vậy, đáp án chỉ có thể là $0$ hoặc $1$. Chỉ cần duyệt qua mảng để tìm một giá trị khác phần tử đầu tiên là có thể quyết định đáp án.
>
> $n\le 100$ nên chỉ cần một lần duyệt.

<!-- thinking:end -->

Nếu tất cả phần tử trong $\textit{nums}$ đều bằng nhau thì không cần phép toán nào; ngược lại, ta có thể chọn toàn bộ mảng làm mảng con và thực hiện một phép toán.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        return int(any(x != nums[0] for x in nums))
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        for (int x : nums) {
            if (x != nums[0]) {
                return 1;
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        for (int x : nums) {
            if (x != nums[0]) {
                return 1;
            }
        }
        return 0;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	for _, x := range nums {
		if x != nums[0] {
			return 1
		}
	}
	return 0
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    for (const x of nums) {
        if (x !== nums[0]) {
            return 1;
        }
    }
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
