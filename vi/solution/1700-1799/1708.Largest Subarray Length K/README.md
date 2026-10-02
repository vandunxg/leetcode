---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1708. Largest Subarray Length K 🔒](https://leetcode.com/problems/largest-subarray-length-k)

[中文文档](/solution/1700-1799/1708.Largest%20Subarray%20Length%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Mảng <code>A</code> lớn hơn mảng <code>B</code> nếu tại chỉ số đầu tiên <code>i</code> thỏa <code>A[i] != B[i]</code>, ta có <code>A[i] &gt; B[i]</code>.</p>

<p>Ví dụ, với cách đánh chỉ số bắt đầu từ <code>0</code>:</p>

<ul>
	<li><code>[1,3,2,4] &gt; [1,2,2,4]</code>, vì tại chỉ số <code>1</code>, <code>3 &gt; 2</code>.</li>
	<li><code>[1,4,4,4] &lt; [2,1,1,1]</code>, vì tại chỉ số <code>0</code>, <code>1 &lt; 2</code>.</li>
</ul>

<p>Đoạn con là một dãy con liên tiếp của mảng.</p>

<p>Cho mảng số nguyên <code>nums</code> gồm các số <strong>khác nhau</strong>, hãy trả về đoạn con <strong>lớn nhất</strong> của <code>nums</code> có độ dài <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,5,2,3], k = 3
<strong>Đầu ra:</strong> [5,2,3]
<strong>Giải thích:</strong> Các đoạn con có kích thước 3 là: [1,4,5], [4,5,2] và [5,2,3].
Trong số đó, [5,2,3] là đoạn lớn nhất.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,5,2,3], k = 4
<strong>Đầu ra:</strong> [4,5,2,3]
<strong>Giải thích:</strong> Các đoạn con có kích thước 4 là: [1,4,5,2] và [4,5,2,3].
Trong số đó, [4,5,2,3] là đoạn lớn nhất.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,5,2,3], k = 1
<strong>Đầu ra:</strong> [5]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả số nguyên trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Điều gì xảy ra nếu các số nguyên trong <code>nums</code> không khác nhau?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Trong các đoạn con độ dài $k$, đoạn lớn nhất theo thứ tự từ điển được quyết định bởi phần tử đầu tiên vì mọi giá trị đều khác nhau.
>
> Các vị trí bắt đầu hợp lệ nằm trong $[0,n-k]$. Chọn chỉ số $i$ của phần tử lớn nhất trong đoạn đó; đáp án là $nums[i..i+k)$. Chỉ cần một lần duyệt.

<!-- thinking:end -->

Các số nguyên trong mảng đều khác nhau, nên trước hết ta tìm chỉ số của phần tử lớn nhất trong đoạn $[0,..n-k]$, rồi lấy $k$ phần tử bắt đầu từ chỉ số đó.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng. Không tính phần không gian của đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestSubarray(self, nums: List[int], k: int) -> List[int]:
        i = nums.index(max(nums[: len(nums) - k + 1]))
        return nums[i : i + k]
```

#### Java

```java
class Solution {
    public int[] largestSubarray(int[] nums, int k) {
        int j = 0;
        for (int i = 1; i < nums.length - k + 1; ++i) {
            if (nums[j] < nums[i]) {
                j = i;
            }
        }
        return Arrays.copyOfRange(nums, j, j + k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> largestSubarray(vector<int>& nums, int k) {
        auto i = max_element(nums.begin(), nums.end() - k + 1);
        return {i, i + k};
    }
};
```

#### Go

```go
func largestSubarray(nums []int, k int) []int {
	j := 0
	for i := 1; i < len(nums)-k+1; i++ {
		if nums[j] < nums[i] {
			j = i
		}
	}
	return nums[j : j+k]
}
```

#### TypeScript

```ts
function largestSubarray(nums: number[], k: number): number[] {
    let j = 0;
    for (let i = 1; i < nums.length - k + 1; ++i) {
        if (nums[j] < nums[i]) {
            j = i;
        }
    }
    return nums.slice(j, j + k);
}
```

#### Rust

```rust
impl Solution {
    pub fn largest_subarray(nums: Vec<i32>, k: i32) -> Vec<i32> {
        let mut j = 0;
        for i in 1..=nums.len() - (k as usize) {
            if nums[i] > nums[j] {
                j = i;
            }
        }
        nums[j..j + (k as usize)].to_vec()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
