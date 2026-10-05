---
comments: true
difficulty: Medium
rating: 1444
source: Biweekly Contest 172 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3779. Minimum Number of Operations to Have Distinct Elements](https://leetcode.com/problems/minimum-number-of-operations-to-have-distinct-elements)

[中文文档](/solution/3700-3799/3779.Minimum%20Number%20of%20Operations%20to%20Have%20Distinct%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Trong một thao tác, bạn xóa <strong>ba phần tử đầu tiên</strong> của mảng hiện tại. Nếu còn ít hơn ba phần tử, <strong>tất cả</strong> phần tử còn lại sẽ bị xóa.</p>

<p>Lặp lại thao tác này cho đến khi mảng rỗng hoặc không còn giá trị trùng lặp.</p>

<p>Trả về một số nguyên biểu thị số thao tác cần thực hiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,8,3,6,5,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong thao tác đầu tiên, ta xóa ba phần tử đầu tiên. Các phần tử còn lại <code>[6, 5, 8]</code> đều khác nhau, nên ta dừng lại. Chỉ cần một thao tác.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau một thao tác, mảng trở nên rỗng, thỏa mãn điều kiện dừng.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,5,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả phần tử trong mảng đều khác nhau, vì vậy không cần thực hiện thao tác nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác loại bỏ ba phần tử đầu tiên, tức là cắt đi một tiền tố có độ dài $3$. Khi duyệt từ phải sang trái, giá trị trùng lặp đầu tiên cho biết mọi phần tử ở vị trí đó hoặc trước đó đều phải bị xóa, cần $\lfloor i/3\rfloor+1$ thao tác.

<!-- thinking:end -->

Ta có thể duyệt mảng $\textit{nums}$ theo thứ tự ngược và sử dụng một bảng băm $\textit{st}$ để ghi lại các phần tử đã duyệt. Khi duyệt đến phần tử $\textit{nums}[i]$, nếu $\textit{nums}[i]$ đã có trong bảng băm $\textit{st}$, điều đó có nghĩa là ta cần xóa tất cả phần tử trong $\textit{nums}[0..i]$, và số thao tác cần thực hiện là $\left\lfloor \frac{i}{3} \right\rfloor + 1$. Nếu chưa có, ta thêm $\textit{nums}[i]$ vào bảng băm $\textit{st}$ và tiếp tục duyệt phần tử tiếp theo.

Sau khi duyệt xong, nếu không tìm thấy phần tử trùng lặp nào, thì tất cả phần tử trong mảng đã khác nhau, không cần thực hiện thao tác nào và đáp án là $0$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        st = set()
        for i in range(len(nums) - 1, -1, -1):
            if nums[i] in st:
                return i // 3 + 1
            st.add(nums[i])
        return 0
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        Set<Integer> st = new HashSet<>();
        for (int i = nums.length - 1; i >= 0; --i) {
            if (!st.add(nums[i])) {
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
    int minOperations(vector<int>& nums) {
        unordered_set<int> st;
        for (int i = nums.size() - 1; ~i; --i) {
            if (st.contains(nums[i])) {
                return i / 3 + 1;
            }
            st.insert(nums[i]);
        }
        return 0;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	st := make(map[int]struct{})
	for i := len(nums) - 1; i >= 0; i-- {
		if _, ok := st[nums[i]]; ok {
			return i/3 + 1
		}
		st[nums[i]] = struct{}{}
	}
	return 0
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    const st = new Set<number>();
    for (let i = nums.length - 1; i >= 0; i--) {
        if (st.has(nums[i])) {
            return Math.floor(i / 3) + 1;
        }
        st.add(nums[i]);
    }
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
