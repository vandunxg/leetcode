---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [740. Delete and Earn](https://leetcode.com/problems/delete-and-earn)

[中文文档](/solution/0700-0799/0740.Delete%20and%20Earn/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Hãy tối đa hóa số điểm bằng cách thực hiện thao tác sau đây bất kỳ số lần nào:</p>

<ul>
	<li>Chọn một phần tử <code>nums[i]</code> bất kỳ và xóa nó để nhận <code>nums[i]</code> điểm. Sau đó, bạn phải xóa <b>mọi</b> phần tử bằng <code>nums[i] - 1</code> và <strong>mọi</strong> phần tử bằng <code>nums[i] + 1</code>.</li>
</ul>

<p>Trả về <em><strong>số điểm tối đa</strong> có thể đạt được khi thực hiện thao tác trên một số lần bất kỳ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,2]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có thể thực hiện các thao tác sau:
- Xóa 4 để nhận 4 điểm. Vì vậy, 3 cũng bị xóa. nums = [2].
- Xóa 2 để nhận 2 điểm. nums = [].
Tổng cộng nhận được 6 điểm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,3,3,3,4]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Có thể thực hiện các thao tác sau:
- Xóa một số 3 để nhận 3 điểm. Tất cả số 2 và số 4 cũng bị xóa. nums = [3,3].
- Xóa thêm một số 3 để nhận 3 điểm. nums = [3].
- Xóa thêm một số 3 nữa để nhận 3 điểm. nums = [].
Tổng cộng nhận được 9 điểm.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chọn $x$ giúp nhận điểm từ mọi bản sao của $x$, nhưng không thể chọn $x-1$ và $x+1$. Vì $n\le 2\times 10^4$ và giá trị tối đa là $10^4$, xét từng lựa chọn riêng sẽ lặp lại nhiều việc.
>
> Với mỗi giá trị, ta chọn tất cả bản sao hoặc không chọn bản nào; hai giá trị kề nhau xung đột với nhau, tương tự recurrence của bài house robber. Gom điểm vào $total[x]$, rồi duyệt theo thứ tự giá trị.
>
> Dùng hai biến rolling lưu điểm tốt nhất đến $i-2$ và $i-1$. Với giá trị $i$, điểm tốt nhất là $\max(first+total[i], second)$. Độ phức tạp thời gian phụ thuộc vào giá trị lớn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        mx = -inf
        for num in nums:
            mx = max(mx, num)
        total = [0] * (mx + 1)
        for num in nums:
            total[num] += num
        first = total[0]
        second = max(total[0], total[1])
        for i in range(2, mx + 1):
            cur = max(first + total[i], second)
            first = second
            second = cur
        return second
```

#### Java

```java
class Solution {
    public int deleteAndEarn(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }

        int[] sums = new int[10010];
        int[] select = new int[10010];
        int[] nonSelect = new int[10010];

        int maxV = 0;
        for (int x : nums) {
            sums[x] += x;
            maxV = Math.max(maxV, x);
        }

        for (int i = 1; i <= maxV; i++) {
            select[i] = nonSelect[i - 1] + sums[i];
            nonSelect[i] = Math.max(select[i - 1], nonSelect[i - 1]);
        }
        return Math.max(select[maxV], nonSelect[maxV]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int deleteAndEarn(vector<int>& nums) {
        vector<int> vals(10010);
        for (int& num : nums) {
            vals[num] += num;
        }
        return rob(vals);
    }

    int rob(vector<int>& nums) {
        int a = 0, b = nums[0];
        for (int i = 1; i < nums.size(); ++i) {
            int c = max(nums[i] + a, b);
            a = b;
            b = c;
        }
        return b;
    }
};
```

#### Go

```go
func deleteAndEarn(nums []int) int {

	max := func(x, y int) int {
		if x > y {
			return x
		}
		return y
	}

	mx := math.MinInt32
	for _, num := range nums {
		mx = max(mx, num)
	}
	total := make([]int, mx+1)
	for _, num := range nums {
		total[num] += num
	}
	first := total[0]
	second := max(total[0], total[1])
	for i := 2; i <= mx; i++ {
		cur := max(first+total[i], second)
		first = second
		second = cur
	}
	return second
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
