---
comments: true
difficulty: Easy
rating: 1213
source: Biweekly Contest 103 Q1
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2656. Maximum Sum With Exactly K Elements](https://leetcode.com/problems/maximum-sum-with-exactly-k-elements)

[中文文档](/solution/2600-2699/2656.Maximum%20Sum%20With%20Exactly%20K%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong> và một số nguyên <code>k</code>. Nhiệm vụ của bạn là thực hiện thao tác sau đây chính xác <strong><code>k</code></strong> lần để tối đa hóa điểm số:</p>

<ol>
	<li>Chọn một phần tử <code>m</code> từ <code>nums</code>.</li>
	<li>Xóa phần tử <code>m</code> đã chọn khỏi mảng.</li>
	<li>Thêm một phần tử mới có giá trị <code>m + 1</code> vào mảng.</li>
	<li>Tăng điểm số thêm <code>m</code>.</li>
</ol>

<p>Hãy trả về <em>điểm số lớn nhất có thể đạt được sau khi thực hiện thao tác chính xác</em> <code>k</code> <em>lần.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5], k = 3
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Ta cần chọn chính xác 3 phần tử từ nums để tổng là lớn nhất.
Ở lần lặp đầu tiên, ta chọn 5. Khi đó tổng là 5 và nums = [1,2,3,4,6]
Ở lần lặp thứ hai, ta chọn 6. Khi đó tổng là 5 + 6 và nums = [1,2,3,4,7]
Ở lần lặp thứ ba, ta chọn 7. Khi đó tổng là 5 + 6 + 7 = 18 và nums = [1,2,3,4,8]
Do đó, ta trả về 18.
Có thể chứng minh rằng 18 là đáp án lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,5,5], k = 2
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Ta cần chọn chính xác 2 phần tử từ nums để tổng là lớn nhất.
Ở lần lặp đầu tiên, ta chọn 5. Khi đó tổng là 5 và nums = [5,5,6]
Ở lần lặp thứ hai, ta chọn 6. Khi đó tổng là 5 + 6 = 11 và nums = [5,5,7]
Do đó, ta trả về 11.
Có thể chứng minh rằng 11 là đáp án lớn nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<p>&nbsp;</p>
<style type="text/css">.spoilerbutton {display:block; border:dashed; padding: 0px 0px; margin:10px 0px; font-size:150%; font-weight: bold; color:#000000; background-color:cyan; outline:0;
}
.spoiler {overflow:hidden;}
.spoiler > div {-webkit-transition: all 0s ease;-moz-transition: margin 0s ease;-o-transition: all 0s ease;transition: margin 0s ease;}
.spoilerbutton[value="Show Message"] + .spoiler > div {margin-top:-500%;}
.spoilerbutton[value="Hide Message"] + .spoiler {padding:5px;}
</style>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước lấy giá trị lớn nhất hiện tại rồi đưa lại vào mảng giá trị lớn hơn nó một đơn vị, thực hiện $k$ lần. Mô phỏng việc thêm phần tử vẫn đủ nhanh với $k \le 100$, nhưng phương án tối ưu luôn tái sử dụng giá trị lớn nhất toàn cục $x$.
>
> Điểm số là $x+(x+1)+\cdots+(x+k-1)=kx+k(k-1)/2$, vì vậy chỉ cần một phép lấy $\max$ là đủ.

<!-- thinking:end -->

Ta nhận thấy để điểm số cuối cùng là lớn nhất, mỗi lần chọn ta nên chọn phần tử lớn nhất có thể. Vì vậy, lần đầu tiên ta chọn phần tử lớn nhất $x$ trong mảng, lần thứ hai chọn $x+1$, lần thứ ba chọn $x+2$, cứ tiếp tục như vậy cho đến lần thứ $k$, khi ta chọn $x+k-1$. Cách chọn này đảm bảo phần tử được chọn luôn là phần tử lớn nhất trong mảng hiện tại, nên điểm số cuối cùng cũng là lớn nhất. Đáp án là tổng của $k$ lần $x$ cộng với $0+1+2+\cdots+(k-1)$, tức là $k \times x + (k - 1) \times k / 2$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximizeSum(self, nums: List[int], k: int) -> int:
        x = max(nums)
        return k * x + k * (k - 1) // 2
```

#### Java

```java
class Solution {
    public int maximizeSum(int[] nums, int k) {
        int x = 0;
        for (int v : nums) {
            x = Math.max(x, v);
        }
        return k * x + k * (k - 1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximizeSum(vector<int>& nums, int k) {
        int x = *max_element(nums.begin(), nums.end());
        return k * x + k * (k - 1) / 2;
    }
};
```

#### Go

```go
func maximizeSum(nums []int, k int) int {
	x := slices.Max(nums)
	return k*x + k*(k-1)/2
}
```

#### TypeScript

```ts
function maximizeSum(nums: number[], k: number): number {
    const x = Math.max(...nums);
    return k * x + (k * (k - 1)) / 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximize_sum(nums: Vec<i32>, k: i32) -> i32 {
        let mut mx = 0;

        for &n in &nums {
            if n > mx {
                mx = n;
            }
        }

        ((0 + k - 1) * k) / 2 + k * mx
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Công thức đóng giống với Lời giải 1, chỉ khác ở cách viết theo ngôn ngữ và phép tính; ta vẫn tìm giá trị lớn nhất rồi áp dụng công thức.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
impl Solution {
    pub fn maximize_sum(nums: Vec<i32>, k: i32) -> i32 {
        let mx = *nums.iter().max().unwrap_or(&0);

        ((0 + k - 1) * k) / 2 + k * mx
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
