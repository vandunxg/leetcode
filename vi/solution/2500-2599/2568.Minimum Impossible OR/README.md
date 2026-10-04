---
comments: true
difficulty: Medium
rating: 1754
source: Biweekly Contest 98 Q3
tags:
    - Bit Manipulation
    - Brainteaser
    - Array
---

<!-- problem:start -->

# [2568. Minimum Impossible OR](https://leetcode.com/problems/minimum-impossible-or)

[中文文档](/solution/2500-2599/2568.Minimum%20Impossible%20OR/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>.</p>

<p>Một số nguyên x được gọi là <strong>biểu diễn được</strong> từ <code>nums</code> nếu tồn tại các số nguyên <code>0 &lt;= index<sub>1</sub> &lt; index<sub>2</sub> &lt; ... &lt; index<sub>k</sub> &lt; nums.length</code> sao cho <code>nums[index<sub>1</sub>] | nums[index<sub>2</sub>] | ... | nums[index<sub>k</sub>] = x</code>. Nói cách khác, một số nguyên biểu diễn được nếu nó có thể được tạo thành bằng phép OR bitwise của một dãy con bất kỳ của <code>nums</code>.</p>

<p>Trả về <em>số <strong>nguyên dương khác 0</strong> nhỏ nhất&nbsp;không </em><em>biểu diễn được từ </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 1 và 2 đã có trong mảng. Ta biết rằng 3 biểu diễn được, vì nums[0] | nums[1] = 2 | 1 = 3. Vì 4 không biểu diễn được, ta trả về 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,3,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể chứng minh rằng 1 là số nhỏ nhất không biểu diễn được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê các lũy thừa của 2

<!-- thinking:start -->

> **Tư duy**
>
> Một giá trị biểu diễn được nếu nó là phép OR bitwise của một tập con khác rỗng. Phép OR chỉ bật các bit, vì vậy nếu mọi lũy thừa của 2 nhỏ hơn $2^k$ đều xuất hiện, thì mọi số nguyên trong $[1,2^{k+1}-1]$ đều biểu diễn được.
>
> Do đó, giá trị nhỏ nhất bị thiếu chắc chắn là một lũy thừa của 2. Lưu mảng vào một set rồi trả về $2^i$ đầu tiên không xuất hiện.

<!-- thinking:end -->

Ta bắt đầu từ số nguyên $1$. Nếu $1$ biểu diễn được thì nó phải xuất hiện trong mảng `nums`. Nếu $2$ biểu diễn được thì nó cũng phải xuất hiện trong mảng `nums`. Nếu cả $1$ và $2$ đều biểu diễn được, thì phép OR bitwise của chúng là $3$ cũng biểu diễn được, và cứ tiếp tục như vậy.

Vì vậy, ta có thể liệt kê các lũy thừa của $2$. Nếu $2^i$ đang được liệt kê không có trong mảng `nums`, thì $2^i$ là số nguyên nhỏ nhất không biểu diễn được.

Độ phức tạp thời gian là $O(n + \log M)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $M$ lần lượt là độ dài của mảng `nums` và giá trị lớn nhất trong mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minImpossibleOR(self, nums: List[int]) -> int:
        s = set(nums)
        return next(1 << i for i in range(32) if 1 << i not in s)
```

#### Java

```java
class Solution {
    public int minImpossibleOR(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int x : nums) {
            s.add(x);
        }
        for (int i = 0;; ++i) {
            if (!s.contains(1 << i)) {
                return 1 << i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minImpossibleOR(vector<int>& nums) {
        unordered_set<int> s(nums.begin(), nums.end());
        for (int i = 0;; ++i) {
            if (!s.count(1 << i)) {
                return 1 << i;
            }
        }
    }
};
```

#### Go

```go
func minImpossibleOR(nums []int) int {
	s := map[int]bool{}
	for _, x := range nums {
		s[x] = true
	}
	for i := 0; ; i++ {
		if !s[1<<i] {
			return 1 << i
		}
	}
}
```

#### TypeScript

```ts
function minImpossibleOR(nums: number[]): number {
    const s: Set<number> = new Set();
    for (const x of nums) {
        s.add(x);
    }
    for (let i = 0; ; ++i) {
        if (!s.has(1 << i)) {
            return 1 << i;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
