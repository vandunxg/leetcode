---
comments: true
difficulty: Hard
rating: 2012
source: Biweekly Contest 138 Q4
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3273. Minimum Amount of Damage Dealt to Bob](https://leetcode.com/problems/minimum-amount-of-damage-dealt-to-bob)

[中文文档](/solution/3200-3299/3273.Minimum%20Amount%20of%20Damage%20Dealt%20to%20Bob/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>power</code> và hai mảng số nguyên <code>damage</code> và <code>health</code>, cả hai đều có độ dài <code>n</code>.</p>

<p>Bob có <code>n</code> kẻ địch, trong đó kẻ địch <code>i</code> sẽ gây cho Bob <code>damage[i]</code> <strong>điểm</strong> sát thương mỗi giây khi chúng còn <em>sống</em> (tức là <code>health[i] &gt; 0</code>).</p>

<p>Mỗi giây, <strong>sau khi</strong> những kẻ địch gây sát thương cho Bob, anh ấy chọn <strong>một</strong> kẻ địch vẫn còn <em>sống</em> và gây cho kẻ địch đó <code>power</code> điểm sát thương.</p>

<p>Hãy xác định <strong>nhỏ nhất</strong> tổng số điểm sát thương gây ra cho Bob trước khi <strong>tất cả</strong> <code>n</code> kẻ địch đều <em>chết</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">power = 4, damage = [1,2,3,4], health = [4,5,6,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">39</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tấn công kẻ địch 3 trong hai giây đầu tiên, sau đó kẻ địch 3 sẽ bị hạ, số điểm sát thương gây ra cho Bob là <code>10 + 10 = 20</code> điểm.</li>
	<li>Tấn công kẻ địch 2 trong hai giây tiếp theo, sau đó kẻ địch 2 sẽ bị hạ, số điểm sát thương gây ra cho Bob là <code>6 + 6 = 12</code> điểm.</li>
	<li>Tấn công kẻ địch 0 trong giây tiếp theo, sau đó kẻ địch 0 sẽ bị hạ, số điểm sát thương gây ra cho Bob là <code>3</code> điểm.</li>
	<li>Tấn công kẻ địch 1 trong hai giây tiếp theo, sau đó kẻ địch 1 sẽ bị hạ, số điểm sát thương gây ra cho Bob là <code>2 + 2 = 4</code> điểm.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">power = 1, damage = [1,1,1,1], health = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tấn công kẻ địch 0 trong giây đầu tiên, sau đó kẻ địch 0 sẽ bị hạ, số điểm sát thương gây ra cho Bob là <code>4</code> điểm.</li>
	<li>Tấn công kẻ địch 1 trong hai giây tiếp theo, sau đó kẻ địch 1 sẽ bị hạ, số điểm sát thương gây ra cho Bob là <code>3 + 3 = 6</code> điểm.</li>
	<li>Tấn công kẻ địch 2 trong ba giây tiếp theo, sau đó kẻ địch 2 sẽ bị hạ, số điểm sát thương gây ra cho Bob là <code>2 + 2 + 2 = 6</code> điểm.</li>
	<li>Tấn công kẻ địch 3 trong bốn giây tiếp theo, sau đó kẻ địch 3 sẽ bị hạ, số điểm sát thương gây ra cho Bob là <code>1 + 1 + 1 + 1 = 4</code> điểm.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">power = 8, damage = [40], health = [59]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">320</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= power &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= n == damage.length == health.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= damage[i], health[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giây, tất cả kẻ địch còn sống gây sát thương, sau đó Bob đánh một kẻ địch với $power$. Vì $n\le 10^5$, ta không thể thử tất cả thứ tự hạ kẻ địch. Kẻ địch $i$ sẽ chết sau $t_i=\lceil health_i/power\rceil$ lần bị đánh; tổng sát thương là tổng của $damage_j$ nhân với số giây kẻ địch đó còn sống.
>
> Đổi chỗ hai kẻ địch liền kề $i,j$ cho ta so sánh $damage_i\cdot t_j$ với $damage_j\cdot t_i$ để xác định kẻ nào nên bị hạ trước. Hãy sắp xếp theo $damage/t$ giảm dần, sau đó tính tổng tiền tố của tốc độ sát thương còn lại. Hiện chưa có phần cài đặt trong cây mã; đây là phần lập luận cho comparator sort.

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
