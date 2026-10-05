---
comments: true
difficulty: Medium
rating: 1820
source: Weekly Contest 519 Q3
---

<!-- problem:start -->

# [4054. Count Shadow Pairs I](https://leetcode.com/problems/count-shadow-pairs-i)

[中文文档](/solution/4000-4099/4054.Count%20Shadow%20Pairs%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Một cặp chỉ số <code>(i, j)</code> được gọi là một <strong>cặp shadow</strong> nếu thỏa mãn tất cả các điều kiện sau:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; n</code></li>
	<li><code>nums[i] &lt; nums[j]</code></li>
	<li><strong>Không tồn tại</strong> chỉ số <code>k</code> sao cho <code>i &lt; k &lt; j</code> và <code>nums[k] &lt; nums[i] &lt; nums[j]</code>.</li>
</ul>

<p>Trả về tổng số <strong>cặp shadow</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,4,1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border-collapse: collapse; text-align: center; width: 70%;">
	<thead>
		<tr>
			<th style="padding: 8px;"><code>(i, j)</code></th>
			<th style="padding: 8px;"><code>nums[i]</code></th>
			<th style="padding: 8px;"><code>nums[j]</code></th>
			<th style="padding: 8px;">Cặp shadow</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>(1, 2)</td>
			<td>1</td>
			<td>4</td>
			<td>Không tồn tại chỉ số <code>k</code> sao cho <code>1 &lt; k &lt; 2</code></td>
		</tr>
		<tr>
			<td>(1, 4)</td>
			<td>1</td>
			<td>5</td>
			<td><code>nums[2] = 4</code> và <code>nums[3] = 1</code> không nhỏ hơn 1</td>
		</tr>
		<tr>
			<td>(3, 4)</td>
			<td>1</td>
			<td>5</td>
			<td>Không tồn tại chỉ số <code>k</code> sao cho <code>3 &lt; k &lt; 4</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,7,6,6,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border-collapse: collapse; text-align: center; width: 70%;">
	<thead>
		<tr>
			<th style="padding: 8px;"><code>(i, j)</code></th>
			<th style="padding: 8px;"><code>nums[i]</code></th>
			<th style="padding: 8px;"><code>nums[j]</code></th>
			<th style="padding: 8px;">Cặp shadow</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>(0, 1)</td>
			<td>6</td>
			<td>7</td>
			<td>Không tồn tại chỉ số <code>k</code> sao cho <code>0 &lt; k &lt; 1</code></td>
		</tr>
		<tr>
			<td>(0, 4)</td>
			<td>6</td>
			<td>7</td>
			<td><code>nums[1] = 7</code>, <code>nums[2] = 6</code> và <code>nums[3] = 6</code> không nhỏ hơn 6</td>
		</tr>
		<tr>
			<td>(2, 4)</td>
			<td>6</td>
			<td>7</td>
			<td><code>nums[3] = 6</code> không nhỏ hơn 6</td>
		</tr>
		<tr>
			<td>(3, 4)</td>
			<td>6</td>
			<td>7</td>
			<td>Không tồn tại chỉ số <code>k</code> sao cho <code>3 &lt; k &lt; 4</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border-collapse: collapse; text-align: center; width: 70%;">
	<thead>
		<tr>
			<th style="padding: 8px;"><code>(i, j)</code></th>
			<th style="padding: 8px;"><code>nums[i]</code></th>
			<th style="padding: 8px;"><code>nums[j]</code></th>
			<th style="padding: 8px;">Cặp shadow</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>(0, 1)</td>
			<td>1</td>
			<td>2</td>
			<td>Không tồn tại chỉ số <code>k</code> sao cho <code>0 &lt; k &lt; 1</code></td>
		</tr>
		<tr>
			<td>(0, 2)</td>
			<td>1</td>
			<td>3</td>
			<td><code>nums[1] = 2</code> không nhỏ hơn 1</td>
		</tr>
		<tr>
			<td>(0, 3)</td>
			<td>1</td>
			<td>4</td>
			<td><code>nums[1] = 2</code> và <code>nums[2] = 3</code> không nhỏ hơn 1</td>
		</tr>
		<tr>
			<td>(1, 2)</td>
			<td>2</td>
			<td>3</td>
			<td>Không tồn tại chỉ số <code>k</code> sao cho <code>1 &lt; k &lt; 2</code></td>
		</tr>
		<tr>
			<td>(1, 3)</td>
			<td>2</td>
			<td>4</td>
			<td><code>nums[2] = 3</code> không nhỏ hơn 2</td>
		</tr>
		<tr>
			<td>(2, 3)</td>
			<td>3</td>
			<td>4</td>
			<td>Không tồn tại chỉ số <code>k</code> sao cho <code>2 &lt; k &lt; 3</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy đáp án là 6.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

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
