---
comments: true
difficulty: Hard
tags:
    - Array
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [644. Maximum Average Subarray II 🔒](https://leetcode.com/problems/maximum-average-subarray-ii)

[中文文档](/solution/0600-0699/0644.Maximum%20Average%20Subarray%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> gồm <code>n</code> phần tử và một số nguyên <code>k</code>.</p>

<p>Hãy tìm mảng con liên tiếp có <strong>độ dài lớn hơn hoặc bằng</strong> <code>k</code> có giá trị trung bình lớn nhất, rồi trả về <em>giá trị đó</em>. Chấp nhận mọi đáp án có sai số tính toán nhỏ hơn <code>10<sup>-5</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,12,-5,-6,50,3], k = 4
<strong>Đầu ra:</strong> 12.75000
<b>Giải thích:
</b>- Với độ dài 4, các giá trị trung bình là [0.5, 12.75, 10.5] và giá trị trung bình lớn nhất là 12.75
- Với độ dài 5, các giá trị trung bình là [10.4, 10.8] và giá trị trung bình lớn nhất là 10.8
- Với độ dài 6, các giá trị trung bình là [9.16667] và giá trị trung bình lớn nhất là 9.16667
Giá trị trung bình lớn nhất đạt được khi chọn mảng con độ dài 4 (tức mảng con [12, -5, -6, 50]) có giá trị trung bình lớn nhất là 12.75; vì vậy, ta trả về 12.75
Lưu ý rằng không xét các mảng con có độ dài &lt; 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5], k = 1
<strong>Đầu ra:</strong> 5.00000
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị trung bình lớn nhất của các mảng con có độ dài ít nhất $k$ cần duyệt với độ phức tạp bậc hai. Điều kiện tồn tại mảng con có giá trị trung bình $\ge v$ là đơn điệu theo $v$.
>
> Tìm kiếm nhị phân trên $v$, trừ $v$ khỏi từng phần tử, rồi kiểm tra có mảng con độ dài $\ge k$ có tổng không âm bằng prefix minimum hay không.

<!-- thinking:end -->

Ta nhận thấy nếu giá trị trung bình của một mảng con có độ dài ít nhất $k$ là $v$, thì giá trị trung bình lớn nhất phải lớn hơn hoặc bằng $v$; nếu không, giá trị trung bình lớn nhất sẽ nhỏ hơn $v$. Vì vậy, có thể dùng tìm kiếm nhị phân để tìm giá trị trung bình lớn nhất.

Hai biên tìm kiếm là gì? Biên trái $l$ là giá trị nhỏ nhất trong mảng, còn biên phải $r$ là giá trị lớn nhất. Tiếp theo, tìm kiếm nhị phân tại trung điểm $mid$ và kiểm tra xem có mảng con độ dài ít nhất $k$ nào có giá trị trung bình lớn hơn hoặc bằng $mid$ hay không. Nếu có, cập nhật biên trái $l$ thành $mid$; nếu không, cập nhật biên phải $r$ thành $mid$. Khi hiệu giữa hai biên nhỏ hơn một số không âm rất nhỏ, tức $r - l < \epsilon$, ta thu được giá trị trung bình lớn nhất. Ở đây, $\epsilon$ là một số dương rất nhỏ, có thể chọn bằng $10^{-5}$.

Mấu chốt của bài toán là kiểm tra xem giá trị trung bình của một mảng con có độ dài ít nhất $k$ có lớn hơn hoặc bằng $v$ hay không.

Giả sử trong mảng $nums$ có một mảng con độ dài $j$, gồm các phần tử $a_1, a_2, \cdots, a_j$, và giá trị trung bình của nó lớn hơn hoặc bằng $v$, tức là

$$
\frac{a_1 + a_2 + \cdots + a_j}{j} \geq v
$$

Khi đó,

$$
a_1 + a_2 + \cdots + a_j \geq v \times j
$$

Tức là,

$$
(a_1 - v) + (a_2 - v) + \cdots + (a_j - v) \geq 0
$$

Nếu trừ $v$ khỏi mỗi phần tử trong mảng $nums$, bài toán ban đầu trở thành kiểm tra xem tổng các phần tử của một mảng con có độ dài ít nhất $k$ có lớn hơn hoặc bằng $0$ hay không. Có thể dùng cửa sổ trượt để giải bài toán này.

