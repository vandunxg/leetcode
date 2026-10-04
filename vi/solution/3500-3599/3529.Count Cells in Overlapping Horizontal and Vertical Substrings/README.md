---
comments: true
difficulty: Medium
rating: 2105
source: Biweekly Contest 155 Q3
tags:
    - Array
    - String
    - Matrix
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3529. Count Cells in Overlapping Horizontal and Vertical Substrings](https://leetcode.com/problems/count-cells-in-overlapping-horizontal-and-vertical-substrings)

[中文文档](/solution/3500-3599/3529.Count%20Cells%20in%20Overlapping%20Horizontal%20and%20Vertical%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <code>m x n</code> <code>grid</code> gồm các ký tự và một chuỗi <code>pattern</code>.</p>

<p><strong data-end="264" data-start="240">Chuỗi con theo chiều ngang</strong> là một dãy ký tự liên tiếp được đọc từ trái sang phải. Nếu đã đến cuối một hàng trước khi đọc xong chuỗi con, chuỗi sẽ quay lại cột đầu tiên của hàng tiếp theo và tiếp tục khi cần. Bạn <strong>không</strong> quay từ hàng cuối cùng về hàng đầu tiên.</p>

<p><strong data-end="484" data-start="462">Chuỗi con theo chiều dọc</strong> là một dãy ký tự liên tiếp được đọc từ trên xuống dưới. Nếu đã đến cuối một cột trước khi đọc xong chuỗi con, chuỗi sẽ quay lại hàng đầu tiên của cột tiếp theo và tiếp tục khi cần. Bạn <strong>không</strong> quay từ cột cuối cùng về cột đầu tiên.</p>

<p>Hãy đếm số ô trong ma trận thỏa mãn điều kiện sau:</p>

<ul>
    <li>Ô đó phải thuộc <strong>ít nhất</strong> một chuỗi con theo chiều ngang và <strong>ít nhất</strong> một chuỗi con theo chiều dọc, trong đó <strong>cả hai</strong> chuỗi con đều bằng với <code>pattern</code> đã cho.</li>
</ul>

<p>Trả về số lượng ô này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3529.Count%20Cells%20in%20Overlapping%20Horizontal%20and%20Vertical%20Substrings/images/gridtwosubstringsdrawio.png" style="width: 150px; height: 187px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;a&quot;,&quot;a&quot;,&quot;c&quot;,&quot;c&quot;],[&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;c&quot;],[&quot;a&quot;,&quot;a&quot;,&quot;b&quot;,&quot;a&quot;],[&quot;c&quot;,&quot;a&quot;,&quot;a&quot;,&quot;c&quot;],[&quot;a&quot;,&quot;a&quot;,&quot;b&quot;,&quot;a&quot;]], pattern = &quot;abaca&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Pattern <code>&quot;abaca&quot;</code> xuất hiện một lần dưới dạng chuỗi con theo chiều ngang (màu xanh dương) và một lần dưới dạng chuỗi con theo chiều dọc (màu đỏ), giao nhau tại một ô (màu tím).</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3529.Count%20Cells%20in%20Overlapping%20Horizontal%20and%20Vertical%20Substrings/images/gridexample2fixeddrawio.png" style="width: 150px; height: 150px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;c&quot;,&quot;a&quot;,&quot;a&quot;,&quot;a&quot;],[&quot;a&quot;,&quot;a&quot;,&quot;b&quot;,&quot;a&quot;],[&quot;b&quot;,&quot;b&quot;,&quot;a&quot;,&quot;a&quot;],[&quot;a&quot;,&quot;a&quot;,&quot;b&quot;,&quot;a&quot;]], pattern = &quot;aba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các ô được tô màu ở trên đều thuộc ít nhất một chuỗi con theo chiều ngang và một chuỗi con theo chiều dọc khớp với pattern <code>&quot;aba&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[&quot;a&quot;]], pattern = &quot;a&quot;</span></p>

<p><strong>Đầu ra:</strong> 1</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>m == grid.length</code></li>
    <li><code>n == grid[i].length</code></li>
    <li><code>1 &lt;= m, n &lt;= 1000</code></li>
    <li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= pattern.length &lt;= m * n</code></li>
    <li><code>grid</code> và <code>pattern</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi theo chiều ngang và chiều dọc sau khi trải phẳng có độ dài $mn \le 10^5$, nên việc trượt $\textit{pattern}$ trên ma trận sẽ quá chậm. Một ô phải thuộc ít nhất một lần khớp theo chiều ngang và một lần khớp theo chiều dọc.
>
> Nối các hàng và cột, đánh dấu phạm vi bao phủ bằng KMP (hoặc thuật toán Z), ánh xạ các vị trí khớp trở lại các ô, rồi đếm phần giao nhau.

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
