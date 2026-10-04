---
comments: true
difficulty: Medium
rating: 2081
source: Weekly Contest 331 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [2560. House Robber IV](https://leetcode.com/problems/house-robber-iv)

[中文文档](/solution/2500-2599/2560.House%20Robber%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Có một dãy nhà liên tiếp trên một con phố, mỗi nhà chứa một số tiền. Ngoài ra có một tên trộm muốn lấy tiền từ các căn nhà, nhưng hắn <strong>không trộm các căn nhà liền kề nhau</strong>.</p>

<p><strong>Khả năng</strong> của tên trộm là số tiền lớn nhất mà hắn lấy từ một căn nhà trong tất cả các căn nhà đã trộm.</p>

<p>Bạn được cho một mảng số nguyên <code>nums</code> biểu thị số tiền được cất trong mỗi căn nhà. Cụ thể hơn, căn nhà thứ <code>i<sup>th</sup></code> tính từ bên trái có <code>nums[i]</code> đô la.</p>

<p>Bạn cũng được cho một số nguyên <code>k</code>, biểu thị <strong>số căn nhà tối thiểu</strong> mà tên trộm sẽ lấy tiền. Luôn có thể trộm ít nhất <code>k</code> căn nhà.</p>

<p>Trả về <em><strong>khả năng nhỏ nhất</strong> của tên trộm trong tất cả các cách trộm ít nhất </em><code>k</code><em> căn nhà có thể thực hiện.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,5,9], k = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Có ba cách trộm ít nhất 2 căn nhà:
- Trộm các căn nhà ở chỉ số 0 và 2. Khả năng là max(nums[0], nums[2]) = 5.
- Trộm các căn nhà ở chỉ số 0 và 3. Khả năng là max(nums[0], nums[3]) = 9.
- Trộm các căn nhà ở chỉ số 1 và 3. Khả năng là max(nums[1], nums[3]) = 9.
Vì vậy, ta trả về min(5, 9, 9) = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,7,9,3,1], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 7 cách trộm các căn nhà. Cách cho khả năng nhỏ nhất là trộm căn nhà ở chỉ số 0 và 4. Trả về max(nums[0], nums[4]) = 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= (nums.length + 1)/2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Khả năng là giá trị lớn nhất trong các căn nhà bị trộm; các căn nhà không được liền kề nhau, phải trộm ít nhất $k$ căn, và cần tối thiểu hóa giá trị lớn nhất đó. Duyệt mọi tập hợp không thể đáp ứng với $n\le 10^5$.
>
> $x$ càng lớn thì việc đạt đủ số lượng yêu cầu càng dễ, nên ta tìm kiếm nhị phân trên $x$. Khi kiểm tra, ta duyệt từ trái sang phải và trộm ngay khi giá trị $\le x$ và căn nhà được chọn trước đó không liền kề. Chọn đủ $k$ căn nhà chứng minh rằng điều kiện khả thi; $\textit{bisect\_left}$ trả về $x$ nhỏ nhất như vậy.

<!-- thinking:end -->

Bài toán yêu cầu tìm khả năng trộm nhỏ nhất của tên trộm. Ta có thể dùng tìm kiếm nhị phân để duyệt qua khả năng trộm của tên trộm. Với khả năng đang xét $x$, ta dùng phương pháp tham lam để xác định xem tên trộm có thể trộm ít nhất $k$ căn nhà hay không. Cụ thể, ta duyệt mảng từ trái sang phải. Với căn nhà hiện tại $i$, nếu $nums[i] \leq x$ và khoảng cách giữa chỉ số của $i$ với căn nhà bị trộm gần nhất lớn hơn $1$, tên trộm có thể trộm căn nhà $i$. Nếu không, tên trộm không thể trộm căn nhà $i$. Ta cộng dồn số căn nhà đã trộm. Nếu số căn nhà đã trộm lớn hơn hoặc bằng $k$, nghĩa là tên trộm có thể trộm ít nhất $k$ căn nhà, và khi đó khả năng trộm $x$ có thể là nhỏ nhất. Ngược lại, khả năng trộm $x$ không phải là nhỏ nhất.

Độ phức tạp thời gian là $O(n \times \log m)$, còn độ phức tạp không gian là $O(1)$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng $nums$ và giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCapability(self, nums: List[int], k: int) -> int:
        def f(x):
            cnt, j = 0, -2
            for i, v in enumerate(nums):
                if v > x or i == j + 1:
                    continue
                cnt += 1
                j = i
            return cnt >= k

        return bisect_left(range(max(nums) + 1), True, key=f)
```

#### Java

```java
class Solution {
    public int minCapability(int[] nums, int k) {
        int left = 0, right = (int) 1e9;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (f(nums, mid) >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private int f(int[] nums, int x) {
        int cnt = 0, j = -2;
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] > x || i == j + 1) {
                continue;
            }
            ++cnt;
            j = i;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCapability(vector<int>& nums, int k) {
        auto f = [&](int x) {
            int cnt = 0, j = -2;
            for (int i = 0; i < nums.size(); ++i) {
                if (nums[i] > x || i == j + 1) {
                    continue;
                }
                ++cnt;
                j = i;
            }
            return cnt >= k;
        };
        int left = 0, right = *max_element(nums.begin(), nums.end());
        while (left < right) {
            int mid = (left + right) >> 1;
            if (f(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func minCapability(nums []int, k int) int {
	return sort.Search(1e9+1, func(x int) bool {
		cnt, j := 0, -2
		for i, v := range nums {
			if v > x || i == j+1 {
				continue
			}
			cnt++
			j = i
		}
		return cnt >= k
	})
}
```

#### TypeScript

```ts
function minCapability(nums: number[], k: number): number {
    const f = (mx: number): boolean => {
        let cnt = 0;
        let j = -2;
        for (let i = 0; i < nums.length; ++i) {
            if (nums[i] <= mx && i - j > 1) {
                ++cnt;
                j = i;
            }
        }
        return cnt >= k;
    };

    let left = 1;
    let right = Math.max(...nums);
    while (left < right) {
        const mid = (left + right) >> 1;
        if (f(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
