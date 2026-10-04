---
comments: true
difficulty: Medium
rating: 1625
source: Weekly Contest 363 Q2
tags:
    - Array
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [2860. Happy Students](https://leetcode.com/problems/happy-students)

[中文文档](/solution/2800-2899/2860.Happy%20Students/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code>, trong đó <code>n</code> là tổng số học sinh trong lớp. Giáo viên chủ nhiệm muốn chọn một nhóm học sinh sao cho tất cả học sinh đều vui.</p>

<p>Học sinh thứ <code>i<sup>th</sup></code> sẽ vui nếu một trong hai điều kiện sau được thỏa mãn:</p>

<ul>
	<li>Học sinh được chọn và tổng số học sinh được chọn <strong>lớn hơn nghiêm ngặt</strong> <code>nums[i]</code>.</li>
	<li>Học sinh không được chọn và tổng số học sinh được chọn <strong>nhỏ hơn</strong> <strong>nghiêm ngặt</strong> <code>nums[i]</code>.</li>
</ul>

<p>Trả về <em>số cách chọn một nhóm học sinh sao cho tất cả học sinh đều vui.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Hai cách có thể là:
Giáo viên chủ nhiệm không chọn học sinh nào.
Giáo viên chủ nhiệm chọn cả hai học sinh để tạo thành nhóm.
Nếu giáo viên chủ nhiệm chỉ chọn một học sinh để tạo thành nhóm thì cả hai học sinh đều không vui. Vì vậy, chỉ có hai cách.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,0,3,3,6,7,2,7]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ba cách có thể là:
Giáo viên chủ nhiệm chọn học sinh có chỉ số = 1 để tạo thành nhóm.
Giáo viên chủ nhiệm chọn các học sinh có chỉ số = 1, 2, 3, 6 để tạo thành nhóm.
Giáo viên chủ nhiệm chọn tất cả học sinh để tạo thành nhóm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một nhóm có kích thước $k$ tồn tại khi và chỉ khi mọi học sinh được chọn đều có $nums[i]<k$ và mọi học sinh không được chọn đều có $nums[i]>k$; một giá trị bằng $k$ sẽ loại trừ $k$ đó. Sau khi sắp xếp, các học sinh được chọn tạo thành một tiền tố, nên chúng ta kiểm tra từng $k\in[0,n]$ tại vị trí phân cách.

<!-- thinking:end -->

Giả sử có $k$ học sinh được chọn, khi đó các điều kiện sau phải được thỏa mãn:

- Nếu $nums[i] = k$ thì không có cách chọn nhóm nào;
- Nếu $nums[i] > k$ thì học sinh $i$ không được chọn;
- Nếu $nums[i] < k$ thì học sinh $i$ được chọn.

Do đó, các học sinh được chọn phải là $k$ phần tử đầu tiên trong mảng $nums$ đã sắp xếp.

Chúng ta liệt kê $k$ trong đoạn $[0,..n]$. Với số học sinh hiện đang được chọn là $i$, ta có thể lấy giá trị lớn nhất của nhóm $i-1$ là $nums[i-1]$. Nếu $i > 0$ và $nums[i-1] \ge i$ thì không có cách chọn nhóm nào; nếu $i < n$ và $nums[i] \le i$ thì cũng không có cách chọn nhóm nào. Ngược lại, có một cách chọn nhóm và đáp án được tăng lên một.

Sau khi kết thúc quá trình liệt kê, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countWays(self, nums: List[int]) -> int:
        nums.sort()
        n = len(nums)
        ans = 0
        for i in range(n + 1):
            if i and nums[i - 1] >= i:
                continue
            if i < n and nums[i] <= i:
                continue
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countWays(List<Integer> nums) {
        Collections.sort(nums);
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i <= n; i++) {
            if ((i == 0 || nums.get(i - 1) < i) && (i == n || nums.get(i) > i)) {
                ans++;
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
    int countWays(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int ans = 0;
        int n = nums.size();
        for (int i = 0; i <= n; ++i) {
            if ((i && nums[i - 1] >= i) || (i < n && nums[i] <= i)) {
                continue;
            }
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func countWays(nums []int) (ans int) {
	sort.Ints(nums)
	n := len(nums)
	for i := 0; i <= n; i++ {
		if (i > 0 && nums[i-1] >= i) || (i < n && nums[i] <= i) {
			continue
		}
		ans++
	}
	return
}
```

#### TypeScript

```ts
function countWays(nums: number[]): number {
    nums.sort((a, b) => a - b);
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i <= n; ++i) {
        if ((i && nums[i - 1] >= i) || (i < n && nums[i] <= i)) {
            continue;
        }
        ++ans;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
