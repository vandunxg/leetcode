---
comments: true
difficulty: Medium
rating: 1715
source: Biweekly Contest 163 Q2
---

<!-- problem:start -->

# [3649. Number of Perfect Pairs](https://leetcode.com/problems/number-of-perfect-pairs)

[Tài liệu tiếng Trung](/solution/3600-3699/3649.Number%20of%20Perfect%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một cặp chỉ số <code>(i, j)</code> được gọi là <strong>hoàn hảo</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>i &lt; j</code></li>
	<li>Đặt <code>a = nums[i]</code>, <code>b = nums[j]</code>. Khi đó:
	<ul>
		<li><code>min(|a - b|, |a + b|) &lt;= min(|a|, |b|)</code></li>
		<li><code>max(|a - b|, |a + b|) &gt;= max(|a|, |b|)</code></li>
	</ul>
	</li>
</ul>

<p>Trả về số lượng cặp hoàn hảo <strong>phân biệt</strong>.</p>

<p><strong>Lưu ý:</strong> Giá trị tuyệt đối <code>|x|</code> là giá trị <strong>không âm</strong> của <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 2 cặp hoàn hảo:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>(i, j)</code></th>
			<th style="border: 1px solid black;"><code>(a, b)</code></th>
			<th style="border: 1px solid black;"><code>min(|a &minus; b|, |a + b|)</code></th>
			<th style="border: 1px solid black;"><code>min(|a|, |b|)</code></th>
			<th style="border: 1px solid black;"><code>max(|a &minus; b|, |a + b|)</code></th>
			<th style="border: 1px solid black;"><code>max(|a|, |b|)</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">(1, 2)</td>
			<td style="border: 1px solid black;">(1, 2)</td>
			<td style="border: 1px solid black;"><code>min(|1 &minus; 2|, |1 + 2|) = 1</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>max(|1 &minus; 2|, |1 + 2|) = 3</code></td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">(2, 3)</td>
			<td style="border: 1px solid black;">(2, 3)</td>
			<td style="border: 1px solid black;"><code>min(|2 &minus; 3|, |2 + 3|) = 1</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>max(|2 &minus; 3|, |2 + 3|) = 5</code></td>
			<td style="border: 1px solid black;">3</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-3,2,-1,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 4 cặp hoàn hảo:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>(i, j)</code></th>
			<th style="border: 1px solid black;"><code>(a, b)</code></th>
			<th style="border: 1px solid black;"><code>min(|a &minus; b|, |a + b|)</code></th>
			<th style="border: 1px solid black;"><code>min(|a|, |b|)</code></th>
			<th style="border: 1px solid black;"><code>max(|a &minus; b|, |a + b|)</code></th>
			<th style="border: 1px solid black;"><code>max(|a|, |b|)</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">(0, 1)</td>
			<td style="border: 1px solid black;">(-3, 2)</td>
			<td style="border: 1px solid black;"><code>min(|-3 - 2|, |-3 + 2|) = 1</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>max(|-3 - 2|, |-3 + 2|) = 5</code></td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">(0, 3)</td>
			<td style="border: 1px solid black;">(-3, 4)</td>
			<td style="border: 1px solid black;"><code>min(|-3 - 4|, |-3 + 4|) = 1</code></td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;"><code>max(|-3 - 4|, |-3 + 4|) = 7</code></td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">(1, 2)</td>
			<td style="border: 1px solid black;">(2, -1)</td>
			<td style="border: 1px solid black;"><code>min(|2 - (-1)|, |2 + (-1)|) = 1</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>max(|2 - (-1)|, |2 + (-1)|) = 3</code></td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">(1, 3)</td>
			<td style="border: 1px solid black;">(2, 4)</td>
			<td style="border: 1px solid black;"><code>min(|2 - 4|, |2 + 4|) = 2</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>max(|2 - 4|, |2 + 4|) = 6</code></td>
			<td style="border: 1px solid black;">4</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,10,100,1000]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cặp hoàn hảo nào. Vì vậy, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp hoàn hảo thường có thể quy về một bất đẳng thức trên các giá trị tuyệt đối. Với $n$ lớn, cần sắp xếp và dùng hai con trỏ thay vì xét mọi cặp.
>
> Sắp xếp các giá trị tuyệt đối. Với mỗi đầu phải $r$, tăng $l$ cho đến khi điều kiện của cặp được thỏa mãn; mọi chỉ số trong $[l,r)$ đều ghép được với $r$.
>
> Cộng độ dài các cửa sổ. Cách này tránh vòng lặp kép trên mọi cặp.

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
