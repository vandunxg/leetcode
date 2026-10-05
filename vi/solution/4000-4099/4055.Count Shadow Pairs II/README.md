---
comments: true
difficulty: Hard
rating: 2630
source: Weekly Contest 519 Q4
---

<!-- problem:start -->

# [4055. Count Shadow Pairs II](https://leetcode.com/problems/count-shadow-pairs-ii)

[Tài liệu tiếng Trung](/solution/4000-4099/4055.Count%20Shadow%20Pairs%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Một cặp chỉ số <code>(i, j)</code> được gọi là một <strong>cặp shadow</strong> nếu thỏa mãn tất cả các điều kiện sau:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; n</code></li>
	<li><code>nums[i] &lt; nums[j]</code></li>
	<li><strong>Không tồn tại</strong> chỉ số <code>k</code> sao cho <code>i &lt; k &lt; j</code> và <code>nums[i] &lt; nums[k] &lt; nums[j]</code>.</li>
</ul>

<p>Trả về tổng số <strong>cặp shadow</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,4,2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

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
			<td>(0, 2)</td>
			<td>3</td>
			<td>4</td>
			<td><code>nums[1] = 1</code> không nằm nghiêm ngặt giữa 3 và 4</td>
		</tr>
		<tr>
			<td>(1, 2)</td>
			<td>1</td>
			<td>4</td>
			<td>Không tồn tại chỉ số <code>k</code> nào sao cho <code>1 &lt; k &lt; 2</code></td>
		</tr>
		<tr>
			<td>(1, 3)</td>
			<td>1</td>
			<td>2</td>
			<td><code>nums[2] = 4</code> không nằm nghiêm ngặt giữa 1 và 2</td>
		</tr>
		<tr>
			<td>(2, 4)</td>
			<td>4</td>
			<td>5</td>
			<td><code>nums[3] = 2</code> không nằm nghiêm ngặt giữa 4 và 5</td>
		</tr>
		<tr>
			<td>(3, 4)</td>
			<td>2</td>
			<td>5</td>
			<td>Không tồn tại chỉ số <code>k</code> nào sao cho <code>3 &lt; k &lt; 4</code></td>
		</tr>
	</tbody>
</table>
</div>

<p>Vì vậy, đáp án là 5.</p>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,7,8,9]</span></p>

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
			<td>(0, 1)</td>
			<td>6</td>
			<td>7</td>
			<td>Không tồn tại chỉ số <code>k</code> nào sao cho <code>0 &lt; k &lt; 1</code></td>
		</tr>
		<tr>
			<td>(1, 2)</td>
			<td>7</td>
			<td>8</td>
			<td>Không tồn tại chỉ số <code>k</code> nào sao cho <code>1 &lt; k &lt; 2</code></td>
		</tr>
		<tr>
			<td>(2, 3)</td>
			<td>8</td>
			<td>9</td>
			<td>Không tồn tại chỉ số <code>k</code> nào sao cho <code>2 &lt; k &lt; 3</code></td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, đáp án là 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n == nums.length &lt;= 5 * 10<sup>4</sup></code></li>
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
