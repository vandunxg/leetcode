---
comments: true
difficulty: Medium
rating: 2165
source: Weekly Contest 433 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3429. Paint House IV](https://leetcode.com/problems/paint-house-iv)

[中文文档](/solution/3400-3499/3429.Paint%20House%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>chẵn</strong> <code>n</code> biểu diễn số ngôi nhà được xếp thành một hàng thẳng, cùng một mảng 2 chiều <code>cost</code> có kích thước <code>n x 3</code>, trong đó <code>cost[i][j]</code> là chi phí sơn ngôi nhà thứ <code>i</code> bằng màu <code>j + 1</code>.</p>

<p>Các ngôi nhà được xem là <strong>đẹp</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Không có <strong>hai</strong> ngôi nhà kề nhau nào được sơn cùng màu.</li>
	<li>Các ngôi nhà <strong>cách đều</strong> hai đầu hàng <strong>không</strong> được sơn cùng màu. Ví dụ, nếu <code>n = 6</code>, các ngôi nhà ở vị trí <code>(0, 5)</code>, <code>(1, 4)</code> và <code>(2, 3)</code> được xem là cách đều nhau.</li>
</ul>

<p>Hãy trả về chi phí <strong>nhỏ nhất</strong> để sơn các ngôi nhà sao cho chúng <strong>đẹp</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, cost = [[3,5,7],[6,2,9],[4,8,1],[7,3,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phương án sơn tối ưu là <code>[1, 2, 3, 2]</code> với chi phí tương ứng là <code>[3, 2, 1, 3]</code>. Phương án này thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Không có hai ngôi nhà kề nhau nào cùng màu.</li>
	<li>Các ngôi nhà ở vị trí 0 và 3 (cách đều hai đầu hàng) không được sơn cùng màu <code>(1 != 2)</code>.</li>
	<li>Các ngôi nhà ở vị trí 1 và 2 (cách đều hai đầu hàng) không được sơn cùng màu <code>(2 != 3)</code>.</li>
</ul>

<p>Chi phí nhỏ nhất để sơn các ngôi nhà sao cho chúng đẹp là <code>3 + 2 + 1 + 3 = 9</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, cost = [[2,4,6],[5,3,8],[7,1,9],[4,6,2],[3,5,7],[8,2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">18</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phương án sơn tối ưu là <code>[1, 3, 2, 3, 1, 2]</code> với chi phí tương ứng là <code>[2, 8, 1, 2, 3, 2]</code>. Phương án này thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Không có hai ngôi nhà kề nhau nào cùng màu.</li>
	<li>Các ngôi nhà ở vị trí 0 và 5 (cách đều hai đầu hàng) không được sơn cùng màu <code>(1 != 2)</code>.</li>
	<li>Các ngôi nhà ở vị trí 1 và 4 (cách đều hai đầu hàng) không được sơn cùng màu <code>(3 != 1)</code>.</li>
	<li>Các ngôi nhà ở vị trí 2 và 3 (cách đều hai đầu hàng) không được sơn cùng màu <code>(2 != 3)</code>.</li>
</ul>

<p>Chi phí nhỏ nhất để sơn các ngôi nhà sao cho chúng đẹp là <code>2 + 8 + 1 + 2 + 3 + 2 = 18</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n</code> là số chẵn.</li>
	<li><code>cost.length == n</code></li>
	<li><code>cost[i].length == 3</code></li>
	<li><code>0 &lt;= cost[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có một số chẵn các ngôi nhà xếp thành một vòng tròn: các ngôi nhà kề nhau phải khác màu, đồng thời hai ngôi nhà đối diện $i$ và $n-1-i$ cũng phải khác màu. Vì $n\le 10^5$, không thể dùng một DP phức tạp hơn.
>
> Mỗi cặp đối xứng chỉ có $3\times 2=6$ cách tô màu hợp lệ. Giữa các cặp liên tiếp, ta chỉ cần đảm bảo hai màu ở phía tiếp giáp khác nhau.
>
> Thực hiện DP trên các cặp: $f[i][c_1][c_2]$ là chi phí nhỏ nhất để sơn cặp thứ $i$ bằng $(c_1,c_2)$. Chuyển trạng thái bằng cách duyệt màu của cặp trước đó, với $O(n)$ trạng thái.

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
