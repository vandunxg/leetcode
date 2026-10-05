---
comments: true
difficulty: Medium
rating: 1531
source: Biweekly Contest 175 Q2
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3824. Minimum K to Reduce Array Within Limit](https://leetcode.com/problems/minimum-k-to-reduce-array-within-limit)

[中文文档](/solution/3800-3899/3824.Minimum%20K%20to%20Reduce%20Array%20Within%20Limit/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>dương</strong> <code>nums</code>.</p>

<p>Với một số nguyên <code>k</code> dương, định nghĩa <code>nonPositive(nums, k)</code> là số <strong>thao tác</strong> <strong>nhỏ nhất</strong> cần thực hiện để mọi phần tử của <code>nums</code> trở thành <strong>không dương</strong>. Trong một thao tác, bạn có thể chọn một chỉ số <code>i</code> và giảm <code>nums[i]</code> đi <code>k</code>.</p>

<p>Trả về một số nguyên biểu thị giá trị <code>k</code> <strong>nhỏ nhất</strong> sao cho <code>nonPositive(nums, k) &lt;= k<sup>2</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,7,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Khi <code>k = 3</code>, <code>nonPositive(nums, k) = 6 &lt;= k<sup>2</sup></code>.</p>

<ul>
	<li>Giảm <code>nums[0] = 3</code> một lần. <code>nums[0]</code> trở thành <code>3 - 3 = 0</code>.</li>
	<li>Giảm <code>nums[1] = 7</code> ba lần. <code>nums[1]</code> trở thành <code>7 - 3 - 3 - 3 = -2</code>.</li>
	<li>Giảm <code>nums[2] = 5</code> hai lần. <code>nums[2]</code> trở thành <code>5 - 3 - 3 = -1</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Khi <code>k = 1</code>, <code>nonPositive(nums, k) = 1 &lt;= k<sup>2</sup></code>.</p>

<ul>
	<li>Giảm <code>nums[0] = 1</code> một lần. <code>nums[0]</code> trở thành <code>1 - 1 = 0</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{nonPositive}(\textit{nums},k)$ là số lần trừ $k$ cần thực hiện để mọi phần tử trở thành không dương. Ta cần tìm $k$ nhỏ nhất sao cho số lần đó $\le k^2$, với $n \le 10^5$.
>
> Khi $k$ lớn hơn, số lần thực hiện chỉ giảm còn $k^2$ tăng, nên điều kiện khả thi có tính đơn điệu.
>
> Với một $k$ cố định, số lần thực hiện là $\sum \lceil nums[i]/k \rceil$. Ta có thể tìm kiếm nhị phân giá trị $k$ khả thi nhỏ nhất trong đoạn $[1,10^5]$.
>
> Mỗi lần kiểm tra duyệt qua mảng một lần, nên độ phức tạp tổng thể là $O(n \log M)$.

<!-- thinking:end -->

Ta nhận thấy khi $k$ tăng, việc thỏa mãn điều kiện trở nên dễ hơn. Điều này cho thấy tính đơn điệu, vì vậy ta có thể dùng tìm kiếm nhị phân để tìm $k$ nhỏ nhất.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 1$ và biên phải là $r = 10^5$. Trong mỗi lần lặp, ta tính giá trị giữa $mid = \lfloor (l + r) / 2 \rfloor$ và kiểm tra xem điều kiện $\text{nonPositive}(\text{nums}, k) \leq k^2$ có được thỏa mãn khi $k = mid$ hay không. Nếu điều kiện được thỏa mãn, ta cập nhật biên phải thành $r = mid$; ngược lại, ta cập nhật biên trái thành $l = mid + 1$. Khi tìm kiếm nhị phân kết thúc, biên trái $l$ chính là $k$ nhỏ nhất cần tìm.

Độ phức tạp thời gian là $O(n \log M)$, trong đó $n$ và $M$ lần lượt là độ dài của mảng $\textit{nums}$ và miền giá trị lớn nhất. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumK(self, nums: List[int]) -> int:
        def check(k: int) -> bool:
            t = 0
            for x in nums:
                t += (x + k - 1) // k
            return t <= k * k

        l, r = 1, 10**5
        while l < r:
            mid = (l + r) >> 1
            if check(mid):
                r = mid
            else:
                l = mid + 1
        return l
```

#### Java

```java
class Solution {
    public int minimumK(int[] nums) {
        int l = 1, r = 100000;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(nums, mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private boolean check(int[] nums, int k) {
        long t = 0;
        for (int x : nums) {
            t += (x + k - 1) / k;
        }
        return t <= 1L * k * k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumK(vector<int>& nums) {
        auto check = [&](int k) -> bool {
            long long t = 0;
            for (int x : nums) {
                t += (x + k - 1) / k;
            }
            return t <= 1LL * k * k;
        };

        int l = 1, r = 1e5;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func minimumK(nums []int) int {
	check := func(k int) bool {
		t := 0
		for _, x := range nums {
			t += (x + k - 1) / k
		}
		return t <= k*k
	}

	return sort.Search(100000, func(k int) bool {
		if k == 0 {
			return false
		}
		return check(k)
	})
}
```

#### TypeScript

```ts
function minimumK(nums: number[]): number {
    const check = (k: number): boolean => {
        let t = 0;
        for (const x of nums) {
            t += Math.floor((x + k - 1) / k);
        }
        return t <= k * k;
    };

    let l = 1,
        r = 100000;
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
