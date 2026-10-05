---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3967. Finish Time of Tasks II 🔒](https://leetcode.com/problems/finish-time-of-tasks-ii)

[中文文档](/solution/3900-3999/3967.Finish%20Time%20of%20Tasks%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số lượng task trong một project, được đánh số từ 0 đến <code>n - 1</code>. Các task này được liên kết thành một <strong>cây vô hướng</strong>. Cây được biểu diễn bằng mảng số nguyên hai chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh vô hướng nối task <code>u<sub>i</sub></code> và task <code>v<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một mảng <code>baseTime</code> có độ dài <code>n</code>, trong đó <code>baseTime[i]</code> là thời gian cần để hoàn thành task <code>i</code>.</p>

<p>Với bất kỳ task nào được chọn làm root, <strong>thời gian hoàn thành</strong> của mỗi task được tính như sau:</p>

<ul>
	<li>Task lá: Thời gian hoàn thành là <code>baseTime[i]</code>.</li>
	<li>Task không phải lá:
	<ul>
		<li>Gọi <code>earliest</code> là thời gian hoàn thành <strong>nhỏ nhất</strong> trong các task con, và <code>latest</code> là thời gian hoàn thành <strong>lớn nhất</strong> trong các task con.</li>
		<li>Gọi <code>ownDuration</code> là <code>(latest - earliest) + baseTime[i]</code>.</li>
		<li>Thời gian hoàn thành của task <code>i</code> là <code>latest + ownDuration</code>.</li>
	</ul>
	</li>
</ul>

<p>Chọn <strong>bất kỳ</strong> task nào làm root và tính thời gian hoàn thành của root đó theo các quy tắc trên.</p>

<p>Trả về thời gian hoàn thành <strong>nhỏ nhất</strong> có thể đạt được trong mọi lựa chọn root.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], baseTime = [9,1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>
<svg height="110" viewbox="50 30 400 124" width="350" xmlns="http://www.w3.org/2000/svg"> <rect fill="white" height="124" width="400" x="50" y="30"></rect> <line stroke="black" stroke-width="2" x1="100" x2="250" y1="80" y2="80"></line> <line stroke="black" stroke-width="2" x1="250" x2="400" y1="80" y2="80"></line> <circle cx="100" cy="80" fill="white" r="30" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="100" y="87">0</text> <text fill="black" font-size="16" text-anchor="middle" x="100" y="131">9</text> <circle cx="250" cy="80" fill="white" r="30" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="250" y="87">1</text> <text fill="black" font-size="16" text-anchor="middle" x="250" y="131">1</text> <circle cx="400" cy="80" fill="white" r="30" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="400" y="87">2</text> <text fill="black" font-size="16" text-anchor="middle" x="400" y="131">5</text> </svg>

<p>Lựa chọn tối ưu là chọn task 1 làm root.</p>

<ul>
	<li>Task 0 là task lá, nên thời gian hoàn thành là <code>baseTime[0] = 9</code>.</li>
	<li>Task 2 là task lá, nên thời gian hoàn thành là <code>baseTime[2] = 5</code>.</li>
	<li>Task 1 có hai task con với thời gian hoàn thành lần lượt là 9 và 5:
	<ul>
		<li><code>earliest = 5</code>, <code>latest = 9</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[1] = (9 - 5) + 1 = 5</code></li>
		<li>Thời gian hoàn thành của task 1 là <code>latest + ownDuration = 9 + 5 = 14</code></li>
	</ul>
	</li>
</ul>

<p>Vì vậy, thời gian hoàn thành nhỏ nhất có thể đạt được trong mọi lựa chọn root là 14.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[0,2]], baseTime = [4,7,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>
<svg height="215" viewbox="48 14 324 232" width="300" xmlns="http://www.w3.org/2000/svg"> <rect fill="white" height="232" width="324" x="48" y="14"></rect> <line stroke="black" stroke-width="2" x1="210" x2="110" y1="60" y2="180"></line> <line stroke="black" stroke-width="2" x1="210" x2="310" y1="60" y2="180"></line> <circle cx="210" cy="60" fill="white" r="32" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="210" y="66">0</text> <text fill="black" font-size="16" text-anchor="middle" x="210" y="110">4</text> <circle cx="110" cy="180" fill="white" r="32" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="110" y="186">1</text> <text fill="black" font-size="16" text-anchor="middle" x="110" y="230">7</text> <circle cx="310" cy="180" fill="white" r="32" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="310" y="186">2</text> <text fill="black" font-size="16" text-anchor="middle" x="310" y="230">6</text> </svg>

<p>Lựa chọn tối ưu là chọn task 0 làm root.</p>

<ul>
	<li>Task 1 là task lá, nên thời gian hoàn thành là <code>baseTime[1] = 7</code>.</li>
	<li>Task 2 là task lá, nên thời gian hoàn thành là <code>baseTime[2] = 6</code>.</li>
	<li>Task 0 có hai task con với thời gian hoàn thành lần lượt là 7 và 6:
	<ul>
		<li><code>earliest = 6</code>, <code>latest = 7</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[0] = (7 - 6) + 4 = 5</code></li>
		<li>Thời gian hoàn thành của task 0 là <code>latest + ownDuration = 7 + 5 = 12</code></li>
	</ul>
	</li>
</ul>

<p>Vì vậy, thời gian hoàn thành nhỏ nhất có thể đạt được trong mọi lựa chọn root là 12.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[0,2],[2,3]], baseTime = [5,8,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>
<svg height="368" viewbox="46 26 380 466" width="300" xmlns="http://www.w3.org/2000/svg"> <rect fill="white" height="466" width="380" x="46" y="26"></rect> <line stroke="black" stroke-width="2" x1="230" x2="110" y1="80" y2="260"></line> <line stroke="black" stroke-width="2" x1="230" x2="350" y1="80" y2="260"></line> <line stroke="black" stroke-width="2" x1="350" x2="350" y1="260" y2="420"></line> <circle cx="230" cy="80" fill="white" r="34" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="230" y="88">0</text> <text fill="black" font-size="16" text-anchor="middle" x="230" y="132">5</text> <circle cx="110" cy="260" fill="white" r="34" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="110" y="268">1</text> <text fill="black" font-size="16" text-anchor="middle" x="110" y="312">8</text> <circle cx="350" cy="260" fill="white" r="34" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="350" y="268">2</text> <text fill="black" font-size="16" text-anchor="middle" x="398" y="266">2</text> <circle cx="350" cy="420" fill="white" r="34" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="18" text-anchor="middle" x="350" y="428">3</text> <text fill="black" font-size="16" text-anchor="middle" x="350" y="472">1</text> </svg>

<p>Lựa chọn tối ưu là chọn task 1 làm root.</p>

<ul>
	<li>Task 3 là task lá, nên thời gian hoàn thành là <code>baseTime[3] = 1</code>.</li>
	<li>Task 2 có một task con là task 3:
	<ul>
		<li><code>earliest = latest = 1</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[2] = 0 + 2 = 2</code></li>
		<li>Thời gian hoàn thành của task 2 là <code>latest + ownDuration = 1 + 2 = 3</code></li>
	</ul>
	</li>
	<li>Task 0 có một task con là task 2:
	<ul>
		<li><code>earliest = latest = 3</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[0] = 0 + 5 = 5</code></li>
		<li>Thời gian hoàn thành của task 0 là <code>latest + ownDuration = 3 + 5 = 8</code></li>
	</ul>
	</li>
	<li>Task 1 có một task con là task 0:
	<ul>
		<li><code>earliest = latest = 8</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[1] = 0 + 8 = 8</code></li>
		<li>Thời gian hoàn thành của task 1 là <code>latest + ownDuration = 8 + 8 = 16</code></li>
	</ul>
	</li>
</ul>

<p>Vì vậy, thời gian hoàn thành nhỏ nhất có thể đạt được trong mọi lựa chọn root là 16.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length = n - 1</code></li>
	<li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li>Đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây vô hướng hợp lệ.</li>
	<li><code>baseTime.length == n</code></li>
	<li><code>1 &lt;= baseTime[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Công thức thời gian hoàn thành giống phần I, nhưng có thể chọn bất kỳ node nào làm root và cần lấy giá trị nhỏ nhất. Nếu chạy lại DFS cho từng root thì quá chậm.
>
> Dùng reroot: trước tiên tính các đại lượng của subtree khi chọn node $0$ làm root, sau đó dùng DFS lần hai để gộp thời gian hoàn thành từ phía parent vào node hiện tại, từ đó thu được đáp án khi node đó là root.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng ở việc reroot công thức của phần I.

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
