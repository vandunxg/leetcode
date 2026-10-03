---
comments: true
difficulty: Medium
rating: 1876
source: Weekly Contest 238 Q2
tags:
    - Greedy
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [1838. Frequency of the Most Frequent Element](https://leetcode.com/problems/frequency-of-the-most-frequent-element)

[中文文档](/solution/1800-1899/1838.Frequency%20of%20the%20Most%20Frequent%20Element/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Tần suất</strong> của một phần tử là số lần phần tử đó xuất hiện trong mảng.</p>

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Trong một thao tác, bạn có thể chọn một chỉ số của <code>nums</code> và tăng phần tử tại chỉ số đó thêm <code>1</code>.</p>

<p>Trả về <em><strong>tần suất lớn nhất có thể</strong> của một phần tử sau khi thực hiện <strong>nhiều nhất</strong> </em><code>k</code><em> thao tác</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,4], k = 5
<strong>Đầu ra:</strong> 3<strong>
Giải thích:</strong> Tăng phần tử đầu tiên ba lần và phần tử thứ hai hai lần để được nums = [4,4,4].
4 có tần suất bằng 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,8,13], k = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có nhiều cách đạt được đáp án tối ưu:
- Tăng phần tử đầu tiên ba lần để được nums = [4,4,8,13]. 4 có tần suất bằng 2.
- Tăng phần tử thứ hai bốn lần để được nums = [1,8,8,13]. 8 có tần suất bằng 2.
- Tăng phần tử thứ ba năm lần để được nums = [1,4,13,13]. 13 có tần suất bằng 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,9,6], k = 2
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Prefix Sum + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể tăng các phần tử nhiều nhất $k$ lần và muốn đạt tần suất cao nhất. Giá trị đích phải là một giá trị ban đầu, còn các phần tử được tăng tạo thành một tiền tố liên tiếp trong mảng đã sắp xếp. Thử mọi điểm cuối bên phải sẽ quá chậm với $n\le 10^5$.
>
> Sau khi sắp xếp, tính khả thi đơn điệu theo độ dài cửa sổ, nên ta có thể tìm kiếm nhị phân tần suất. Prefix sum giúp kiểm tra xem có cửa sổ nào với độ dài đó có thể được tăng lên bằng giá trị ngoài cùng bên phải với chi phí không quá $k$ hay không.

<!-- thinking:end -->

Từ mô tả bài toán, ta có thể rút ra ba kết luận:

1. Sau một số thao tác, phần tử có tần suất cao nhất trong mảng phải là một phần tử của mảng ban đầu. Vì sao? Giả sử các phần tử được thao tác là $a_1, a_2, \cdots, a_m$, trong đó giá trị lớn nhất là $a_m$. Tất cả các phần tử này được đổi thành cùng một giá trị $x$, với $x \geq a_m$. Khi đó ta cũng có thể đổi tất cả chúng thành $a_m$ mà không làm tăng số thao tác.
2. Các phần tử được thao tác phải tạo thành một mảng con liên tiếp trong mảng đã sắp xếp.
3. Nếu một tần suất $m$ thỏa mãn điều kiện, thì mọi $m' < m$ cũng thỏa mãn điều kiện. Điều này gợi ý ta dùng tìm kiếm nhị phân để tìm tần suất lớn nhất thỏa mãn điều kiện.

Do đó, ta có thể sắp xếp mảng $nums$, rồi tính mảng prefix sum $s$ của mảng đã sắp xếp, trong đó $s[i]$ biểu diễn tổng của $i$ phần tử đầu tiên.

Tiếp theo, ta đặt biên trái của tìm kiếm nhị phân là $l = 1$ và biên phải là $r = n$. Ở mỗi bước tìm kiếm nhị phân, ta lấy giá trị giữa $m = (l + r + 1) / 2$, rồi kiểm tra xem có tồn tại mảng con liên tiếp độ dài $m$ sao cho mọi phần tử trong mảng con có thể được đổi thành một phần tử trong mảng và số thao tác không vượt quá $k$ hay không. Nếu tồn tại mảng con như vậy, ta cập nhật biên trái $l$ thành $m$; ngược lại, cập nhật biên phải $r$ thành $m - 1$.

