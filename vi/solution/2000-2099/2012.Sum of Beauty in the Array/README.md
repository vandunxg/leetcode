---
comments: true
difficulty: Medium
rating: 1467
source: Weekly Contest 259 Q2
tags:
    - Array
---

<!-- problem:start -->

# [2012. Sum of Beauty in the Array](https://leetcode.com/problems/sum-of-beauty-in-the-array)

[Tài liệu tiếng Trung](/solution/2000-2099/2012.Sum%20of%20Beauty%20in%20the%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh <strong>chỉ số từ 0</strong>. Với mỗi chỉ số <code>i</code> (<code>1 &lt;= i &lt;= nums.length - 2</code>), <strong>độ đẹp</strong> của <code>nums[i]</code> được xác định như sau:</p>

<ul>
	<li><code>2</code>, nếu <code>nums[j] &lt; nums[i] &lt; nums[k]</code> đúng với <strong>mọi</strong> <code>0 &lt;= j &lt; i</code> và <strong>mọi</strong> <code>i &lt; k &lt;= nums.length - 1</code>.</li>
	<li><code>1</code>, nếu <code>nums[i - 1] &lt; nums[i] &lt; nums[i + 1]</code> và điều kiện trước đó không thỏa mãn.</li>
	<li><code>0</code>, nếu không điều kiện nào ở trên thỏa mãn.</li>
</ul>

<p>Trả về <em><strong>tổng độ đẹp</strong> của tất cả </em><code>nums[i]</code><em> với </em><code>1 &lt;= i &lt;= nums.length - 2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Với mỗi chỉ số i trong khoảng 1 &lt;= i &lt;= 1:
- Độ đẹp của nums[1] bằng 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,6,4]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Với mỗi chỉ số i trong khoảng 1 &lt;= i &lt;= 2:
- Độ đẹp của nums[1] bằng 1.
- Độ đẹp của nums[2] bằng 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Với mỗi chỉ số i trong khoảng 1 &lt;= i &lt;= 1:
- Độ đẹp của nums[1] bằng 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý giá trị nhỏ nhất bên phải + Duyệt để duy trì giá trị lớn nhất bên trái

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 10^5$, việc quét lại các giá trị cực trị bên trái/phải cho mỗi $i$ sẽ có độ phức tạp bậc hai. Độ đẹp bằng $2$ cần $nums[i]$ lớn hơn nghiêm ngặt mọi giá trị bên trái và nhỏ hơn nghiêm ngặt mọi giá trị bên phải; độ đẹp bằng $1$ chỉ xét các phần tử kề nó.
>
> Ta tiền xử lý các giá trị nhỏ nhất của hậu tố $right[i]$; một biến $l$ được cập nhật để theo dõi giá trị lớn nhất bên trái.
>
> Với mỗi chỉ số ở giữa, trước tiên kiểm tra $l < nums[i] < right[i+1]$ để xác định độ đẹp bằng $2$, nếu không thì kiểm tra bộ ba phần tử liền kề để xác định độ đẹp bằng $1$.

<!-- thinking:end -->

Ta có thể tiền xử lý mảng giá trị nhỏ nhất bên phải $right$, trong đó $right[i]$ biểu diễn giá trị nhỏ nhất trong đoạn $nums[i..n-1]$.

Sau đó, ta duyệt mảng $nums$ từ trái sang phải, đồng thời duy trì giá trị lớn nhất $l$ ở bên trái. Với mỗi vị trí $i$, ta kiểm tra điều kiện $l < nums[i] < right[i + 1]$. Nếu điều kiện đúng, ta cộng $2$ vào đáp án. Nếu không, ta kiểm tra điều kiện $nums[i - 1] < nums[i] < nums[i + 1]$. Nếu điều kiện đúng, ta cộng $1$ vào đáp án.

Sau khi duyệt xong, ta thu được đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfBeauties(self, nums: List[int]) -> int:
        n = len(nums)
        right = [nums[-1]] * n
        for i in range(n - 2, -1, -1):
            right[i] = min(right[i + 1], nums[i])
        ans = 0
        l = nums[0]
        for i in range(1, n - 1):
            r = right[i + 1]
            if l < nums[i] < r:
                ans += 2
            elif nums[i - 1] < nums[i] < nums[i + 1]:
                ans += 1
            l = max(l, nums[i])
        return ans
```

#### Java

```java
class Solution {
    public int sumOfBeauties(int[] nums) {
        int n = nums.length;
        int[] right = new int[n];
        right[n - 1] = nums[n - 1];
        for (int i = n - 2; i > 0; --i) {
            right[i] = Math.min(right[i + 1], nums[i]);
        }
        int ans = 0;
        int l = nums[0];
        for (int i = 1; i < n - 1; ++i) {
            int r = right[i + 1];
            if (l < nums[i] && nums[i] < r) {
                ans += 2;
            } else if (nums[i - 1] < nums[i] && nums[i] < nums[i + 1]) {
                ans += 1;
            }
            l = Math.max(l, nums[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfBeauties(vector<int>& nums) {
        int n = nums.size();
        vector<int> right(n, nums[n - 1]);
        for (int i = n - 2; i; --i) {
            right[i] = min(right[i + 1], nums[i]);
        }
        int ans = 0;
        for (int i = 1, l = nums[0]; i < n - 1; ++i) {
            int r = right[i + 1];
            if (l < nums[i] && nums[i] < r) {
                ans += 2;
            } else if (nums[i - 1] < nums[i] && nums[i] < nums[i + 1]) {
                ans += 1;
            }
            l = max(l, nums[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfBeauties(nums []int) (ans int) {
	n := len(nums)
	right := make([]int, n)
	right[n-1] = nums[n-1]
	for i := n - 2; i > 0; i-- {
		right[i] = min(right[i+1], nums[i])
	}
	for i, l := 1, nums[0]; i < n-1; i++ {
		r := right[i+1]
		if l < nums[i] && nums[i] < r {
			ans += 2
		} else if nums[i-1] < nums[i] && nums[i] < nums[i+1] {
			ans++
		}
		l = max(l, nums[i])
	}
	return
}
```

#### TypeScript

```ts
function sumOfBeauties(nums: number[]): number {
    const n = nums.length;
    const right: number[] = Array(n).fill(nums[n - 1]);
    for (let i = n - 2; i; --i) {
        right[i] = Math.min(right[i + 1], nums[i]);
    }
    let ans = 0;
    for (let i = 1, l = nums[0]; i < n - 1; ++i) {
        const r = right[i + 1];
        if (l < nums[i] && nums[i] < r) {
            ans += 2;
        } else if (nums[i - 1] < nums[i] && nums[i] < nums[i + 1]) {
            ans += 1;
        }
        l = Math.max(l, nums[i]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_of_beauties(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut right: Vec<i32> = vec![0; n];
        right[n - 1] = nums[n - 1];
        for i in (1..n - 1).rev() {
            right[i] = right[i + 1].min(nums[i]);
        }
        let mut ans = 0;
        let mut l = nums[0];
        for i in 1..n - 1 {
            let r = right[i + 1];
            if l < nums[i] && nums[i] < r {
                ans += 2;
            } else if nums[i - 1] < nums[i] && nums[i] < nums[i + 1] {
                ans += 1;
            }
            l = l.max(nums[i]);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
