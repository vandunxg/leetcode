---
comments: true
difficulty: Easy
rating: 1246
source: Weekly Contest 260 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2016. Maximum Difference Between Increasing Elements](https://leetcode.com/problems/maximum-difference-between-increasing-elements)

[中文文档](/solution/2000-2099/2016.Maximum%20Difference%20Between%20Increasing%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code> có kích thước <code>n</code>, hãy tìm <strong>hiệu lớn nhất</strong> giữa <code>nums[i]</code> và <code>nums[j]</code> (tức là <code>nums[j] - nums[i]</code>), sao cho <code>0 &lt;= i &lt; j &lt; n</code> và <code>nums[i] &lt; nums[j]</code>.</p>

<p>Trả về <em><strong>hiệu lớn nhất</strong>. </em>Nếu không tồn tại cặp <code>i</code> và <code>j</code> thỏa mãn, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,<strong><u>1</u></strong>,<strong><u>5</u></strong>,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Hiệu lớn nhất đạt được với i = 1 và j = 2, nums[j] - nums[i] = 5 - 1 = 4.
Lưu ý rằng với i = 1 và j = 0, hiệu nums[j] - nums[i] = 7 - 1 = 6, nhưng i &gt; j nên không hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,4,3,2]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Không tồn tại i và j nào sao cho i &lt; j và nums[i] &lt; nums[j].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [<strong><u>1</u></strong>,5,2,<strong><u>10</u></strong>]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Hiệu lớn nhất đạt được với i = 0 và j = 3, nums[j] - nums[i] = 10 - 1 = 9.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì giá trị nhỏ nhất của tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 1000$ cho phép dùng vòng lặp kép, nhưng ta chỉ cần tìm hiệu lớn nhất $nums[j]-nums[i]$ với $i<j$. Với một điểm kết thúc bên phải cố định, phần tử bên trái tốt nhất là giá trị nhỏ nhất của tiền tố.
>
> Theo dõi $mi$: nếu $x>mi$ thì cập nhật hiệu, nếu không thì thay $mi$ bằng $x$. Giữ kết quả là $-1$ nếu không tồn tại cặp tăng.

<!-- thinking:end -->

Ta sử dụng biến $\textit{mi}$ để biểu diễn giá trị nhỏ nhất trong các phần tử đã duyệt, và biến $\textit{ans}$ để biểu diễn hiệu lớn nhất. Ban đầu, đặt $\textit{mi}$ bằng $+\infty$ và $\textit{ans}$ bằng $-1$.

Duyệt qua mảng. Với phần tử hiện tại $x$, nếu $x \gt \textit{mi}$, cập nhật $\textit{ans}$ thành $\max(\textit{ans}, x - \textit{mi})$. Ngược lại, cập nhật $\textit{mi}$ thành $x$.

Sau khi duyệt xong, trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumDifference(self, nums: List[int]) -> int:
        mi = inf
        ans = -1
        for x in nums:
            if x > mi:
                ans = max(ans, x - mi)
            else:
                mi = x
        return ans
```

#### Java

```java
class Solution {
    public int maximumDifference(int[] nums) {
        int mi = 1 << 30;
        int ans = -1;
        for (int x : nums) {
            if (x > mi) {
                ans = Math.max(ans, x - mi);
            } else {
                mi = x;
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
    int maximumDifference(vector<int>& nums) {
        int mi = 1 << 30;
        int ans = -1;
        for (int& x : nums) {
            if (x > mi) {
                ans = max(ans, x - mi);
            } else {
                mi = x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumDifference(nums []int) int {
	mi := 1 << 30
	ans := -1
	for _, x := range nums {
		if mi < x {
			ans = max(ans, x-mi)
		} else {
			mi = x
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maximumDifference(nums: number[]): number {
    let [ans, mi] = [-1, Infinity];
    for (const x of nums) {
        if (x > mi) {
            ans = Math.max(ans, x - mi);
        } else {
            mi = x;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_difference(nums: Vec<i32>) -> i32 {
        let mut mi = i32::MAX;
        let mut ans = -1;

        for &x in &nums {
            if x > mi {
                ans = ans.max(x - mi);
            } else {
                mi = x;
            }
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maximumDifference = function (nums) {
    let [ans, mi] = [-1, Infinity];
    for (const x of nums) {
        if (x > mi) {
            ans = Math.max(ans, x - mi);
        } else {
            mi = x;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
