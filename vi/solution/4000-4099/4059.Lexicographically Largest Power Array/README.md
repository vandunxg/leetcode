---
comments: true
difficulty: Hard
rating: 2321
source: Weekly Contest 520 Q4
---

<!-- problem:start -->

# [4059. Lexicographically Largest Power Array](https://leetcode.com/problems/lexicographically-largest-power-array)

[Tài liệu tiếng Trung](/solution/4000-4099/4059.Lexicographically%20Largest%20Power%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>. Bạn có thể sắp xếp lại các phần tử để tạo thành bất kỳ <span data-keyword="permutation-array">hoán vị</span> <code>perm</code> nào.</p>

<p>Định nghĩa một mảng <code>power</code> có độ dài 15. Với mỗi <code>0 &lt;= i &lt; 15</code>, <code>power[i]</code> là số nguyên <code>j</code> lớn nhất, với <code>0 &lt;= j &lt;= n</code>, sao cho <code>j</code> phần tử đầu tiên của <code>perm</code> đều có bit thứ <code>(14 - i)<sup>th</sup></code> được <span data-keyword="set-bit">bật</span>.</p>

<p>Các vị trí bit được đánh số từ phải sang trái, bắt đầu từ bit thứ <code>0<sup>th</sup></code>.</p>

<p>Trả về mảng <code>power</code> <span data-keyword="lexicographically-larger-array">lớn nhất theo thứ tự từ điển</span> có thể có.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0,0,0,0,0,0,0,0,0,0,0,2,1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code>perm = [7, 5]</code>.</p>

<ul>
	<li>Cả hai phần tử đều bật bit 2, nên <code>power[12] = 2</code>.</li>
	<li>Phần tử đầu tiên bật bit 1, nhưng phần tử thứ hai thì không, nên <code>power[13] = 1</code>.</li>
	<li>Cả hai phần tử đều bật bit 0, nên <code>power[14] = 2</code>.</li>
</ul>

<p>Tất cả các bit cao hơn đều không được bật trong phần tử đầu tiên, nên các phần tử còn lại đều bằng 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0,0,0,0,0,0,0,0,0,0,0,1,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code>perm = [7, 3, 1]</code>.</p>

<ul>
	<li>Phần tử đầu tiên bật bit 2, nhưng phần tử thứ hai thì không, nên <code>power[12] = 1</code>.</li>
	<li>Hai phần tử đầu tiên đều bật bit 1, nhưng phần tử thứ ba thì không, nên <code>power[13] = 2</code>.</li>
	<li>Cả ba phần tử đều bật bit 0, nên <code>power[14] = 3</code>.</li>
</ul>

<p>Tất cả các bit cao hơn đều không được bật trong phần tử đầu tiên, nên các phần tử còn lại đều bằng 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; 2<sup>15</sup></code></li>
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
