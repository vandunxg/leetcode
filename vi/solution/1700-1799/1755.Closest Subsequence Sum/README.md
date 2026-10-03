---
comments: true
difficulty: Hard
rating: 2363
source: Weekly Contest 227 Q4
tags:
    - Bit Manipulation
    - Array
    - Two Pointers
    - Dynamic Programming
    - Bitmask
    - Sorting
    - Meet in the Middle
---

<!-- problem:start -->

# [1755. Closest Subsequence Sum](https://leetcode.com/problems/closest-subsequence-sum)

[中文文档](/solution/1700-1799/1755.Closest%20Subsequence%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và một số nguyên <code>goal</code>.</p>

<p>Bạn cần chọn một dãy con của <code>nums</code> sao cho tổng các phần tử gần <code>goal</code> nhất có thể. Nghĩa là nếu tổng các phần tử của dãy con là <code>sum</code>, bạn cần <strong>tối thiểu hóa hiệu tuyệt đối</strong> <code>abs(sum - goal)</code>.</p>

<p>Trả về giá trị <em><strong>nhỏ nhất</strong> có thể của</em> <code>abs(sum - goal)</code>.</p>

<p>Lưu ý rằng dãy con của một mảng là mảng thu được bằng cách xóa một số phần tử <strong>(có thể xóa tất cả hoặc không xóa phần tử nào)</strong> khỏi mảng ban đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,-7,3,5], goal = 6
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chọn toàn bộ mảng làm dãy con, tổng bằng 6.
Giá trị này bằng goal nên hiệu tuyệt đối là 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,-9,15,-2], goal = -5
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chọn dãy con [7,-9,-2], có tổng bằng -4.
Hiệu tuyệt đối là abs(-4 - (-5)) = abs(1) = 1, đây là giá trị nhỏ nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], goal = -7
<strong>Đầu ra:</strong> 7
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 40</code></li>
	<li><code>-10<sup>7</sup> &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= goal &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tìm tổng dãy con gần $\textit{goal}$ nhất. Vì $n\le 40$, $2^n$ quá lớn nhưng $2^{n/2}$ có thể chấp nhận được.
