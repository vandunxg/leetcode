---
comments: true
difficulty: Medium
rating: 1423
source: Biweekly Contest 29 Q3
tags:
    - Array
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [1493. Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element)

[中文文档](/solution/1400-1499/1493.Longest%20Subarray%20of%201%27s%20After%20Deleting%20One%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng nhị phân <code>nums</code>, hãy xóa một phần tử khỏi mảng.</p>

<p>Trả về <em>kích thước của mảng con không rỗng dài nhất chỉ chứa </em><code>1</code><em> trong mảng kết quả</em>. Trả về <code>0</code> nếu không có mảng con nào như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,0,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Sau khi xóa phần tử ở vị trí 2, [1,1,1] chứa 3 phần tử có giá trị là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,1,1,0,1,1,0,1]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Sau khi xóa phần tử ở vị trí 4, mảng con dài nhất chỉ chứa các phần tử có giá trị 1 trong [0,1,1,1,1,1,0,1] là [1,1,1,1,1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bắt buộc phải xóa một phần tử.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>nums[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Vì phải xóa chính xác một phần tử, đáp án là số lượng số 1 liên tiếp ngay bên trái $i$ cộng với số lượng ngay bên phải. $n\le 10^5$. Ta tính trước số lượng số 1 dài nhất kết thúc trước $i$ và bắt đầu sau $i$, rồi lấy giá trị lớn nhất khi xét phần tử bị xóa.

<!-- thinking:end -->

Ta có thể liệt kê từng vị trí $i$ cần xóa, sau đó tính số lượng số 1 liên tiếp ở bên trái và bên phải, rồi lấy giá trị lớn nhất.

Cụ thể, ta dùng hai mảng $left$ và $right$ có độ dài $n+1$, trong đó $left[i]$ biểu thị số lượng số 1 liên tiếp kết thúc tại `nums[i-1]`, còn $right[i]$ biểu thị số lượng số 1 liên tiếp bắt đầu tại `nums[i]`.

Đáp án cuối cùng là $\max_{0 \leq i < n} \{left[i] + right[i+1]\}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        n = len(nums)
        left = [0] * (n + 1)
        right = [0] * (n + 1)
        for i, x in enumerate(nums, 1):
            if x:
                left[i] = left[i - 1] + 1
        for i in range(n - 1, -1, -1):
            if nums[i]:
                right[i] = right[i + 1] + 1
        return max(left[i] + right[i + 1] for i in range(n))
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int n = nums.length;
        int[] left = new int[n + 1];
        int[] right = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            if (nums[i - 1] == 1) {
                left[i] = left[i - 1] + 1;
            }
        }
        for (int i = n - 1; i >= 0; --i) {
            if (nums[i] == 1) {
                right[i] = right[i + 1] + 1;
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, left[i] + right[i + 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums) {
        int n = nums.size();
        vector<int> left(n + 1);
        vector<int> right(n + 1);
        for (int i = 1; i <= n; ++i) {
            if (nums[i - 1]) {
                left[i] = left[i - 1] + 1;
            }
        }
        for (int i = n - 1; ~i; --i) {
            if (nums[i]) {
                right[i] = right[i + 1] + 1;
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, left[i] + right[i + 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubarray(nums []int) (ans int) {
	n := len(nums)
	left := make([]int, n+1)
	right := make([]int, n+1)
	for i := 1; i <= n; i++ {
		if nums[i-1] == 1 {
			left[i] = left[i-1] + 1
		}
	}
	for i := n - 1; i >= 0; i-- {
		if nums[i] == 1 {
			right[i] = right[i+1] + 1
		}
	}
	for i := 0; i < n; i++ {
		ans = max(ans, left[i]+right[i+1])
	}
	return
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[]): number {
    const n = nums.length;
    const left: number[] = Array(n + 1).fill(0);
    const right: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        if (nums[i - 1]) {
            left[i] = left[i - 1] + 1;
        }
    }
    for (let i = n - 1; ~i; --i) {
        if (nums[i]) {
            right[i] = right[i + 1] + 1;
        }
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans = Math.max(ans, left[i] + right[i + 1]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_subarray(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut left = vec![0; n + 1];
        let mut right = vec![0; n + 1];

        for i in 1..=n {
            if nums[i - 1] == 1 {
                left[i] = left[i - 1] + 1;
            }
        }

        for i in (0..n).rev() {
            if nums[i] == 1 {
                right[i] = right[i + 1] + 1;
            }
        }

        let mut ans = 0;
        for i in 0..n {
            ans = ans.max(left[i] + right[i + 1]);
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 lưu hai mảng. Tương đương với việc tìm cửa sổ dài nhất chứa nhiều nhất một số $0$; sau khi xóa số $0$ đó (hoặc một số $1$), độ dài còn lại là $i-j$. Mở rộng đầu phải và thu hẹp cửa sổ khi số lượng số 0 vượt quá $1$.

<!-- thinking:end -->

Thực chất, bài toán yêu cầu tìm mảng con dài nhất chứa nhiều nhất một số $0$. Độ dài còn lại sau khi xóa một phần tử khỏi mảng con này chính là đáp án.

Vì vậy, ta có thể dùng hai con trỏ $j$ và $i$ lần lượt trỏ đến biên trái và biên phải của mảng con, ban đầu $j = 0$, $i = 0$. Ngoài ra, ta dùng biến $cnt$ để ghi nhận số lượng số $0$ trong mảng con.

Tiếp theo, ta di chuyển con trỏ phải $i$. Nếu `nums[i] = 0`, ta tăng $cnt$ lên $1$. Khi $cnt > 1$, ta cần di chuyển con trỏ trái $j$ cho đến khi $cnt \leq 1$. Sau đó, ta cập nhật đáp án, tức là $ans = \max(ans, i - j)$. Tiếp tục di chuyển con trỏ phải $i$ cho đến khi $i$ chạm cuối mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng `nums`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        ans = 0
        cnt = j = 0
        for i, x in enumerate(nums):
            cnt += x ^ 1
            while cnt > 1:
                cnt -= nums[j] ^ 1
                j += 1
            ans = max(ans, i - j)
        return ans
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int ans = 0, n = nums.length;
        for (int i = 0, j = 0, cnt = 0; i < n; ++i) {
            cnt += nums[i] ^ 1;
            while (cnt > 1) {
                cnt -= nums[j++] ^ 1;
            }
            ans = Math.max(ans, i - j);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums) {
        int ans = 0, n = nums.size();
        for (int i = 0, j = 0, cnt = 0; i < n; ++i) {
            cnt += nums[i] ^ 1;
            while (cnt > 1) {
                cnt -= nums[j++] ^ 1;
            }
            ans = max(ans, i - j);
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubarray(nums []int) (ans int) {
	cnt, j := 0, 0
	for i, x := range nums {
		cnt += x ^ 1
		for ; cnt > 1; j++ {
			cnt -= nums[j] ^ 1
		}
		ans = max(ans, i-j)
	}
	return
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[]): number {
    let [ans, cnt, j] = [0, 0, 0];
    for (let i = 0; i < nums.length; ++i) {
        cnt += nums[i] ^ 1;
        while (cnt > 1) {
            cnt -= nums[j++] ^ 1;
        }
        ans = Math.max(ans, i - j);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_subarray(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans = 0;
        let mut j = 0;
        let mut cnt = 0;

        for i in 0..n {
            cnt += nums[i] ^ 1;
            while cnt > 1 {
                cnt -= nums[j] ^ 1;
                j += 1;
            }
            ans = ans.max(i - j);
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Hai con trỏ (Tối ưu hóa)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 2 có thể thu hẹp cửa sổ khi số lượng số 0 vượt quá một. Vì chỉ cần độ dài lớn nhất, ta chỉ di chuyển đầu trái một lần mỗi khi số lượng quá lớn và duy trì cửa sổ không giảm. Đáp án là $n-l-1$.

<!-- thinking:end -->

Trong Lời giải 2, ta di chuyển con trỏ trái trong một vòng lặp cho đến khi $cnt \leq 1$. Vì bài toán yêu cầu mảng con dài nhất, ta không cần giảm độ dài mảng con. Do đó, nếu $\textit{cnt} \gt 1$, ta chỉ di chuyển con trỏ trái một lần, còn con trỏ phải tiếp tục di chuyển sang phải. Điều này đảm bảo độ dài mảng con không giảm.

Cuối cùng, đáp án trả về là $n - l - 1$, trong đó $l$ là vị trí của con trỏ trái.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng `nums`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        cnt = l = 0
        for x in nums:
            cnt += x ^ 1
            if cnt > 1:
                cnt -= nums[l] ^ 1
                l += 1
        return len(nums) - l - 1
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int cnt = 0, l = 0;
        for (int x : nums) {
            cnt += x ^ 1;
            if (cnt > 1) {
                cnt -= nums[l++] ^ 1;
            }
        }
        return nums.length - l - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums) {
        int cnt = 0, l = 0;
        for (int x : nums) {
            cnt += x ^ 1;
            if (cnt > 1) {
                cnt -= nums[l++] ^ 1;
            }
        }
        return nums.size() - l - 1;
    }
};
```

#### Go

```go
func longestSubarray(nums []int) int {
	cnt, l := 0, 0
	for _, x := range nums {
		cnt += x ^ 1
		if cnt > 1 {
			cnt -= nums[l] ^ 1
			l++
		}
	}
	return len(nums) - l - 1
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[]): number {
    let [cnt, l] = [0, 0];
    for (const x of nums) {
        cnt += x ^ 1;
        if (cnt > 1) {
            cnt -= nums[l++] ^ 1;
        }
    }
    return nums.length - l - 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_subarray(nums: Vec<i32>) -> i32 {
        let mut cnt = 0;
        let mut l = 0;

        for &x in &nums {
            cnt += x ^ 1;
            if cnt > 1 {
                cnt -= nums[l] ^ 1;
                l += 1;
            }
        }

        (nums.len() - l - 1) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
