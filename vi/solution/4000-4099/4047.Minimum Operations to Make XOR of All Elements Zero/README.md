---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [4047. Minimum Operations to Make XOR of All Elements Zero 🔒](https://leetcode.com/problems/minimum-operations-to-make-xor-of-all-elements-zero)

[中文文档](/solution/4000-4099/4047.Minimum%20Operations%20to%20Make%20XOR%20of%20All%20Elements%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> gồm các số nguyên <strong>dương</strong>.</p>

<p>Bạn có thể thực hiện <strong>thao tác</strong> sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Chọn hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code> sao cho <code>nums[i] != nums[j]</code>, rồi thay <code>nums[i]</code> hoặc <code>nums[j]</code> bằng <code>nums[i] ^ nums[j]</code>, trong đó <code>^</code> biểu thị phép <strong>XOR bit</strong>.</li>
</ul>

<p>Trả về số thao tác <strong>ít nhất</strong> cần thực hiện để phép <strong>XOR bit</strong> của tất cả phần tử trong <code>nums</code> bằng 0. Nếu không thể thực hiện, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,1,4,8,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi thao tác tối ưu là:</p>

<ul>
	<li>Chọn các chỉ số 0 và 1, rồi thay <code>nums[0]</code> bằng <code>8 ^ 1 = 9</code>. Mảng trở thành <code>[9, 1, 4, 8, 2]</code>.</li>
	<li>Chọn các chỉ số 2 và 3, rồi thay <code>nums[3]</code> bằng <code>4 ^ 8 = 12</code>. Mảng trở thành <code>[9, 1, 4, 12, 2]</code>.</li>
	<li>Chọn các chỉ số 0 và 4, rồi thay <code>nums[0]</code> bằng <code>9 ^ 2 = 11</code>. Mảng trở thành <code>[11, 1, 4, 12, 2]</code>.</li>
</ul>

<p>XOR của tất cả phần tử trong <code>nums</code> là <code>11 ^ 1 ^ 4 ^ 12 ^ 2 = 0</code>, nên đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>XOR của tất cả phần tử trong <code>nums</code> là <code>1 ^ 2 ^ 3 = 0</code>, nên không cần thực hiện thao tác nào.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể khiến XOR của tất cả phần tử trong <code>nums</code> bằng 0, nên đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2000</code></li>
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
