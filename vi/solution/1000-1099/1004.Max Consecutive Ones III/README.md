---
comments: true
difficulty: Medium
rating: 1655
source: Weekly Contest 126 Q3
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii)

[中文文档](/solution/1000-1099/1004.Max%20Consecutive%20Ones%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng nhị phân <code>nums</code> và số nguyên <code>k</code>. Nếu được phép lật nhiều nhất <code>k</code> số <code>0</code> thành <code>1</code>, hãy trả về số lượng <code>1</code> liên tiếp lớn nhất trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> [1,1,1,0,0,<u><strong>1</strong>,1,1,1,1,<strong>1</strong></u>]
Các số in đậm đã được lật từ 0 thành 1. Mảng con dài nhất được gạch chân.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> [0,0,<u>1,1,<strong>1</strong>,<strong>1</strong>,1,1,1,<strong>1</strong>,1,1</u>,0,0,0,1,1,1,1]
Các số in đậm đã được lật từ 0 thành 1. Mảng con dài nhất được gạch chân.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> chỉ có thể là 0 hoặc 1.</li>
	<li><code>0 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra số lượng số 0 trong mọi mảng con có độ phức tạp $O(n^2)$, không đáp ứng được khi $n\le 10^5$. Bài toán lật nhiều nhất $k$ số 0 để có dãy số 1 liên tiếp dài nhất tương đương với tìm cửa sổ dài nhất chứa không quá $k$ số 0.
>
> Khi đầu phải tiến lên và số lượng số 0 vượt quá $k$, đầu trái cần tiến lên để cửa sổ lại hợp lệ. Vì chỉ cần tìm độ dài lớn nhất, ta có thể để cửa sổ tăng đơn điệu: đầu trái mỗi lượt chỉ cần tiến tối đa một bước.
>
> Ta dùng $l$ và $\textit{cnt}$ để theo dõi cửa sổ hiện tại. Sau khi đầu phải đã đi qua mọi chỉ số, $n-l$ là độ dài cửa sổ hợp lệ dài nhất.

<!-- thinking:end -->

Duyệt mảng và dùng biến $\textit{cnt}$ để lưu số lượng số 0 hiện có trong cửa sổ. Khi $\textit{cnt} > k$, dịch biên trái của cửa sổ sang phải một vị trí.

Sau khi duyệt xong, độ dài cửa sổ là số lượng số 1 liên tiếp lớn nhất.

Lưu ý rằng ở trên, ta không cần lặp để dịch biên trái nhiều lần; chỉ cần dịch sang phải một vị trí. Do bài toán yêu cầu số lượng số 1 liên tiếp lớn nhất nên độ dài cửa sổ chỉ tăng, không giảm. Vì vậy, không cần lặp để dịch biên trái.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

Bài toán tương tự:

- [487. Max Consecutive Ones II](https://github.com/doocs/leetcode/blob/main/solution/0400-0499/0487.Max%20Consecutive%20Ones%20II/README_EN.md)
- [2024. Maximize the Confusion of an Exam](https://github.com/doocs/leetcode/blob/main/solution/2000-2099/2024.Maximize%20the%20Confusion%20of%20an%20Exam/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        l = cnt = 0
        for x in nums:
            cnt += x ^ 1
            if cnt > k:
                cnt -= nums[l] ^ 1
                l += 1
        return len(nums) - l
```

#### Java

```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int l = 0, cnt = 0;
        for (int x : nums) {
            cnt += x ^ 1;
            if (cnt > k) {
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
    int longestOnes(vector<int>& nums, int k) {
        int l = 0, cnt = 0;
        for (int x : nums) {
            cnt += x ^ 1;
            if (cnt > k) {
                cnt -= nums[l++] ^ 1;
            }
        }
        return nums.size() - l;
    }
};
```

#### Go

```go
func longestOnes(nums []int, k int) int {
	l, cnt := 0, 0
	for _, x := range nums {
		cnt += x ^ 1
		if cnt > k {
			cnt -= nums[l] ^ 1
			l++
		}
	}
	return len(nums) - l
}
```

#### TypeScript

```ts
function longestOnes(nums: number[], k: number): number {
    let [l, cnt] = [0, 0];
    for (const x of nums) {
        cnt += x ^ 1;
        if (cnt > k) {
            cnt -= nums[l++] ^ 1;
        }
    }
    return nums.length - l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
