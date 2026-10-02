---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Array
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [561. Array Partition](https://leetcode.com/problems/array-partition)

[中文文档](/solution/0500-0599/0561.Array%20Partition/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> gồm <code>2n</code> số nguyên. Hãy chia các số này thành <code>n</code> cặp <code>(a<sub>1</sub>, b<sub>1</sub>), (a<sub>2</sub>, b<sub>2</sub>), ..., (a<sub>n</sub>, b<sub>n</sub>)</code> sao cho tổng <code>min(a<sub>i</sub>, b<sub>i</sub>)</code> với mọi <code>i</code> là <strong>lớn nhất</strong>. Trả về <em>tổng lớn nhất đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,3,2]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các cách ghép cặp có thể có (không xét thứ tự các phần tử) gồm:
1. (1, 4), (2, 3) -&gt; min(1, 4) + min(2, 3) = 1 + 2 = 3
2. (1, 3), (2, 4) -&gt; min(1, 3) + min(2, 4) = 1 + 2 = 3
3. (1, 2), (3, 4) -&gt; min(1, 2) + min(3, 4) = 1 + 3 = 4
Vậy tổng lớn nhất có thể đạt được là 4.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,2,6,5,1,2]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Cách ghép cặp tối ưu là (2, 1), (2, 5), (6, 6). min(2, 1) + min(2, 5) + min(6, 6) = 1 + 2 + 6 = 9.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>nums.length == 2 * n</code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ghép các số thành từng cặp rồi cộng số nhỏ hơn trong mỗi cặp; mục tiêu là tối đa hóa tổng này. Số lớn hơn trong mỗi cặp không đóng góp vào tổng, vì vậy nên giữ nó nhỏ nhất có thể bằng cách ghép các số gần nhau.
>
> Sắp xếp rồi ghép các phần tử liền kề; tổng các phần tử ở vị trí cách nhau một phần tử là tối ưu. Có thể biến mọi cặp chéo thành cặp không giao nhau mà không làm giảm tổng.

<!-- thinking:end -->

Với một cặp số $(a, b)$, giả sử $a \leq b$, khi đó $\min(a, b) = a$. Để tổng lớn nhất có thể, ta nên chọn $b$ càng gần $a$ càng tốt, nhờ vậy giữ lại được số lớn hơn.

Vì vậy, ta sắp xếp mảng $nums$, ghép từng hai số liền kề thành một cặp rồi cộng số đầu tiên của mỗi cặp.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        nums.sort()
        return sum(nums[::2])
```

#### Java

```java
class Solution {
    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);
        int ans = 0;
        for (int i = 0; i < nums.length; i += 2) {
            ans += nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int ans = 0;
        for (int i = 0; i < nums.size(); i += 2) {
            ans += nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func arrayPairSum(nums []int) (ans int) {
	sort.Ints(nums)
	for i := 0; i < len(nums); i += 2 {
		ans += nums[i]
	}
	return
}
```

#### Rust

```rust
impl Solution {
    pub fn array_pair_sum(mut nums: Vec<i32>) -> i32 {
        nums.sort();
        nums.iter().step_by(2).sum()
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var arrayPairSum = function (nums) {
    nums.sort((a, b) => a - b);
    return nums.reduce((acc, cur, i) => (i % 2 === 0 ? acc + cur : acc), 0);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
