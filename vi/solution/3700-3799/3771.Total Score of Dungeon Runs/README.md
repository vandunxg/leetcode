---
comments: true
difficulty: Medium
rating: 1981
source: Weekly Contest 479 Q3
tags:
    - Array
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [3771. Total Score of Dungeon Runs](https://leetcode.com/problems/total-score-of-dungeon-runs)

[中文文档](/solution/3700-3799/3771.Total%20Score%20of%20Dungeon%20Runs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>dương</strong> <code>hp</code> và hai mảng số nguyên <strong>dương</strong>, <strong>đánh số từ 1</strong> là <code>damage</code> và <code>requirement</code>.</p>

<p>Có một dungeon gồm <code>n</code> phòng bẫy được đánh số từ 1 đến <code>n</code>. Khi vào phòng <code>i</code>, số điểm sức khỏe của bạn giảm đi <code>damage[i]</code>. Sau khi giảm, nếu số điểm sức khỏe còn lại <strong>ít nhất</strong> là <code>requirement[i]</code>, bạn nhận được <strong>1 điểm</strong> cho phòng đó.</p>

<p>Gọi <code>score(j)</code> là số <strong>điểm</strong> bạn nhận được nếu bắt đầu với <code>hp</code> điểm sức khỏe và lần lượt đi qua các phòng <code>j</code>, <code>j + 1</code>, ..., <code>n</code>.</p>

<p>Trả về số nguyên <code>score(1) + score(2) + ... + score(n)</code>, là tổng điểm khi xét tất cả các phòng bắt đầu.</p>

<p><strong>Lưu ý</strong>: Bạn không thể bỏ qua phòng nào. Bạn vẫn có thể kết thúc hành trình ngay cả khi số điểm sức khỏe trở thành số không dương.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">hp = 11, damage = [3,6,7], requirement = [4,2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>score(1) = 2</code>, <code>score(2) = 1</code>, <code>score(3) = 0</code>. Tổng điểm là <code>2 + 1 + 0 = 3</code>.</p>

<p>Ví dụ, <code>score(1) = 2</code> vì bạn nhận được 2 điểm nếu bắt đầu từ phòng 1.</p>

<ul>
	<li>Ban đầu bạn có 11 điểm sức khỏe.</li>
	<li>Đi vào phòng 1. Số điểm sức khỏe lúc này là <code>11 - 3 = 8</code>. Bạn nhận được 1 điểm vì <code>8 &gt;= 4</code>.</li>
	<li>Đi vào phòng 2. Số điểm sức khỏe lúc này là <code>8 - 6 = 2</code>. Bạn nhận được 1 điểm vì <code>2 &gt;= 2</code>.</li>
	<li>Đi vào phòng 3. Số điểm sức khỏe lúc này là <code>2 - 7 = -5</code>. Bạn không nhận được điểm nào vì <code>-5 &lt; 5</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">hp = 2, damage = [10000,1], requirement = [1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>score(1) = 0</code>, <code>score(2) = 1</code>. Tổng điểm là <code>0 + 1 = 1</code>.</p>

<p><code>score(1) = 0</code> vì bạn không nhận được điểm nào nếu bắt đầu từ phòng 1.</p>

<ul>
	<li>Ban đầu bạn có 2 điểm sức khỏe.</li>
	<li>Đi vào phòng 1. Số điểm sức khỏe lúc này là <code>2 - 10000 = -9998</code>. Bạn không nhận được điểm nào vì <code>-9998 &lt; 1</code>.</li>
	<li>Đi vào phòng 2. Số điểm sức khỏe lúc này là <code>-9998 - 1 = -9999</code>. Bạn không nhận được điểm nào vì <code>-9999 &lt; 1</code>.</li>
</ul>

<p><code>score(2) = 1</code> vì bạn nhận được 1 điểm nếu bắt đầu từ phòng 2.</p>

<ul>
	<li>Ban đầu bạn có 2 điểm sức khỏe.</li>
	<li>Đi vào phòng 2. Số điểm sức khỏe lúc này là <code>2 - 1 = 1</code>. Bạn nhận được 1 điểm vì <code>1 &gt;= 1</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= hp &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= n == damage.length == requirement.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= damage[i], requirement[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $score(j)$ là tổng điểm nhận được từ phòng $j$ đến phòng $n$, và ta cần tính tổng $score$ trên mọi điểm bắt đầu. HP giảm dần theo hành trình, và một phòng cho điểm khi HP còn lại ít nhất là $requirement[i]$. Duyệt từ phải sang trái, ta duy trì ngưỡng HP cần có tại mỗi phòng và đếm xem có bao nhiêu điểm bắt đầu vẫn nhận được điểm ở phòng đó.

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
