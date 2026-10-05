---
comments: true
difficulty: Medium
rating: 1762
source: Biweekly Contest 184 Q3
tags:
    - Greedy
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3952. Maximum Total Value of Covered Indices](https://leetcode.com/problems/maximum-total-value-of-covered-indices)

[Tài liệu tiếng Trung](/solution/3900-3999/3952.Maximum%20Total%20Value%20of%20Covered%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một chuỗi nhị phân <code>s</code> có độ dài <code>n</code>, trong đó <code>s[i] == &#39;1&#39;</code> nghĩa là chỉ số <code>i</code> ban đầu chứa một <strong>token</strong>, còn <code>s[i] == &#39;0&#39;</code> nghĩa là không chứa token.</p>

<p>Bạn có thể thực hiện thao tác sau một số lần bất kỳ:</p>

<ul>
	<li>Chọn một token hiện đang ở chỉ số <code>i</code>, với <code>i &gt; 0</code>, sao cho token này <strong>chưa từng được di chuyển</strong>.</li>
	<li>Di chuyển token này từ chỉ số <code>i</code> sang chỉ số <code>i - 1</code>.</li>
</ul>

<p>Một chỉ số được coi là <strong>được phủ</strong> nếu sau khi thực hiện tất cả các lần di chuyển, nó chứa một token.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng giá trị lớn nhất</strong> của <code>nums</code> tại các chỉ số được phủ sau khi thực hiện các thao tác một cách tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,2,6,1], s = &quot;0101&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu, các chỉ số 1 và 3 chứa token.</li>
	<li>Di chuyển token từ chỉ số 3 sang chỉ số 2.</li>
	<li>Di chuyển token từ chỉ số 1 sang chỉ số 0.</li>
	<li>Các chỉ số được phủ là <code>[0, 2]</code>, nên tổng giá trị là <code>nums[0] + nums[2] = 9 + 6 = 15</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,1,4], s = &quot;001&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu, chỉ có chỉ số 2 chứa token.</li>
	<li>Để đạt giá trị tối ưu, giữ token ở chỉ số 2.</li>
	<li>Chỉ số được phủ là <code>[2]</code>, nên tổng giá trị là <code>nums[2] = 4</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,3,5], s = &quot;011&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu, các chỉ số 1 và 2 chứa token.</li>
	<li>Di chuyển token từ chỉ số 1 sang chỉ số 0.</li>
	<li>Các chỉ số được phủ là <code>[0, 2]</code>, nên tổng giá trị là <code>nums[0] + nums[2] = 9 + 5 = 14</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length == s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>​​​​​​​<code>s[i]</code> chỉ có thể là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi dấu có thể di chuyển sang trái nhiều nhất một lần, và hai dấu không thể dùng chung một chỉ số. Với $n\le 10^5$, ta cần một quyết định tuyến tính.
>
> Duyệt từ trái sang phải, mỗi dấu có thể đứng yên hoặc dịch sang $i-1$ khi ô đó còn trống. Greedy nên đưa một dấu về phía giá trị $\textit{nums}$ lớn hơn, đồng thời để các dấu đi trước chiếm các vị trí trống sớm hơn.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần tư duy dừng ở phép gán mỗi dấu với nhiều nhất một lần dịch sang trái.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python

```

#### Java

```java

```

#### C++

```cpp

```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
