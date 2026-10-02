---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [487. Max Consecutive Ones II 🔒](https://leetcode.com/problems/max-consecutive-ones-ii)

[中文文档](/solution/0400-0499/0487.Max%20Consecutive%20Ones%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng nhị phân <code>nums</code>, hãy trả về <em>số lượng </em><code>1</code><em> liên tiếp lớn nhất trong mảng nếu bạn được phép lật tối đa một</em> <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,1,1,0]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 
- Nếu lật số 0 đầu tiên, nums trở thành [1,1,1,1,0] và ta có 4 số 1 liên tiếp.
- Nếu lật số 0 thứ hai, nums trở thành [1,0,1,1,1] và ta có 3 số 1 liên tiếp.
Số lượng số 1 liên tiếp lớn nhất là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,1,1,0,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 
- Nếu lật số 0 đầu tiên, nums trở thành [1,1,1,1,0,1] và ta có 4 số 1 liên tiếp.
- Nếu lật số 0 thứ hai, nums trở thành [1,0,1,1,1,1] và ta có 4 số 1 liên tiếp.
Số lượng số 1 liên tiếp lớn nhất là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu các số đầu vào lần lượt đến từ một stream vô hạn thì sao? Nói cách khác, bạn không thể lưu tất cả số từ stream vì dữ liệu quá lớn để chứa trong bộ nhớ. Bạn có thể giải bài toán hiệu quả không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding window

<!-- thinking:start -->

> **Tư duy**
>
> Ta được phép xem tối đa một $0$ là $1$. Vì vậy, cần tìm window dài nhất chứa tối đa một số 0.
>
> Mở rộng đầu phải và đếm số 0 bằng $x\oplus 1$; khi xuất hiện số 0 thứ hai, dịch đầu trái sang phải một vị trí. Vì chỉ cần tìm độ dài lớn nhất nên đáp án là $n-l$.
>
> Đây là trường hợp $k=1$ của bài toán “window dài nhất với tối đa $k$ lần thay thế”; ta không chủ động thu hẹp window.

<!-- thinking:end -->

Ta duyệt mảng và dùng biến $\textit{cnt}$ để lưu số lượng số 0 hiện tại trong window. Khi $\textit{cnt} > 1$, ta dịch ranh giới trái của window sang phải một vị trí.

Sau khi duyệt xong, độ dài của window là số lượng số 1 liên tiếp lớn nhất.

Lưu ý rằng trong quá trình trên, ta không cần lặp để dịch ranh giới trái của window sang phải. Thay vào đó, ta chỉ cần dịch ranh giới trái đúng một vị trí. Vì bài toán yêu cầu tìm số lượng số 1 liên tiếp lớn nhất, độ dài window chỉ tăng chứ không giảm. Do đó, không cần lặp để dịch ranh giới trái.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        l = cnt = 0
        for x in nums:
            cnt += x ^ 1
            if cnt > 1:
                cnt -= nums[l] ^ 1
                l += 1
        return len(nums) - l
```

#### Java

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int l = 0, cnt = 0;
        for (int x : nums) {
            cnt += x ^ 1;
            if (cnt > 1) {
                cnt -= nums[l++] ^ 1;
            }
        }
        return nums.length - l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int l = 0, cnt = 0;
        for (int x : nums) {
            cnt += x ^ 1;
            if (cnt > 1) {
                cnt -= nums[l++] ^ 1;
            }
        }
        return nums.size() - l;
    }
};
```

#### Go

```go
func findMaxConsecutiveOnes(nums []int) int {
	l, cnt := 0, 0
	for _, x := range nums {
		cnt += x ^ 1
		if cnt > 1 {
			cnt -= nums[l] ^ 1
			l++
		}
	}
	return len(nums) - l
}
```

#### TypeScript

```ts
function findMaxConsecutiveOnes(nums: number[]): number {
    let [l, cnt] = [0, 0];
    for (const x of nums) {
        cnt += x ^ 1;
        if (cnt > 1) {
            cnt -= nums[l++] ^ 1;
        }
    }
    return nums.length - l;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMaxConsecutiveOnes = function (nums) {
    let [l, cnt] = [0, 0];
    for (const x of nums) {
        cnt += x ^ 1;
        if (cnt > 1) {
            cnt -= nums[l++] ^ 1;
        }
    }
    return nums.length - l;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
