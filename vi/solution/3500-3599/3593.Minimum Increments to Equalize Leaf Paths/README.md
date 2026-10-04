---
comments: true
difficulty: Medium
rating: 1959
source: Weekly Contest 455 Q3
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3593. Minimum Increments to Equalize Leaf Paths](https://leetcode.com/problems/minimum-increments-to-equalize-leaf-paths)

[中文文档](/solution/3500-3599/3593.Minimum%20Increments%20to%20Equalize%20Leaf%20Paths/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một cây vô hướng có gốc là nút 0, gồm <code>n</code> nút được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh nối nút <code>u<sub>i</sub></code> với nút <code>v<sub>i</sub></code>.</p>

<p>Mỗi nút <code>i</code> có một chi phí tương ứng là <code>cost[i]</code>, biểu thị chi phí khi đi qua nút đó.</p>

<p><strong>Điểm số</strong> của một đường đi được định nghĩa là tổng chi phí của tất cả các nút trên đường đi.</p>

<p>Mục tiêu là làm cho điểm số của tất cả các đường đi <strong>từ gốc đến lá</strong> <strong>bằng nhau</strong> bằng cách <strong>tăng</strong> chi phí của một số nút bất kỳ lên <strong>một lượng không âm</strong> bất kỳ.</p>

<p>Trả về <strong>số nút nhỏ nhất</strong> cần tăng chi phí để làm cho điểm số của tất cả các đường đi từ gốc đến lá bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[0,2]], cost = [2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3593.Minimum%20Increments%20to%20Equalize%20Leaf%20Paths/images/screenshot-2025-05-28-at-134018.png" style="width: 180px; height: 145px;" /></p>

<p>Có hai đường đi từ gốc đến lá:</p>

<ul>
	<li>Đường đi <code>0 &rarr; 1</code> có điểm số là <code>2 + 1 = 3</code>.</li>
	<li>Đường đi <code>0 &rarr; 2</code> có điểm số là <code>2 + 3 = 5</code>.</li>
</ul>

<p>Để làm cho tất cả điểm số của các đường đi từ gốc đến lá bằng 5, tăng chi phí của nút 1 thêm 2.<br />
Chỉ có một nút được tăng chi phí, nên kết quả là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], cost = [5,1,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3593.Minimum%20Increments%20to%20Equalize%20Leaf%20Paths/images/screenshot-2025-05-28-at-134249.png" style="width: 230px; height: 75px;" /></p>

<p>Chỉ có <b> </b>một đường đi từ gốc đến lá:</p>

<ul>
	<li>
	<p>Đường đi <code>0 &rarr; 1 &rarr; 2</code> có điểm số là <code>5 + 1 + 4 = 10</code>.</p>
	</li>
</ul>

<p>Vì chỉ tồn tại một đường đi từ gốc đến lá, điểm số của tất cả các đường đi hiển nhiên bằng nhau, nên kết quả là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[0,4],[0,1],[1,2],[1,3]], cost = [3,4,1,1,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3593.Minimum%20Increments%20to%20Equalize%20Leaf%20Paths/images/screenshot-2025-05-28-at-135704.png" style="width: 267px; height: 250px;" /></p>

<p>Có ba đường đi từ gốc đến lá:</p>

<ul>
	<li>Đường đi <code>0 &rarr; 4</code> có điểm số là <code>3 + 7 = 10</code>.</li>
	<li>Đường đi <code>0 &rarr; 1 &rarr; 2</code> có điểm số là <code>3 + 4 + 1 = 8</code>.</li>
	<li>Đường đi <code>0 &rarr; 1 &rarr; 3</code> có điểm số là <code>3 + 4 + 1 = 8</code>.</li>
</ul>

<p>Để làm cho tất cả điểm số của các đường đi từ gốc đến lá bằng 10, tăng chi phí của nút 1 thêm 2. Do đó, kết quả là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>cost.length == n</code></li>
	<li><code>1 &lt;= cost[i] &lt;= 10<sup>9</sup></code></li>
	<li>Đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ có thể tăng chi phí của các nút để tổng điểm số trên mọi đường đi từ gốc đến lá bằng nhau, đồng thời số nút được tăng là ít nhất có thể. Tổng chung này không nhỏ hơn điểm số của đường đi dài nhất hiện tại, và việc tăng chi phí của một nút tổ tiên sẽ ảnh hưởng đến toàn bộ cây con.
>
> Tính từ dưới lên điểm số lớn nhất trên một đường đi từ gốc đến lá trong mỗi cây con. Với mỗi nút con có điểm số đường đi nhỏ hơn giá trị lớn nhất này, ta chỉ cần tăng chi phí một lần tại nút con đó thay vì tăng tại nhiều nút lá. Đếm số nút con như vậy.

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
