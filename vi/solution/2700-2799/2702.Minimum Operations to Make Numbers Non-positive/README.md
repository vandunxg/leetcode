---
comments: true
difficulty: Hard
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [2702. Minimum Operations to Make Numbers Non-positive 🔒](https://leetcode.com/problems/minimum-operations-to-make-numbers-non-positive)

[中文文档](/solution/2700-2799/2702.Minimum%20Operations%20to%20Make%20Numbers%20Non-positive/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh <strong>0-indexed</strong> và hai số nguyên <code>x</code>, <code>y</code>. Trong một thao tác, bạn phải chọn một chỉ số <code>i</code> sao cho <code>0 &lt;= i &lt; nums.length</code> và thực hiện các bước sau:</p>

<ul>
	<li>Giảm <code>nums[i]</code> đi <code>x</code>.</li>
	<li>Giảm các giá trị tại mọi chỉ số khác chỉ số thứ <code>i<sup>th</sup></code> đi <code>y</code>.</li>
</ul>

<p>Trả về <em>số thao tác ít nhất để tất cả các số nguyên trong </em><code>nums</code> <em><strong>nhỏ hơn hoặc bằng không.</strong></em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,1,7,6], x = 4, y = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Cần thực hiện ba thao tác. Một dãy thao tác tối ưu là:
Thao tác 1: Chọn i = 3. Khi đó, nums = [1,2,-1,3,4].
Thao tác 2: Chọn i = 3. Khi đó, nums = [-1,0,-3,-1,2].
Thao tác 3: Chọn i = 4. Khi đó, nums = [-3,-2,-5,-3,-2].
Lúc này, tất cả các số trong nums đều không dương. Vì vậy, ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1], x = 2, y = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể thực hiện thao tác một lần với i = 1. Khi đó, nums trở thành [0,0,0]. Tất cả các số dương đã được xử lý, nên ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= y &lt; x &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác trừ $x$ ở một chỉ số và trừ $y$ ở các chỉ số còn lại. Việc liệt kê chỉ số được tác động trong từng thao tác sẽ tăng theo số thao tác, trong khi các giá trị và đáp án đều có thể đạt $10^9$, nên không thể mô phỏng trực tiếp.
>
> Số thao tác khả thi $t$ có tính đơn điệu: nếu $t$ thao tác là đủ thì mọi số thao tác lớn hơn cũng đủ, vì vậy ta tìm kiếm nhị phân trên $t$. Để kiểm tra một giá trị, ta trừ $y$ toàn cục khỏi mỗi phần tử $t$ lần; phần còn dương phải được xử lý thêm bằng các lần tác động có hiệu lực $x-y$. Cộng số lần bổ sung này và so sánh với $t$.

<!-- thinking:end -->

Ta nhận thấy rằng nếu số thao tác $t$ có thể khiến tất cả các số nhỏ hơn hoặc bằng $0$, thì với mọi $t' > t$, số thao tác $t'$ cũng có thể khiến tất cả các số nhỏ hơn hoặc bằng $0$. Vì vậy, ta có thể sử dụng tìm kiếm nhị phân để tìm số thao tác nhỏ nhất.

Ta đặt biên trái của tìm kiếm nhị phân là $l=0$ và biên phải là $r=\max(nums)$. Mỗi lần thực hiện tìm kiếm nhị phân, ta tìm giá trị giữa $mid=\lfloor\frac{l+r}{2}\rfloor$, sau đó xác định xem có cách thực hiện thao tác nào không vượt quá $mid$ để khiến tất cả các số nhỏ hơn hoặc bằng $0$ hay không. Nếu có, ta cập nhật biên phải $r = mid$; nếu không, ta cập nhật biên trái

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], x: int, y: int) -> int:
        def check(t: int) -> bool:
            cnt = 0
            for v in nums:
                if v > t * y:
                    cnt += ceil((v - t * y) / (x - y))
            return cnt <= t

        l, r = 0, max(nums)
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
    private int[] nums;
    private int x;
    private int y;

    public int minOperations(int[] nums, int x, int y) {
        this.nums = nums;
        this.x = x;
        this.y = y;
        int l = 0, r = 0;
        for (int v : nums) {
            r = Math.max(r, v);
        }
        while (l < r) {
            int mid = (l + r) >>> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private boolean check(int t) {
        long cnt = 0;
        for (int v : nums) {
            if (v > (long) t * y) {
                cnt += (v - (long) t * y + x - y - 1) / (x - y);
            }
        }
        return cnt <= t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int x, int y) {
        int l = 0, r = *max_element(nums.begin(), nums.end());
        auto check = [&](int t) {
            long long cnt = 0;
            for (int v : nums) {
                if (v > 1LL * t * y) {
                    cnt += (v - 1LL * t * y + x - y - 1) / (x - y);
                }
            }
            return cnt <= t;
        };
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
func minOperations(nums []int, x int, y int) int {
	check := func(t int) bool {
		cnt := 0
		for _, v := range nums {
			if v > t*y {
				cnt += (v - t*y + x - y - 1) / (x - y)
			}
		}
		return cnt <= t
	}

	l, r := 0, slices.Max(nums)
	for l < r {
		mid := (l + r) >> 1
		if check(mid) {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function minOperations(nums: number[], x: number, y: number): number {
    let l = 0;
    let r = Math.max(...nums);
    const check = (t: number): boolean => {
        let cnt = 0;
        for (const v of nums) {
            if (v > t * y) {
                cnt += Math.ceil((v - t * y) / (x - y));
            }
        }
        return cnt <= t;
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
