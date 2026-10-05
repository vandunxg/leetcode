---
comments: true
difficulty: Medium
rating: 1492
source: Biweekly Contest 174 Q2
tags:
    - Greedy
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3810. Minimum Operations to Reach Target Array](https://leetcode.com/problems/minimum-operations-to-reach-target-array)

[中文文档](/solution/3800-3899/3810.Minimum%20Operations%20to%20Reach%20Target%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>nums</code> và <code>target</code>, mỗi mảng có độ dài <code>n</code>, trong đó <code>nums[i]</code> là giá trị hiện tại tại chỉ số <code>i</code> và <code>target[i]</code> là giá trị mong muốn tại chỉ số <code>i</code>.</p>

<p>Bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý (kể cả không lần nào):</p>

<ul>
	<li>Chọn một giá trị nguyên <code>x</code></li>
	<li>Tìm tất cả các <strong>đoạn liên tiếp cực đại</strong> mà <code>nums[i] == x</code> (một đoạn là <strong>cực đại</strong> nếu không thể mở rộng sang trái hoặc phải mà vẫn giữ tất cả các giá trị bằng <code>x</code>)</li>
	<li>Với mỗi đoạn như vậy <code>[l, r]</code>, cập nhật <strong>đồng thời</strong>:
	<ul>
		<li><code>nums[l] = target[l], nums[l + 1] = target[l + 1], ..., nums[r] = target[r]</code></li>
	</ul>
	</li>
</ul>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để biến <code>nums</code> thành <code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], target = [2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Chọn <code>x = 1</code>: đoạn cực đại <code>[0, 0]</code> được cập nhật -&gt; nums trở thành <code>[2, 2, 3]</code></li>
	<li>Chọn <code>x = 2</code>: đoạn cực đại <code>[0, 1]</code> được cập nhật (<code>nums[0]</code> vẫn là 2, <code>nums[1]</code> trở thành 1) -&gt; <code>nums</code> trở thành <code>[2, 1, 3]</code></li>
	<li>Vì vậy, cần 2 thao tác để biến <code>nums</code> thành <code>target</code>.​​​​​​​​​​​​​​</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1,4], target = [5,1,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>x = 4</code>: các đoạn cực đại <code>[0, 0]</code> và <code>[2, 2]</code> được cập nhật (<code>nums[2]</code> vẫn là 4) -&gt; <code>nums</code> trở thành <code>[5, 1, 4]</code></li>
	<li>Vì vậy, cần 1 thao tác để biến <code>nums</code> thành <code>target</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,3,7], target = [5,5,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>x = 7</code>: các đoạn cực đại <code>[0, 0]</code> và <code>[2, 2]</code> được cập nhật -&gt; <code>nums</code> trở thành <code>[5, 3, 9]</code></li>
	<li>Chọn <code>x = 3</code>: đoạn cực đại <code>[1, 1]</code> được cập nhật -&gt; <code>nums</code> trở thành <code>[5, 5, 9]</code></li>
	<li>Vì vậy, cần 2 thao tác để biến <code>nums</code> thành <code>target</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length == target.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], target[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác thay thế mọi đoạn liên tiếp cực đại của một giá trị $x$ bằng các phần tử tương ứng trong $\textit{target}$. Vì $n \le 10^5$, mô phỏng các lần ghi đè sẽ phải duyệt lại mảng.
>
> Tất cả các đoạn của $x$ thay đổi cùng lúc, nên mỗi giá trị $x$ bị lệch chỉ cần một thao tác, bất kể nó xuất hiện trong bao nhiêu đoạn.
>
> Các vị trí đã khớp không cần tính. Đáp án là số lượng giá trị phân biệt trong $nums[i]$ vẫn khác target.
>
> Một set chứa các giá trị ban đầu đó có kích thước bằng số thao tác nhỏ nhất.

<!-- thinking:end -->

Theo mô tả bài toán, ta chỉ cần đếm số lượng giá trị phân biệt $\text{nums}[i]$ sao cho $\text{nums}[i] \ne \text{target}[i]$. Vì vậy, ta có thể dùng một hash table để lưu các giá trị $\text{nums}[i]$ phân biệt này, rồi cuối cùng trả về kích thước của hash table.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], target: List[int]) -> int:
        s = {x for x, y in zip(nums, target) if x != y}
        return len(s)
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int[] target) {
        Set<Integer> s = new HashSet<>();
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] != target[i]) {
                s.add(nums[i]);
            }
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, vector<int>& target) {
        unordered_set<int> s;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] != target[i]) {
                s.insert(nums[i]);
            }
        }
        return s.size();
    }
};
```

#### Go

```go
func minOperations(nums []int, target []int) int {
	s := make(map[int]struct{})
	for i := 0; i < len(nums); i++ {
		if nums[i] != target[i] {
			s[nums[i]] = struct{}{}
		}
	}
	return len(s)
}
```

#### TypeScript

```ts
function minOperations(nums: number[], target: number[]): number {
    const s = new Set<number>();
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== target[i]) {
            s.add(nums[i]);
        }
    }
    return s.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
