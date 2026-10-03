---
comments: true
difficulty: Medium
rating: 1939
source: Weekly Contest 228 Q3
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1760. Minimum Limit of Balls in a Bag](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag)

[中文文档](/solution/1700-1799/1760.Minimum%20Limit%20of%20Balls%20in%20a%20Bag/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho mảng số nguyên <code>nums</code>, trong đó túi thứ <code>i<sup>th</sup></code> chứa <code>nums[i]</code> quả bóng. Bạn cũng được cho số nguyên <code>maxOperations</code>.</p>

<p>Bạn có thể thực hiện thao tác sau nhiều nhất <code>maxOperations</code> lần:</p>

<ul>
	<li>Chọn một túi bóng bất kỳ và chia thành hai túi mới, mỗi túi chứa một số bóng <strong>dương</strong>.

    <ul>
    <li>Ví dụ, một túi có <code>5</code> quả bóng có thể trở thành hai túi mới lần lượt có <code>1</code> và <code>4</code> quả bóng, hoặc hai túi có <code>2</code> và <code>3</code> quả bóng.</li>
    </ul>
    </li>

</ul>

<p>Mức phạt là số bóng <strong>lớn nhất</strong> trong một túi. Bạn muốn <strong>tối thiểu hóa</strong> mức phạt sau các thao tác.</p>

<p>Trả về <em>mức phạt nhỏ nhất có thể sau khi thực hiện các thao tác</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9], maxOperations = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Chia túi có 9 quả bóng thành hai túi lần lượt có 6 và 3 quả bóng. [<strong><u>9</u></strong>] -&gt; [6,3].
- Chia túi có 6 quả bóng thành hai túi lần lượt có 3 và 3 quả bóng. [<strong><u>6</u></strong>,3] -&gt; [3,3,3].
Túi có nhiều bóng nhất chứa 3 quả, nên mức phạt là 3 và ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,8,2], maxOperations = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Chia túi có 8 quả bóng thành hai túi lần lượt có 4 và 4 quả bóng. [2,4,<strong><u>8</u></strong>,2] -&gt; [2,4,4,4,2].
- Chia túi có 4 quả bóng thành hai túi lần lượt có 2 và 2 quả bóng. [2,<strong><u>4</u></strong>,4,4,2] -&gt; [2,2,2,4,4,2].
- Chia túi có 4 quả bóng thành hai túi lần lượt có 2 và 2 quả bóng. [2,2,2,<strong><u>4</u></strong>,4,2] -&gt; [2,2,2,2,2,4,2].
- Chia túi có 4 quả bóng thành hai túi lần lượt có 2 và 2 quả bóng. [2,2,2,2,2,<strong><u>4</u></strong>,2] -&gt; [2,2,2,2,2,2,2,2].
Túi có nhiều bóng nhất chứa 2 quả, nên mức phạt là 2 và ta trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maxOperations, nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác chia một túi thành hai phần chứa số bóng dương. Ta tối thiểu hóa giá trị lớn nhất cuối cùng với ngân sách $\textit{maxOperations}$. Giá trị lớn nhất càng cao thì điều kiện càng dễ thỏa mãn, nên tính khả thi là đơn điệu.
>
> Tìm kiếm nhị phân giới hạn $mx$: một túi có $x$ quả bóng cần $(x-1)//mx$ lần chia. Nếu tổng số lần chia không vượt ngân sách, thử một $mx$ nhỏ hơn. Đáp án là giới hạn nhỏ nhất khả thi.

<!-- thinking:end -->

Bài toán yêu cầu tối thiểu hóa chi phí, chính là số bóng lớn nhất trong một túi. Khi giá trị lớn nhất tăng, số thao tác giảm nên điều kiện càng dễ thỏa mãn.

Do đó, ta có thể dùng tìm kiếm nhị phân để tìm số bóng lớn nhất trong một túi và xác định xem có thể đạt được giá trị đó trong không quá $\textit{maxOperations}$ thao tác hay không.

Cụ thể, ta đặt biên trái của tìm kiếm nhị phân là $l = 1$ và biên phải là $r = \max(\textit{nums})$. Sau đó, ta liên tục tìm kiếm nhị phân trên giá trị giữa $\textit{mid} = \frac{l + r}{2}$. Với mỗi $\textit{mid}$, ta tính số thao tác cần thiết. Nếu số thao tác nhỏ hơn hoặc bằng $\textit{maxOperations}$, nghĩa là $\textit{mid}$ thỏa mãn điều kiện, và ta cập nhật biên phải $r$ thành $\textit{mid}$. Ngược lại, ta cập nhật biên trái $l$ thành $\textit{mid} + 1$.

Cuối cùng, ta trả về biên trái $l$.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài và giá trị lớn nhất của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSize(self, nums: List[int], maxOperations: int) -> int:
        def check(mx: int) -> bool:
            return sum((x - 1) // mx for x in nums) <= maxOperations

        return bisect_left(range(1, max(nums) + 1), True, key=check) + 1
```

#### Java

```java
class Solution {
    public int minimumSize(int[] nums, int maxOperations) {
        int l = 1, r = Arrays.stream(nums).max().getAsInt();
        while (l < r) {
            int mid = (l + r) >> 1;
            long s = 0;
            for (int x : nums) {
                s += (x - 1) / mid;
            }
            if (s <= maxOperations) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSize(vector<int>& nums, int maxOperations) {
        int l = 1, r = ranges::max(nums);
        while (l < r) {
            int mid = (l + r) >> 1;
            long long s = 0;
            for (int x : nums) {
                s += (x - 1) / mid;
            }
            if (s <= maxOperations) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func minimumSize(nums []int, maxOperations int) int {
	r := slices.Max(nums)
	return 1 + sort.Search(r, func(mx int) bool {
		mx++
		s := 0
		for _, x := range nums {
			s += (x - 1) / mx
		}
		return s <= maxOperations
	})
}
```

#### TypeScript

```ts
function minimumSize(nums: number[], maxOperations: number): number {
    let [l, r] = [1, Math.max(...nums)];
    while (l < r) {
        const mid = (l + r) >> 1;
        const s = nums.map(x => ((x - 1) / mid) | 0).reduce((a, b) => a + b);
        if (s <= maxOperations) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_size(nums: Vec<i32>, max_operations: i32) -> i32 {
        let mut l = 1;
        let mut r = *nums.iter().max().unwrap();

        while l < r {
            let mid = (l + r) / 2;
            let mut s: i64 = 0;

            for &x in &nums {
                s += ((x - 1) / mid) as i64;
            }

            if s <= max_operations as i64 {
                r = mid;
            } else {
                l = mid + 1;
            }
        }

        l
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} maxOperations
 * @return {number}
 */
var minimumSize = function (nums, maxOperations) {
    let [l, r] = [1, Math.max(...nums)];
    while (l < r) {
        const mid = (l + r) >> 1;
        const s = nums.map(x => ((x - 1) / mid) | 0).reduce((a, b) => a + b);
        if (s <= maxOperations) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
};
```

#### C#

```cs
public class Solution {
    public int MinimumSize(int[] nums, int maxOperations) {
        int l = 1, r = nums.Max();
        while (l < r) {
            int mid = (l + r) >> 1;
            long s = 0;
            foreach (int x in nums) {
                s += (x - 1) / mid;
            }
            if (s <= maxOperations) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
