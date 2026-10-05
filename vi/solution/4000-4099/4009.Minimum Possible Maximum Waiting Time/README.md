---
comments: true
difficulty: Hard
rating: 2498
source: Biweekly Contest 188 Q4
tags:
    - Memoization
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [4009. Minimum Possible Maximum Waiting Time](https://leetcode.com/problems/minimum-possible-maximum-waiting-time)

[中文文档](/solution/4000-4099/4009.Minimum%20Possible%20Maximum%20Waiting%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>demand</code>, trong đó <code>demand[i]</code> là lượng nhiên liệu mà chiếc xe thứ <code>i<sup>th</sup></code> cần.</p>

<p>Đồng thời cho một mảng số nguyên <code>fuel</code> có độ dài 2. Có <strong>chính xác</strong> hai trụ bơm nhiên liệu, được đánh số 0 và 1, trong đó <code>fuel[j]</code> là lượng nhiên liệu ban đầu có sẵn ở trụ <code>j</code>.</p>

<p>Các xe được phép bắt đầu đổ nhiên liệu theo thứ tự chỉ số <strong>tăng dần</strong>. Xe 0 được phép bắt đầu tại thời điểm 0, và với mỗi <code>i &gt; 0</code>, xe <code>i</code> được phép bắt đầu <strong>chính xác</strong> khi xe <code>i - 1</code> bắt đầu đổ nhiên liệu.</p>

<p>Quá trình đổ nhiên liệu tuân theo các quy tắc sau:</p>

<ul>
\t<li>Mỗi trụ có thể phục vụ <strong>nhiều nhất</strong> một xe tại một thời điểm.</li>
\t<li>Khi một xe được phép bắt đầu đổ nhiên liệu, bạn phải chọn một trụ còn <strong>ít nhất</strong> <code>demand[i]</code> nhiên liệu. Nếu cả hai trụ đều còn đủ nhiên liệu, bạn có thể chọn <strong>một trong hai</strong>, bất kể thời điểm chúng rảnh.</li>
\t<li>Xe phải chờ cho đến khi trụ đã chọn rảnh và bắt đầu đổ nhiên liệu <strong>ngay lập tức</strong>. Xe không thể đổi trụ hoặc cố tình chờ sau khi trụ đã chọn rảnh.</li>
\t<li>Khi một xe bắt đầu đổ nhiên liệu, lượng nhiên liệu còn lại ở trụ đã chọn giảm đi <code>demand[i]</code>, và trụ đó vẫn bận trong <code>demand[i]</code> giây.</li>
\t<li>Một khi đã bắt đầu, quá trình đổ nhiên liệu không thể bị gián đoạn.</li>
\t<li>Nếu không trụ nào còn ít nhất <code>demand[i]</code> nhiên liệu khi xe <code>i</code> được phép bắt đầu, quá trình kết thúc và không thể phục vụ các xe tiếp theo.</li>
</ul>

<p><strong>Thời gian chờ</strong> của một xe là khoảng thời gian từ lúc xe được phép bắt đầu đổ nhiên liệu đến lúc nó thực sự bắt đầu.</p>

<p>Trả về giá trị <strong>nhỏ nhất</strong> có thể của <strong>thời gian chờ lớn nhất</strong> trong số các xe được phục vụ, xét trên mọi cách phân công giúp <strong>tối đa hóa</strong> số xe được phục vụ. Nếu không thể phục vụ xe nào, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">demand = [6,8,4,6,5], fuel = [16,13]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách phân công sau phục vụ cả năm xe:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse; text-align:center;">
\t<tbody>
\t\t<tr>
\t\t\t<th>Xe</th>
\t\t\t<th>Được phép bắt đầu lúc</th>
\t\t\t<th>Bắt đầu đổ nhiên liệu lúc</th>
\t\t\t<th>Trụ được dùng</th>
\t\t\t<th>Nhiên liệu còn lại trước khi bắt đầu<br />
\t\t\t\t(trụ 0, trụ 1)</th>
\t\t\t<th>Thời gian chờ</th>
\t\t</tr>
\t\t<tr>
\t\t\t<td>0</td>
\t\t\t<td>0</td>
\t\t\t<td>0</td>
\t\t\t<td>0</td>
\t\t\t<td>(16, 13)</td>
\t\t\t<td>0</td>
\t\t</tr>
\t\t<tr>
\t\t\t<td>1</td>
\t\t\t<td>0</td>
\t\t\t<td>0</td>
\t\t\t<td>1</td>
\t\t\t<td>(10, 13)</td>
\t\t\t<td>0</td>
\t\t</tr>
\t\t<tr>
\t\t\t<td>2</td>
\t\t\t<td>0</td>
\t\t\t<td>6</td>
\t\t\t<td>0</td>
\t\t\t<td>(10, 5)</td>
\t\t\t<td>6</td>
\t\t</tr>
\t\t<tr>
\t\t\t<td>3</td>
\t\t\t<td>6</td>
\t\t\t<td>10</td>
\t\t\t<td>0</td>
\t\t\t<td>(6, 5)</td>
\t\t\t<td>4</td>
\t\t</tr>
\t\t<tr>
\t\t\t<td>4</td>
\t\t\t<td>10</td>
\t\t\t<td>10</td>
\t\t\t<td>1</td>
\t\t\t<td>(0, 5)</td>
\t\t\t<td>0</td>
\t\t</tr>
\t</tbody>
</table>

<p>Vậy cả năm xe đều được phục vụ và thời gian chờ lớn nhất là 6.</p>

<p>Để phục vụ cả năm xe, trụ 0 phải phục vụ các xe có nhu cầu 6, 4 và 6, còn trụ 1 phải phục vụ các xe có nhu cầu 8 và 5. Do đó, xe 2 phải chờ đến thời điểm 6 để trụ 0 rảnh, nên không có cách phân công nào phục vụ cả năm xe với thời gian chờ lớn nhất nhỏ hơn 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">demand = [10,15], fuel = [12,17]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
\t<li>Tại thời điểm 0, xe 0 được phép bắt đầu và bắt đầu đổ nhiên liệu ở trụ 0.</li>
\t<li>Xe 1 được phép bắt đầu tại thời điểm 0 (khi xe 0 bắt đầu) và lập tức bắt đầu đổ nhiên liệu ở trụ 1.</li>
\t<li>Cả hai xe đều bắt đầu mà không phải chờ, nên thời gian chờ lớn nhất là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">demand = [10,5], fuel = [8,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
\t<li>Tại thời điểm 0, xe 0 được phép bắt đầu. Tuy nhiên, không trụ nào có đủ nhiên liệu để phục vụ xe này, nên quá trình kết thúc ngay lập tức.</li>
\t<li>Không có xe nào được phục vụ, nên đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
\t<li><code>1 &lt;= demand.length &lt;= 50</code></li>
\t<li><code>1 &lt;= demand[i] &lt;= 20</code></li>
\t<li><code>fuel.length == 2</code></li>
\t<li><code>1 &lt;= fuel[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các xe được phép bắt đầu theo thứ tự chỉ số và chỉ có hai trụ với lượng nhiên liệu giới hạn. Việc phân công mỗi xe cho một trụ là một bài toán tìm kiếm $2^n$, không thể thực hiện với $n\le 50$.
>
> Trước tiên, ta tối đa hóa tiền tố số xe được phục vụ, sau đó tối thiểu hóa thời gian chờ lớn nhất trong các cách phân công đó. Vì vậy, state cần lưu lượng nhiên liệu còn lại và thời điểm rảnh tiếp theo của cả hai trụ.
>
> Nhu cầu tối đa là $20$ và dung lượng tối đa là $50$, nên các chiều nhiên liệu đủ nhỏ để tìm kiếm có memoization trên tiền tố đã phục vụ và state của hai trụ.

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
