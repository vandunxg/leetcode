---
comments: true
difficulty: Medium
rating: 1427
source: Weekly Contest 244 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [1887. Reduction Operations to Make the Array Elements Equal](https://leetcode.com/problems/reduction-operations-to-make-the-array-elements-equal)

[中文文档](/solution/1800-1899/1887.Reduction%20Operations%20to%20Make%20the%20Array%20Elements%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, mục tiêu của bạn là làm cho tất cả phần tử trong <code>nums</code> bằng nhau. Để thực hiện một thao tác, hãy làm theo các bước sau:</p>

<ol>
	<li>Tìm giá trị <strong>lớn nhất</strong> trong <code>nums</code>. Gọi chỉ số của nó là <code>i</code> (<strong>đánh chỉ số từ 0</strong>) và giá trị là <code>largest</code>. Nếu có nhiều phần tử cùng đạt giá trị lớn nhất, chọn <code>i</code> nhỏ nhất.</li>
	<li>Tìm giá trị <strong>lớn thứ hai</strong> trong <code>nums</code>, <strong>nhỏ hơn nghiêm ngặt</strong> <code>largest</code>. Gọi giá trị đó là <code>nextLargest</code>.</li>
	<li>Giảm <code>nums[i]</code> xuống <code>nextLargest</code>.</li>
</ol>

<p>Trả về <em>số thao tác để làm cho tất cả phần tử trong </em><code>nums</code><em> bằng nhau</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,1,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>&nbsp;Cần 3 thao tác để làm tất cả phần tử trong nums bằng nhau:
1. largest = 5 tại chỉ số 0. nextLargest = 3. Giảm nums[0] xuống 3. nums = [<u>3</u>,1,3].
2. largest = 3 tại chỉ số 0. nextLargest = 1. Giảm nums[0] xuống 1. nums = [<u>1</u>,1,3].
3. largest = 3 tại chỉ số 2. nextLargest = 1. Giảm nums[2] xuống 1. nums = [1,1,<u>1</u>].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>&nbsp;Tất cả phần tử trong nums đã bằng nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,2,3]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>&nbsp;Cần 4 thao tác để làm tất cả phần tử trong nums bằng nhau:
1. largest = 3 tại chỉ số 4. nextLargest = 2. Giảm nums[4] xuống 2. nums = [1,1,2,2,<u>2</u>].
2. largest = 2 tại chỉ số 2. nextLargest = 1. Giảm nums[2] xuống 1. nums = [1,1,<u>1</u>,2,2].
3. largest = 2 tại chỉ số 3. nextLargest = 1. Giảm nums[3] xuống 1. nums = [1,1,1,<u>1</u>,2].
4. largest = 2 tại chỉ số 4. nextLargest = 1. Giảm nums[4] xuống 1. nums = [1,1,1,1,<u>1</u>].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 5 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác thay giá trị lớn nhất hiện tại bằng giá trị nhỏ hơn gần nhất. Mô phỏng từng phần tử lớn nhất sẽ chậm.
>
> Sau khi sắp xếp, mỗi giá trị lớn hơn mới xuất hiện sẽ thêm một bước mà tất cả phần tử phía sau phải thực hiện. $cnt$ đếm số bước này; mỗi phần tử cộng thêm $cnt$ vào đáp án.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng $\textit{nums}$, sau đó duyệt từ phần tử thứ hai của mảng. Nếu phần tử hiện tại khác phần tử trước đó, ta tăng $\textit{cnt}$, biểu thị số thao tác cần để giảm phần tử hiện tại về giá trị nhỏ nhất. Sau đó, ta cộng $\textit{cnt}$ vào $\textit{ans}$ và tiếp tục với phần tử tiếp theo.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reductionOperations(self, nums: List[int]) -> int:
        nums.sort()
        ans = cnt = 0
        for a, b in pairwise(nums):
            if a != b:
                cnt += 1
            ans += cnt
        return ans
```

#### Java

```java
class Solution {
    public int reductionOperations(int[] nums) {
        Arrays.sort(nums);
        int ans = 0, cnt = 0;
        for (int i = 1; i < nums.length; ++i) {
            if (nums[i] != nums[i - 1]) {
                ++cnt;
            }
            ans += cnt;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int reductionOperations(vector<int>& nums) {
        ranges::sort(nums);
        int ans = 0, cnt = 0;
        for (int i = 1; i < nums.size(); ++i) {
            cnt += nums[i] != nums[i - 1];
            ans += cnt;
        }
        return ans;
    }
};
```

#### Go

```go
func reductionOperations(nums []int) (ans int) {
	sort.Ints(nums)
	cnt := 0
	for i, x := range nums[1:] {
		if x != nums[i] {
			cnt++
		}
		ans += cnt
	}
	return
}
```

#### TypeScript

```ts
function reductionOperations(nums: number[]): number {
    nums.sort((a, b) => a - b);
    let [ans, cnt] = [0, 0];
    for (let i = 1; i < nums.length; ++i) {
        if (nums[i] !== nums[i - 1]) {
            ++cnt;
        }
        ans += cnt;
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var reductionOperations = function (nums) {
    nums.sort((a, b) => a - b);
    let [ans, cnt] = [0, 0];
    for (let i = 1; i < nums.length; ++i) {
        if (nums[i] !== nums[i - 1]) {
            ++cnt;
        }
        ans += cnt;
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int ReductionOperations(int[] nums) {
        Array.Sort(nums);
        int ans = 0, cnt = 0;
        for (int i = 1; i < nums.Length; i++) {
            if (nums[i] != nums[i - 1]) {
                ++cnt;
            }
            ans += cnt;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
