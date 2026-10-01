---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [413. Arithmetic Slices](https://leetcode.com/problems/arithmetic-slices)

[中文文档](/solution/0400-0499/0413.Arithmetic%20Slices/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng số nguyên được gọi là cấp số cộng nếu có <strong>ít nhất ba phần tử</strong> và hiệu giữa hai phần tử liên tiếp bất kỳ đều bằng nhau.</p>

<ul>
	<li>Ví dụ, <code>[1,3,5,7,9]</code>, <code>[7,7,7,7]</code> và <code>[3,-1,-5,-9]</code> đều là các cấp số cộng.</li>
</ul>

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về số <strong>mảng con</strong> của <code>nums</code> tạo thành cấp số cộng.</p>

<p><strong>Mảng con</strong> là một dãy con gồm các phần tử liên tiếp trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong nums có 3 mảng con tạo thành cấp số cộng: [1, 2, 3], [2, 3, 4] và chính mảng [1,2,3,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5000</code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra mọi mảng con có độ dài ít nhất $3$ sẽ tốn $O(n^2)$. Với $n\le 5000$, cách này có thể vẫn chạy được, nhưng một đoạn có hiệu cố định có thể được đếm bằng công thức trực tiếp.
>
> Chỉ cần một lượt duyệt, duy trì hiệu hiện tại và biến đếm $\textit{cnt}$ cho các mảng con mới trong đoạn: tăng $\textit{cnt}$ khi hiệu không đổi, nếu không thì đặt lại.
>
> Một đoạn có độ dài $L$ đóng góp $1+2+\cdots+(L-2)$ mảng con; cộng $\textit{cnt}$ ở mỗi bước sẽ tính được tổng này.

<!-- thinking:end -->

Ta dùng $d$ để biểu diễn hiệu hiện tại giữa hai phần tử liền kề, còn $cnt$ biểu diễn độ dài của cấp số cộng hiện tại. Ban đầu, $d = 3000$, $cnt = 2$.

Ta duyệt mảng `nums`. Với hai phần tử liền kề $a$ và $b$, nếu $b - a = d$, phần tử $b$ cũng thuộc cấp số cộng hiện tại, nên ta tăng $cnt$ thêm 1. Ngược lại, $b$ không thuộc cấp số cộng hiện tại; ta cập nhật $d = b - a$ và đặt $cnt = 2$. Nếu $cnt \ge 3$, độ dài cấp số cộng hiện tại ít nhất là 3 và số mảng con cấp số cộng là $cnt - 2$; ta cộng giá trị này vào đáp án.

Sau khi duyệt xong, ta thu được đáp án.

Trong phần cài đặt, ta cũng có thể khởi tạo $cnt$ bằng $0$ và đặt lại $cnt$ về $0$ mỗi khi khởi tạo lại. Khi cập nhật đáp án, ta cộng trực tiếp $cnt$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng `nums`.

Bài tương tự:

- [1513. Number of Substrings With Only 1s](https://github.com/doocs/leetcode/blob/main/solution/1500-1599/1513.Number%20of%20Substrings%20With%20Only%201s/README_EN.md)
- [2348. Number of Zero-Filled Subarrays](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2348.Number%20of%20Zero-Filled%20Subarrays/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfArithmeticSlices(self, nums: List[int]) -> int:
        ans = cnt = 0
        d = 3000
        for a, b in pairwise(nums):
            if b - a == d:
                cnt += 1
            else:
                d = b - a
                cnt = 0
            ans += cnt
        return ans
```

#### Java

```java
class Solution {
    public int numberOfArithmeticSlices(int[] nums) {
        int ans = 0, cnt = 0;
        int d = 3000;
        for (int i = 0; i < nums.length - 1; ++i) {
            if (nums[i + 1] - nums[i] == d) {
                ++cnt;
            } else {
                d = nums[i + 1] - nums[i];
                cnt = 0;
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
    int numberOfArithmeticSlices(vector<int>& nums) {
        int ans = 0, cnt = 0;
        int d = 3000;
        for (int i = 0; i < nums.size() - 1; ++i) {
            if (nums[i + 1] - nums[i] == d) {
                ++cnt;
            } else {
                d = nums[i + 1] - nums[i];
                cnt = 0;
            }
            ans += cnt;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfArithmeticSlices(nums []int) (ans int) {
	cnt, d := 0, 3000
	for i, b := range nums[1:] {
		a := nums[i]
		if b-a == d {
			cnt++
		} else {
			d = b - a
			cnt = 0
		}
		ans += cnt
	}
	return
}
```

#### TypeScript

```ts
function numberOfArithmeticSlices(nums: number[]): number {
    let ans = 0;
    let cnt = 0;
    let d = 3000;
    for (let i = 0; i < nums.length - 1; ++i) {
        const a = nums[i];
        const b = nums[i + 1];
        if (b - a == d) {
            ++cnt;
        } else {
            d = b - a;
            cnt = 0;
        }
        ans += cnt;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
