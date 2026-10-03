---
comments: true
difficulty: Medium
rating: 1414
source: Weekly Contest 256 Q2
tags:
    - Array
    - String
    - Divide and Conquer
    - Quickselect
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1985. Find the Kth Largest Integer in the Array](https://leetcode.com/problems/find-the-kth-largest-integer-in-the-array)

[中文文档](/solution/1900-1999/1985.Find%20the%20Kth%20Largest%20Integer%20in%20the%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>nums</code> và một số nguyên <code>k</code>. Mỗi chuỗi trong <code>nums</code> biểu diễn một số nguyên không có các số 0 ở đầu.</p>

<p>Hãy trả về <em>chuỗi biểu diễn số nguyên lớn thứ </em><code>k<sup>th</sup></code><em><strong> trong </strong></em><code>nums</code>.</p>

<p><strong>Lưu ý</strong>: Các số trùng nhau được tính riêng biệt. Ví dụ, nếu <code>nums</code> là <code>[&quot;1&quot;,&quot;2&quot;,&quot;2&quot;]</code>, thì <code>&quot;2&quot;</code> là số nguyên lớn nhất, <code>&quot;2&quot;</code> là số nguyên lớn thứ hai và <code>&quot;1&quot;</code> là số nguyên lớn thứ ba.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;3&quot;,&quot;6&quot;,&quot;7&quot;,&quot;10&quot;], k = 4
<strong>Đầu ra:</strong> &quot;3&quot;
<strong>Giải thích:</strong>
Các số trong nums theo thứ tự không giảm là [&quot;3&quot;,&quot;6&quot;,&quot;7&quot;,&quot;10&quot;].
Số nguyên lớn thứ 4<sup>th</sup> trong nums là &quot;3&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;2&quot;,&quot;21&quot;,&quot;12&quot;,&quot;1&quot;], k = 3
<strong>Đầu ra:</strong> &quot;2&quot;
<strong>Giải thích:</strong>
Các số trong nums theo thứ tự không giảm là [&quot;1&quot;,&quot;2&quot;,&quot;12&quot;,&quot;21&quot;].
Số nguyên lớn thứ 3<sup>rd</sup> trong nums là &quot;2&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;0&quot;,&quot;0&quot;], k = 2
<strong>Đầu ra:</strong> &quot;0&quot;
<strong>Giải thích:</strong>
Các số trong nums theo thứ tự không giảm là [&quot;0&quot;,&quot;0&quot;].
Số nguyên lớn thứ 2<sup>nd</sup> trong nums là &quot;0&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i].length &lt;= 100</code></li>
	<li><code>nums[i]</code> chỉ gồm các chữ số.</li>
	<li><code>nums[i]</code> sẽ không có các số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp hoặc Quickselect

<!-- thinking:start -->

> **Tư duy**
>
> Các phần tử là chuỗi biểu diễn số thập phân nên không được so sánh theo thứ tự từ điển. Sau khi phân tích thành số nguyên, ta cần tìm số lớn thứ $k$; chỉ cần dùng heap để chọn.
>
> $\texttt{nlargest}(k,\cdot)$ với khóa $\texttt{int}$ trả về phần tử đó ở vị trí $k-1$.

<!-- thinking:end -->

Ta có thể sắp xếp các chuỗi trong mảng $\textit{nums}$ theo thứ tự giảm dần dựa trên giá trị số nguyên, sau đó lấy phần tử thứ $k$. Ngoài ra, ta có thể dùng thuật toán quickselect để tìm số nguyên lớn thứ $k$.

Độ phức tạp thời gian là $O(n \times \log n)$ hoặc $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(\log n)$ hoặc $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthLargestNumber(self, nums: List[str], k: int) -> str:
        return nlargest(k, nums, key=lambda x: int(x))[k - 1]
```

#### Java

```java
class Solution {
    public String kthLargestNumber(String[] nums, int k) {
        Arrays.sort(
            nums, (a, b) -> a.length() == b.length() ? b.compareTo(a) : b.length() - a.length());
        return nums[k - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    string kthLargestNumber(vector<string>& nums, int k) {
        nth_element(nums.begin(), nums.begin() + k - 1, nums.end(), [](const string& a, const string& b) {
            return a.size() == b.size() ? a > b : a.size() > b.size();
        });
        return nums[k - 1];
    }
};
```

#### Go

```go
func kthLargestNumber(nums []string, k int) string {
	sort.Slice(nums, func(i, j int) bool {
		a, b := nums[i], nums[j]
		if len(a) == len(b) {
			return a > b
		}
		return len(a) > len(b)
	})
	return nums[k-1]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
