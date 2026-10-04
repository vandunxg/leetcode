---
comments: true
difficulty: Hard
rating: 2359
source: Biweekly Contest 158 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [3575. Maximum Good Subtree Score](https://leetcode.com/problems/maximum-good-subtree-score)

[中文文档](/solution/3500-3599/3575.Maximum%20Good%20Subtree%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một cây vô hướng có gốc tại node 0 gồm <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>. Mỗi node <code>i</code> có một giá trị nguyên <code>vals[i]</code>, và node cha của nó được cho bởi <code>par[i]</code>.</p>

<p>Một <strong>tập con</strong> các node trong <strong>cây con</strong> của một node được gọi là <strong>tốt</strong> nếu mỗi chữ số từ 0 đến 9 xuất hiện <strong>không quá</strong> một lần trong biểu diễn thập phân của các giá trị thuộc những node được chọn.</p>

<p><strong>Điểm số</strong> của một tập con tốt là tổng giá trị của các node trong tập đó.</p>

<p>Định nghĩa một mảng <code>maxScore</code> có độ dài <code>n</code>, trong đó <code>maxScore[u]</code> biểu thị tổng <strong>lớn nhất</strong> có thể của các giá trị trong một tập con tốt thuộc cây con gốc tại node <code>u</code>, bao gồm chính <code>u</code> và tất cả hậu duệ của nó.</p>

<p>Trả về tổng tất cả các giá trị trong <code>maxScore</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">vals = [2,3], par = [-1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3575.Maximum%20Good%20Subtree%20Score/images/screenshot-2025-04-29-at-150754.png" style="height: 84px; width: 180px;" /></p>

<ul>
	<li>Cây con gốc tại node 0 gồm các node <code>{0, 1}</code>. Tập con <code>{2, 3}</code> là<i> </i>tốt vì các chữ số 2 và 3 chỉ xuất hiện một lần. Điểm số của tập con này là <code>2 + 3 = 5</code>.</li>
	<li>Cây con gốc tại node 1 chỉ gồm node <code>{1}</code>. Tập con <code>{3}</code> là<i> </i>tốt. Điểm số của tập con này là 3.</li>
	<li>Mảng <code>maxScore</code> là <code>[5, 3]</code>, và tổng tất cả các giá trị trong <code>maxScore</code> là <code>5 + 3 = 8</code>. Do đó, đáp án là 8.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">vals = [1,5,2], par = [-1,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3575.Maximum%20Good%20Subtree%20Score/images/screenshot-2025-04-29-at-151408.png" style="width: 205px; height: 140px;" /></strong></p>

<ul>
	<li>Cây con gốc tại node 0 gồm các node <code>{0, 1, 2}</code>. Tập con <code>{1, 5, 2}</code> là<i> </i>tốt vì các chữ số 1, 5 và 2 chỉ xuất hiện một lần. Điểm số của tập con này là <code>1 + 5 + 2 = 8</code>.</li>
	<li>Cây con gốc tại node 1 chỉ gồm node <code>{1}</code>. Tập con <code>{5}</code> là<i> </i>tốt. Điểm số của tập con này là 5.</li>
	<li>Cây con gốc tại node 2 chỉ gồm node <code>{2}</code>. Tập con <code>{2}</code> là<i> </i>tốt. Điểm số của tập con này là 2.</li>
	<li>Mảng <code>maxScore</code> là <code>[8, 5, 2]</code>, và tổng tất cả các giá trị trong <code>maxScore</code> là <code>8 + 5 + 2 = 15</code>. Do đó, đáp án là 15.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">vals = [34,1,2], par = [-1,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">42</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3575.Maximum%20Good%20Subtree%20Score/images/screenshot-2025-04-29-at-151747.png" style="height: 80px; width: 256px;" /></p>

<ul>
	<li>Cây con gốc tại node 0 gồm các node <code>{0, 1, 2}</code>. Tập con <code>{34, 1, 2}</code> là<i> </i>tốt vì các chữ số 3, 4, 1 và 2 chỉ xuất hiện một lần. Điểm số của tập con này là <code>34 + 1 + 2 = 37</code>.</li>
	<li>Cây con gốc tại node 1 gồm các node <code>{1, 2}</code>. Tập con <code>{1, 2}</code> là<i> </i>tốt vì các chữ số 1 và 2 chỉ xuất hiện một lần. Điểm số của tập con này là <code>1 + 2 = 3</code>.</li>
	<li>Cây con gốc tại node 2 chỉ gồm node <code>{2}</code>. Tập con <code>{2}</code> là<i> </i>tốt. Điểm số của tập con này là 2.</li>
	<li>Mảng <code>maxScore</code> là <code>[37, 3, 2]</code>, và tổng tất cả các giá trị trong <code>maxScore</code> là <code>37 + 3 + 2 = 42</code>. Do đó, đáp án là 42.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">vals = [3,22,5], par = [-1,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">18</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Cây con gốc tại node 0 gồm các node <code>{0, 1, 2}</code>. Tập con <code>{3, 22, 5}</code> là<i> </i>không tốt vì chữ số 2 xuất hiện hai lần. Do đó, tập con <code>{3, 5}</code> là hợp lệ. Điểm số của tập con này là <code>3 + 5 = 8</code>.</li>
	<li>Cây con gốc tại node 1 gồm các node <code>{1, 2}</code>. Tập con <code>{22, 5}</code> là<i> </i>không tốt vì chữ số 2 xuất hiện hai lần. Do đó, tập con <code>{5}</code> là hợp lệ. Điểm số của tập con này là 5.</li>
	<li>Cây con gốc tại node 2 gồm <code>{2}</code>. Tập con <code>{5}</code> là<i> </i>tốt. Điểm số của tập con này là 5.</li>
	<li>Mảng <code>maxScore</code> là <code>[8, 5, 5]</code>, và tổng tất cả các giá trị trong <code>maxScore</code> là <code>8 + 5 + 5 = 18</code>. Do đó, đáp án là 18.</li>
</ul>

<ul>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == vals.length &lt;= 500</code></li>
	<li><code>1 &lt;= vals[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>par.length == n</code></li>
	<li><code>par[0] == -1</code></li>
	<li><code>0 &lt;= par[i] &lt; n</code> với <code>i</code> trong <code>[1, n - 1]</code></li>
	<li>Đầu vào được tạo sao cho mảng cha <code>par</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ số thập phân $0$– $9$ của một tập con tốt xuất hiện không quá một lần, nên ta có thể biểu diễn tập con bằng một mask $10$-bit. Với $n \le 500$, ta gộp từng cây con: hợp nhất điểm số của các mask đôi một không giao nhau từ các node con với giá trị riêng của node.
>
> Các cây con đóng góp độc lập vào $\textit{maxScore}[u]$; đáp án là tổng theo mọi $u$. Nếu các chữ số trong giá trị của một node bị trùng, không thể chọn riêng node đó.

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