Cuối cùng, trả về biên trái $l$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFrequency(self, nums: List[int], k: int) -> int:
        def check(m: int) -> bool:
            for i in range(m, n + 1):
                if nums[i - 1] * m - (s[i] - s[i - m]) <= k:
                    return True
            return False

        n = len(nums)
        nums.sort()
        s = list(accumulate(nums, initial=0))
        l, r = 1, n
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    private int[] nums;
    private long[] s;
    private int k;

    public int maxFrequency(int[] nums, int k) {
        this.k = k;
        this.nums = nums;
        Arrays.sort(nums);
        int n = nums.length;
        s = new long[n + 1];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        int l = 1, r = n;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int m) {
        for (int i = m; i <= nums.length; ++i) {
            if (1L * nums[i - 1] * m - (s[i] - s[i - m]) <= k) {
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
    int maxFrequency(vector<int>& nums, int k) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        long long s[n + 1];
        s[0] = 0;
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        int l = 1, r = n;
        auto check = [&](int m) {
            for (int i = m; i <= n; ++i) {
                if (1LL * nums[i - 1] * m - (s[i] - s[i - m]) <= k) {
                    return true;
                }
            }
            return false;
        };
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maxFrequency(nums []int, k int) int {
	n := len(nums)
	sort.Ints(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	check := func(m int) bool {
		for i := m; i <= n; i++ {
			if nums[i-1]*m-(s[i]-s[i-m]) <= k {
				return true
			}
		}
		return false
	}
	l, r := 1, n
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function maxFrequency(nums: number[], k: number): number {
    const n = nums.length;
    nums.sort((a, b) => a - b);
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        s[i] = s[i - 1] + nums[i - 1];
    }
    let [l, r] = [1, n];
    const check = (m: number): boolean => {
        for (let i = m; i <= n; ++i) {
            if (nums[i - 1] * m - (s[i] - s[i - m]) <= k) {
                return true;
            }
        }
        return false;
    };
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cần prefix sum và tìm kiếm nhị phân. Chi phí của cửa sổ tăng theo điểm cuối bên phải, nên hai con trỏ là đủ: khi $i$ tiến lên, ta tăng các phần tử trong cửa sổ lên $nums[i]$ và dịch $j$ khi chi phí vượt quá $k$. Một lượt duyệt sẽ tìm được cửa sổ hợp lệ dài nhất.

<!-- thinking:end -->

Ta cũng có thể dùng hai con trỏ để duy trì một cửa sổ trượt, trong đó mọi phần tử trong cửa sổ có thể được đổi thành giá trị lớn nhất trong cửa sổ. Số thao tác cho các phần tử trong cửa sổ là $s$, và $s \leq k$.

Ban đầu, ta đặt con trỏ trái $j$ trỏ đến phần tử đầu tiên của mảng, đồng thời con trỏ phải $i$ cũng trỏ đến phần tử đầu tiên. Sau đó, mỗi lần ta dịch con trỏ phải $i$, tăng tất cả phần tử trong cửa sổ lên $nums[i]$. Khi đó, số thao tác cần thêm là $(nums[i] - nums[i - 1]) \times (i - j)$. Nếu số thao tác này vượt quá $k$, ta cần dịch con trỏ trái $j$ cho đến khi số thao tác cho các phần tử trong cửa sổ không vượt quá $k$. Sau đó, ta cập nhật đáp án bằng độ dài lớn nhất của cửa sổ.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFrequency(self, nums: List[int], k: int) -> int:
        nums.sort()
        ans = 1
        s = j = 0
        for i in range(1, len(nums)):
            s += (nums[i] - nums[i - 1]) * (i - j)
            while s > k:
                s -= nums[i] - nums[j]
                j += 1
            ans = max(ans, i - j + 1)
        return ans
```

#### Java

```java
class Solution {
    public int maxFrequency(int[] nums, int k) {
        Arrays.sort(nums);
        int ans = 1;
        long s = 0;
        for (int i = 1, j = 0; i < nums.length; ++i) {
            s += 1L * (nums[i] - nums[i - 1]) * (i - j);
            while (s > k) {
                s -= nums[i] - nums[j++];
            }
            ans = Math.max(ans, i - j + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxFrequency(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int ans = 1;
        long long s = 0;
        for (int i = 1, j = 0; i < nums.size(); ++i) {
            s += 1LL * (nums[i] - nums[i - 1]) * (i - j);
            while (s > k) {
                s -= nums[i] - nums[j++];
            }
            ans = max(ans, i - j + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func maxFrequency(nums []int, k int) int {
	sort.Ints(nums)
	ans := 1
	s := 0
	for i, j := 1, 0; i < len(nums); i++ {
		s += (nums[i] - nums[i-1]) * (i - j)
		for ; s > k; j++ {
			s -= nums[i] - nums[j]
		}
		ans = max(ans, i-j+1)
	}
	return ans
}
```

#### TypeScript

```ts
function maxFrequency(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    let ans = 1;
    let [s, j] = [0, 0];
    for (let i = 1; i < nums.length; ++i) {
        s += (nums[i] - nums[i - 1]) * (i - j);
        while (s > k) {
            s -= nums[i] - nums[j++];
        }
        ans = Math.max(ans, i - j + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
