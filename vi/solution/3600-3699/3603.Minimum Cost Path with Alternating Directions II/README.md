---
comments: true
difficulty: Medium
rating: 1639
source: Biweekly Contest 160 Q2
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3603. Minimum Cost Path with Alternating Directions II](https://leetcode.com/problems/minimum-cost-path-with-alternating-directions-ii)

[中文文档](/solution/3600-3699/3603.Minimum%20Cost%20Path%20with%20Alternating%20Directions%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>m</code> và <code>n</code> lần lượt biểu thị số hàng và số cột của một grid.</p>

<p>Chi phí đi vào ô <code>(i, j)</code> được xác định là <code>(i + 1) * (j + 1)</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2D <code>waitCost</code>, trong đó <code>waitCost[i][j]</code> xác định chi phí <strong>chờ</strong> tại ô đó.</p>

<p>Đường đi luôn bắt đầu bằng việc đi vào ô <code>(0, 0)</code> ở bước 1 và trả chi phí đi vào ô.</p>

<p>Ở mỗi bước, bạn tuân theo quy luật luân phiên:</p>

<ul>
    <li>Ở các giây được đánh số <strong>lẻ</strong>, bạn phải di chuyển <strong>sang phải</strong> hoặc <strong>xuống dưới</strong> đến một ô <strong>liền kề</strong>, đồng thời trả chi phí đi vào ô đó.</li>
    <li>Ở các giây được đánh số <strong>chẵn</strong>, bạn phải <strong>chờ</strong> tại chỗ trong <strong>đúng</strong> một giây và trả <code>waitCost[i][j]</code> trong giây đó.</li>
</ul>

<p>Trả về tổng chi phí <strong>nhỏ nhất</strong> cần thiết để đi đến ô <code>(m - 1, n - 1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 1, n = 2, waitCost = [[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<ul>
    <li>Bắt đầu tại ô <code>(0, 0)</code> ở giây 1 với chi phí đi vào ô là <code>(0 + 1) * (0 + 1) = 1</code>.</li>
    <li><strong>Giây 1</strong>: Di chuyển sang phải đến ô <code>(0, 1)</code> với chi phí đi vào ô là <code>(0 + 1) * (1 + 1) = 2</code>.</li>
</ul>

<p>Vì vậy, tổng chi phí là <code>1 + 2 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 2, waitCost = [[3,5],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<ul>
    <li>Bắt đầu tại ô <code>(0, 0)</code> ở giây 1 với chi phí đi vào ô là <code>(0 + 1) * (0 + 1) = 1</code>.</li>
    <li><strong>Giây 1</strong>: Di chuyển xuống dưới đến ô <code>(1, 0)</code> với chi phí đi vào ô là <code>(1 + 1) * (0 + 1) = 2</code>.</li>
    <li><strong>Giây 2</strong>: Chờ tại ô <code>(1, 0)</code>, trả <code>waitCost[1][0] = 2</code>.</li>
    <li><strong>Giây 3</strong>: Di chuyển sang phải đến ô <code>(1, 1)</code> với chi phí đi vào ô là <code>(1 + 1) * (1 + 1) = 4</code>.</li>
</ul>

<p>Vì vậy, tổng chi phí là <code>1 + 2 + 2 + 4 = 9</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 3, waitCost = [[6,1,4],[3,2,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<ul>
    <li>Bắt đầu tại ô <code>(0, 0)</code> ở giây 1 với chi phí đi vào ô là <code>(0 + 1) * (0 + 1) = 1</code>.</li>
    <li><strong>Giây 1</strong>: Di chuyển sang phải đến ô <code>(0, 1)</code> với chi phí đi vào ô là <code>(0 + 1) * (1 + 1) = 2</code>.</li>
    <li><strong>Giây 2</strong>: Chờ tại ô <code>(0, 1)</code>, trả <code>waitCost[0][1] = 1</code>.</li>
    <li><strong>Giây 3</strong>: Di chuyển xuống dưới đến ô <code>(1, 1)</code> với chi phí đi vào ô là <code>(1 + 1) * (1 + 1) = 4</code>.</li>
    <li><strong>Giây 4</strong>: Chờ tại ô <code>(1, 1)</code>, trả <code>waitCost[1][1] = 2</code>.</li>
    <li><strong>Giây 5</strong>: Di chuyển sang phải đến ô <code>(1, 2)</code> với chi phí đi vào ô là <code>(1 + 1) * (2 + 1) = 6</code>.</li>
</ul>

<p>Vì vậy, tổng chi phí là <code>1 + 2 + 1 + 4 + 2 + 6 = 16</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
    <li><code>2 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
    <li><code>waitCost.length == m</code></li>
    <li><code>waitCost[0].length == n</code></li>
    <li><code>0 &lt;= waitCost[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ được di chuyển sang phải hoặc xuống dưới, đồng thời các giây lẻ phải di chuyển còn các giây chẵn phải chờ, nên một đường đi từ $(0,0)$ đến $(m-1,n-1)$ được xác định bởi chuỗi di chuyển, với các lần chờ được chèn giữa hai lần di chuyển liên tiếp.
>
> Không thể liệt kê tất cả đường đi, nhưng $m\cdot n\le 10^5$ cho phép dùng quy hoạch động tuyến tính. Để đến $(i,j)$ cần đúng $i+j$ lần di chuyển. Mọi ô trừ ô bắt đầu đều phải chờ trước lần di chuyển tiếp theo; ô đích không cần chờ sau khi đến.
>
> Chi phí đi vào ô là $(i+1)(j+1)$. Chuyển trạng thái từ ô phía trên hoặc bên trái, cộng phí đi vào ô, đồng thời cộng $\textit{waitCost}[i][j]$ cho các ô không phải ô đích. Ô bắt đầu chỉ chịu chi phí đi vào ô. Tọa độ xác định tính chẵn lẻ của thời gian chờ, nên tính chất cấu trúc con tối ưu được đảm bảo.

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
