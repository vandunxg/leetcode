---
comments: true
difficulty: Medium
rating: 1832
source: Biweekly Contest 173 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3796. Find Maximum Value in a Constrained Sequence](https://leetcode.com/problems/find-maximum-value-in-a-constrained-sequence)

[中文文档](/solution/3700-3799/3796.Find%20Maximum%20Value%20in%20a%20Constrained%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>, một mảng số nguyên 2 chiều <code>restrictions</code> và một mảng số nguyên <code>diff</code> có độ dài <code>n - 1</code>. Nhiệm vụ của bạn là xây dựng một dãy có độ dài <code>n</code>, ký hiệu là <code>a[0], a[1], ..., a[n - 1]</code>, sao cho dãy thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>a[0]</code> bằng 0.</li>
	<li>Mọi phần tử trong dãy đều <strong>không âm</strong>.</li>
	<li>Với mọi chỉ số <code>i</code> (<code>0 &lt;= i &lt;= n - 2</code>), <code>abs(a[i] - a[i + 1]) &lt;= diff[i]</code>.</li>
	<li>Với mỗi <code>restrictions[i] = [idx, maxVal]</code>, giá trị tại vị trí <code>idx</code> trong dãy không được vượt quá <code>maxVal</code> (tức là <code>a[idx] &lt;= maxVal</code>).</li>
</ul>

<p>Mục tiêu của bạn là xây dựng một dãy hợp lệ sao cho <strong>giá trị lớn nhất</strong> trong dãy đạt <strong>giá trị lớn nhất có thể</strong>, đồng thời thỏa mãn tất cả các điều kiện trên.</p>

<p>Trả về một số nguyên biểu thị <strong>giá trị lớn nhất</strong> xuất hiện trong dãy tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10, restrictions = [[3,1],[8,1]], diff = [2,2,3,1,4,5,1,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Dãy <code>a = [0, 2, 4, 1, 2, 6, 2, 1, 1, 3]</code> thỏa mãn các ràng buộc đã cho (<code>a[3] &lt;= 1</code> và <code>a[8] &lt;= 1</code>).</li>
	<li>Giá trị lớn nhất trong dãy là 6.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 8, restrictions = [[3,2]], diff = [3,5,2,4,2,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Dãy <code>a = [0, 3, 3, 2, 6, 8, 11, 12]</code> thỏa mãn ràng buộc đã cho (<code>a[3] &lt;= 2</code>).</li>
	<li>Giá trị lớn nhất trong dãy là 12.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= restrictions.length &lt;= n - 1</code></li>
	<li><code>restrictions[i].length == 2</code></li>
	<li><code>restrictions[i] = [idx, maxVal]</code></li>
	<li><code>1 &lt;= idx &lt; n</code></li>
	<li><code>1 &lt;= maxVal &lt;= 10<sup>6</sup></code></li>
	<li><code>diff.length == n - 1</code></li>
	<li><code>1 &lt;= diff[i] &lt;= 10</code></li>
	<li>Các giá trị của <code>restrictions[i][0]</code> là duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Dãy bắt đầu bằng $0$, hiệu giữa các giá trị kề nhau không vượt quá $\textit{diff}[i]$, và một số chỉ số có giới hạn trên. Trước hết, ta hình thành các độ cao đỉnh khi chưa xét ràng buộc, sau đó lan truyền từng giới hạn sang trái và phải qua các giới hạn $\textit{diff}$ rồi lấy giá trị nhỏ nhất tại từng vị trí; đáp án là giá trị lớn nhất trong các độ cao đó.

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
