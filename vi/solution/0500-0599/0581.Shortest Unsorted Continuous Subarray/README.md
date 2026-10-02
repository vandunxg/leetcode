---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - Array
    - Two Pointers
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [581. Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray)

[中文文档](/solution/0500-0599/0581.Shortest%20Unsorted%20Continuous%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy tìm một <b>mảng con liên tiếp</b> sao cho nếu chỉ sắp xếp mảng con này theo thứ tự không giảm thì toàn bộ mảng cũng được sắp xếp theo thứ tự không giảm.</p>

<p>Hãy trả về <em>độ dài của mảng con ngắn nhất thỏa mãn điều kiện trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,6,4,8,10,9,15]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Cần sắp xếp [6, 4, 8, 10, 9] theo thứ tự tăng dần để toàn bộ mảng được sắp xếp tăng dần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này với độ phức tạp thời gian <code>O(n)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con ngắn nhất cần tìm là đoạn mà khi sắp xếp sẽ khiến toàn bộ mảng được sắp xếp. Sắp xếp một bản sao rồi so sánh: hai vị trí đầu tiên và cuối cùng khác nhau xác định hai biên của đoạn.
>
> Với $n \le 10^4$, có thể dùng thuật toán sắp xếp. Nếu không có vị trí nào khác nhau thì mảng đã được sắp xếp và độ dài là $0$.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng rồi so sánh với mảng ban đầu để tìm vị trí ngoài cùng bên trái và bên phải có giá trị khác nhau. Độ dài đoạn giữa hai vị trí đó là độ dài của mảng con liên tiếp ngắn nhất cần sắp xếp.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        arr = sorted(nums)
        l, r = 0, len(nums) - 1
        while l <= r and nums[l] == arr[l]:
            l += 1
        while l <= r and nums[r] == arr[r]:
            r -= 1
        return r - l + 1
```

#### Java

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] arr = nums.clone();
        Arrays.sort(arr);
        int l = 0, r = arr.length - 1;
        while (l <= r && nums[l] == arr[l]) {
            l++;
        }
        while (l <= r && nums[r] == arr[r]) {
            r--;
        }
        return r - l + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        vector<int> arr = nums;
        sort(arr.begin(), arr.end());
        int l = 0, r = arr.size() - 1;
        while (l <= r && arr[l] == nums[l]) {
            l++;
        }
        while (l <= r && arr[r] == nums[r]) {
            r--;
        }
        return r - l + 1;
    }
};
```

#### Go

```go
func findUnsortedSubarray(nums []int) int {
	arr := make([]int, len(nums))
	copy(arr, nums)
	sort.Ints(arr)
	l, r := 0, len(arr)-1
	for l <= r && nums[l] == arr[l] {
		l++
	}
	for l <= r && nums[r] == arr[r] {
		r--
	}
	return r - l + 1
}
```

#### TypeScript

