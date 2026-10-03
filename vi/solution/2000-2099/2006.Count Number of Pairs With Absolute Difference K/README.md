---
comments: true
difficulty: Easy
rating: 1271
source: Biweekly Contest 61 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2006. Count Number of Pairs With Absolute Difference K](https://leetcode.com/problems/count-number-of-pairs-with-absolute-difference-k)

[中文文档](/solution/2000-2099/2006.Count%20Number%20of%20Pairs%20With%20Absolute%20Difference%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, trả về <em>số cặp</em> <code>(i, j)</code> <em>trong đó</em> <code>i &lt; j</code> <em>và</em> <code>|nums[i] - nums[j]| == k</code>.</p>

<p>Giá trị của <code>|x|</code> được định nghĩa như sau:</p>

<ul>
	<li><code>x</code> nếu <code>x &gt;= 0</code>.</li>
	<li><code>-x</code> nếu <code>x &lt; 0</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2,1], k = 1
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các cặp có hiệu tuyệt đối bằng 1 là:
- [<strong><u>1</u></strong>,<strong><u>2</u></strong>,2,1]
- [<strong><u>1</u></strong>,2,<strong><u>2</u></strong>,1]
- [1,<strong><u>2</u></strong>,2,<strong><u>1</u></strong>]
- [1,2,<strong><u>2</u></strong>,<strong><u>1</u></strong>]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3], k = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cặp nào có hiệu tuyệt đối bằng 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1,5,4], k = 2
<strong>Đầu ra:</strong> 3
<b>Giải thích:</b> Các cặp có hiệu tuyệt đối bằng 2 là:
- [<strong><u>3</u></strong>,2,<strong><u>1</u></strong>,5,4]
- [<strong><u>3</u></strong>,2,1,<strong><u>5</u></strong>,4]
- [3,<strong><u>2</u></strong>,1,5,<strong><u>4</u></strong>]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 99</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 200$, việc liệt kê tất cả các cặp $i<j$ và kiểm tra $|nums[i]-nums[j]|=k$ chỉ có khoảng $2 \times 10^4$ cặp, nên đáp ứng được giới hạn. Các giá trị và $k$ đều nhỏ, vì vậy không cần cấu trúc dữ liệu bổ sung.
>
> Hai vòng lặp lồng nhau khớp trực tiếp với định nghĩa bài toán.

<!-- thinking:end -->

Ta nhận thấy độ dài của mảng $nums$ không vượt quá $200$, nên có thể liệt kê tất cả các cặp $(i, j)$ với $i < j$, rồi kiểm tra xem $|nums[i] - nums[j]|$ có bằng $k$ hay không. Nếu có, ta tăng đáp án lên một.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countKDifference(self, nums: List[int], k: int) -> int:
        n = len(nums)
        return sum(
            abs(nums[i] - nums[j]) == k for i in range(n) for j in range(i + 1, n)
        )
```

#### Java

```java
class Solution {
    public int countKDifference(int[] nums, int k) {
        int ans = 0;
        for (int i = 0, n = nums.length; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (Math.abs(nums[i] - nums[j]) == k) {
                    ++ans;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countKDifference(vector<int>& nums, int k) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                ans += abs(nums[i] - nums[j]) == k;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countKDifference(nums []int, k int) int {
	n := len(nums)
	ans := 0
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			if abs(nums[i]-nums[j]) == k {
				ans++
			}
		}
	}
	return ans
}

func abs(x int) int {
	if x > 0 {
		return x
	}
	return -x
}
```

#### TypeScript

```ts
function countKDifference(nums: number[], k: number): number {
    let ans = 0;
    let cnt = new Map();
    for (let num of nums) {
        ans += (cnt.get(num - k) || 0) + (cnt.get(num + k) || 0);
        cnt.set(num, (cnt.get(num) || 0) + 1);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_k_difference(nums: Vec<i32>, k: i32) -> i32 {
        let mut res = 0;
        let n = nums.len();
        for i in 0..n - 1 {
            for j in i..n {
                if (nums[i] - nums[j]).abs() == k {
                    res += 1;
                }
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table hoặc Mảng

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt qua mọi cặp. Ta chỉ cần đếm các phần tử có hiệu bằng $k$: với $x$ hiện tại, các giá trị $x-k$ và $x+k$ đã xuất hiện trước đó sẽ tạo thành cặp với nó.
>
> Một bộ đếm tích lũy các cặp này, sau đó ghi nhận $x$, nhờ đó có thể đếm trong một lần duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể dùng một hash table hoặc mảng để ghi lại số lần xuất hiện của mỗi số trong mảng $nums$. Sau đó, ta duyệt qua từng số $x$ trong mảng $nums$ và kiểm tra xem $x + k$ và $x - k$ có trong mảng $nums$ hay không. Nếu có, ta tăng đáp án lên tổng số lần xuất hiện của $x + k$ và $x - k$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countKDifference(self, nums: List[int], k: int) -> int:
        ans = 0
        cnt = Counter()
        for num in nums:
            ans += cnt[num - k] + cnt[num + k]
            cnt[num] += 1
        return ans
```

#### Java

```java
class Solution {
    public int countKDifference(int[] nums, int k) {
        int ans = 0;
        int[] cnt = new int[110];
        for (int num : nums) {
            if (num >= k) {
                ans += cnt[num - k];
            }
            if (num + k <= 100) {
                ans += cnt[num + k];
            }
            ++cnt[num];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countKDifference(vector<int>& nums, int k) {
        int ans = 0;
        int cnt[110]{};
        for (int num : nums) {
            if (num >= k) {
                ans += cnt[num - k];
            }
            if (num + k <= 100) {
                ans += cnt[num + k];
            }
            ++cnt[num];
        }
        return ans;
    }
};
```

#### Go

```go
func countKDifference(nums []int, k int) (ans int) {
	cnt := [110]int{}
	for _, num := range nums {
		if num >= k {
			ans += cnt[num-k]
		}
		if num+k <= 100 {
			ans += cnt[num+k]
		}
		cnt[num]++
	}
	return
}
```

#### Rust

```rust
impl Solution {
    pub fn count_k_difference(nums: Vec<i32>, k: i32) -> i32 {
        let mut arr = [0; 101];
        let mut res = 0;
        for num in nums {
            if num - k >= 1 {
                res += arr[(num - k) as usize];
            }
            if num + k <= 100 {
                res += arr[(num + k) as usize];
            }
            arr[num as usize] += 1;
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
