---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Math
    - Floyd Cycle Detection
---

<!-- problem:start -->

# [957. Prison Cells After N Days](https://leetcode.com/problems/prison-cells-after-n-days)

[中文文档](/solution/0900-0999/0957.Prison%20Cells%20After%20N%20Days/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>8</code> ô nhà tù xếp thành một hàng; mỗi ô hoặc có người ở, hoặc để trống.</p>

<p>Mỗi ngày, trạng thái có người ở hay để trống của mỗi ô thay đổi theo các quy tắc sau:</p>

<ul>
	<li>Nếu hai ô liền kề với một ô đều có người ở hoặc đều để trống, thì ô đó sẽ có người ở.</li>
	<li>Trong các trường hợp còn lại, ô đó sẽ để trống.</li>
</ul>

<p><strong>Lưu ý</strong> rằng vì nhà tù được xếp thành một hàng, ô đầu tiên và ô cuối cùng không có đủ hai ô liền kề.</p>

<p>Cho mảng số nguyên <code>cells</code>, trong đó <code>cells[i] == 1</code> nếu ô thứ <code>i<sup>th</sup></code> có người ở và <code>cells[i] == 0</code> nếu ô thứ <code>i<sup>th</sup></code> để trống; đồng thời cho số nguyên <code>n</code>.</p>

<p>Hãy trả về trạng thái nhà tù sau <code>n</code> ngày (tức là sau <code>n</code> lần thay đổi như mô tả ở trên).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> cells = [0,1,0,1,1,0,0,1], n = 7
<strong>Output:</strong> [0,0,1,1,0,0,0,0]
<strong>Giải thích:</strong> Bảng sau tóm tắt trạng thái nhà tù trong từng ngày:
Ngày 0: [0, 1, 0, 1, 1, 0, 0, 1]
Ngày 1: [0, 1, 1, 0, 0, 0, 0, 0]
Ngày 2: [0, 0, 0, 0, 1, 1, 1, 0]
Ngày 3: [0, 1, 1, 0, 0, 1, 0, 0]
Ngày 4: [0, 0, 0, 0, 0, 1, 0, 0]
Ngày 5: [0, 1, 1, 1, 0, 1, 0, 0]
Ngày 6: [0, 0, 1, 0, 1, 1, 0, 0]
Ngày 7: [0, 0, 1, 1, 0, 0, 0, 0]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> cells = [1,0,0,1,0,0,1,0], n = 1000000000
<strong>Output:</strong> [0,0,1,1,1,1,1,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>cells.length == 8</code></li>
	<li><code>cells[i]</code>&nbsp;bằng <code>0</code> hoặc <code>1</code>.</li>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các ô cập nhật trạng thái dựa trên hàng xóm, còn $n$ có thể lên đến $10^9$, nên không thể mô phỏng từng ngày. Chỉ có $8$ ô và hai đầu hàng đều trở thành ô trống sau ngày đầu tiên, vì vậy tối đa chỉ có $2^6$ trạng thái và diễn tiến sẽ lặp chu kỳ. Lưu mỗi trạng thái cùng ngày tương ứng; khi gặp lại một trạng thái, lấy $n$ modulo độ dài chu kỳ để nhảy thẳng đến ngày cần tìm.

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
