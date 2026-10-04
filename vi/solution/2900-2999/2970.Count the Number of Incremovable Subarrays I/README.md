---
comments: true
difficulty: Easy
rating: 1563
source: Biweekly Contest 120 Q1
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Enumeration
---

<!-- problem:start -->

# [2970. Count the Number of Incremovable Subarrays I](https://leetcode.com/problems/count-the-number-of-incremovable-subarrays-i)

[中文文档](/solution/2900-2999/2970.Count%20the%20Number%20of%20Incremovable%20Subarrays%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>dương</strong> <code>nums</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Một subarray của <code>nums</code> được gọi là <strong>incremovable</strong> nếu sau khi loại bỏ subarray đó, <code>nums</code> trở thành một mảng <strong>tăng nghiêm ngặt</strong>. Ví dụ, subarray <code>[3, 4]</code> là một subarray incremovable của <code>[5, 3, 4, 6, 7]</code> vì sau khi loại bỏ subarray này, mảng <code>[5, 3, 4, 6, 7]</code> trở thành <code>[5, 6, 7]</code>, là một mảng tăng nghiêm ngặt.</p>

<p>Trả về <em>tổng số subarray <strong>incremovable</strong> của</em> <code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng mảng rỗng được xem là tăng nghiêm ngặt.</p>

<p>Một <strong>subarray</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> 10 subarray incremovable là: [1], [2], [3], [4], [1,2], [2,3], [3,4], [1,2,3], [2,3,4] và [1,2,3,4], vì sau khi loại bỏ bất kỳ subarray nào trong số này, nums đều trở thành một mảng tăng nghiêm ngặt. Lưu ý rằng không thể chọn một subarray rỗng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,5,7,8]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> 7 subarray incremovable là: [5], [6], [5,7], [6,5], [5,7,8], [6,5,7] và [6,5,7,8].
Có thể chứng minh rằng nums chỉ có 7 subarray incremovable.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8,7,6,6]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 3 subarray incremovable là: [8,7,6], [7,6,6] và [8,7,6,6]. Lưu ý rằng [8,7] không phải là subarray incremovable vì sau khi loại bỏ [8,7], nums trở thành [6,6], được sắp xếp theo thứ tự tăng dần nhưng không tăng nghiêm ngặt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi xóa, phần còn lại phải tăng nghiêm ngặt: đó có thể là một prefix tăng, một suffix tăng, hoặc cả hai được nối với nhau tại một điểm tiếp giáp vẫn tăng. Hãy tìm prefix tăng nghiêm ngặt dài nhất kết thúc tại $i$; nếu đó là toàn bộ mảng, mọi subarray đều có thể loại bỏ và số lượng là $n(n+1)/2$.
>
> Nếu không, duyệt một suffix tăng từ phải sang trái, lùi $i$ cho đến khi $nums[i]<nums[j]$, rồi cộng $i+2$ prefix (bao gồm cả prefix rỗng) cho mỗi vị trí bắt đầu của suffix. Vì $n \le 50$, cùng hai con trỏ này cũng được dùng cho phần II.

<!-- thinking:end -->

Theo mô tả bài toán, sau khi loại bỏ một subarray, các phần tử còn lại phải tăng nghiêm ngặt. Do đó, có một số trường hợp như sau:

1. Các phần tử còn lại chỉ bao gồm prefix của mảng $nums$ (có thể rỗng);
2. Các phần tử còn lại chỉ bao gồm suffix của mảng $nums$;
3. Các phần tử còn lại bao gồm cả prefix và suffix của mảng $nums$.

Trường hợp thứ hai và thứ ba có thể gộp thành một, tức là các phần tử còn lại bao gồm suffix của mảng $nums$. Vì vậy, tổng cộng có hai trường hợp:

1. Các phần tử còn lại chỉ bao gồm prefix của mảng $nums$ (có thể rỗng);
2. Các phần tử còn lại bao gồm suffix của mảng $nums$.

Trước tiên, xét trường hợp thứ nhất, tức là các phần tử còn lại chỉ bao gồm prefix của mảng $nums$. Ta dùng một con trỏ $i$ trỏ đến phần tử cuối của prefix tăng dài nhất của mảng $nums$, tức là $nums[0] \lt nums[1] \lt \cdots \lt nums[i]$; khi đó số phần tử còn lại là $n - i - 1$, trong đó $n$ là độ dài của mảng $nums$. Vì vậy, trong trường hợp này, để các phần tử còn lại tăng nghiêm ngặt, ta có thể chọn loại bỏ các subarray sau:

1. $nums[i+1,...,n-1]$;
2. $nums[i,...,n-1]$;
3. $nums[i-1,...,n-1]$;
4. $nums[i-2,...,n-1]$;
5. $\cdots$;
6. $nums[0,...,n-1]$.

Có tổng cộng $i + 2$ trường hợp, nên trong trường hợp này, số subarray cần loại bỏ là $i + 2$.

Tiếp theo, xét trường hợp thứ hai, tức là các phần tử còn lại bao gồm suffix của mảng $nums$. Ta dùng một con trỏ $j$ trỏ đến phần tử đầu tiên của suffix tăng của mảng $nums$. Ta duyệt $j$ làm phần tử đầu tiên của suffix tăng trong khoảng $[n - 1,...,1]$. Mỗi lần, ta cần di chuyển con trỏ $i$ để $nums[i] \lt nums[j]$, khi đó số subarray cần loại bỏ tăng thêm $i + 2$. Khi $nums[j - 1] \ge nums[j]$, ta dừng duyệt vì suffix lúc này không còn tăng nghiêm ngặt.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

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
    public int incremovableSubarrayCount(int[] nums) {
        int i = 0, n = nums.length;
        while (i + 1 < n && nums[i] < nums[i + 1]) {
            ++i;
        }
        if (i == n - 1) {
            return n * (n + 1) / 2;
        }
        int ans = i + 2;
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
    int incremovableSubarrayCount(vector<int>& nums) {
        int i = 0, n = nums.size();
        while (i + 1 < n && nums[i] < nums[i + 1]) {
            ++i;
        }
        if (i == n - 1) {
            return n * (n + 1) / 2;
        }
        int ans = i + 2;
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
func incremovableSubarrayCount(nums []int) int {
	i, n := 0, len(nums)
	for i+1 < n && nums[i] < nums[i+1] {
		i++
	}
	if i == n-1 {
		return n * (n + 1) / 2
	}
	ans := i + 2
	for j := n - 1; j > 0; j-- {
		for i >= 0 && nums[i] >= nums[j] {
			i--
		}
		ans += i + 2
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
