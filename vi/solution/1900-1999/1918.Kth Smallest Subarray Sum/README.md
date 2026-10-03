---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Sliding Window
---

<!-- problem:start -->

# [1918. Kth Smallest Subarray Sum 🔒](https://leetcode.com/problems/kth-smallest-subarray-sum)

[中文文档](/solution/1900-1999/1918.Kth%20Smallest%20Subarray%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>, hãy trả về <em>tổng mảng con </em><code>k<sup>th</sup></code><em> <strong>nhỏ nhất</strong>.</em></p>

<p><strong>Mảng con</strong> được định nghĩa là một dãy phần tử <strong>liên tiếp, không rỗng</strong> trong một mảng. <strong>Tổng mảng con</strong> là tổng của tất cả phần tử trong mảng con đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3], k = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Các mảng con của [2,1,3] là:
- [2] có tổng bằng 2
- [1] có tổng bằng 1
- [3] có tổng bằng 3
- [2,1] có tổng bằng 3
- [1,3] có tổng bằng 4
- [2,1,3] có tổng bằng 6
Sắp xếp các tổng theo thứ tự từ nhỏ đến lớn được 1, 2, 3, <u>3</u>, 4, 6. Tổng mảng con nhỏ thứ 4 là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,5,5], k = 7
<strong>Đầu ra:</strong> 10
<strong>Giải thích: </strong>Các mảng con của [3,3,5,5] là:
- [3] có tổng bằng 3
- [3] có tổng bằng 3
- [5] có tổng bằng 5
- [5] có tổng bằng 5
- [3,3] có tổng bằng 6
- [3,5] có tổng bằng 8
- [5,5] có tổng bằng 10
- [3,3,5] có tổng bằng 11
- [3,5,5] có tổng bằng 13
- [3,3,5,5] có tổng bằng 16
Sắp xếp các tổng theo thứ tự từ nhỏ đến lớn được 3, 3, 5, 5, 6, 8, <u>10</u>, 11, 13, 16. Tổng mảng con nhỏ thứ 7 là 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n&nbsp;&lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= n * (n + 1) / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Có $O(n^2)$ mảng con; việc tạo tất cả chúng là quá nặng khi $n\le 2\times 10^4$. Vì mọi giá trị đều dương, tổng mảng con tăng đơn điệu theo độ dài.
>
> Số lượng mảng con có tổng $\le s$ không giảm khi $s$ tăng. Ta tìm kiếm nhị phân trên $s$ và đếm số lượng đó bằng cửa sổ hai con trỏ trong thời gian tuyến tính.
>
> Khoảng tìm kiếm là $[\min nums,\sum nums]$; cận trái cuối cùng chính là tổng nhỏ thứ $k$.

<!-- thinking:end -->

Ta nhận thấy tất cả phần tử trong mảng đều là số nguyên dương. Tổng mảng con $s$ càng lớn thì càng có nhiều mảng con có tổng nhỏ hơn hoặc bằng $s$. Tính đơn điệu này cho phép ta dùng tìm kiếm nhị phân để giải bài toán.

Ta tìm kiếm nhị phân trên tổng mảng con, khởi tạo biên trái và biên phải lần lượt là giá trị nhỏ nhất trong mảng $\textit{nums}$ và tổng của tất cả phần tử trong mảng. Mỗi lần, ta tính số lượng mảng con có tổng nhỏ hơn hoặc bằng giá trị giữa hiện tại. Nếu số lượng lớn hơn hoặc bằng $k$, điều đó có nghĩa là giá trị giữa hiện tại $s$ có thể là tổng mảng con nhỏ thứ $k$, nên ta thu hẹp biên phải. Ngược lại, ta tăng biên trái. Sau khi kết thúc tìm kiếm nhị phân, biên trái sẽ là tổng mảng con nhỏ thứ $k$.

Bài toán được đưa về việc tính số lượng mảng con trong một mảng có tổng nhỏ hơn hoặc bằng $s$, ta có thể tính số lượng này bằng hàm $f(s)$.

Hàm $f(s)$ được tính như sau:

- Khởi tạo hai con trỏ $j$ và $i$, lần lượt biểu diễn biên trái và biên phải của cửa sổ hiện tại, với $j = i = 0$. Đồng thời, khởi tạo tổng các phần tử trong cửa sổ $t = 0$.
- Dùng biến $\textit{cnt}$ để ghi nhận số lượng mảng con có tổng nhỏ hơn hoặc bằng $s$, ban đầu $\textit{cnt} = 0$.
- Duyệt qua mảng $\textit{nums}$. Với mỗi phần tử $\textit{nums}[i]$, thêm phần tử đó vào cửa sổ, tức là $t = t + \textit{nums}[i]$. Nếu $t > s$, dịch biên trái của cửa sổ sang phải cho đến khi $t \leq s$, tức là liên tục thực hiện $t -= \textit{nums}[j]$ và $j = j + 1$. Sau đó cập nhật $\textit{cnt}$ thành $\textit{cnt} = \textit{cnt} + i - j + 1$. Tiếp tục với phần tử kế tiếp cho đến khi duyệt hết mảng.

