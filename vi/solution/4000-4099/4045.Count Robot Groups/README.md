---
comments: true
difficulty: Medium
rating: 1888
source: Weekly Contest 518 Q3
---

<!-- problem:start -->

# [4045. Count Robot Groups](https://leetcode.com/problems/count-robot-groups)

[中文文档](/solution/4000-4099/4045.Count%20Robot%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <span data-keyword="strictly-increasing-array">tăng dần nghiêm ngặt</span> <code>position</code>, trong đó <code>position[i]</code> là vị trí ban đầu của robot thứ <code>i<sup>th</sup></code> tại thời điểm <code>t = 0</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>speed</code>, trong đó <code>speed[i]</code> là tốc độ không đổi của robot thứ <code>i<sup>th</sup></code>, tính theo đơn vị mỗi giây, và một số nguyên <code>distance</code>.</p>

<p>Thời gian là <strong>liên tục</strong> và được đo bằng giây. Một robot hoặc nhóm có tốc độ <code>v</code> sẽ di chuyển <code>v * t</code> đơn vị sang phải trong bất kỳ khoảng thời gian <code>t</code> giây nào.</p>

<p>Bất cứ khi nào khoảng cách giữa hai robot hoặc hai nhóm không lớn hơn <code>distance</code>, chúng sẽ gộp thành một nhóm duy nhất.</p>

<p>Nếu nhiều robot hoặc nhóm thỏa điều kiện gộp tại cùng một thời điểm, tất cả các lần gộp đều diễn ra <strong>đồng thời</strong>. Cụ thể, mọi tập hợp liên thông gồm các robot hoặc nhóm mà vị trí của hai phần tử liên tiếp chênh lệch không quá <code>distance</code> đều được gộp thành một nhóm.</p>

<p>Sau khi gộp, nhóm mới nhận vị trí hiện tại và tốc độ của <strong>robot ngoài cùng bên phải</strong> trong nhóm đó. Một khi đã gộp, các robot không bao giờ tách ra.</p>

<p>Hãy trả về số nhóm còn lại sau khi mọi lần gộp có thể xảy ra đã hoàn tất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">position = [1,5,6,20], speed = [4,3,2,3], distance = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4045.Count%20Robot%20Groups/images/c4drawio.png" style="width: 500px; height: 397px;" /></strong></p>

<ul>
	<li>Ban đầu, các nhóm là {R<sub>1</sub>}, {R<sub>2</sub>}, {R<sub>3</sub>} và {R<sub>​​​​​​​4</sub>}.</li>
	<li>Tại <code>t = 0</code>, robot R<sub>2</sub> và R<sub>3</sub> lần lượt ở vị trí 5 và 6 gộp lại vì chúng cách nhau 1 đơn vị. Nhóm mới di chuyển với vị trí và tốc độ của robot ngoài cùng bên phải R<sub>3</sub>. Các nhóm lúc này là {R<sub>1</sub>}, {R<sub>2</sub>, R<sub>3</sub>} và {R<sub>​4</sub>}.</li>
	<li>Sau đó tại <code>t = 2</code>, robot R<sub>1</sub> đuổi kịp nhóm {R<sub>2</sub>, R<sub>3</sub>} và gộp với nhóm này. Các nhóm lúc này là {R<sub>1</sub>, R<sub>2</sub>, R<sub>3</sub>} và {R<sub>​4</sub>}.</li>
</ul>

<p>Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">position = [1,5,9], speed = [3,2,2], distance = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4045.Count%20Robot%20Groups/images/c5.png" style="width: 500px; height: 310px;" /></strong></p>

<ul>
	<li>Ban đầu, các nhóm là {R<sub>1</sub>}, {R<sub>2</sub>} và {R<sub>3</sub>}.</li>
	<li>Tại <code>t = 2</code>, robot R<sub>1</sub> đuổi kịp robot R<sub>2</sub> và gộp với nó. Nhóm mới di chuyển với vị trí và tốc độ của robot ngoài cùng bên phải R<sub>2</sub>. Các nhóm lúc này là {R<sub>1</sub>, R<sub>2</sub>} và {R<sub>3</sub>}.</li>
</ul>

<p>Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">position = [9], speed = [8], distance = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu chỉ có một nhóm. Vì vậy, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= position.length == speed.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= position[i], speed[i], distance &lt;= 10<sup>9</sup></code></li>
	<li><code>position</code> tăng dần nghiêm ngặt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các robot di chuyển sang phải với tốc độ không đổi và gộp lại khi cách nhau không quá $\textit{distance}$; nhóm mới nhận vị trí và tốc độ của phần tử ngoài cùng bên phải. Với $n=10^5$, ta không thể mô phỏng mọi lần gặp nhau theo thời gian liên tục.
>
> Việc một nhóm có đuổi kịp nhóm bên phải hay không phụ thuộc vào tốc độ tương đối và khả năng thu hẹp khoảng cách xuống còn $\textit{distance}$. Duyệt từ phải sang trái giúp duy trì đại diện của nhóm hiện tại (robot ngoài cùng bên phải): robot bên trái sẽ gia nhập nếu có thể đuổi kịp, nếu không thì bắt đầu một nhóm mới.
>
> Cấu trúc của bài toán giống bài toán car fleet, và một lượt duyệt tuyến tính là đủ để tính số nhóm cuối cùng.

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
