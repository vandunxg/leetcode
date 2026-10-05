---
comments: true
difficulty: Hard
rating: 2162
source: Weekly Contest 487 Q4
tags:
    - Array
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [3830. Longest Alternating Subarray After Removing At Most One Element](https://leetcode.com/problems/longest-alternating-subarray-after-removing-at-most-one-element)

[中文文档](/solution/3800-3899/3830.Longest%20Alternating%20Subarray%20After%20Removing%20At%20Most%20One%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <span data-keyword="subarray">mảng con</span> <code>nums[l..r]</code> là <strong>luân phiên</strong> nếu thỏa mãn một trong các điều kiện sau:</p>

<ul>
	<li><code>nums[l] &lt; nums[l + 1] &gt; nums[l + 2] &lt; nums[l + 3] &gt; ...</code></li>
	<li><code>nums[l] &gt; nums[l + 1] &lt; nums[l + 2] &gt; nums[l + 3] &lt; ...</code></li>
</ul>

<p>Nói cách khác, khi so sánh các phần tử kề nhau trong mảng con, các phép so sánh luân phiên giữa <strong>lớn hơn nghiêm ngặt</strong> và <strong>nhỏ hơn nghiêm ngặt</strong>.</p>

<p>Bạn có thể xóa <strong>nhiều nhất một</strong> phần tử khỏi <code>nums</code>. Sau đó, chọn một mảng con luân phiên từ <code>nums</code>.</p>

<p>Trả về một số nguyên biểu thị <strong>độ dài</strong> <strong>lớn nhất</strong> của mảng con luân phiên mà bạn có thể chọn.</p>

<p>Mảng con có độ dài 1 được xem là luân phiên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn không xóa phần tử nào.</li>
	<li>Chọn toàn bộ mảng <code>[<u><strong>2, 1, 3, 2</strong></u>]</code>. Mảng này luân phiên vì <code>2 &gt; 1 &lt; 3 &gt; 2</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,1,2,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn xóa <code>nums[3]</code>, tức là <code>[3, 2, 1, <u><strong>2</strong></u>, 3, 2, 1]</code>. Mảng trở thành <code>[3, 2, 1, 3, 2, 1]</code>.</li>
	<li>Chọn mảng con <code>[3, <strong><u>2, 1, 3, 2</u></strong>, 1]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [100000,100000]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn không xóa phần tử nào.</li>
	<li>Chọn mảng con <code>[100000, <u><strong>100000</strong></u>]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tách tiền tố-hậu tố + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con luân phiên chuyển đổi giữa $<$ và $>$; ta được xóa nhiều nhất một phần tử. Với $n \le 10^5$, không thể thử mọi vị trí xóa.
>
> Nếu không xóa phần tử nào, đoạn dài nhất có thể được tính bằng recurrence từ trái sang phải dựa trên phép so sánh cuối cùng. Khi xóa $i$, ta thử nối đoạn bên trái tại $i-1$ với đoạn bên phải tại $i+1$.
>
> Tính trước, theo cả hai hướng, độ dài mảng con luân phiên dài nhất kết thúc tại $i$ và bắt đầu tại $i$.
>
> Lấy giá trị lớn nhất khi không xóa phần tử nào, sau đó thử xóa từng phần tử và chỉ cộng prefix, suffix tương ứng khi $nums[i-1]$ và $nums[i+1]$ vẫn luân phiên.

<!-- thinking:end -->

Ta dùng hai mảng $l_1$ và $l_2$ lần lượt biểu diễn độ dài mảng con luân phiên dài nhất kết thúc tại vị trí $i$, trong đó phép so sánh cuối cùng là "<" và ">". Tương tự, ta dùng $r_1$ và $r_2$ lần lượt biểu diễn độ dài mảng con luân phiên dài nhất bắt đầu tại vị trí $i$, trong đó phép so sánh đầu tiên là "<" và ">".

Ta có thể tính $l_1$ và $l_2$ bằng một lần duyệt từ trái sang phải, sau đó tính $r_1$ và $r_2$ bằng một lần duyệt từ phải sang trái.

Tiếp theo, ta khởi tạo đáp án bằng $\max(\max(l_1), \max(l_2))$, biểu diễn độ dài mảng con luân phiên dài nhất khi không xóa phần tử nào.

Sau đó, ta liệt kê vị trí $i$ của phần tử bị xóa. Nếu sau khi xóa vị trí $i$, các vị trí $i-1$ và $i+1$ vẫn có thể tạo thành một quan hệ luân phiên, ta có thể cộng $l_1[i-1]$ và $r_1[i+1]$ (hoặc $l_2[i-1]$ và $r_2[i+1]$) để cập nhật đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestAlternating(self, nums: List[int]) -> int:
        n = len(nums)
        l1 = [1] * n
        l2 = [1] * n
        r1 = [1] * n
        r2 = [1] * n
        ans = 0
        for i in range(1, n):
            if nums[i - 1] < nums[i]:
                l1[i] = l2[i - 1] + 1
            elif nums[i - 1] > nums[i]:
                l2[i] = l1[i - 1] + 1
            ans = max(ans, l1[i], l2[i])
        for i in range(n - 2, -1, -1):
            if nums[i + 1] > nums[i]:
                r1[i] = r2[i + 1] + 1
            elif nums[i + 1] < nums[i]:
                r2[i] = r1[i + 1] + 1
        for i in range(1, n - 1):
            if nums[i - 1] < nums[i + 1]:
                ans = max(ans, l2[i - 1] + r2[i + 1])
            elif nums[i - 1] > nums[i + 1]:
                ans = max(ans, l1[i - 1] + r1[i + 1])
        return ans
```

#### Java

```java
class Solution {
    public int longestAlternating(int[] nums) {
        int n = nums.length;
        int[] l1 = new int[n];
        int[] l2 = new int[n];
        int[] r1 = new int[n];
        int[] r2 = new int[n];

        for (int i = 0; i < n; i++) {
            l1[i] = 1;
            l2[i] = 1;
            r1[i] = 1;
            r2[i] = 1;
        }

        int ans = 0;

        for (int i = 1; i < n; i++) {
            if (nums[i - 1] < nums[i]) {
                l1[i] = l2[i - 1] + 1;
            } else if (nums[i - 1] > nums[i]) {
                l2[i] = l1[i - 1] + 1;
            }
            ans = Math.max(ans, l1[i]);
            ans = Math.max(ans, l2[i]);
        }

        for (int i = n - 2; i >= 0; i--) {
            if (nums[i + 1] > nums[i]) {
                r1[i] = r2[i + 1] + 1;
            } else if (nums[i + 1] < nums[i]) {
                r2[i] = r1[i + 1] + 1;
            }
        }

        for (int i = 1; i < n - 1; i++) {
            if (nums[i - 1] < nums[i + 1]) {
                ans = Math.max(ans, l2[i - 1] + r2[i + 1]);
            } else if (nums[i - 1] > nums[i + 1]) {
                ans = Math.max(ans, l1[i - 1] + r1[i + 1]);
            }
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestAlternating(vector<int>& nums) {
        int n = nums.size();
        vector<int> l1(n, 1), l2(n, 1), r1(n, 1), r2(n, 1);

        int ans = 0;

        for (int i = 1; i < n; i++) {
            if (nums[i - 1] < nums[i]) {
                l1[i] = l2[i - 1] + 1;
            } else if (nums[i - 1] > nums[i]) {
                l2[i] = l1[i - 1] + 1;
            }
            ans = max(ans, l1[i]);
            ans = max(ans, l2[i]);
        }

        for (int i = n - 2; i >= 0; i--) {
            if (nums[i + 1] > nums[i]) {
                r1[i] = r2[i + 1] + 1;
            } else if (nums[i + 1] < nums[i]) {
                r2[i] = r1[i + 1] + 1;
            }
        }

        for (int i = 1; i < n - 1; i++) {
            if (nums[i - 1] < nums[i + 1]) {
                ans = max(ans, l2[i - 1] + r2[i + 1]);
            } else if (nums[i - 1] > nums[i + 1]) {
                ans = max(ans, l1[i - 1] + r1[i + 1]);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func longestAlternating(nums []int) int {
	n := len(nums)
	l1 := make([]int, n)
	l2 := make([]int, n)
	r1 := make([]int, n)
	r2 := make([]int, n)

	for i := 0; i < n; i++ {
		l1[i] = 1
		l2[i] = 1
		r1[i] = 1
		r2[i] = 1
	}

	ans := 0

	for i := 1; i < n; i++ {
		if nums[i-1] < nums[i] {
			l1[i] = l2[i-1] + 1
		} else if nums[i-1] > nums[i] {
			l2[i] = l1[i-1] + 1
		}

		ans = max(ans, l1[i], l2[i])
	}

	for i := n - 2; i >= 0; i-- {
		if nums[i+1] > nums[i] {
			r1[i] = r2[i+1] + 1
		} else if nums[i+1] < nums[i] {
			r2[i] = r1[i+1] + 1
		}
	}

	for i := 1; i < n-1; i++ {
		if nums[i-1] < nums[i+1] {
			if l2[i-1]+r2[i+1] > ans {
				ans = l2[i-1] + r2[i+1]
			}
		} else if nums[i-1] > nums[i+1] {
			if l1[i-1]+r1[i+1] > ans {
				ans = l1[i-1] + r1[i+1]
			}
		}
	}

	return ans
}
```

#### TypeScript

```ts
function longestAlternating(nums: number[]): number {
    const n = nums.length;
    const l1 = new Array<number>(n).fill(1);
    const l2 = new Array<number>(n).fill(1);
    const r1 = new Array<number>(n).fill(1);
    const r2 = new Array<number>(n).fill(1);

    let ans = 0;

    for (let i = 1; i < n; i++) {
        if (nums[i - 1] < nums[i]) {
            l1[i] = l2[i - 1] + 1;
        } else if (nums[i - 1] > nums[i]) {
            l2[i] = l1[i - 1] + 1;
        }
        ans = Math.max(ans, l1[i]);
        ans = Math.max(ans, l2[i]);
    }

    for (let i = n - 2; i >= 0; i--) {
        if (nums[i + 1] > nums[i]) {
            r1[i] = r2[i + 1] + 1;
        } else if (nums[i + 1] < nums[i]) {
            r2[i] = r1[i + 1] + 1;
        }
    }

    for (let i = 1; i < n - 1; i++) {
        if (nums[i - 1] < nums[i + 1]) {
            ans = Math.max(ans, l2[i - 1] + r2[i + 1]);
        } else if (nums[i - 1] > nums[i + 1]) {
            ans = Math.max(ans, l1[i - 1] + r1[i + 1]);
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_alternating(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut l1 = vec![1; n];
        let mut l2 = vec![1; n];
        let mut r1 = vec![1; n];
        let mut r2 = vec![1; n];

        let mut ans = 0;

        for i in 1..n {
            if nums[i - 1] < nums[i] {
                l1[i] = l2[i - 1] + 1;
            } else if nums[i - 1] > nums[i] {
                l2[i] = l1[i - 1] + 1;
            }
            ans = ans.max(l1[i]);
            ans = ans.max(l2[i]);
        }

        for i in (0..n - 1).rev() {
            if nums[i + 1] > nums[i] {
                r1[i] = r2[i + 1] + 1;
            } else if nums[i + 1] < nums[i] {
                r2[i] = r1[i + 1] + 1;
            }
        }

        for i in 1..n - 1 {
            if nums[i - 1] < nums[i + 1] {
                ans = ans.max(l2[i - 1] + r2[i + 1]);
            } else if nums[i - 1] > nums[i + 1] {
                ans = ans.max(l1[i - 1] + r1[i + 1]);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
