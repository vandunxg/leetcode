---
comments: true
difficulty: Easy
rating: 1245
source: Biweekly Contest 3 Q1
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [1099. Two Sum Less Than K 🔒](https://leetcode.com/problems/two-sum-less-than-k)

[中文文档](/solution/1000-1099/1099.Two%20Sum%20Less%20Than%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về tổng <code>sum</code> lớn nhất sao cho tồn tại <code>i &lt; j</code> thỏa mãn <code>nums[i] + nums[j] = sum</code> và <code>sum &lt; k</code>. Nếu không tồn tại <code>i</code>, <code>j</code> nào thỏa mãn điều kiện này, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [34,23,1,24,75,33,54,8], k = 60
<strong>Đầu ra:</strong> 58
<strong>Giải thích: </strong>Ta có thể chọn 34 và 24 để được tổng 58, nhỏ hơn 60.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,20,30], k = 15
<strong>Đầu ra:</strong> -1
<strong>Giải thích: </strong>Trong trường hợp này, không thể tìm được cặp số có tổng nhỏ hơn 15.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Binary search

<!-- thinking:start -->

> **Tư duy**
>
> Nếu thử mọi cặp, việc tìm tổng lớn nhất nhỏ hơn nghiêm ngặt $k$ có độ phức tạp bậc hai. Sau khi sắp xếp, với mỗi $x$, ta cần tìm giá trị lớn nhất ở bên phải nhỏ hơn $k-x$, có thể thực hiện bằng binary search.
>
> Với chỉ số $i$, `bisect_left` trên đoạn $(i,n)$ tìm vị trí của $k-x$; nếu chỉ số ngay trước đó vẫn lớn hơn $i$, ta dùng nó để cập nhật đáp án.
>
> Nếu không có cặp nào thỏa mãn, đáp án vẫn là $-1$.

<!-- thinking:end -->

Trước tiên, sắp xếp mảng $nums$ và khởi tạo đáp án bằng $-1$.

Tiếp theo, với mỗi phần tử $nums[i]$, tìm giá trị $nums[j]$ lớn nhất sao cho $nums[j] + nums[i] < k$. Ta có thể dùng binary search để tăng tốc quá trình tìm kiếm. Nếu tìm được $nums[j]$ như vậy, cập nhật đáp án theo $ans = \max(ans, nums[i] + nums[j])$.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def twoSumLessThanK(self, nums: List[int], k: int) -> int:
        nums.sort()
        ans = -1
        for i, x in enumerate(nums):
            j = bisect_left(nums, k - x, lo=i + 1) - 1
            if i < j:
                ans = max(ans, x + nums[j])
        return ans
```

#### Java

```java
class Solution {
    public int twoSumLessThanK(int[] nums, int k) {
        Arrays.sort(nums);
        int ans = -1;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            int j = search(nums, k - nums[i], i + 1, n) - 1;
            if (i < j) {
                ans = Math.max(ans, nums[i] + nums[j]);
            }
        }
        return ans;
    }

    private int search(int[] nums, int x, int l, int r) {
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int twoSumLessThanK(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int ans = -1, n = nums.size();
        for (int i = 0; i < n; ++i) {
            int j = lower_bound(nums.begin() + i + 1, nums.end(), k - nums[i]) - nums.begin() - 1;
            if (i < j) {
                ans = max(ans, nums[i] + nums[j]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func twoSumLessThanK(nums []int, k int) int {
	sort.Ints(nums)
	ans := -1
	for i, x := range nums {
		j := sort.SearchInts(nums[i+1:], k-x) + i
		if v := nums[i] + nums[j]; i < j && ans < v {
			ans = v
		}
	}
	return ans
}
```

#### TypeScript

```ts
function twoSumLessThanK(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    let ans = -1;
    for (let i = 0, j = nums.length - 1; i < j;) {
        const s = nums[i] + nums[j];
        if (s < k) {
            ans = Math.max(ans, s);
            ++i;
        } else {
            --j;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Two pointers

<!-- thinking:start -->

> **Tư duy**
>
> Thực hiện một lần binary search cho mỗi $x$ có độ phức tạp $O(n\log n)$. Two pointers tận dụng tính đơn điệu: nếu tổng còn nhỏ hơn $k$, dịch đầu trái sang phải; nếu tổng ít nhất bằng $k$, dịch đầu phải sang trái.
>
> Một lượt duyệt sẽ xét các cặp chỉ số khác nhau nằm trên Pareto front với hằng số nhỏ hơn.

<!-- thinking:end -->

Tương tự lời giải 1, trước tiên sắp xếp mảng $nums$ và khởi tạo đáp án bằng $-1$.

Tiếp theo, dùng two pointers $i$ và $j$ lần lượt trỏ đến hai đầu mảng. Mỗi lần, kiểm tra tổng $s = nums[i] + nums[j]$ có nhỏ hơn $k$ hay không. Nếu nhỏ hơn, cập nhật đáp án theo $ans = \max(ans, s)$ rồi dịch $i$ sang phải một vị trí; nếu không, dịch $j$ sang trái một vị trí.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def twoSumLessThanK(self, nums: List[int], k: int) -> int:
        nums.sort()
        i, j = 0, len(nums) - 1
        ans = -1
        while i < j:
            if (s := nums[i] + nums[j]) < k:
                ans = max(ans, s)
                i += 1
            else:
                j -= 1
        return ans
```

#### Java

```java
class Solution {
    public int twoSumLessThanK(int[] nums, int k) {
        Arrays.sort(nums);
        int ans = -1;
        for (int i = 0, j = nums.length - 1; i < j;) {
            int s = nums[i] + nums[j];
            if (s < k) {
                ans = Math.max(ans, s);
                ++i;
            } else {
                --j;
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
    int twoSumLessThanK(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int ans = -1;
        for (int i = 0, j = nums.size() - 1; i < j;) {
            int s = nums[i] + nums[j];
            if (s < k) {
                ans = max(ans, s);
                ++i;
            } else {
                --j;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func twoSumLessThanK(nums []int, k int) int {
	sort.Ints(nums)
	ans := -1
	for i, j := 0, len(nums)-1; i < j; {
		if s := nums[i] + nums[j]; s < k {
			ans = max(ans, s)
			i++
		} else {
			j--
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
