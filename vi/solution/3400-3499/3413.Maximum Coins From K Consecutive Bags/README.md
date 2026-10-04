---
comments: true
difficulty: Medium
rating: 2373
source: Weekly Contest 431 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [3413. Maximum Coins From K Consecutive Bags](https://leetcode.com/problems/maximum-coins-from-k-consecutive-bags)

[中文文档](/solution/3400-3499/3413.Maximum%20Coins%20From%20K%20Consecutive%20Bags/README.md)

## Mô tả

<!-- description:start -->

<p>Trên một trục số có vô hạn túi, mỗi tọa độ tương ứng với một túi. Một số túi trong đó chứa xu.</p>

<p>Bạn được cho một mảng 2 chiều <code>coins</code>, trong đó <code>coins[i] = [l<sub>i</sub>, r<sub>i</sub>, c<sub>i</sub>]</code> biểu thị rằng mọi túi từ <code>l<sub>i</sub></code> đến <code>r<sub>i</sub></code> đều chứa <code>c<sub>i</sub></code> xu.</p>

<p>Các đoạn trong <code>coins</code> không chồng lấn.</p>

<p>Bạn cũng được cho một số nguyên <code>k</code>.</p>

<p>Hãy trả về lượng xu <strong>lớn nhất</strong> có thể thu được bằng cách thu thập <code>k</code> túi liên tiếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coins = [[8,10,1],[1,3,2],[5,6,4]], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn các túi tại các vị trí <code>[3, 4, 5, 6]</code> cho lượng xu lớn nhất: <code>2 + 0 + 4 + 4 = 10</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coins = [[1,10,3]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn các túi tại các vị trí <code>[1, 2]</code> cho lượng xu lớn nhất: <code>3 + 3 = 6</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= coins.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
	<li><code>coins[i] == [l<sub>i</sub>, r<sub>i</sub>, c<sub>i</sub>]</code></li>
	<li><code>1 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= c<sub>i</sub> &lt;= 1000</code></li>
	<li>Các đoạn đã cho không chồng lấn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số của túi có thể lên tới $10^9$ và $k$ có thể rất lớn, nên ta không thể duyệt từng túi. Các túi có xu được mô tả bởi các đoạn rời nhau $[l_i,r_i]$ với mỗi túi chứa $c_i$ xu, và ta cần tìm tổng lớn nhất trên mọi $k$ túi liên tiếp.
>
> Có thể dịch một cửa sổ tối ưu cho đến khi đầu trái hoặc đầu phải của nó chạm vào một đầu mút của đoạn. Sau khi sắp xếp các đoạn, bài toán trở thành tìm một cửa sổ có độ dài cố định trên các đoạn đó.
>
> Dùng tổng tiền tố để tính lượng xu khi lấy $k$ túi bắt đầu từ một vị trí cho trước. Ta thử các cửa sổ có đầu trái trùng với một $l_i$ hoặc đầu phải trùng với một $r_i$, rồi giữ lại giá trị lớn nhất.

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
