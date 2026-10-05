---
comments: true
difficulty: Medium
rating: 1529
source: Weekly Contest 474 Q2
tags:
    - Greedy
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [3732. Maximum Product of Three Elements After One Replacement](https://leetcode.com/problems/maximum-product-of-three-elements-after-one-replacement)

[中文文档](/solution/3700-3799/3732.Maximum%20Product%20of%20Three%20Elements%20After%20One%20Replacement/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn <strong>phải</strong> thay thế <strong>chính xác một</strong> phần tử trong mảng bằng <strong>bất kỳ</strong> giá trị số nguyên nào trong phạm vi <code>[-10<sup>5</sup>, 10<sup>5</sup>]</code> (bao gồm cả hai đầu mút).</p>

<p>Sau khi thực hiện phép thay thế này, hãy xác định <strong>tích lớn nhất có thể</strong> của <strong>bất kỳ ba</strong> phần tử nào tại <strong>các chỉ số khác nhau</strong> trong mảng đã thay đổi.</p>

<p>Trả về một số nguyên biểu thị <strong>tích lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-5,7,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3500000</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thay thế 0 bằng -10<sup>5</sup> cho ta mảng <code>[-5, 7, -10<sup>5</sup>]</code>, có tích <code>(-5) * 7 * (-10<sup>5</sup>) = 3500000</code>. Tích lớn nhất là 3500000.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-4,-2,-1,-3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1200000</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có hai cách để đạt được tích lớn nhất:</p>

<ul>
	<li><code>[-4, -2, -3]</code> &rarr; thay thế -2 bằng 10<sup>5</sup> &rarr; tích = <code>(-4) * 10<sup>5</sup> * (-3) = 1200000</code>.</li>
	<li><code>[-4, -1, -3]</code> &rarr; thay thế -1 bằng 10<sup>5</sup> &rarr; tích = <code>(-4) * 10<sup>5</sup> * (-3) = 1200000</code>.</li>
</ul>
Tích lớn nhất là 1200000.</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,10,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cách nào thay thế một phần tử bằng một số nguyên khác mà không còn số 0 trong mảng. Vì vậy, tích của cả ba phần tử luôn bằng 0, và tích lớn nhất là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Chính xác một phần tử có thể được thay thế bằng bất kỳ số nguyên nào trong $[-10^5,10^5]$. Tích lớn nhất sẽ là một trong các giá trị sau: tích của hai giá trị nhỏ nhất nhân với $10^5$, tích của hai giá trị lớn nhất nhân với $10^5$, hoặc giá trị nhỏ nhất nhân với giá trị lớn nhất rồi nhân với $-10^5$. Việc sắp xếp giúp xác định bốn giá trị biên đó.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể thay thế một phần tử trong mảng bằng bất kỳ số nguyên nào trong phạm vi $[-10^5, 10^5]$. Để tối đa hóa tích của ba phần tử, ta có thể xét các trường hợp sau:

1. Chọn hai phần tử nhỏ nhất trong mảng và thay thế phần tử thứ ba bằng $10^5$.
2. Chọn hai phần tử lớn nhất trong mảng và thay thế phần tử thứ ba bằng $10^5$.
3. Chọn phần tử nhỏ nhất và hai phần tử lớn nhất trong mảng, rồi thay thế phần tử ở giữa bằng $-10^5$.

Tích lớn nhất trong ba trường hợp trên là đáp án.

Do đó, trước tiên ta có thể sắp xếp mảng, sau đó tính tích cho ba trường hợp trên và trả về giá trị lớn nhất trong số đó.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        nums.sort()
        a, b = nums[0], nums[1]
        c, d = nums[-2], nums[-1]
        x = 10**5
        return max(a * b * x, c * d * x, a * d * -x)
```

#### Java

```java
class Solution {
    public long maxProduct(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        long a = nums[0], b = nums[1];
        long c = nums[n - 2], d = nums[n - 1];
        final int x = 100000;
        return Math.max(Math.max(a * b * x, c * d * x), -a * d * x);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxProduct(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        long long a = nums[0], b = nums[1];
        long long c = nums[n - 2], d = nums[n - 1];
        const int x = 100000;
        return max({a * b * x, c * d * x, -a * d * x});
    }
};
```

#### Go

```go
func maxProduct(nums []int) int64 {
	sort.Ints(nums)
	n := len(nums)
	a, b := int64(nums[0]), int64(nums[1])
	c, d := int64(nums[n-2]), int64(nums[n-1])
	const x int64 = 100000
	return max(a*b*x, c*d*x, -a*d*x)
}
```

#### TypeScript

```ts
function maxProduct(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const [a, b] = [nums[0], nums[1]];
    const [c, d] = [nums[n - 2], nums[n - 1]];
    const x = 100000;
    return Math.max(a * b * x, c * d * x, -a * d * x);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
