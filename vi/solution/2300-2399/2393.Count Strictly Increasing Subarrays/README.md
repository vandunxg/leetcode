---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [2393. Count Strictly Increasing Subarrays 🔒](https://leetcode.com/problems/count-strictly-increasing-subarrays)

[中文文档](/solution/2300-2399/2393.Count%20Strictly%20Increasing%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên <strong>dương</strong>.</p>

<p>Trả về <em>số lượng <strong>mảng con</strong> của </em><code>nums</code><em> được sắp xếp theo thứ tự <strong>tăng dần nghiêm ngặt</strong>.</em></p>

<p>Một <strong>mảng con</strong> là một phần <strong>liên tiếp</strong> của một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,4,4,6]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Các mảng con tăng dần nghiêm ngặt là:
- Các mảng con có độ dài 1: [1], [3], [5], [4], [4], [6].
- Các mảng con có độ dài 2: [1,3], [3,5], [4,6].
- Các mảng con có độ dài 3: [1,3,5].
Tổng số mảng con là 6 + 3 + 1 = 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Mọi mảng con đều tăng dần nghiêm ngặt. Có thể chọn 15 mảng con khác nhau.
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

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các mảng con liên tiếp tăng dần nghiêm ngặt. Gom theo phần tử cuối, số lượng chính là độ dài đoạn tăng hiện tại; không cần liệt kê các đầu trái.
>
> Ta duy trì độ dài đoạn tăng $cnt$ kết thúc tại vị trí hiện tại: tăng khi giá trị tăng, ngược lại đặt lại thành $1$, rồi cộng $cnt$ vào đáp án.

<!-- thinking:end -->

Ta có thể đếm số mảng con tăng dần nghiêm ngặt kết thúc tại mỗi phần tử, sau đó cộng tất cả lại.

Ta dùng biến $\textit{cnt}$ để ghi nhận số mảng con tăng dần nghiêm ngặt kết thúc tại phần tử hiện tại, ban đầu $\textit{cnt} = 1$. Sau đó, ta duyệt mảng từ phần tử thứ hai. Nếu phần tử hiện tại lớn hơn phần tử ngay trước đó, $\textit{cnt}$ tăng thêm $1$. Nếu không, $\textit{cnt}$ được đặt lại thành $1$. Khi đó, số mảng con tăng dần nghiêm ngặt kết thúc tại phần tử hiện tại chính là $\textit{cnt}$, và ta cộng giá trị này vào đáp án.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int]) -> int:
        ans = cnt = 1
        for x, y in pairwise(nums):
            if x < y:
                cnt += 1
            else:
                cnt = 1
            ans += cnt
        return ans
```

#### Java

```java
class Solution {
    public long countSubarrays(int[] nums) {
        long ans = 1, cnt = 1;
        for (int i = 1; i < nums.length; ++i) {
            if (nums[i - 1] < nums[i]) {
                ++cnt;
            } else {
                cnt = 1;
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
    long long countSubarrays(vector<int>& nums) {
        long long ans = 1, cnt = 1;
        for (int i = 1; i < nums.size(); ++i) {
            if (nums[i - 1] < nums[i]) {
                ++cnt;
            } else {
                cnt = 1;
            }
            ans += cnt;
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int) int64 {
	ans, cnt := 1, 1
	for i, x := range nums[1:] {
		if nums[i] < x {
			cnt++
		} else {
			cnt = 1
		}
		ans += cnt
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[]): number {
    let [ans, cnt] = [1, 1];
    for (let i = 1; i < nums.length; ++i) {
        if (nums[i - 1] < nums[i]) {
            ++cnt;
        } else {
            cnt = 1;
        }
        ans += cnt;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
