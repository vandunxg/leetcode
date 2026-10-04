---
comments: true
difficulty: Hard
rating: 2152
source: Biweekly Contest 120 Q3
tags:
    - Array
    - Two Pointers
    - Binary Search
---

<!-- problem:start -->

# [2972. Count the Number of Incremovable Subarrays II](https://leetcode.com/problems/count-the-number-of-incremovable-subarrays-ii)

[中文文档](/solution/2900-2999/2972.Count%20the%20Number%20of%20Incremovable%20Subarrays%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>dương</strong> <strong>được đánh chỉ số từ 0</strong> <code>nums</code>.</p>

<p>Một mảng con của <code>nums</code> được gọi là <strong>incremovable</strong> nếu sau khi xóa mảng con đó, <code>nums</code> trở thành <strong>tăng nghiêm ngặt</strong>. Ví dụ, mảng con <code>[3, 4]</code> là một mảng con incremovable của <code>[5, 3, 4, 6, 7]</code> vì sau khi xóa mảng con này, mảng <code>[5, 3, 4, 6, 7]</code> trở thành <code>[5, 6, 7]</code>, là một mảng tăng nghiêm ngặt.</p>

<p>Trả về <em>tổng số mảng con <strong>incremovable</strong> của</em> <code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng một mảng rỗng được xem là tăng nghiêm ngặt.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> 10 mảng con incremovable là: [1], [2], [3], [4], [1,2], [2,3], [3,4], [1,2,3], [2,3,4] và [1,2,3,4], vì sau khi xóa bất kỳ mảng con nào trong số này, nums đều trở thành mảng tăng nghiêm ngặt. Lưu ý rằng không thể chọn một mảng con rỗng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,5,7,8]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> 7 mảng con incremovable là: [5], [6], [5,7], [6,5], [5,7,8], [6,5,7] và [6,5,7,8].
Có thể chứng minh rằng nums chỉ có 7 mảng con incremovable.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8,7,6,6]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 3 mảng con incremovable là: [8,7,6], [7,6,6] và [8,7,6,6]. Lưu ý rằng [8,7] không phải là mảng con incremovable vì sau khi xóa [8,7], nums trở thành [6,6], được sắp xếp theo thứ tự tăng dần nhưng không tăng nghiêm ngặt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Phát biểu bài toán giống phần I, nhưng $n \le 10^5$ khiến việc liệt kê các lần xóa là không thể. Phần còn lại vẫn là một tiền tố tăng, một hậu tố tăng hoặc cả hai, và mỗi con trỏ chỉ di chuyển nhiều nhất một lần qua mỗi phía.
>
> Thuật toán giống phần I: một mảng tăng hoàn toàn trả về số tam giác; nếu không, con trỏ hậu tố đi sang trái, con trỏ tiền tố quay lui, rồi ta cộng dồn. Tổng thời gian là $O(n)$.

<!-- thinking:end -->

Theo mô tả bài toán, sau khi xóa một mảng con, các phần tử còn lại phải tăng nghiêm ngặt. Vì vậy, có một số trường hợp:

1. Các phần tử còn lại chỉ bao gồm tiền tố của mảng $nums$ (có thể rỗng);
2. Các phần tử còn lại chỉ bao gồm hậu tố của mảng $nums$;
3. Các phần tử còn lại bao gồm cả tiền tố và hậu tố của mảng $nums$.

Trường hợp thứ hai và thứ ba có thể gộp thành một, nghĩa là các phần tử còn lại bao gồm hậu tố của mảng $nums$. Do đó, tổng cộng có hai trường hợp:

1. Các phần tử còn lại chỉ bao gồm tiền tố của mảng $nums$ (có thể rỗng);
2. Các phần tử còn lại bao gồm hậu tố của mảng $nums$.

