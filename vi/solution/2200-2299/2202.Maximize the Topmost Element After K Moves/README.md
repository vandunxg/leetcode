---
comments: true
difficulty: Medium
rating: 1716
source: Weekly Contest 284 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2202. Maximize the Topmost Element After K Moves](https://leetcode.com/problems/maximize-the-topmost-element-after-k-moves)

[中文文档](/solution/2200-2299/2202.Maximize%20the%20Topmost%20Element%20After%20K%20Moves/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> biểu diễn các phần tử trong một <b>chồng</b>, trong đó <code>nums[0]</code> là phần tử trên cùng của chồng.</p>

<p>Trong một thao tác, bạn có thể thực hiện <strong>một trong hai</strong> việc sau:</p>

<ul>
	<li>Nếu chồng không rỗng, <strong>lấy</strong> phần tử trên cùng của chồng ra.</li>
	<li>Nếu đã lấy ra ít nhất một phần tử, <strong>thêm</strong> một phần tử bất kỳ trong số đó trở lại chồng. Phần tử này trở thành phần tử trên cùng mới.</li>
</ul>

<p>Bạn cũng được cho một số nguyên <code>k</code>, biểu thị tổng số thao tác cần thực hiện.</p>

<p>Trả về <em><strong>giá trị lớn nhất</strong> có thể có của phần tử trên cùng sau khi thực hiện <strong>đúng</strong></em> <code>k</code> <em>thao tác</em>. Nếu không thể tạo ra một chồng không rỗng sau <code>k</code> thao tác, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,2,2,4,0,6], k = 4
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Một cách để phần tử trên cùng của chồng là 5 sau 4 thao tác như sau:
- Bước 1: Lấy phần tử trên cùng ra = 5. Chồng trở thành [2,2,4,0,6].
- Bước 2: Lấy phần tử trên cùng ra = 2. Chồng trở thành [2,4,0,6].
- Bước 3: Lấy phần tử trên cùng ra = 2. Chồng trở thành [4,0,6].
- Bước 4: Thêm 5 trở lại chồng. Chồng trở thành [5,4,0,6].
Lưu ý rằng đây không phải cách duy nhất để kết thúc với 5 ở trên cùng. Có thể chứng minh rằng 5 là đáp án lớn nhất có thể đạt được sau 4 thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2], k = 1
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Ở thao tác đầu tiên, lựa chọn duy nhất của chúng ta là lấy phần tử trên cùng của chồng ra.
Vì không thể tạo ra một chồng không rỗng sau một thao tác, ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i], k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác lấy phần tử trên cùng ra hoặc thêm lại một giá trị đã lấy ra trước đó. $k$ có thể lên tới $10^9$, nên không thể mô phỏng từng bước. Phần tử trên cùng sau đúng $k$ thao tác chỉ rơi vào một vài trường hợp có thể so sánh.
>
> Nếu $k = 0$, phần tử trên cùng là $nums[0]$. Với mảng chỉ có một phần tử, sau một số lẻ thao tác thì giá trị duy nhất đã bị lấy ra và kết quả là $-1$, còn sau một số chẵn thao tác thì phần tử trên cùng vẫn là chính nó.
>
> Khi $n \ge 2$, bất kỳ giá trị nào trong số $k-1$ giá trị đầu tiên bị lấy ra cũng có thể được thêm lại ở thao tác cuối, nên một ứng viên là $\max(nums[0..k-2])$. Nếu $k < n$, lần lấy ra thứ $k$ cũng có thể để lộ $nums[k]$. Chọn giá trị lớn hơn trong hai ứng viên này.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTop(self, nums: List[int], k: int) -> int:
        if k == 0:
            return nums[0]
        n = len(nums)
        if n == 1:
            if k % 2:
                return -1
            return nums[0]
        ans = max(nums[: k - 1], default=-1)
        if k < n:
            ans = max(ans, nums[k])
        return ans
```

#### Java

```java
class Solution {
    public int maximumTop(int[] nums, int k) {
        if (k == 0) {
            return nums[0];
        }
        int n = nums.length;
        if (n == 1) {
            if (k % 2 == 1) {
                return -1;
            }
            return nums[0];
        }
        int ans = -1;
        for (int i = 0; i < Math.min(k - 1, n); ++i) {
            ans = Math.max(ans, nums[i]);
        }
        if (k < n) {
            ans = Math.max(ans, nums[k]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumTop(vector<int>& nums, int k) {
        if (k == 0) return nums[0];
        int n = nums.size();
        if (n == 1) {
            if (k % 2) return -1;
            return nums[0];
        }
        int ans = -1;
        for (int i = 0; i < min(k - 1, n); ++i) ans = max(ans, nums[i]);
        if (k < n) ans = max(ans, nums[k]);
        return ans;
    }
};
```

#### Go

```go
func maximumTop(nums []int, k int) int {
	if k == 0 {
		return nums[0]
	}
	n := len(nums)
	if n == 1 {
		if k%2 == 1 {
			return -1
		}
		return nums[0]
	}
	ans := -1
	for i := 0; i < min(k-1, n); i++ {
		ans = max(ans, nums[i])
	}
	if k < n {
		ans = max(ans, nums[k])
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
