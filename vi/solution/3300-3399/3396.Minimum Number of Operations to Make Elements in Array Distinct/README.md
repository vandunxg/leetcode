---
comments: true
difficulty: Easy
rating: 1299
source: Weekly Contest 429 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3396. Minimum Number of Operations to Make Elements in Array Distinct](https://leetcode.com/problems/minimum-number-of-operations-to-make-elements-in-array-distinct)

[中文文档](/solution/3300-3399/3396.Minimum%20Number%20of%20Operations%20to%20Make%20Elements%20in%20Array%20Distinct/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Bạn cần đảm bảo các phần tử trong mảng là <strong>phân biệt</strong>. Để thực hiện việc này, bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Xóa 3 phần tử ở đầu mảng. Nếu mảng có ít hơn 3 phần tử, xóa tất cả phần tử còn lại.</li>
</ul>

<p><strong>Lưu ý</strong> rằng một mảng rỗng được xem là có các phần tử phân biệt. Hãy trả về số thao tác <strong>ít nhất</strong> cần thực hiện để các phần tử trong mảng là phân biệt.<!-- notionvc: 210ee4f2-90af-4cdf-8dbc-96d1fa8f67c7 --></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,2,3,3,5,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Trong thao tác đầu tiên, 3 phần tử đầu tiên bị xóa, mảng trở thành <code>[4, 2, 3, 3, 5, 7]</code>.</li>
	<li>Trong thao tác thứ hai, 3 phần tử tiếp theo bị xóa, mảng trở thành <code>[3, 5, 7]</code>, với các phần tử phân biệt.</li>
</ul>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,5,6,4,4]</span></p>

<p><strong>Đầu ra:</strong> 2</p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Trong thao tác đầu tiên, 3 phần tử đầu tiên bị xóa, mảng trở thành <code>[4, 4]</code>.</li>
	<li>Trong thao tác thứ hai, tất cả phần tử còn lại bị xóa, mảng trở thành mảng rỗng.</li>
</ul>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,7,8,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng đã có các phần tử phân biệt. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác loại bỏ ba phần tử đầu tiên cho đến khi phần còn lại không có phần tử trùng lặp. Với $n \le 100$, ta tìm hậu tố dài nhất không có phần tử trùng lặp bằng cách duyệt từ phải sang trái.
>
> Chèn các giá trị vào một set khi đi về bên trái. Lần đầu gặp phần tử trùng lặp, điều đó có nghĩa là các chỉ số $0..i$ phải bị xóa, cần $\lfloor i/3 \rfloor+1$ thao tác.
>
> Nếu toàn bộ mảng không có phần tử trùng lặp, đáp án là $0$.

<!-- thinking:end -->

Ta có thể duyệt mảng $\textit{nums}$ theo thứ tự ngược và sử dụng một hash table $\textit{s}$ để ghi lại các phần tử đã duyệt. Khi gặp một phần tử $\textit{nums}[i]$, nếu $\textit{nums}[i]$ đã có trong hash table $\textit{s}$, điều đó có nghĩa là ta cần xóa tất cả phần tử trong $\textit{nums}[0..i]$. Số thao tác cần thực hiện là $\left\lfloor \frac{i}{3} \right\rfloor + 1$. Ngược lại, ta thêm $\textit{nums}[i]$ vào hash table $\textit{s}$ và tiếp tục với phần tử tiếp theo.

Sau khi duyệt xong, nếu không tìm thấy phần tử trùng lặp, các phần tử trong mảng đã phân biệt và không cần thực hiện thao tác nào, nên đáp án là $0$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        s = set()
        for i in range(len(nums) - 1, -1, -1):
            if nums[i] in s:
                return i // 3 + 1
            s.add(nums[i])
        return 0
```

#### Java

```java
class Solution {
    public int minimumOperations(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int i = nums.length - 1; i >= 0; --i) {
            if (!s.add(nums[i])) {
                return i / 3 + 1;
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<int>& nums) {
        unordered_set<int> s;
        for (int i = nums.size() - 1; ~i; --i) {
            if (s.contains(nums[i])) {
                return i / 3 + 1;
            }
            s.insert(nums[i]);
        }
        return 0;
    }
};
```

#### Go

```go
func minimumOperations(nums []int) int {
	s := map[int]bool{}
	for i := len(nums) - 1; i >= 0; i-- {
		if s[nums[i]] {
			return i/3 + 1
		}
		s[nums[i]] = true
	}
	return 0
}
```

#### TypeScript

```ts
function minimumOperations(nums: number[]): number {
    const s = new Set<number>();
    for (let i = nums.length - 1; ~i; --i) {
        if (s.has(nums[i])) {
            return Math.ceil((i + 1) / 3);
        }
        s.add(nums[i]);
    }
    return 0;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn minimum_operations(nums: Vec<i32>) -> i32 {
        let mut s = HashSet::new();
        for i in (0..nums.len()).rev() {
            if !s.insert(nums[i]) {
                return (i / 3) as i32 + 1;
            }
        }
        0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