Trước tiên, tính tổng $s$ của hiệu giữa $k$ phần tử đầu tiên và $v$. Nếu $s \geq 0$, tồn tại mảng con độ dài ít nhất $k$ có tổng phần tử lớn hơn hoặc bằng $0$.

Nếu không, tiếp tục duyệt các phần tử $nums[j]$. Gọi tổng hiệu giữa $j$ phần tử đầu tiên và $v$ hiện tại là $s_j$. Ta duy trì giá trị nhỏ nhất $mi$ của tổng hiệu giữa prefix sum và $v$ trong phạm vi $[0,..j-k]$. Nếu tồn tại $s_j \geq mi$, tức là có mảng con độ dài ít nhất $k$ với tổng phần tử lớn hơn hoặc bằng $0$; khi đó trả về $true$.

Nếu không, tiếp tục duyệt các phần tử $nums[j]$ cho đến hết mảng.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài mảng $nums$ và hiệu giữa giá trị lớn nhất và nhỏ nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        def check(v: float) -> bool:
            s = sum(nums[:k]) - k * v
            if s >= 0:
                return True
            t = mi = 0
            for i in range(k, len(nums)):
                s += nums[i] - v
                t += nums[i - k] - v
                mi = min(mi, t)
                if s >= mi:
                    return True
            return False

        eps = 1e-5
        l, r = min(nums), max(nums)
        while r - l >= eps:
            mid = (l + r) / 2
            if check(mid):
                l = mid
            else:
                r = mid
        return l
```

#### Java

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        double eps = 1e-5;
        double l = 1e10, r = -1e10;
        for (int x : nums) {
            l = Math.min(l, x);
            r = Math.max(r, x);
        }
        while (r - l >= eps) {
            double mid = (l + r) / 2;
            if (check(nums, k, mid)) {
                l = mid;
            } else {
                r = mid;
            }
        }
        return l;
    }

    private boolean check(int[] nums, int k, double v) {
        double s = 0;
        for (int i = 0; i < k; ++i) {
            s += nums[i] - v;
        }
        if (s >= 0) {
            return true;
        }
        double t = 0;
        double mi = 0;
        for (int i = k; i < nums.length; ++i) {
            s += nums[i] - v;
            t += nums[i - k] - v;
            mi = Math.min(mi, t);
            if (s >= mi) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double eps = 1e-5;
        double l = *min_element(nums.begin(), nums.end());
        double r = *max_element(nums.begin(), nums.end());
        auto check = [&](double v) {
            double s = 0;
            for (int i = 0; i < k; ++i) {
                s += nums[i] - v;
            }
            if (s >= 0) {
                return true;
            }
            double t = 0;
            double mi = 0;
            for (int i = k; i < nums.size(); ++i) {
                s += nums[i] - v;
                t += nums[i - k] - v;
                mi = min(mi, t);
                if (s >= mi) {
                    return true;
                }
            }
            return false;
        };
        while (r - l >= eps) {
            double mid = (l + r) / 2;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid;
            }
        }
        return l;
    }
};
```

#### Go

```go
func findMaxAverage(nums []int, k int) float64 {
	eps := 1e-5
	l := float64(slices.Min(nums))
	r := float64(slices.Max(nums))
	check := func(v float64) bool {
		s := 0.0
		for _, x := range nums[:k] {
			s += float64(x) - v
		}
		if s >= 0 {
			return true
		}
		t := 0.0
		mi := 0.0
		for i := k; i < len(nums); i++ {
			s += float64(nums[i]) - v
			t += float64(nums[i-k]) - v
			mi = math.Min(mi, t)
			if s >= mi {
				return true
			}
		}
		return false
	}
	for r-l >= eps {
		mid := (l + r) / 2
		if check(mid) {
			l = mid
		} else {
			r = mid
		}
	}
	return l
}
```

#### TypeScript

```ts
function findMaxAverage(nums: number[], k: number): number {
    const eps = 1e-5;
    let l = Math.min(...nums);
    let r = Math.max(...nums);
    const check = (v: number): boolean => {
        let s = nums.slice(0, k).reduce((a, b) => a + b) - v * k;
        if (s >= 0) {
            return true;
        }
        let t = 0;
        let mi = 0;
        for (let i = k; i < nums.length; ++i) {
            s += nums[i] - v;
            t += nums[i - k] - v;
            mi = Math.min(mi, t);
            if (s >= mi) {
                return true;
            }
        }
        return false;
    };
    while (r - l >= eps) {
        const mid = (l + r) / 2;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
