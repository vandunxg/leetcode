---
comments: true
difficulty: Medium
rating: 1314
source: Biweekly Contest 102 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2640. Find the Score of All Prefixes of an Array](https://leetcode.com/problems/find-the-score-of-all-prefixes-of-an-array)

[中文文档](/solution/2600-2699/2640.Find%20the%20Score%20of%20All%20Prefixes%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Ta định nghĩa <strong>mảng chuyển đổi</strong> <code>conver</code> của một mảng <code>arr</code> như sau:</p>

<ul>
	<li><code>conver[i] = arr[i] + max(arr[0..i])</code>, trong đó <code>max(arr[0..i])</code> là giá trị lớn nhất của <code>arr[j]</code> với <code>0 &lt;= j &lt;= i</code>.</li>
</ul>

<p>Ta cũng định nghĩa <strong>điểm số</strong> của một mảng <code>arr</code> là tổng các giá trị trong mảng chuyển đổi của <code>arr</code>.</p>

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code>, hãy trả về <em>một mảng</em> <code>ans</code> <em>có độ dài</em> <code>n</code>, <em>trong đó</em> <code>ans[i]</code> <em>là điểm số của tiền tố</em> <code>nums[0..i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,7,5,10]
<strong>Đầu ra:</strong> [4,10,24,36,56]
<strong>Giải thích:</strong>
Với tiền tố [2], mảng chuyển đổi là [4], nên điểm số là 4
Với tiền tố [2, 3], mảng chuyển đổi là [4, 6], nên điểm số là 10
Với tiền tố [2, 3, 7], mảng chuyển đổi là [4, 6, 14], nên điểm số là 24
Với tiền tố [2, 3, 7, 5], mảng chuyển đổi là [4, 6, 14, 12], nên điểm số là 36
Với tiền tố [2, 3, 7, 5, 10], mảng chuyển đổi là [4, 6, 14, 12, 20], nên điểm số là 56
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,4,8,16]
<strong>Đầu ra:</strong> [2,4,8,16,32,64]
<strong>Giải thích:</strong>
Với tiền tố [1], mảng chuyển đổi là [2], nên điểm số là 2
Với tiền tố [1, 1], mảng chuyển đổi là [2, 2], nên điểm số là 4
Với tiền tố [1, 1, 2], mảng chuyển đổi là [2, 2, 4], nên điểm số là 8
Với tiền tố [1, 1, 2, 4], mảng chuyển đổi là [2, 2, 4, 8], nên điểm số là 16
Với tiền tố [1, 1, 2, 4, 8], mảng chuyển đổi là [2, 2, 4, 8, 16], nên điểm số là 32
Với tiền tố [1, 1, 2, 4, 8, 16], mảng chuyển đổi là [2, 2, 4, 8, 16, 32], nên điểm số là 64
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

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số của một tiền tố là tổng tiền tố của mảng chuyển đổi, trong đó mỗi phần tử bằng $nums[i]$ cộng với giá trị lớn nhất tính đến $i$. Nếu tính lại giá trị lớn nhất cho mỗi tiền tố thì độ phức tạp là bậc hai, không phù hợp với $n \le 10^5$.
>
> Giá trị lớn nhất đang xét $mx$ có thể được cập nhật trong một lần duyệt; cộng thêm điểm số trước đó sẽ thu được $ans[i]$. Như vậy, việc chuyển đổi và tính tổng tiền tố được thực hiện đồng thời.

<!-- thinking:end -->

Ta dùng biến $mx$ để lưu giá trị lớn nhất của $i$ phần tử đầu tiên trong mảng $nums$, đồng thời dùng mảng $ans[i]$ để lưu điểm số của $i$ phần tử đầu tiên trong mảng $nums$.

Tiếp theo, ta duyệt qua mảng $nums$. Với mỗi phần tử $nums[i]$, ta cập nhật $mx$, tức là $mx = \max(mx, nums[i])$, rồi cập nhật $ans[i]$. Nếu $i = 0$ thì $ans[i] = nums[i] + mx$; ngược lại, $ans[i] = nums[i] + mx + ans[i - 1]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Không tính phần không gian dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPrefixScore(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [0] * n
        mx = 0
        for i, x in enumerate(nums):
            mx = max(mx, x)
            ans[i] = x + mx + (0 if i == 0 else ans[i - 1])
        return ans
```

#### Java

```java
class Solution {
    public long[] findPrefixScore(int[] nums) {
        int n = nums.length;
        long[] ans = new long[n];
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            mx = Math.max(mx, nums[i]);
            ans[i] = nums[i] + mx + (i == 0 ? 0 : ans[i - 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> findPrefixScore(vector<int>& nums) {
        int n = nums.size();
        vector<long long> ans(n);
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            mx = max(mx, nums[i]);
            ans[i] = nums[i] + mx + (i == 0 ? 0 : ans[i - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func findPrefixScore(nums []int) []int64 {
	n := len(nums)
	ans := make([]int64, n)
	mx := 0
	for i, x := range nums {
		mx = max(mx, x)
		ans[i] = int64(x + mx)
		if i > 0 {
			ans[i] += ans[i-1]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findPrefixScore(nums: number[]): number[] {
    const n = nums.length;
    const ans: number[] = new Array(n);
    let mx: number = 0;
    for (let i = 0; i < n; ++i) {
        mx = Math.max(mx, nums[i]);
        ans[i] = nums[i] + mx + (i === 0 ? 0 : ans[i - 1]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
