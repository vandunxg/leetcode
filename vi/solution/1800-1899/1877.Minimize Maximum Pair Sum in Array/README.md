---
comments: true
difficulty: Medium
rating: 1301
source: Biweekly Contest 53 Q2
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [1877. Minimize Maximum Pair Sum in Array](https://leetcode.com/problems/minimize-maximum-pair-sum-in-array)

[中文文档](/solution/1800-1899/1877.Minimize%20Maximum%20Pair%20Sum%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Tổng cặp</strong> của một cặp <code>(a,b)</code> bằng <code>a + b</code>. <strong>Tổng cặp lớn nhất</strong> là <strong>tổng cặp</strong> lớn nhất trong một danh sách các cặp.</p>

<ul>
	<li>Ví dụ, nếu có các cặp <code>(1,5)</code>, <code>(2,3)</code> và <code>(4,4)</code>, <strong>tổng cặp lớn nhất</strong> sẽ là <code>max(1+5, 2+3, 4+4) = max(6, 5, 8) = 8</code>.</li>
</ul>

<p>Cho mảng <code>nums</code> có độ dài <code>n</code> <strong>chẵn</strong>, hãy ghép các phần tử của <code>nums</code> thành <code>n / 2</code> cặp sao cho:</p>

<ul>
	<li>Mỗi phần tử của <code>nums</code> thuộc <strong>đúng một</strong> cặp, và</li>
	<li><strong>tổng cặp lớn nhất </strong>được <strong>tối thiểu hóa</strong>.</li>
</ul>

<p>Trả về <em><strong>tổng cặp lớn nhất</strong> đã được tối thiểu hóa sau khi ghép các phần tử một cách tối ưu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,5,2,3]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các phần tử có thể được ghép thành các cặp (3,3) và (5,2).
Tổng cặp lớn nhất là max(3+3, 5+2) = max(6, 7) = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,5,4,2,4,6]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Các phần tử có thể được ghép thành các cặp (3,5), (4,4) và (6,2).
Tổng cặp lớn nhất là max(3+5, 4+4, 6+2) = max(8, 8, 8) = 8.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n</code> is <strong>even</strong>.</li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ghép mảng sao cho tổng cặp lớn nhất được tối thiểu hóa. Ghép hai số lớn với nhau sẽ tạo ra một tổng lớn.
>
> Sau khi sắp xếp, ghép số nhỏ nhất với số lớn nhất, số nhỏ thứ hai với số lớn thứ hai, rồi lấy giá trị lớn nhất trong các tổng đó.

<!-- thinking:end -->

Để tối thiểu hóa tổng cặp lớn nhất trong mảng, ta có thể ghép số nhỏ nhất với số lớn nhất, số nhỏ thứ hai với số lớn thứ hai, và tiếp tục như vậy.

Do đó, trước hết ta sắp xếp mảng, rồi dùng hai con trỏ trỏ đến hai đầu mảng. Tính tổng hai số mà hai con trỏ trỏ tới, cập nhật tổng cặp lớn nhất, sau đó đưa con trỏ trái sang phải một bước và con trỏ phải sang trái một bước. Lặp lại cho đến khi hai con trỏ gặp nhau, ta sẽ thu được tổng cặp lớn nhất nhỏ nhất.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minPairSum(self, nums: List[int]) -> int:
        nums.sort()
        return max(x + nums[-i - 1] for i, x in enumerate(nums[: len(nums) >> 1]))
```

#### Java

```java
class Solution {
    public int minPairSum(int[] nums) {
        Arrays.sort(nums);
        int ans = 0, n = nums.length;
        for (int i = 0; i < n >> 1; ++i) {
            ans = Math.max(ans, nums[i] + nums[n - i - 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minPairSum(vector<int>& nums) {
        ranges::sort(nums);
        int ans = 0, n = nums.size();
        for (int i = 0; i < n >> 1; ++i) {
            ans = max(ans, nums[i] + nums[n - i - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func minPairSum(nums []int) (ans int) {
	sort.Ints(nums)
	n := len(nums)
	for i, x := range nums[:n>>1] {
		ans = max(ans, x+nums[n-1-i])
	}
	return
}
```

#### TypeScript

```ts
function minPairSum(nums: number[]): number {
    nums.sort((a, b) => a - b);
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < n >> 1; ++i) {
        ans = Math.max(ans, nums[i] + nums[n - 1 - i]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_pair_sum(nums: Vec<i32>) -> i32 {
        let mut nums = nums;
        nums.sort();
        let mut ans = 0;
        let n = nums.len();
        for i in 0..n / 2 {
            ans = ans.max(nums[i] + nums[n - i - 1]);
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var minPairSum = function (nums) {
    nums.sort((a, b) => a - b);
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < n >> 1; ++i) {
        ans = Math.max(ans, nums[i] + nums[n - 1 - i]);
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int MinPairSum(int[] nums) {
        Array.Sort(nums);
        int ans = 0, n = nums.Length;
        for (int i = 0; i < n >> 1; ++i) {
            ans = Math.Max(ans, nums[i] + nums[n - i - 1]);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