Cuối cùng, trả về $cnt$ làm kết quả của hàm $f(s)$.

Độ phức tạp thời gian là $O(n \times \log S)$, trong đó $n$ là độ dài của mảng $\textit{nums}$ và $S$ là tổng của tất cả phần tử trong mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthSmallestSubarraySum(self, nums: List[int], k: int) -> int:
        def f(s):
            t = j = 0
            cnt = 0
            for i, x in enumerate(nums):
                t += x
                while t > s:
                    t -= nums[j]
                    j += 1
                cnt += i - j + 1
            return cnt >= k

        l, r = min(nums), sum(nums)
        return l + bisect_left(range(l, r + 1), True, key=f)
```

#### Java

```java
class Solution {
    public int kthSmallestSubarraySum(int[] nums, int k) {
        int l = 1 << 30, r = 0;
        for (int x : nums) {
            l = Math.min(l, x);
            r += x;
        }
        while (l < r) {
            int mid = (l + r) >> 1;
            if (f(nums, mid) >= k) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private int f(int[] nums, int s) {
        int t = 0, j = 0;
        int cnt = 0;
        for (int i = 0; i < nums.length; ++i) {
            t += nums[i];
            while (t > s) {
                t -= nums[j++];
            }
            cnt += i - j + 1;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kthSmallestSubarraySum(vector<int>& nums, int k) {
        int l = 1 << 30, r = 0;
        for (int& x : nums) {
            l = min(l, x);
            r += x;
        }
        auto f = [&](int s) {
            int cnt = 0, t = 0;
            for (int i = 0, j = 0; i < nums.size(); ++i) {
                t += nums[i];
                while (t > s) {
                    t -= nums[j++];
                }
                cnt += i - j + 1;
            }
            return cnt;
        };
        while (l < r) {
            int mid = (l + r) >> 1;
            if (f(mid) >= k) {
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
func kthSmallestSubarraySum(nums []int, k int) int {
	l, r := 1<<30, 0
	for _, x := range nums {
		l = min(l, x)
		r += x
	}
	f := func(s int) (cnt int) {
		t := 0
		for i, j := 0, 0; i < len(nums); i++ {
			t += nums[i]
			for t > s {
				t -= nums[j]
				j++
			}
			cnt += i - j + 1
		}
		return
	}
	for l < r {
		mid := (l + r) >> 1
		if f(mid) >= k {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return l
}
```

#### Typescript

```ts
function kthSmallestSubarraySum(nums: number[], k: number): number {
    let l = Math.min(...nums);
    let r = nums.reduce((sum, x) => sum + x, 0);

    const f = (s: number): number => {
        let cnt = 0;
        let t = 0;
        let j = 0;

        for (let i = 0; i < nums.length; i++) {
            t += nums[i];
            while (t > s) {
                t -= nums[j];
                j++;
            }
            cnt += i - j + 1;
        }
        return cnt;
    };

    while (l < r) {
        const mid = (l + r) >> 1;
        if (f(mid) >= k) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

#### Rust

```rust
impl Solution {
    pub fn kth_smallest_subarray_sum(nums: Vec<i32>, k: i32) -> i32 {
        let mut l = *nums.iter().min().unwrap();
        let mut r: i32 = nums.iter().sum();

        let f = |s: i32| -> i32 {
            let (mut cnt, mut t, mut j) = (0, 0, 0);

            for i in 0..nums.len() {
                t += nums[i];
                while t > s {
                    t -= nums[j];
                    j += 1;
                }
                cnt += (i - j + 1) as i32;
            }
            cnt
        };

        while l < r {
            let mid = (l + r) / 2;
            if f(mid) >= k {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        l
    }
}
```

#### Scala

```scala
object Solution {
    def kthSmallestSubarraySum(nums: Array[Int], k: Int): Int = {
        var l = Int.MaxValue
        var r = 0

        for (x <- nums) {
            l = l.min(x)
            r += x
        }

        def f(s: Int): Int = {
            var cnt = 0
            var t = 0
            var j = 0

            for (i <- nums.indices) {
                t += nums(i)
                while (t > s) {
                    t -= nums(j)
                    j += 1
                }
                cnt += i - j + 1
            }
            cnt
        }

        while (l < r) {
            val mid = (l + r) / 2
            if (f(mid) >= k) r = mid
            else l = mid + 1
        }
        l
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
