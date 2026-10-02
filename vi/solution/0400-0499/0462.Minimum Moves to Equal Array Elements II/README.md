---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [462. Minimum Moves to Equal Array Elements II](https://leetcode.com/problems/minimum-moves-to-equal-array-elements-ii)

[中文文档](/solution/0400-0499/0462.Minimum%20Moves%20to%20Equal%20Array%20Elements%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có kích thước <code>n</code>, hãy trả về <em>số bước ít nhất cần thực hiện để mọi phần tử trong mảng bằng nhau</em>.</p>

<p>Mỗi bước, bạn có thể tăng hoặc giảm một phần tử của mảng đi <code>1</code>.</p>

<p>Các test case được thiết kế sao cho đáp án có thể biểu diễn bằng số nguyên <strong>32-bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Chỉ cần hai bước (lưu ý mỗi bước chỉ tăng hoặc giảm một phần tử):
[<u>1</u>,2,3]  =&gt;  [2,2,<u>3</u>]  =&gt;  [2,2,2]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,10,2,9]
<strong>Đầu ra:</strong> 16
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Trung vị

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước làm thay đổi một phần tử đi $1$; mục tiêu là đưa mọi phần tử về cùng một giá trị. Chọn một trong hai đầu mút làm giá trị chung chỉ khiến tổng khoảng cách tăng lên.
>
> Tổng khoảng cách trên trục số đạt nhỏ nhất tại trung vị: hai đầu mút đóng góp một hằng số, nên bài toán được rút gọn còn các điểm bên trong. Sắp xếp, chọn giá trị ở giữa rồi tính tổng độ lệch tuyệt đối.
>
> Nếu độ dài chẵn, chọn một trong hai giá trị chính giữa đều được vì tổng khoảng cách bằng nhau.

<!-- thinking:end -->

Có thể quy bài toán này về việc tìm một điểm trên trục số sao cho tổng khoảng cách từ $n$ điểm đến điểm đó là nhỏ nhất. Đáp án là trung vị của $n$ điểm.

Trung vị có tính chất làm nhỏ nhất tổng khoảng cách từ tất cả các số đến nó.

Chứng minh:

Xét dãy đã sắp xếp $a_1, a_2, \cdots, a_n$ và giả sử $x$ là điểm làm nhỏ nhất tổng khoảng cách từ các phần tử trong dãy đến nó. Rõ ràng, $x$ phải nằm giữa $a_1$ và $a_n$. Tổng khoảng cách từ $a_1$ và $a_n$ đến $x$ luôn bằng $a_n - a_1$, nên có thể bỏ qua hai đầu mút và chỉ xét $a_2, a_3, \cdots, a_{n-1}$. Bài toán được rút gọn thành tìm điểm làm nhỏ nhất tổng khoảng cách trong dãy con này. Lặp lại lập luận, ta thấy trung vị của dãy làm nhỏ nhất tổng khoảng cách đến các số còn lại.

Trong bài này, trước tiên ta sắp xếp mảng, tìm trung vị rồi tính tổng khoảng cách từ mọi số đến trung vị.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$.

Bài toán liên quan:

- [296. Best Meeting Point](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0296.Best%20Meeting%20Point/README_EN.md)
- [2448. Minimum Cost to Make Array Equal](https://github.com/doocs/leetcode/blob/main/solution/2400-2499/2448.Minimum%20Cost%20to%20Make%20Array%20Equal/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves2(self, nums: List[int]) -> int:
        nums.sort()
        k = nums[len(nums) >> 1]
        return sum(abs(v - k) for v in nums)
```

#### Java

```java
class Solution {
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int k = nums[nums.length >> 1];
        int ans = 0;
        for (int v : nums) {
            ans += Math.abs(v - k);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int k = nums[nums.size() >> 1];
        int ans = 0;
        for (int& v : nums) {
            ans += abs(v - k);
        }
        return ans;
    }
};
```

#### Go

```go
func minMoves2(nums []int) int {
	sort.Ints(nums)
	k := nums[len(nums)>>1]
	ans := 0
	for _, v := range nums {
		ans += abs(v - k)
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minMoves2(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const k = nums[nums.length >> 1];
    return nums.reduce((r, v) => r + Math.abs(v - k), 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_moves2(mut nums: Vec<i32>) -> i32 {
        nums.sort();
        let k = nums[nums.len() / 2];
        let mut ans = 0;
        for num in nums.iter() {
            ans += (num - k).abs();
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
