---
comments: true
difficulty: Hard
rating: 2650
source: Weekly Contest 270 Q4
tags:
    - Depth-First Search
    - Graph
    - Array
    - Eulerian Path
    - Eulerian Circuit
    - Semi-Eulerian Graph
---

<!-- problem:start -->

# [2097. Valid Arrangement of Pairs](https://leetcode.com/problems/valid-arrangement-of-pairs)

[中文文档](/solution/2000-2099/2097.Valid%20Arrangement%20of%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>pairs</code> được <strong>đánh chỉ số từ 0</strong>, trong đó <code>pairs[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>. Một cách sắp xếp <code>pairs</code> là <strong>hợp lệ</strong> nếu với mọi chỉ số <code>i</code> thỏa mãn <code>1 &lt;= i &lt; pairs.length</code>, ta có <code>end<sub>i-1</sub> == start<sub>i</sub></code>.</p>

<p>Hãy trả về <em><strong>bất kỳ</strong> cách sắp xếp hợp lệ nào của </em><code>pairs</code>.</p>

<p><strong>Lưu ý:</strong> Dữ liệu đầu vào được tạo sao cho luôn tồn tại một cách sắp xếp hợp lệ của <code>pairs</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> pairs = [[5,1],[4,5],[11,9],[9,4]]
<strong>Đầu ra:</strong> [[11,9],[9,4],[4,5],[5,1]]
<strong>Giải thích:
</strong>Đây là một cách sắp xếp hợp lệ vì end<sub>i-1</sub> luôn bằng start<sub>i</sub>.
end<sub>0</sub> = 9 == 9 = start<sub>1</sub>
end<sub>1</sub> = 4 == 4 = start<sub>2</sub>
end<sub>2</sub> = 5 == 5 = start<sub>3</sub>
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> pairs = [[1,3],[3,2],[2,1]]
<strong>Đầu ra:</strong> [[1,3],[3,2],[2,1]]
<strong>Giải thích:</strong>
Đây là một cách sắp xếp hợp lệ vì end<sub>i-1</sub> luôn bằng start<sub>i</sub>.
end<sub>0</sub> = 3 == 3 = start<sub>1</sub>
end<sub>1</sub> = 2 == 2 = start<sub>2</sub>
Hai cách sắp xếp [[2,1],[1,3],[3,2]] và [[3,2],[2,1],[1,3]] cũng hợp lệ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> pairs = [[1,2],[1,3],[2,1]]
<strong>Đầu ra:</strong> [[1,2],[2,1],[1,3]]
<strong>Giải thích:</strong>
Đây là một cách sắp xếp hợp lệ vì end<sub>i-1</sub> luôn bằng start<sub>i</sub>.
end<sub>0</sub> = 2 == 2 = start<sub>1</sub>
end<sub>1</sub> = 1 == 1 = start<sub>2</sub>
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pairs.length &lt;= 10<sup>5</sup></code></li>
	<li><code>pairs[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub>, end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>start<sub>i</sub> != end<sub>i</sub></code></li>
	<li>Không có hai cặp nào hoàn toàn giống nhau.</li>
	<li>Luôn <strong>tồn tại</strong> một cách sắp xếp hợp lệ của <code>pairs</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp phải nối tiếp nhau, tương đương với một đường đi Euler trong đồ thị có hướng gồm các cạnh $start \to end$. Thuật toán Hierholzer chạy trong thời gian tuyến tính với $m \le 10^5$. Ta bắt đầu tại một đỉnh có bậc ra lớn hơn bậc vào đúng một đơn vị; nếu không có thì có thể bắt đầu ở bất kỳ đâu.
>
> Đưa các cạnh vào kết quả theo thứ tự hậu tự rồi đảo ngược để thu được cách sắp xếp. Các tab code đang để trống; phần cốt lõi là cách xây dựng đường đi Euler.

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