>
> Chia mảng thành hai nửa, dùng DFS để liệt kê mọi tổng tập con ở mỗi nửa, rồi với mỗi tổng bên trái, tìm kiếm nhị phân $\textit{goal}-l$ trong các tổng bên phải đã sắp xếp và giữ lại hai ứng viên gần nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAbsDifference(self, nums: List[int], goal: int) -> int:
        n = len(nums)
        left = set()
        right = set()

        self.getSubSeqSum(0, 0, nums[: n // 2], left)
        self.getSubSeqSum(0, 0, nums[n // 2 :], right)

        result = inf
        right = sorted(right)
        rl = len(right)

        for l in left:
            remaining = goal - l
            idx = bisect_left(right, remaining)

            if idx < rl:
                result = min(result, abs(remaining - right[idx]))

            if idx > 0:
                result = min(result, abs(remaining - right[idx - 1]))

        return result

    def getSubSeqSum(self, i: int, curr: int, arr: List[int], result: Set[int]):
        if i == len(arr):
            result.add(curr)
            return

        self.getSubSeqSum(i + 1, curr, arr, result)
        self.getSubSeqSum(i + 1, curr + arr[i], arr, result)
```

#### Java

```java
class Solution {
    public int minAbsDifference(int[] nums, int goal) {
        int n = nums.length;
        List<Integer> lsum = new ArrayList<>();
        List<Integer> rsum = new ArrayList<>();
        dfs(nums, lsum, 0, n / 2, 0);
        dfs(nums, rsum, n / 2, n, 0);

        rsum.sort(Integer::compareTo);
        int res = Integer.MAX_VALUE;

        for (Integer x : lsum) {
            int target = goal - x;
            int left = 0, right = rsum.size();
            while (left < right) {
                int mid = (left + right) >> 1;
                if (rsum.get(mid) < target) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            if (left < rsum.size()) {
                res = Math.min(res, Math.abs(target - rsum.get(left)));
            }
            if (left > 0) {
                res = Math.min(res, Math.abs(target - rsum.get(left - 1)));
            }
        }

        return res;
    }

    private void dfs(int[] nums, List<Integer> sum, int i, int n, int cur) {
        if (i == n) {
            sum.add(cur);
            return;
        }

        dfs(nums, sum, i + 1, n, cur);
        dfs(nums, sum, i + 1, n, cur + nums[i]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minAbsDifference(vector<int>& nums, int goal) {
        int n = nums.size();
        vector<int> lsum;
        vector<int> rsum;
        dfs(nums, lsum, 0, n / 2, 0);
        dfs(nums, rsum, n / 2, n, 0);

        sort(rsum.begin(), rsum.end());
        int res = INT_MAX;

        for (int x : lsum) {
            int target = goal - x;
            int left = 0, right = rsum.size();
            while (left < right) {
                int mid = (left + right) >> 1;
                if (rsum[mid] < target) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            if (left < rsum.size()) {
                res = min(res, abs(target - rsum[left]));
            }
            if (left > 0) {
                res = min(res, abs(target - rsum[left - 1]));
            }
        }

        return res;
    }

private:
    void dfs(vector<int>& nums, vector<int>& sum, int i, int n, int cur) {
        if (i == n) {
            sum.emplace_back(cur);
            return;
        }

        dfs(nums, sum, i + 1, n, cur);
        dfs(nums, sum, i + 1, n, cur + nums[i]);
    }
};
```

#### Go

```go
func minAbsDifference(nums []int, goal int) int {
	n := len(nums)
	lsum := make([]int, 0)
	rsum := make([]int, 0)

	dfs(nums[:n/2], &lsum, 0, 0)
	dfs(nums[n/2:], &rsum, 0, 0)

	sort.Ints(rsum)
	res := math.MaxInt32

	for _, x := range lsum {
		t := goal - x
		l, r := 0, len(rsum)
		for l < r {
			m := int(uint(l+r) >> 1)
			if rsum[m] < t {
				l = m + 1
			} else {
				r = m
			}
		}
		if l < len(rsum) {
			res = min(res, abs(t-rsum[l]))
		}
		if l > 0 {
			res = min(res, abs(t-rsum[l-1]))
		}
	}

	return res
}

func dfs(nums []int, sum *[]int, i, cur int) {
	if i == len(nums) {
		*sum = append(*sum, cur)
		return
	}

	dfs(nums, sum, i+1, cur)
	dfs(nums, sum, i+1, cur+nums[i])
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 vẫn là tìm kiếm meet-in-the-middle; DFS chỉ thử bỏ qua rồi chọn phần tử. Thuật toán không đổi, nhưng mã ngắn gọn hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAbsDifference(self, nums: List[int], goal: int) -> int:
        def dfs(arr, res, i, s):
            if i == len(arr):
                res.add(s)
                return
            dfs(arr, res, i + 1, s)
            dfs(arr, res, i + 1, s + arr[i])

        n = len(nums)
        left, right = set(), set()
        dfs(nums[: n >> 1], left, 0, 0)
        dfs(nums[n >> 1 :], right, 0, 0)
        right = sorted(right)
        ans = inf
        for l in left:
            x = goal - l
            i = bisect_left(right, x)
            if i < len(right):
                ans = min(ans, abs(x - right[i]))
            if i:
                ans = min(ans, abs(x - right[i - 1]))
        return ans
```

#### Java

```java
class Solution {
    public int minAbsDifference(int[] nums, int goal) {
        int n = nums.length;
        Set<Integer> left = new HashSet<>();
        Set<Integer> right = new HashSet<>();
        dfs(nums, 0, n >> 1, 0, left);
        dfs(nums, n >> 1, n, 0, right);
        List<Integer> rs = new ArrayList<>(right);
        Collections.sort(rs);
        int ans = Integer.MAX_VALUE;
        for (int x : left) {
            int y = goal - x;
            int l = 0, r = rs.size();
            while (l < r) {
                int mid = (l + r) >> 1;
                if (rs.get(mid) >= y) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            if (l < rs.size()) {
                ans = Math.min(ans, Math.abs(y - rs.get(l)));
            }
            if (l > 0) {
                ans = Math.min(ans, Math.abs(y - rs.get(l - 1)));
            }
        }
        return ans;
    }

    private void dfs(int[] arr, int i, int n, int s, Set<Integer> res) {
        if (i == n) {
            res.add(s);
            return;
        }
        dfs(arr, i + 1, n, s, res);
        dfs(arr, i + 1, n, s + arr[i], res);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minAbsDifference(vector<int>& nums, int goal) {
        int n = nums.size();
        vector<int> left;
        vector<int> right;
        dfs(nums, left, 0, n >> 1, 0);
        dfs(nums, right, n >> 1, n, 0);
        sort(right.begin(), right.end());
        int ans = INT_MAX;
        for (int x : left) {
            int y = goal - x;
            int idx = lower_bound(right.begin(), right.end(), y) - right.begin();
            if (idx < right.size()) ans = min(ans, abs(y - right[idx]));
            if (idx) ans = min(ans, abs(y - right[idx - 1]));
        }
        return ans;
    }

private:
    void dfs(vector<int>& arr, vector<int>& res, int i, int n, int s) {
        if (i == n) {
            res.emplace_back(s);
            return;
        }
        dfs(arr, res, i + 1, n, s);
        dfs(arr, res, i + 1, n, s + arr[i]);
    }
};
```

#### Go

```go
func minAbsDifference(nums []int, goal int) int {
	n := len(nums)
	left := []int{}
	right := []int{}
	dfs(nums[:n>>1], &left, 0, 0)
	dfs(nums[n>>1:], &right, 0, 0)
	sort.Ints(right)
	ans := math.MaxInt32
	for _, x := range left {
		y := goal - x
		l, r := 0, len(right)
		for l < r {
			mid := (l + r) >> 1
			if right[mid] >= y {
				r = mid
			} else {
				l = mid + 1
			}
		}
		if l < len(right) {
			ans = min(ans, abs(y-right[l]))
		}
		if l > 0 {
			ans = min(ans, abs(y-right[l-1]))
		}
	}
	return ans
}

func dfs(arr []int, res *[]int, i, s int) {
	if i == len(arr) {
		*res = append(*res, s)
		return
	}
	dfs(arr, res, i+1, s)
	dfs(arr, res, i+1, s+arr[i])
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
