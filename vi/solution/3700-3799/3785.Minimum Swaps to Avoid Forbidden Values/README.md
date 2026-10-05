---
comments: true
difficulty: Hard
rating: 2051
source: Weekly Contest 481 Q3
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3785. Minimum Swaps to Avoid Forbidden Values](https://leetcode.com/problems/minimum-swaps-to-avoid-forbidden-values)

[中文文档](/solution/3700-3799/3785.Minimum%20Swaps%20to%20Avoid%20Forbidden%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums</code> và <code>forbidden</code>, mỗi mảng có độ dài <code>n</code>.</p>

<p>Bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý (kể cả không lần nào):</p>

<ul>
	<li>Chọn hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code>, rồi hoán đổi <code>nums[i]</code> với <code>nums[j]</code>.</li>
</ul>

<p>Hãy trả về số lần hoán đổi <strong>ít nhất</strong> cần thực hiện sao cho với mọi chỉ số <code>i</code>, giá trị của <code>nums[i]</code> <strong>không bằng</strong> <code>forbidden[i]</code>. Nếu không thể đảm bảo mọi giá trị tại chỉ số đều khác giá trị bị cấm, hãy trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], forbidden = [3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi hoán đổi tối ưu:</p>

<ul>
	<li>Chọn các chỉ số <code>i = 0</code> và <code>j = 1</code> trong <code>nums</code> rồi hoán đổi chúng, thu được <code>nums = [2, 1, 3]</code>.</li>
	<li>Sau lần hoán đổi này, với mọi chỉ số <code>i</code>, <code>nums[i]</code> đều khác <code>forbidden[i]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,6,6,5], forbidden = [4,6,5,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>
Một chuỗi hoán đổi tối ưu:

<ul>
	<li>Chọn các chỉ số <code>i = 0</code> và <code>j = 2</code> trong <code>nums</code> rồi hoán đổi chúng, thu được <code>nums = [6, 6, 4, 5]</code>.</li>
	<li>Chọn các chỉ số <code>i = 1</code> và <code>j = 3</code> trong <code>nums</code> rồi hoán đổi chúng, thu được <code>nums = [6, 5, 4, 6]</code>.</li>
	<li>Sau các lần hoán đổi này, với mọi chỉ số <code>i</code>, <code>nums[i]</code> đều khác <code>forbidden[i]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,7], forbidden = [8,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>
Không thể làm cho <code>nums[i]</code> khác <code>forbidden[i]</code> tại mọi chỉ số.</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2], forbidden = [2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không cần hoán đổi vì tại mọi chỉ số, <code>nums[i]</code> đã khác <code>forbidden[i]</code>, nên đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length == forbidden.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], forbidden[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một xung đột là chỉ số có $nums[i]=forbidden[i]$. Nếu một giá trị không có vị trí an toàn nào trong số các chỉ số này, bài toán là không thể thực hiện. Các xung đột được giải quyết bằng cách hoán đổi giữa chúng; số lần hoán đổi ít nhất được quyết định bởi số lượng xung đột (với trường hợp $0$/$1$ được xử lý riêng).

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
