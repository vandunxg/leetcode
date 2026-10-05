---
comments: true
difficulty: Hard
rating: 2044
source: Weekly Contest 518 Q4
---

<!-- problem:start -->

# [4046. Minimum Cost Path With At Most K Turns](https://leetcode.com/problems/minimum-cost-path-with-at-most-k-turns)

[中文文档](/solution/4000-4099/4046.Minimum%20Cost%20Path%20With%20At%20Most%20K%20Turns/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>grid</code> có kích thước <code>m x n</code>, trong đó <code>grid[i][j]</code> biểu thị chi phí đi qua ô <code>(i, j)</code>, và một số nguyên <code>k</code>.</p>

<p>Bạn bắt đầu tại ô <strong>trên cùng bên trái</strong> <code>(0, 0)</code> và muốn đi đến ô <strong>dưới cùng bên phải</strong> <code>(m - 1, n - 1)</code>.</p>

<p>Từ mỗi ô, bạn có thể di chuyển một bước theo một trong bốn hướng: <strong>lên</strong>, <strong>xuống</strong>, <strong>trái</strong> hoặc <strong>phải</strong>.</p>

<p>Chi phí của một đường đi là tổng giá trị của tất cả các ô đã đi qua, <strong>bao gồm</strong> cả ô bắt đầu và ô kết thúc. Nếu một ô được đi qua nhiều hơn một lần, giá trị của ô đó được cộng vào mỗi lần đi qua.</p>

<p>Trả về chi phí đường đi <strong>nhỏ nhất</strong> có thể để đến <code>(m - 1, n - 1)</code> bằng <strong>không quá</strong> <code>k</code> lần rẽ. Nếu không tồn tại đường đi như vậy, trả về <code>-1</code>.</p>

<p>Một <strong>lần rẽ</strong> xảy ra khi hướng di chuyển thay đổi giữa hai bước liên tiếp. Ví dụ, di chuyển sang phải rồi xuống được tính là một lần rẽ, còn di chuyển sang phải rồi tiếp tục sang phải thì không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[2,7,3],[1,4,5]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một đường đi tối ưu là <code>(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (1, 2)</code>. Các bước di chuyển lần lượt là xuống, phải, phải.</li>
	<li>Hướng di chuyển thay đổi từ xuống sang phải một lần, nên đường đi sử dụng chính xác <code>k = 1</code> lần rẽ.</li>
	<li>Tổng chi phí đường đi là <code>2 + 1 + 4 + 5 = 12</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[4,1,9],[3,2,5],[4,8,6]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Một đường đi tối ưu là <code>(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (1, 2) &rarr; (2, 2)</code>. Các bước di chuyển lần lượt là xuống, phải, phải, xuống.</li>
	<li>Hướng di chuyển thay đổi từ xuống sang phải và từ phải sang xuống, nên đường đi sử dụng chính xác <code>k = 2</code> lần rẽ.</li>
	<li>Tổng chi phí đường đi là <code>4 + 3 + 2 + 5 + 6 = 20</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,9],[3,4]], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không thể đến <code>(1, 1)</code> bằng <code>k = 0</code> lần rẽ. Do đó, đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m == grid.length &lt;= 75</code></li>
	<li><code>1 &lt;= n == grid[i].length &lt;= 75</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 1000</code></li>
	<li><code>0 &lt;= k &lt; min(m, n)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Kích thước của grid tối đa là $75\times 75$ và $k<\min(m,n)$. Đường đi ngắn nhất nếu bỏ qua giới hạn số lần rẽ có thể rẽ quá nhiều lần; việc tìm kiếm trực tiếp trên đường đi có thể phải đi qua lại các ô.
>
> Trạng thái cần bao gồm vị trí, hướng di chuyển đến ô hiện tại và số lần rẽ đã sử dụng. Vì chi phí của các ô không âm, có thể áp dụng Dijkstra hoặc quy hoạch động theo chi phí.
>
> Đáp án là chi phí nhỏ nhất trong các trạng thái có số lần rẽ từ $0\ldots k$ tại đích, hoặc $-1$ nếu không trạng thái nào trong số đó có thể đến được.

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
