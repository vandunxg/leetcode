---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Binary Search
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum)

[中文文档](/solution/0400-0499/0410.Split%20Array%20Largest%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy chia <code>nums</code> thành <code>k</code> mảng con không rỗng sao cho tổng lớn nhất trong các mảng con là <strong>nhỏ nhất có thể</strong>.</p>

<p>Hãy trả về <em>tổng lớn nhất nhỏ nhất có thể sau khi chia mảng</em>.</p>

<p><strong>Mảng con</strong> là một đoạn liên tiếp của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,2,5,10,8], k = 2
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Có bốn cách chia mảng thành hai mảng con.
Cách chia tốt nhất là thành [7,2,5] và [10,8], khi đó tổng lớn nhất trong hai mảng con chỉ là 18.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5], k = 2
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Có bốn cách chia mảng thành hai mảng con.
Cách chia tốt nhất là thành [1,2,3] và [4,5], khi đó tổng lớn nhất trong hai mảng con chỉ là 9.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= min(50, nums.length)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Có quá nhiều cách đặt $k-1$ điểm chia để liệt kê hết. Khi giới hạn tổng mỗi mảng con tăng, việc chia mảng chỉ càng dễ thỏa mãn hơn, nên tính khả thi đơn điệu theo giới hạn này.
>
> Tìm kiếm nhị phân trên giới hạn tổng $\textit{mid}$. Duyệt tham lam từ trái sang phải, mỗi khi thêm phần tử tiếp theo khiến tổng vượt quá $\textit{mid}$ thì bắt đầu một mảng con mới; sau đó kiểm tra có thể chia thành không quá $k$ mảng con hay không. Phạm vi tìm kiếm là $[\max(\textit{nums}),\sum \textit{nums}]$.
>
> Giới hạn nhỏ nhất vẫn khả thi chính là đáp án; bài toán tổ hợp được chuyển thành một bước kiểm tra tuyến tính.

<!-- thinking:end -->

Ta nhận thấy giới hạn tổng lớn nhất của các mảng con càng cao thì càng cần ít mảng con. Nếu một giới hạn tổng thỏa mãn điều kiện, mọi giới hạn lớn hơn cũng chắc chắn thỏa mãn. Vì vậy, ta có thể tìm kiếm nhị phân trên giới hạn tổng lớn nhất để tìm giá trị nhỏ nhất thỏa mãn điều kiện.

Ta đặt biên trái của tìm kiếm nhị phân là $left = \max(nums)$ và biên phải là $right = sum(nums)$. Ở mỗi bước, lấy giá trị giữa $mid = \lfloor \frac{left + right}{2} \rfloor$, rồi kiểm tra có thể chia mảng sao cho tổng lớn nhất của các mảng con không vượt quá $mid$ hay không. Nếu có, $mid$ có thể là đáp án nhỏ nhất thỏa mãn điều kiện nên cập nhật biên phải thành $mid$. Nếu không, cập nhật biên trái thành $mid + 1$.

Làm thế nào để kiểm tra có thể chia mảng sao cho tổng lớn nhất của các mảng con không vượt quá $mid$? Ta duyệt tham lam từ trái sang phải và lần lượt cộng các phần tử vào mảng con hiện tại. Nếu cộng phần tử đang xét khiến tổng vượt quá $mid$, ta bắt đầu mảng con tiếp theo từ phần tử đó. Nếu có thể chia mảng thành không quá $k$ mảng con và tổng mỗi mảng con không vượt quá $mid$, thì $mid$ thỏa mãn điều kiện; ngược lại thì không.

Độ phức tạp thời gian là $O(n \times \log m)$ và độ phức tạp không gian là $O(1)$. Ở đây, $n$ là độ dài mảng, còn $m$ là tổng tất cả phần tử trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitArray(self, nums: List[int], k: int) -> int:
        def check(mx):
            s, cnt = inf, 0
            for x in nums:
                s += x
                if s > mx:
                    s = x
                    cnt += 1
            return cnt <= k

        left, right = max(nums), sum(nums)
        return left + bisect_left(range(left, right + 1), True, key=check)
```

#### Java

```java
class Solution {
    public int splitArray(int[] nums, int k) {
        int left = 0, right = 0;
        for (int x : nums) {
            left = Math.max(left, x);
            right += x;
        }
        while (left < right) {
            int mid = (left + right) >> 1;
            if (check(nums, mid, k)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private boolean check(int[] nums, int mx, int k) {
        int s = 1 << 30, cnt = 0;
        for (int x : nums) {
            s += x;
            if (s > mx) {
                ++cnt;
                s = x;
            }
        }
        return cnt <= k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int splitArray(vector<int>& nums, int k) {
        int left = 0, right = 0;
        for (int& x : nums) {
            left = max(left, x);
            right += x;
        }
        auto check = [&](int mx) {
            int s = 1 << 30, cnt = 0;
            for (int& x : nums) {
                s += x;
                if (s > mx) {
                    s = x;
                    ++cnt;
                }
            }
            return cnt <= k;
        };
        while (left < right) {
            int mid = (left + right) >> 1;
            if (check(mid)) {
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
func splitArray(nums []int, k int) int {
	left, right := 0, 0
	for _, x := range nums {
		left = max(left, x)
		right += x
	}
	return left + sort.Search(right-left, func(mx int) bool {
		mx += left
		s, cnt := 1<<30, 0
		for _, x := range nums {
			s += x
			if s > mx {
				s = x
				cnt++
			}
		}
		return cnt <= k
	})
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var splitArray = function (nums, k) {
    let l = Math.max(...nums);
    let r = nums.reduce((a, b) => a + b);

    const check = mx => {
        let [s, cnt] = [0, 0];
        for (const x of nums) {
            s += x;
            if (s > mx) {
                s = x;
                if (++cnt === k) return false;
            }
        }
        return true;
    };

    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
};
```

#### TypeScript

```ts
function splitArray(nums: number[], k: number): number {
    let l = Math.max(...nums);
    let r = nums.reduce((a, b) => a + b);

    const check = (mx: number) => {
        let [s, cnt] = [0, 0];
        for (const x of nums) {
            s += x;
            if (s > mx) {
                s = x;
                if (++cnt === k) return false;
            }
        }
        return true;
    };

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
