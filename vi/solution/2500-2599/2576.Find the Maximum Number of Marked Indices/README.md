---
comments: true
difficulty: Medium
rating: 1843
source: Weekly Contest 334 Q3
tags:
    - Greedy
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2576. Find the Maximum Number of Marked Indices](https://leetcode.com/problems/find-the-maximum-number-of-marked-indices)

[中文文档](/solution/2500-2599/2576.Find%20the%20Maximum%20Number%20of%20Marked%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code>.</p>

<p>Ban đầu, tất cả các chỉ số đều chưa được đánh dấu. Bạn được phép thực hiện thao tác sau nhiều lần tùy ý:</p>

<ul>
	<li>Chọn hai chỉ số <strong>chưa được đánh dấu khác nhau</strong> <code>i</code> và <code>j</code> sao cho <code>2 * nums[i] &lt;= nums[j]</code>, sau đó đánh dấu <code>i</code> và <code>j</code>.</li>
</ul>

<p>Hãy trả về <em>số lượng chỉ số lớn nhất có thể đánh dấu trong <code>nums</code> bằng cách thực hiện thao tác trên nhiều lần tùy ý</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,5,2,4]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Trong thao tác đầu tiên, chọn i = 2 và j = 1. Thao tác này hợp lệ vì 2 * nums[2] &lt;= nums[1]. Sau đó đánh dấu chỉ số 2 và 1.
Có thể chứng minh rằng không còn thao tác hợp lệ nào khác, nên đáp án là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,2,5,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Trong thao tác đầu tiên, chọn i = 3 và j = 0. Thao tác này hợp lệ vì 2 * nums[3] &lt;= nums[0]. Sau đó đánh dấu chỉ số 3 và 0.
Trong thao tác thứ hai, chọn i = 1 và j = 2. Thao tác này hợp lệ vì 2 * nums[1] &lt;= nums[2]. Sau đó đánh dấu chỉ số 1 và 2.
Vì không còn thao tác nào khác, đáp án là 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,6,8]
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Không có thao tác hợp lệ nào, nên đáp án là 0.

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
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

### Lời giải 1: Tham lam + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp có thể được đánh dấu khi hai lần giá trị nhỏ hơn không vượt quá giá trị lớn hơn; mỗi chỉ số chỉ được sử dụng nhiều nhất một lần. Có nhiều nhất $\lfloor n/2\rfloor$ cặp, vì vậy ta nên thử ghép nửa nhỏ hơn với nửa lớn hơn.
>
> Sau khi sắp xếp, con trỏ trái nằm ở nửa có giá trị nhỏ hơn, còn nửa bên phải được duyệt từ vị trí trung vị. Nếu một giá trị lớn hơn hoặc bằng hai lần giá trị bên trái, ta tạo được một cặp và dịch con trỏ trái sang phải. Đáp án là hai lần số lần ghép thành công.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể tạo nhiều nhất $n / 2$ cặp chỉ số, trong đó $n$ là độ dài của mảng $\textit{nums}$.

Để đánh dấu được nhiều chỉ số nhất, ta có thể sắp xếp mảng $\textit{nums}$. Tiếp theo, ta duyệt từng phần tử $\textit{nums}[j]$ ở nửa bên phải của mảng, đồng thời dùng con trỏ $\textit{i}$ trỏ đến phần tử nhỏ nhất ở nửa bên trái. Nếu $\textit{nums}[i] \times 2 \leq \textit{nums}[j]$, ta có thể đánh dấu các chỉ số $\textit{i}$ và $\textit{j}$, rồi dịch $\textit{i}$ sang phải một vị trí. Tiếp tục duyệt các phần tử ở nửa bên phải cho đến cuối mảng. Khi đó, số chỉ số có thể đánh dấu là $\textit{i} \times 2$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxNumOfMarkedIndices(self, nums: List[int]) -> int:
        nums.sort()
        i, n = 0, len(nums)
        for x in nums[(n + 1) // 2 :]:
            if nums[i] * 2 <= x:
                i += 1
        return i * 2
```

#### Java

```java
class Solution {
    public int maxNumOfMarkedIndices(int[] nums) {
        Arrays.sort(nums);
        int i = 0, n = nums.length;
        for (int j = (n + 1) / 2; j < n; ++j) {
            if (nums[i] * 2 <= nums[j]) {
                ++i;
            }
        }
        return i * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxNumOfMarkedIndices(vector<int>& nums) {
        ranges::sort(nums);
        int i = 0, n = nums.size();
        for (int j = (n + 1) / 2; j < n; ++j) {
            if (nums[i] * 2 <= nums[j]) {
                ++i;
            }
        }
        return i * 2;
    }
};
```

#### Go

```go
func maxNumOfMarkedIndices(nums []int) (ans int) {
	sort.Ints(nums)
	i, n := 0, len(nums)
	for _, x := range nums[(n+1)/2:] {
		if nums[i]*2 <= x {
			i++
		}
	}
	return i * 2
}
```

#### TypeScript

```ts
function maxNumOfMarkedIndices(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let i = 0;
    for (let j = (n + 1) >> 1; j < n; ++j) {
        if (nums[i] * 2 <= nums[j]) {
            ++i;
        }
    }
    return i * 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_num_of_marked_indices(mut nums: Vec<i32>) -> i32 {
        nums.sort();
        let mut i = 0;
        let n = nums.len();
        for j in (n + 1) / 2..n {
            if nums[i] * 2 <= nums[j] {
                i += 1;
            }
        }
        (i * 2) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
