---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum)

[中文文档](/solution/0900-0999/0930.Binary%20Subarrays%20With%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng nhị phân <code>nums</code> và số nguyên <code>goal</code>, hãy trả về <em>số lượng </em><strong>mảng con</strong><em> không rỗng có tổng bằng </em><code>goal</code>.</p>

<p><strong>Mảng con</strong> là một đoạn liên tiếp của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,1,0,1], goal = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Bốn mảng con được in đậm và gạch chân bên dưới:
[<u><strong>1,0,1</strong></u>,0,1]
[<u><strong>1,0,1,0</strong></u>,1]
[1,<u><strong>0,1,0,1</strong></u>]
[1,0,<u><strong>1,0,1</strong></u>]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,0,0,0], goal = 0
<strong>Đầu ra:</strong> 15
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>nums[i]</code> bằng <code>0</code> hoặc <code>1</code>.</li>
	<li><code>0 &lt;= goal &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm mảng con trong mảng $0/1$ có tổng bằng $\textit{goal}$. $n$ khá lớn nên liệt kê mọi đoạn sẽ quá chậm. Khi tổng tiền tố là $s$, số tiền tố trước đó bằng $s-\textit{goal}$ chính là số mảng con hợp lệ kết thúc tại đây. Một bộ đếm tần suất tiền tố, khởi tạo với $cnt[0]=1$, cho phép tính kết quả mỗi bước trong $O(1)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
        cnt = Counter({0: 1})
        ans = s = 0
        for v in nums:
            s += v
            ans += cnt[s - goal]
            cnt[s] += 1
        return ans
```

#### Java

```java
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        int[] cnt = new int[nums.length + 1];
        cnt[0] = 1;
        int ans = 0, s = 0;
        for (int v : nums) {
            s += v;
            if (s - goal >= 0) {
                ans += cnt[s - goal];
            }
            ++cnt[s];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        int cnt[nums.size() + 1];
        memset(cnt, 0, sizeof cnt);
        cnt[0] = 1;
        int ans = 0, s = 0;
        for (int& v : nums) {
            s += v;
            if (s - goal >= 0) {
                ans += cnt[s - goal];
            }
            ++cnt[s];
        }
        return ans;
    }
};
```

#### Go

```go
func numSubarraysWithSum(nums []int, goal int) (ans int) {
	cnt := map[int]int{0: 1}
	s := 0
	for _, v := range nums {
		s += v
		ans += cnt[s-goal]
		cnt[s]++
	}
	return
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} goal
 * @return {number}
 */
var numSubarraysWithSum = function (nums, goal) {
    const cnt = new Array(nums.length + 1).fill(0);
    cnt[0] = 1;
    let ans = 0;
    let s = 0;
    for (const v of nums) {
        s += v;
        if (s >= goal) {
            ans += cnt[s - goal];
        }
        ++cnt[s];
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 dùng thêm bộ nhớ tuyến tính. Vì mảng không âm, tổng cửa sổ đơn điệu theo đầu trái. Hai pointer trái lần lượt giữ chỉ số đầu tiên sao cho tổng lớn hơn $\textit{goal}$ và chỉ số đầu tiên sao cho tổng lớn hơn hoặc bằng $\textit{goal}$; hiệu của chúng là số cửa sổ có tổng đúng bằng $\textit{goal}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
        i1 = i2 = s1 = s2 = j = ans = 0
        n = len(nums)
        while j < n:
            s1 += nums[j]
            s2 += nums[j]
            while i1 <= j and s1 > goal:
                s1 -= nums[i1]
                i1 += 1
            while i2 <= j and s2 >= goal:
                s2 -= nums[i2]
                i2 += 1
            ans += i2 - i1
            j += 1
        return ans
```

#### Java

```java
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        int i1 = 0, i2 = 0, s1 = 0, s2 = 0, j = 0, ans = 0;
        int n = nums.length;
        while (j < n) {
            s1 += nums[j];
            s2 += nums[j];
            while (i1 <= j && s1 > goal) {
                s1 -= nums[i1++];
            }
            while (i2 <= j && s2 >= goal) {
                s2 -= nums[i2++];
            }
            ans += i2 - i1;
            ++j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        int i1 = 0, i2 = 0, s1 = 0, s2 = 0, j = 0, ans = 0;
        int n = nums.size();
        while (j < n) {
            s1 += nums[j];
            s2 += nums[j];
            while (i1 <= j && s1 > goal) s1 -= nums[i1++];
            while (i2 <= j && s2 >= goal) s2 -= nums[i2++];
            ans += i2 - i1;
            ++j;
        }
        return ans;
    }
};
```

#### Go

```go
func numSubarraysWithSum(nums []int, goal int) int {
	i1, i2, s1, s2, j, ans, n := 0, 0, 0, 0, 0, 0, len(nums)
	for j < n {
		s1 += nums[j]
		s2 += nums[j]
		for i1 <= j && s1 > goal {
			s1 -= nums[i1]
			i1++
		}
		for i2 <= j && s2 >= goal {
			s2 -= nums[i2]
			i2++
		}
		ans += i2 - i1
		j++
	}
	return ans
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} goal
 * @return {number}
 */
var numSubarraysWithSum = function (nums, goal) {
    let i1 = 0,
        i2 = 0,
        s1 = 0,
        s2 = 0,
        j = 0,
        ans = 0;
    const n = nums.length;
    while (j < n) {
        s1 += nums[j];
        s2 += nums[j];
        while (i1 <= j && s1 > goal) s1 -= nums[i1++];
        while (i2 <= j && s2 >= goal) s2 -= nums[i2++];
        ans += i2 - i1;
        ++j;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