```ts
function findUnsortedSubarray(nums: number[]): number {
    const arr = [...nums];
    arr.sort((a, b) => a - b);
    let [l, r] = [0, arr.length - 1];
    while (l <= r && arr[l] === nums[l]) {
        ++l;
    }
    while (l <= r && arr[r] === nums[r]) {
        --r;
    }
    return r - l + 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_unsorted_subarray(nums: Vec<i32>) -> i32 {
        let mut arr = nums.clone();
        arr.sort();
        let mut l = 0usize;
        while l < nums.len() && nums[l] == arr[l] {
            l += 1;
        }
        if l == nums.len() {
            return 0;
        }
        let mut r = nums.len() - 1;
        while r > l && nums[r] == arr[r] {
            r -= 1;
        }
        (r - l + 1) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duy trì giá trị lớn nhất bên trái và nhỏ nhất bên phải

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp làm tăng thêm thừa số log. Khi duyệt từ trái sang phải, nếu một giá trị nhỏ hơn giá trị lớn nhất của prefix thì nó nằm trong đoạn chưa được sắp xếp, vì vậy biên phải được mở rộng. Khi duyệt từ phải sang trái, dùng giá trị nhỏ nhất của suffix để xác định biên trái.
>
> Cả hai biên đều bắt đầu ở $-1$; nếu biên phải không thay đổi thì trả về $0$. Thuật toán cần hai lượt duyệt tuyến tính và không gian hằng số.

<!-- thinking:end -->

Ta có thể duyệt mảng từ trái sang phải và duy trì giá trị lớn nhất $mx$. Nếu giá trị hiện tại nhỏ hơn $mx$, nghĩa là nó không ở đúng vị trí, nên ta cập nhật biên phải $r$ thành vị trí hiện tại. Tương tự, ta duyệt mảng từ phải sang trái và duy trì giá trị nhỏ nhất $mi$. Nếu giá trị hiện tại lớn hơn $mi$, nghĩa là nó không ở đúng vị trí, nên ta cập nhật biên trái $l$ thành vị trí hiện tại. Ban đầu, đặt $l$ và $r$ bằng $-1$. Nếu cả hai biên không được cập nhật, nghĩa là mảng đã được sắp xếp và ta trả về $0$. Nếu không, trả về $r - l + 1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        mi, mx = inf, -inf
        l = r = -1
        n = len(nums)
        for i, x in enumerate(nums):
            if mx > x:
                r = i
            else:
                mx = x
            if mi < nums[n - i - 1]:
                l = n - i - 1
            else:
                mi = nums[n - i - 1]
        return 0 if r == -1 else r - l + 1
```

#### Java

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        final int inf = 1 << 30;
        int n = nums.length;
        int l = -1, r = -1;
        int mi = inf, mx = -inf;
        for (int i = 0; i < n; ++i) {
            if (mx > nums[i]) {
                r = i;
            } else {
                mx = nums[i];
            }
            if (mi < nums[n - i - 1]) {
                l = n - i - 1;
            } else {
                mi = nums[n - i - 1];
            }
        }
        return r == -1 ? 0 : r - l + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        const int inf = 1e9;
        int n = nums.size();
        int l = -1, r = -1;
        int mi = inf, mx = -inf;
        for (int i = 0; i < n; ++i) {
            if (mx > nums[i]) {
                r = i;
            } else {
                mx = nums[i];
            }
            if (mi < nums[n - i - 1]) {
                l = n - i - 1;
            } else {
                mi = nums[n - i - 1];
            }
        }
        return r == -1 ? 0 : r - l + 1;
    }
};
```

#### Go

```go
func findUnsortedSubarray(nums []int) int {
	const inf = 1 << 30
	n := len(nums)
	l, r := -1, -1
	mi, mx := inf, -inf
	for i, x := range nums {
		if mx > x {
			r = i
		} else {
			mx = x
		}
		if mi < nums[n-i-1] {
			l = n - i - 1
		} else {
			mi = nums[n-i-1]
		}
	}
	if r == -1 {
		return 0
	}
	return r - l + 1
}
```

#### TypeScript

```ts
function findUnsortedSubarray(nums: number[]): number {
    let [l, r] = [-1, -1];
    let [mi, mx] = [Infinity, -Infinity];
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        if (mx > nums[i]) {
            r = i;
        } else {
            mx = nums[i];
        }
        if (mi < nums[n - i - 1]) {
            l = n - i - 1;
        } else {
            mi = nums[n - i - 1];
        }
    }
    return r === -1 ? 0 : r - l + 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_unsorted_subarray(nums: Vec<i32>) -> i32 {
        let inf = 1 << 30;
        let n = nums.len();
        let mut l = -1;
        let mut r = -1;
        let mut mi = inf;
        let mut mx = -inf;

        for i in 0..n {
            if mx > nums[i] {
                r = i as i32;
            } else {
                mx = nums[i];
            }

            if mi < nums[n - i - 1] {
                l = (n - i - 1) as i32;
            } else {
                mi = nums[n - i - 1];
            }
        }

        if r == -1 {
            0
        } else {
            r - l + 1
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
