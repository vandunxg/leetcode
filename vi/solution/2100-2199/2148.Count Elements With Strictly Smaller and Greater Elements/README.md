---
comments: true
difficulty: Easy
rating: 1201
source: Weekly Contest 277 Q1
tags:
    - Array
    - Counting
    - Sorting
---

<!-- problem:start -->

# [2148. Count Elements With Strictly Smaller and Greater Elements](https://leetcode.com/problems/count-elements-with-strictly-smaller-and-greater-elements)

[中文文档](/solution/2100-2199/2148.Count%20Elements%20With%20Strictly%20Smaller%20and%20Greater%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <em>số lượng phần tử có <strong>cả</strong> một phần tử nhỏ hơn nghiêm ngặt và một phần tử lớn hơn nghiêm ngặt xuất hiện trong </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [11,7,2,15]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Phần tử 7 có phần tử 2 nhỏ hơn nghiêm ngặt và phần tử 11 lớn hơn nghiêm ngặt.
Phần tử 11 có phần tử 7 nhỏ hơn nghiêm ngặt và phần tử 15 lớn hơn nghiêm ngặt.
Tổng cộng có 2 phần tử có cả một phần tử nhỏ hơn nghiêm ngặt và một phần tử lớn hơn nghiêm ngặt xuất hiện trong <code>nums</code>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-3,3,3,90]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Phần tử 3 có phần tử -3 nhỏ hơn nghiêm ngặt và phần tử 90 lớn hơn nghiêm ngặt.
Vì có hai phần tử mang giá trị 3, tổng cộng có 2 phần tử có cả một phần tử nhỏ hơn nghiêm ngặt và một phần tử lớn hơn nghiêm ngặt xuất hiện trong <code>nums</code>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm giá trị nhỏ nhất và lớn nhất

<!-- thinking:start -->

> **Tư duy**
>
> Một phần tử có cả phần tử nhỏ hơn nghiêm ngặt và phần tử lớn hơn nghiêm ngặt khi và chỉ khi nó không phải là một trong hai giá trị cực biên của mảng. Ta chỉ cần đếm các giá trị nằm giữa giá trị nhỏ nhất và lớn nhất.
>
> Tính $\textit{mi}$ và $\textit{mx}$, sau đó đếm các $x$ thỏa mãn $\textit{mi}<x<\textit{mx}$.
>
> Duyệt mảng hai lần và chỉ dùng thêm bộ nhớ hằng số.

<!-- thinking:end -->

Theo mô tả bài toán, trước tiên ta có thể tìm giá trị nhỏ nhất $\textit{mi}$ và giá trị lớn nhất $\textit{mx}$ của mảng $\textit{nums}$. Sau đó, duyệt qua mảng $\textit{nums}$ và đếm số phần tử thỏa mãn $\textit{mi} < x < \textit{mx}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countElements(self, nums: List[int]) -> int:
        mi, mx = min(nums), max(nums)
        return sum(mi < x < mx for x in nums)
```

#### Java

```java
class Solution {
    public int countElements(int[] nums) {
        int mi = Arrays.stream(nums).min().getAsInt();
        int mx = Arrays.stream(nums).max().getAsInt();
        int ans = 0;
        for (int x : nums) {
            if (mi < x && x < mx) {
                ans++;
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
    int countElements(vector<int>& nums) {
        auto [mi, mx] = ranges::minmax_element(nums);
        return ranges::count_if(nums, [mi, mx](int x) { return *mi < x && x < *mx; });
    }
};
```

#### Go

```go
func countElements(nums []int) (ans int) {
	mi := slices.Min(nums)
	mx := slices.Max(nums)
	for _, x := range nums {
		if mi < x && x < mx {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countElements(nums: number[]): number {
    const mi = Math.min(...nums);
    const mx = Math.max(...nums);
    return nums.filter(x => mi < x && x < mx).length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
