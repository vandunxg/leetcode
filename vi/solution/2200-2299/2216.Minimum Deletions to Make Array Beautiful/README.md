---
comments: true
difficulty: Medium
rating: 1509
source: Weekly Contest 286 Q2
tags:
    - Stack
    - Greedy
    - Array
---

<!-- problem:start -->

# [2216. Minimum Deletions to Make Array Beautiful](https://leetcode.com/problems/minimum-deletions-to-make-array-beautiful)

[中文文档](/solution/2200-2299/2216.Minimum%20Deletions%20to%20Make%20Array%20Beautiful/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Mảng <code>nums</code> là <strong>đẹp</strong> nếu:</p>

<ul>
	<li><code>nums.length</code> là số chẵn.</li>
	<li><code>nums[i] != nums[i + 1]</code> với mọi <code>i % 2 == 0</code>.</li>
</ul>

<p>Lưu ý rằng mảng rỗng được xem là đẹp.</p>

<p>Bạn có thể xóa tùy ý số lượng phần tử khỏi <code>nums</code>. Khi xóa một phần tử, tất cả phần tử bên phải phần tử bị xóa sẽ <strong>dịch sang trái một vị trí</strong> để lấp đầy khoảng trống, còn tất cả phần tử bên trái phần tử bị xóa sẽ <strong>giữ nguyên</strong>.</p>

<p>Trả về <em><strong>số lượng phần tử ít nhất</strong> cần xóa khỏi </em><code>nums</code><em> để biến nó thành một mảng </em><em>đẹp.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,3,5]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn có thể xóa <code>nums[0]</code> hoặc <code>nums[1]</code> để biến <code>nums</code> thành [1,2,3,5], đây là một mảng đẹp. Có thể chứng minh rằng cần ít nhất 1 lần xóa để biến <code>nums</code> thành mảng đẹp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,2,3,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bạn có thể xóa <code>nums[0]</code> và <code>nums[5]</code> để biến nums thành [1,2,2,3], đây là một mảng đẹp. Có thể chứng minh rằng cần ít nhất 2 lần xóa để biến nums thành mảng đẹp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng đẹp có độ dài chẵn và $nums[2k] \neq nums[2k+1]$ với mọi cặp. Với $n \le 10^5$, không thể thử tất cả các tập phần tử cần xóa. Khi ghép cặp từ trái sang phải, nếu $nums[i]=nums[i+1]$ thì không thể giữ nguyên cả cặp, vì vậy phải xóa một phần tử.
>
> Xóa $nums[i]$ hiện tại (tăng chỉ số lên một và tăng số lần xóa); nếu hai giá trị khác nhau, giữ cặp đó và bỏ qua hai chỉ số. Nếu độ dài còn lại là lẻ, xóa thêm một phần tử ở cuối.

<!-- thinking:end -->

Theo mô tả bài toán, ta biết rằng một mảng đẹp có số lượng phần tử chẵn. Nếu chia các phần tử kề nhau của mảng thành từng nhóm hai phần tử, thì hai phần tử trong mỗi nhóm phải khác nhau. Điều này có nghĩa là các phần tử trong cùng một nhóm không được trùng nhau, còn các phần tử ở các nhóm khác nhau có thể trùng nhau.

Vì vậy, ta duyệt mảng từ trái sang phải. Khi gặp hai phần tử kề nhau bằng nhau, ta xóa một trong hai phần tử, tức là tăng số lần xóa lên một; ngược lại, ta có thể giữ lại cả hai phần tử.

Cuối cùng, ta kiểm tra xem độ dài mảng sau khi xóa có phải là số chẵn hay không. Nếu không, ta cần xóa thêm một phần tử để độ dài mảng cuối cùng là số chẵn.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Ta chỉ cần duyệt qua mảng một lần. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDeletion(self, nums: List[int]) -> int:
        n = len(nums)
        i = ans = 0
        while i < n - 1:
            if nums[i] == nums[i + 1]:
                ans += 1
                i += 1
            else:
                i += 2
        ans += (n - ans) % 2
        return ans
```

#### Java

```java
class Solution {
    public int minDeletion(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                ++ans;
            } else {
                ++i;
            }
        }
        ans += (n - ans) % 2;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDeletion(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                ++ans;
            } else {
                ++i;
            }
        }
        ans += (n - ans) % 2;
        return ans;
    }
};
```

#### Go

```go
func minDeletion(nums []int) (ans int) {
	n := len(nums)
	for i := 0; i < n-1; i++ {
		if nums[i] == nums[i+1] {
			ans++
		} else {
			i++
		}
	}
	ans += (n - ans) % 2
	return
}
```

#### TypeScript

```ts
function minDeletion(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n - 1; ++i) {
        if (nums[i] === nums[i + 1]) {
            ++ans;
        } else {
            ++i;
        }
    }
    ans += (n - ans) % 2;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_deletion(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans = 0;
        let mut i = 0;
        while i < n - 1 {
            if nums[i] == nums[i + 1] {
                ans += 1;
                i += 1;
            } else {
                i += 2;
            }
        }
        ans += (n - ans) % 2;
        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 quyết định việc xóa trên từng cặp phần tử kề nhau. Ta có thể thay vào đó gộp một đoạn liên tiếp các giá trị bằng nhau: các phần tử dư trong đoạn chắc chắn phải bị xóa, một bản sao được giữ lại làm phần tử ở chỉ số chẵn và ghép cặp với giá trị khác tiếp theo.
>
> Vòng lặp bên trong bỏ qua các phần tử bằng nhau và đếm số lần xóa, sau đó con trỏ nhảy qua phần tử được ghép cặp. Việc xử lý tính chẵn lẻ vẫn giống như trước. Độ phức tạp vẫn là $O(n)$ vì con trỏ tiến dần qua các đoạn bằng nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDeletion(self, nums: List[int]) -> int:
        n = len(nums)
        ans = i = 0
        while i < n:
            j = i + 1
            while j < n and nums[j] == nums[i]:
                j += 1
                ans += 1
            i = j + 1
        ans += (n - ans) % 2
        return ans
```

#### Java

```java
class Solution {
    public int minDeletion(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && nums[j] == nums[i]) {
                ++j;
                ++ans;
            }
            i = j + 1;
        }
        ans += (n - ans) % 2;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDeletion(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && nums[j] == nums[i]) {
                ++j;
                ++ans;
            }
            i = j + 1;
        }
        ans += (n - ans) % 2;
        return ans;
    }
};
```

#### Go

```go
func minDeletion(nums []int) (ans int) {
	n := len(nums)
	for i := 0; i < n; {
		j := i + 1
		for ; j < n && nums[j] == nums[i]; j++ {
			ans++
		}
		i = j + 1
	}
	ans += (n - ans) % 2
	return
}
```

#### TypeScript

```ts
function minDeletion(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n;) {
        let j = i + 1;
        for (; j < n && nums[j] === nums[i]; ++j) {
            ++ans;
        }
        i = j + 1;
    }
    ans += (n - ans) % 2;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_deletion(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans = 0;
        let mut i = 0;
        while i < n {
            let mut j = i + 1;
            while j < n && nums[j] == nums[i] {
                ans += 1;
                j += 1;
            }
            i = j + 1;
        }
        ans += (n - ans) % 2;
        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
