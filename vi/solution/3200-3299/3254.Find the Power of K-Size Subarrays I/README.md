---
comments: true
difficulty: Medium
rating: 1266
source: Biweekly Contest 137 Q1
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [3254. Find the Power of K-Size Subarrays I](https://leetcode.com/problems/find-the-power-of-k-size-subarrays-i)

[中文文档](/solution/3200-3299/3254.Find%20the%20Power%20of%20K-Size%20Subarrays%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <em>dương</em> <code>k</code>.</p>

<p><strong>Power</strong> của một mảng được định nghĩa như sau:</p>

<ul>
	<li>Phần tử <strong>lớn nhất</strong> của mảng nếu <em>tất cả</em> các phần tử của mảng là các số <strong>liên tiếp</strong> và được <strong>sắp xếp</strong> theo thứ tự <strong>tăng dần</strong>.</li>
	<li>-1 nếu không thỏa mãn điều kiện trên.</li>
</ul>

<p>Bạn cần tìm <strong>power</strong> của mọi <span data-keyword="subarray-nonempty">mảng con</span> có kích thước <code>k</code> của <code>nums</code>.</p>

<p>Trả về một mảng số nguyên <code>results</code> có kích thước <code>n - k + 1</code>, trong đó <code>results[i]</code> là <em>power</em> của <code>nums[i..(i + k - 1)]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,3,2,5], k = 3</span></p>

<p><strong>Đầu ra:</strong> [3,4,-1,-1,-1]</p>

<p><strong>Giải thích:</strong></p>

<p>Có 5 mảng con có kích thước 3 của <code>nums</code>:</p>

<ul>
	<li><code>[1, 2, 3]</code> có phần tử lớn nhất là 3.</li>
	<li><code>[2, 3, 4]</code> có phần tử lớn nhất là 4.</li>
	<li><code>[3, 4, 3]</code> có các phần tử <strong>không liên tiếp</strong>.</li>
	<li><code>[4, 3, 2]</code> có các phần tử <strong>không được sắp xếp</strong>.</li>
	<li><code>[3, 2, 5]</code> có các phần tử <strong>không liên tiếp</strong>.</li>
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
	<li><code>1 &lt;= n == nums.length &lt;= 500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Power của một cửa sổ là phần tử lớn nhất khi các phần tử tăng liên tiếp, nếu không thì là $-1$. Với $n\le 500$, ta có thể duyệt lại từng cửa sổ, nhưng các cửa sổ liên tiếp dùng chung một đoạn tăng.
>
> Gọi $f[i]$ là độ dài của đoạn tăng liên tiếp kết thúc tại $i$. Khi đó $f[i]\ge k$ khi và chỉ khi $[i-k+1,i]$ hợp lệ, và power là $\textit{nums}[i]$. Chỉ cần một công thức truy hồi, sau đó sinh kết quả theo điểm kết thúc.

<!-- thinking:end -->

Ta định nghĩa một mảng $f$, trong đó $f[i]$ biểu thị độ dài của dãy tăng liên tiếp kết thúc tại phần tử thứ $i$. Ban đầu, $f[i] = 1$.

Tiếp theo, ta duyệt mảng $\textit{nums}$ để tính các giá trị của mảng $f$. Nếu $nums[i] = nums[i - 1] + 1$, thì $f[i] = f[i - 1] + 1$; ngược lại, $f[i] = 1$.

Sau đó, ta duyệt mảng $f$ trong phạm vi $[k - 1, n)$. Nếu $f[i] \ge k$, ta thêm $\textit{nums}[i]$ vào mảng kết quả; ngược lại, ta thêm $-1$.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

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

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu một mảng $f$ có kích thước $O(n)$. Đoạn tăng chỉ cần biết điểm bắt đầu bên trái $j$: đặt lại $j$ thành $i$ khi hiệu giữa hai phần tử kề nhau không bằng $1$. Cửa sổ hợp lệ khi chỉ số bên trái của nó vẫn $\ge j$. Dùng một con trỏ chạy giúp giảm không gian phụ xuống $O(1)$.

<!-- thinking:end -->

Gọi con trỏ $j$ là điểm bắt đầu của đoạn hiện tại, trong đó hiệu giữa mọi cặp phần tử kề nhau đều chính xác bằng $1$. Duyệt mảng từ trái sang phải: nếu $i > 0$ và $\textit{nums}[i] \neq \textit{nums}[i - 1] + 1$, cập nhật $j$ thành $i$.

Khi $i \ge k - 1$, cửa sổ hiện tại là $[i - k + 1,\ i]$. Nếu $i - k + 1 < j$, có một cặp phần tử kề nhau trong cửa sổ có hiệu lớn hơn $1$, nên power là $-1$; ngược lại, cửa sổ gồm các số nguyên liên tiếp theo thứ tự tăng dần, và power là giá trị lớn nhất $\textit{nums}[i]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### TypeScript

```ts
export function resultsArray(nums: number[], k: number): number[] {
    const n = nums.length;
    const ans: number[] = [];

    for (let i = 0, j = 0; i < n; i++) {
        if (i && nums[i - 1] + 1 !== nums[i]) j = i;
        if (i >= k - 1) {
            ans.push(i - k + 1 < j ? -1 : nums[i]);
        }
    }

    return ans;
}
```

#### JavaScript

```js
export function resultsArray(nums, k) {
    const n = nums.length;
    const ans = [];

    for (let i = 0, j = 0; i < n; i++) {
        if (i && nums[i - 1] + 1 !== nums[i]) j = i;
        if (i >= k - 1) {
            ans.push(i - k + 1 < j ? -1 : nums[i]);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
