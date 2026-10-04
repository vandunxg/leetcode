---
comments: true
difficulty: Medium
rating: 1604
source: Weekly Contest 392 Q3
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3107. Minimum Operations to Make Median of Array Equal to K](https://leetcode.com/problems/minimum-operations-to-make-median-of-array-equal-to-k)

[中文文档](/solution/3100-3199/3107.Minimum%20Operations%20to%20Make%20Median%20of%20Array%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>không âm</strong> <code>k</code>. Trong một thao tác, bạn có thể tăng hoặc giảm một phần tử bất kỳ đi 1.</p>

<p>Trả về số thao tác <strong>ít nhất</strong> cần thực hiện để <strong>trung vị</strong> của <code>nums</code> <em>bằng</em> <code>k</code>.</p>

<p>Trung vị của một mảng được định nghĩa là phần tử ở giữa của mảng khi được sắp xếp theo thứ tự không giảm. Nếu có hai lựa chọn cho trung vị, chọn giá trị lớn hơn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,6,8,5], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể trừ một khỏi <code>nums[1]</code> và <code>nums[4]</code> để thu được <code>[2, 4, 6, 8, 4]</code>. Trung vị của mảng kết quả bằng <code>k</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,6,8,5], k = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể cộng một vào <code>nums[1]</code> hai lần và cộng một vào <code>nums[2]</code> một lần để thu được <code>[2, 7, 7, 8, 5]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5,6], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trung vị của mảng đã bằng <code>k</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Trung vị là giá trị ở giữa sau khi sắp xếp. Việc liệt kê các phần tử cần thay đổi khi chưa sắp xếp mảng sẽ khiến số trường hợp tăng theo cấp số tổ hợp; với $n$ như trong đề bài, ta có thể sắp xếp trong $O(n\log n)$.
>
> Phần tử ở giữa phải trở thành $k$. Nếu nó lớn hơn $k$, chỉ cần giảm các giá trị bên trái vẫn lớn hơn $k$; nếu nó không lớn hơn, chỉ cần tăng các giá trị bên phải vẫn nhỏ hơn $k$.
>
> Sắp xếp mảng, lấy chỉ số giữa $m$, cộng $|nums[m]-k|$, sau đó duyệt phía tương ứng cho đến khi mọi giá trị đều thỏa mãn điều kiện với $k$. Tổng số tăng thêm chính là số thao tác ít nhất.

<!-- thinking:end -->

Trước hết, ta sắp xếp mảng $nums$ và tìm vị trí $m$ của trung vị. Số thao tác ban đầu cần thực hiện là $|nums[m] - k|$.

Tiếp theo, ta xét hai trường hợp:

- Nếu $nums[m] > k$, thì mọi phần tử bên phải của $m$ đều lớn hơn hoặc bằng $k$. Ta chỉ cần giảm các phần tử lớn hơn $k$ ở bên trái $m$ xuống $k$.
- Nếu $nums[m] \le k$, thì mọi phần tử bên trái của $m$ đều nhỏ hơn hoặc bằng $k$. Ta chỉ cần tăng các phần tử nhỏ hơn $k$ ở bên phải $m$ lên $k$.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperationsToMakeMedianK(self, nums: List[int], k: int) -> int:
        nums.sort()
        n = len(nums)
        m = n >> 1
        ans = abs(nums[m] - k)
        if nums[m] > k:
            for i in range(m - 1, -1, -1):
                if nums[i] <= k:
                    break
                ans += nums[i] - k
        else:
            for i in range(m + 1, n):
                if nums[i] >= k:
                    break
                ans += k - nums[i]
        return ans
```

#### Java

```java
class Solution {
    public long minOperationsToMakeMedianK(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int m = n >> 1;
        long ans = Math.abs(nums[m] - k);
        if (nums[m] > k) {
            for (int i = m - 1; i >= 0 && nums[i] > k; --i) {
                ans += nums[i] - k;
            }
        } else {
            for (int i = m + 1; i < n && nums[i] < k; ++i) {
                ans += k - nums[i];
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
    long long minOperationsToMakeMedianK(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int m = n >> 1;
        long long ans = abs(nums[m] - k);
        if (nums[m] > k) {
            for (int i = m - 1; i >= 0 && nums[i] > k; --i) {
                ans += nums[i] - k;
            }
        } else {
            for (int i = m + 1; i < n && nums[i] < k; ++i) {
                ans += k - nums[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minOperationsToMakeMedianK(nums []int, k int) (ans int64) {
	sort.Ints(nums)
	n := len(nums)
	m := n >> 1
	ans = int64(abs(nums[m] - k))
	if nums[m] > k {
		for i := m - 1; i >= 0 && nums[i] > k; i-- {
			ans += int64(nums[i] - k)
		}
	} else {
		for i := m + 1; i < n && nums[i] < k; i++ {
			ans += int64(k - nums[i])
		}
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minOperationsToMakeMedianK(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const m = n >> 1;
    let ans = Math.abs(nums[m] - k);
    if (nums[m] > k) {
        for (let i = m - 1; i >= 0 && nums[i] > k; --i) {
            ans += nums[i] - k;
        }
    } else {
        for (let i = m + 1; i < n && nums[i] < k; ++i) {
            ans += k - nums[i];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
