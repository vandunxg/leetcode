---
comments: true
difficulty: Medium
rating: 2215
source: Weekly Contest 504 Q3
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3947. Maximum Number of Items From Sale II](https://leetcode.com/problems/maximum-number-of-items-from-sale-ii)

[中文文档](/solution/3900-3999/3947.Maximum%20Number%20of%20Items%20From%20Sale%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>items</code>, trong đó <code>items[i] = [factor<sub>i</sub>, price<sub>i</sub>]</code> biểu diễn vật phẩm thứ <code>i<sup>th</sup></code>. Ngoài ra, cho một số nguyên <code>budget</code>.</p>

<p>Có vô hạn bản sao của mỗi vật phẩm để mua. Bạn có thể mua bao nhiêu bản sao của bất kỳ vật phẩm nào tùy ý sao cho tổng chi phí của các bản sao đã mua không vượt quá <code>budget</code>.</p>

<p>Sau khi mua vật phẩm, bạn có thể nhận các bản sao miễn phí theo những quy tắc sau:</p>

<ul>
	<li>Mỗi bản sao đã mua của vật phẩm <code>i</code> có thể cho bạn <strong>nhiều nhất một</strong> bản sao miễn phí của một vật phẩm khác <code>j</code>.</li>
	<li>Vật phẩm miễn phí phải thỏa mãn <code>i != j</code> và <code>factor<sub>i</sub></code> chia hết <code>factor<sub>j</sub></code>.</li>
	<li>Với mỗi cặp có thứ tự <code>(i, j)</code>, bạn có thể nhận một bản sao miễn phí của vật phẩm <code>j</code> từ các lần mua vật phẩm <code>i</code> <strong>nhiều nhất một lần</strong>, bất kể bạn mua bao nhiêu bản sao vật phẩm <code>i</code>.</li>
	<li>Cùng một vật phẩm <code>j</code> có thể được nhận nhiều lần miễn phí nếu nó được nhận từ việc mua các loại vật phẩm khác nhau.</li>
</ul>

<p>Trả về <strong>tổng số bản sao vật phẩm lớn nhất</strong> mà bạn có thể nhận được, bao gồm cả bản sao đã mua và bản sao miễn phí, khi chi phí cho các vật phẩm đã mua không vượt quá <code>budget</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">items = [[1,6],[2,4],[3,5]], budget = 19</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bạn có thể mua 2 bản sao vật phẩm 0 và 1 bản sao vật phẩm 1 với tổng chi phí <code>2 * 6 + 4 = 16</code>, không vượt quá <code>budget = 19</code>.</li>
	<li>Một bản sao đã mua của vật phẩm 0 cho 1 bản sao miễn phí của vật phẩm 1, vì <code>factor<sub>0</sub> = 1</code> chia hết <code>factor<sub>1</sub> = 2</code>.</li>
	<li>Bản sao đã mua còn lại của vật phẩm 0 cho 1 bản sao miễn phí của vật phẩm 2, vì <code>factor<sub>0</sub> = 1</code> chia hết <code>factor<sub>2</sub> = 3</code>.</li>
	<li>Bạn có 3 bản sao đã mua và 2 bản sao miễn phí, tổng cộng là 5 bản sao vật phẩm.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">items = [[2,8],[1,10],[6,6],[4,12],[5,20],[5,17]], budget = 35</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bạn có thể mua 2 bản sao vật phẩm 0, 1 bản sao vật phẩm 1 và 1 bản sao vật phẩm 2 với tổng chi phí <code>2 * 8 + 10 + 6 = 32</code>, không vượt quá <code>budget = 35</code>.</li>
	<li>Một bản sao đã mua của vật phẩm 0 cho 1 bản sao miễn phí của vật phẩm 2, vì <code>factor<sub>0</sub> = 2</code> chia hết <code>factor<sub>2</sub> = 6</code>.</li>
	<li>Bản sao đã mua còn lại của vật phẩm 0 cho 1 bản sao miễn phí của vật phẩm 3, vì <code>factor<sub>0</sub> = 2</code> chia hết <code>factor<sub>3</sub> = 4</code>.</li>
	<li>Bản sao đã mua của vật phẩm 1 cho 1 bản sao miễn phí của vật phẩm 2, vì <code>factor<sub>1</sub> = 1</code> chia hết <code>factor<sub>2</sub> = 6</code>.</li>
	<li>Mua vật phẩm 2 không cho bản sao miễn phí nào, vì <code>factor<sub>2</sub> = 6</code> không chia hết factor của bất kỳ vật phẩm nào khác.</li>
	<li>Bạn có 4 bản sao đã mua và 3 bản sao miễn phí, tổng cộng là 7 bản sao vật phẩm.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= items.length &lt;= 10<sup>5</sup></code></li>
	<li><code>items[i] = [factor<sub>i</sub>, price<sub>i</sub>]</code></li>
	<li><code>1 &lt;= factor<sub>i</sub> &lt;= items.length</code></li>
	<li><code>1 &lt;= price<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= budget &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> So với phần I, cả số lượng vật phẩm và ngân sách đều rất lớn, nên không thể dùng ba lô. Quà tặng miễn phí tuân theo tính chia hết và mỗi $\textit{factor}$ không vượt quá $n$, vì vậy ta có thể đếm số loại vật phẩm mà một factor mở khóa bằng các bội số.
>
> Dạng của một phương án tối ưu vẫn là “một vật phẩm kích hoạt đã trả tiền, sau đó dùng ngân sách còn lại cho vật phẩm rẻ nhất”. Số lượng quà tặng của mỗi vật phẩm đầu tiên ứng viên phải được tính trong thời gian gần tuyến tính và so sánh với giá của nó.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng ở việc thay thế ba lô bằng phép đếm các bội số.

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