Trước hết, xét trường hợp thứ nhất, tức là các phần tử còn lại chỉ bao gồm tiền tố của mảng $nums$. Ta có thể dùng một con trỏ $i$ trỏ đến phần tử cuối cùng của tiền tố tăng dài nhất của mảng $nums$, tức là $nums[0] \lt nums[1] \lt \cdots \lt nums[i]$. Khi đó, số phần tử còn lại là $n - i - 1$, trong đó $n$ là độ dài mảng $nums$. Vì vậy, để các phần tử còn lại tăng nghiêm ngặt, ta có thể chọn xóa các mảng con sau:

1. $nums[i+1,...,n-1]$;
2. $nums[i,...,n-1]$;
3. $nums[i-1,...,n-1]$;
4. $nums[i-2,...,n-1]$;
5. $\cdots$;
6. $nums[0,...,n-1]$.

Có tổng cộng $i + 2$ trường hợp, nên trong trường hợp này, số mảng con cần xóa là $i + 2$.

Tiếp theo, xét trường hợp thứ hai, tức là các phần tử còn lại bao gồm hậu tố của mảng $nums$. Ta có thể dùng một con trỏ $j$ trỏ đến phần tử đầu tiên của hậu tố tăng của mảng $nums$. Ta liệt kê $j$ là phần tử đầu tiên của hậu tố tăng trong phạm vi $[n - 1,...,1]$. Mỗi lần, ta cần di chuyển con trỏ $i$ để $nums[i] \lt nums[j]$, sau đó số mảng con cần xóa tăng thêm $i + 2$. Khi $nums[j - 1] \ge nums[j]$, ta dừng việc liệt kê vì hậu tố không còn tăng nghiêm ngặt.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def incremovableSubarrayCount(self, nums: List[int]) -> int:
        i, n = 0, len(nums)
        while i + 1 < n and nums[i] < nums[i + 1]:
            i += 1
        if i == n - 1:
            return n * (n + 1) // 2
        ans = i + 2
        j = n - 1
        while j:
            while i >= 0 and nums[i] >= nums[j]:
                i -= 1
            ans += i + 2
            if nums[j - 1] >= nums[j]:
                break
            j -= 1
        return ans
```

#### Java

```java
class Solution {
    public long incremovableSubarrayCount(int[] nums) {
        int i = 0, n = nums.length;
        while (i + 1 < n && nums[i] < nums[i + 1]) {
            ++i;
        }
        if (i == n - 1) {
            return n * (n + 1L) / 2;
        }
        long ans = i + 2;
        for (int j = n - 1; j > 0; --j) {
            while (i >= 0 && nums[i] >= nums[j]) {
                --i;
            }
            ans += i + 2;
            if (nums[j - 1] >= nums[j]) {
                break;
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
    long long incremovableSubarrayCount(vector<int>& nums) {
        int i = 0, n = nums.size();
        while (i + 1 < n && nums[i] < nums[i + 1]) {
            ++i;
        }
        if (i == n - 1) {
            return n * (n + 1LL) / 2;
        }
        long long ans = i + 2;
        for (int j = n - 1; j > 0; --j) {
            while (i >= 0 && nums[i] >= nums[j]) {
                --i;
            }
            ans += i + 2;
            if (nums[j - 1] >= nums[j]) {
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func incremovableSubarrayCount(nums []int) int64 {
	i, n := 0, len(nums)
	for i+1 < n && nums[i] < nums[i+1] {
		i++
	}
	if i == n-1 {
		return int64(n * (n + 1) / 2)
	}
	ans := int64(i + 2)
	for j := n - 1; j > 0; j-- {
		for i >= 0 && nums[i] >= nums[j] {
			i--
		}
		ans += int64(i + 2)
		if nums[j-1] >= nums[j] {
			break
		}
	}
	return ans
}
```

#### TypeScript

```ts
function incremovableSubarrayCount(nums: number[]): number {
    const n = nums.length;
    let i = 0;
    while (i + 1 < n && nums[i] < nums[i + 1]) {
        i++;
    }
    if (i === n - 1) {
        return (n * (n + 1)) / 2;
    }
    let ans = i + 2;
    for (let j = n - 1; j; --j) {
        while (i >= 0 && nums[i] >= nums[j]) {
            --i;
        }
        ans += i + 2;
        if (nums[j - 1] >= nums[j]) {
            break;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
