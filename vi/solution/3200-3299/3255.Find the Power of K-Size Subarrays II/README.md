---
comments: true
difficulty: Medium
rating: 1595
source: Biweekly Contest 137 Q2
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [3255. Find the Power of K-Size Subarrays II](https://leetcode.com/problems/find-the-power-of-k-size-subarrays-ii)

[中文文档](/solution/3200-3299/3255.Find%20the%20Power%20of%20K-Size%20Subarrays%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <em>dương</em> <code>k</code>.</p>

<p><strong>Power</strong> của một mảng được định nghĩa như sau:</p>

<ul>
	<li>Phần tử <strong>lớn nhất</strong> của mảng nếu <em>tất cả</em> phần tử của mảng là các số <strong>liên tiếp</strong> và được <strong>sắp xếp</strong> theo thứ tự <strong>tăng dần</strong>.</li>
	<li>-1 trong các trường hợp còn lại.</li>
</ul>

<p>Bạn cần tìm <strong>power</strong> của tất cả <span data-keyword="subarray-nonempty">dãy con</span> có kích thước <code>k</code> của <code>nums</code>.</p>

<p>Trả về một mảng số nguyên <code>results</code> có kích thước <code>n - k + 1</code>, trong đó <code>results[i]</code> là <em>power</em> của <code>nums[i..(i + k - 1)]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,3,2,5], k = 3</span></p>

<p><strong>Đầu ra:</strong> [3,4,-1,-1,-1]</p>

<p><strong>Giải thích:</strong></p>

<p>Có 5 dãy con có kích thước 3 trong <code>nums</code>:</p>

<ul>
	<li><code>[1, 2, 3]</code> có phần tử lớn nhất là 3.</li>
	<li><code>[2, 3, 4]</code> có phần tử lớn nhất là 4.</li>
	<li><code>[3, 4, 3]</code> có các phần tử <strong>không</strong> liên tiếp.</li>
	<li><code>[4, 3, 2]</code> có các phần tử <strong>không</strong> được sắp xếp.</li>
	<li><code>[3, 2, 5]</code> có các phần tử <strong>không</strong> liên tiếp.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,2,2,2], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1]</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,3,2,3,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,3,-1,3,-1]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Phát biểu giống Lời giải I, nhưng $n\le 10^5$ không cho phép duyệt $k$ ô cho mỗi cửa sổ. Độ dài đoạn tăng liên tiếp kết thúc tại $i$ vẫn quyết định $[i-k+1,i]$.
>
> $f[i]$ được tính như Lời giải I: tăng khi $\textit{nums}[i]=\textit{nums}[i-1]+1$, ngược lại đặt lại thành $1$. Nếu $f[i]\ge k$ tại một điểm kết thúc bên phải, ta đưa $\textit{nums}[i]$ vào kết quả. Thời gian tuyến tính đáp ứng được giới hạn.

<!-- thinking:end -->

Ta định nghĩa một mảng $f$, trong đó $f[i]$ biểu diễn độ dài của dãy tăng liên tiếp kết thúc tại phần tử thứ $i$. Ban đầu, $f[i] = 1$.

Tiếp theo, ta duyệt mảng $\textit{nums}$ để tính các giá trị của mảng $f$. Nếu $nums[i] = nums[i - 1] + 1$, thì $f[i] = f[i - 1] + 1$; ngược lại, $f[i] = 1$.

Sau đó, ta duyệt mảng $f$ trong phạm vi $[k - 1, n)$. Nếu $f[i] \ge k$, ta thêm $\textit{nums}[i]$ vào mảng kết quả; nếu không, ta thêm $-1$.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def resultsArray(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        f = [1] * n
        for i in range(1, n):
            if nums[i] == nums[i - 1] + 1:
                f[i] = f[i - 1] + 1
        return [nums[i] if f[i] >= k else -1 for i in range(k - 1, n)]
```

#### Java

```java
class Solution {
    public int[] resultsArray(int[] nums, int k) {
        int n = nums.length;
        int[] f = new int[n];
        Arrays.fill(f, 1);
        for (int i = 1; i < n; ++i) {
            if (nums[i] == nums[i - 1] + 1) {
                f[i] = f[i - 1] + 1;
            }
        }
        int[] ans = new int[n - k + 1];
        for (int i = k - 1; i < n; ++i) {
            ans[i - k + 1] = f[i] >= k ? nums[i] : -1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> resultsArray(vector<int>& nums, int k) {
        int n = nums.size();
        int f[n];
        f[0] = 1;
        for (int i = 1; i < n; ++i) {
            f[i] = nums[i] == nums[i - 1] + 1 ? f[i - 1] + 1 : 1;
        }
        vector<int> ans;
        for (int i = k - 1; i < n; ++i) {
            ans.push_back(f[i] >= k ? nums[i] : -1);
        }
        return ans;
    }
};
```

#### Go

```go
func resultsArray(nums []int, k int) (ans []int) {
	n := len(nums)
	f := make([]int, n)
	f[0] = 1
	for i := 1; i < n; i++ {
		if nums[i] == nums[i-1]+1 {
			f[i] = f[i-1] + 1
		} else {
			f[i] = 1
		}
	}
	for i := k - 1; i < n; i++ {
		if f[i] >= k {
			ans = append(ans, nums[i])
		} else {
			ans = append(ans, -1)
		}
	}
	return
}
```

#### TypeScript

```ts
function resultsArray(nums: number[], k: number): number[] {
    const n = nums.length;
    const f: number[] = Array(n).fill(1);
    for (let i = 1; i < n; ++i) {
        if (nums[i] === nums[i - 1] + 1) {
            f[i] = f[i - 1] + 1;
        }
    }
    const ans: number[] = [];
    for (let i = k - 1; i < n; ++i) {
        ans.push(f[i] >= k ? nums[i] : -1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
