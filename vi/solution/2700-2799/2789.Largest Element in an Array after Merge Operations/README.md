---
comments: true
difficulty: Medium
rating: 1484
source: Weekly Contest 355 Q2
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2789. Largest Element in an Array after Merge Operations](https://leetcode.com/problems/largest-element-in-an-array-after-merge-operations)

[中文文档](/solution/2700-2799/2789.Largest%20Element%20in%20an%20Array%20after%20Merge%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> được đánh chỉ số từ <strong>0</strong>, gồm các số nguyên dương.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Chọn một chỉ số&nbsp;<code>i</code> sao cho <code>0 &lt;= i &lt; nums.length - 1</code> và <code>nums[i] &lt;= nums[i + 1]</code>. Thay thế phần tử <code>nums[i + 1]</code> bằng <code>nums[i] + nums[i + 1]</code>, rồi xóa phần tử <code>nums[i]</code> khỏi mảng.</li>
</ul>

<p>Trả về <em>giá trị của phần tử <b>lớn nhất</b> mà bạn có thể nhận được trong mảng cuối cùng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,7,9,3]
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau trên mảng:
- Chọn i = 0. Mảng sau thao tác sẽ là nums = [<u>5</u>,7,9,3].
- Chọn i = 1. Mảng sau thao tác sẽ là nums = [5,<u>16</u>,3].
- Chọn i = 0. Mảng sau thao tác sẽ là nums = [<u>21</u>,3].
Phần tử lớn nhất trong mảng cuối cùng là 21. Có thể chứng minh rằng ta không thể nhận được phần tử lớn hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,3,3]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Chọn i = 1. Mảng sau thao tác sẽ là nums = [5,<u>6</u>].
- Chọn i = 0. Mảng sau thao tác sẽ là nums = [<u>11</u>].
Mảng cuối cùng chỉ có một phần tử, đó là 11.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gộp theo thứ tự ngược

<!-- thinking:start -->

> **Tư duy**
>
> Hai giá trị kề nhau chỉ có thể được gộp thành tổng khi $nums[i]\le nums[i+1]$; ta muốn tìm giá trị lớn nhất có thể xuất hiện. Nếu gộp từ trái sang phải, các số nhỏ sẽ bị dùng quá sớm và cản trở những chuỗi gộp về sau.
>
> Ta duyệt từ phải sang trái và khi $nums[i]\le nums[i+1]$, cộng giá trị bên phải vào $nums[i]$, qua đó gộp thêm một hậu tố đã được tối đa hóa. Phần tử lớn nhất còn lại chính là đáp án.

<!-- thinking:end -->

Theo mô tả bài toán, để tối đa hóa phần tử lớn nhất trong mảng sau khi gộp, ta nên gộp các phần tử bên phải trước, khiến các phần tử bên phải lớn nhất có thể. Nhờ đó, ta thực hiện được nhiều thao tác gộp nhất và cuối cùng nhận được phần tử lớn nhất.

Vì vậy, ta có thể duyệt mảng từ phải sang trái. Với mỗi vị trí $i$, trong đó $i \in [0, n - 2]$, nếu $nums[i] \leq nums[i + 1]$, ta cập nhật $nums[i]$ thành $nums[i] + nums[i + 1]$. Thao tác này tương đương với việc gộp $nums[i]$ và $nums[i + 1]$, sau đó xóa $nums[i]$.

Cuối cùng, phần tử lớn nhất trong mảng chính là phần tử lớn nhất trong mảng sau khi gộp.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxArrayValue(self, nums: List[int]) -> int:
        for i in range(len(nums) - 2, -1, -1):
            if nums[i] <= nums[i + 1]:
                nums[i] += nums[i + 1]
        return max(nums)
```

#### Java

```java
class Solution {
    public long maxArrayValue(int[] nums) {
        int n = nums.length;
        long ans = nums[n - 1], t = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] <= t) {
                t += nums[i];
            } else {
                t = nums[i];
            }
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxArrayValue(vector<int>& nums) {
        int n = nums.size();
        long long ans = nums[n - 1], t = nums[n - 1];
        for (int i = n - 2; ~i; --i) {
            if (nums[i] <= t) {
                t += nums[i];
            } else {
                t = nums[i];
            }
            ans = max(ans, t);
        }
        return ans;
    }
};
```

#### Go

```go
func maxArrayValue(nums []int) int64 {
	n := len(nums)
	ans, t := nums[n-1], nums[n-1]
	for i := n - 2; i >= 0; i-- {
		if nums[i] <= t {
			t += nums[i]
		} else {
			t = nums[i]
		}
		ans = max(ans, t)
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function maxArrayValue(nums: number[]): number {
    for (let i = nums.length - 2; i >= 0; --i) {
        if (nums[i] <= nums[i + 1]) {
            nums[i] += nums[i + 1];
        }
    }
    return Math.max(...nums);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
