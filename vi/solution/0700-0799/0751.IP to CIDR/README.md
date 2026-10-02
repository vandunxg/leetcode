---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - String
---

<!-- problem:start -->

# [751. IP to CIDR 🔒](https://leetcode.com/problems/ip-to-cidr)

[中文文档](/solution/0700-0799/0751.IP%20to%20CIDR/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Địa chỉ IP</strong> là số nguyên không dấu 32 bit có định dạng, trong đó mỗi nhóm 8 bit được viết thành số thập phân và ký tự dấu chấm <code>&#39;.&#39;</code> ngăn cách các nhóm.</p>

<ul>
	<li>Ví dụ, số nhị phân <code>00001111 10001000 11111111 01101011</code> (đã thêm khoảng trắng để dễ đọc) khi định dạng thành địa chỉ IP sẽ là <code>&quot;15.136.255.107&quot;</code>.</li>
</ul>

<p><strong>CIDR block</strong> là định dạng dùng để biểu diễn một tập địa chỉ IP cụ thể. Nó là chuỗi gồm địa chỉ IP cơ sở, dấu gạch chéo và độ dài prefix <code>k</code>. Các địa chỉ thuộc block là những IP có <strong><code>k</code> bit đầu tiên</strong> giống với địa chỉ IP cơ sở.</p>

<ul>
	<li>Ví dụ, <code>&quot;123.45.67.89/20&quot;</code> là CIDR block có độ dài prefix <code>20</code>. Mọi địa chỉ IP có biểu diễn nhị phân khớp với <code>01111011 00101101 0100xxxx xxxxxxxx</code>, trong đó <code>x</code> có thể là <code>0</code> hoặc <code>1</code>, đều thuộc tập mà CIDR block bao phủ.</li>
</ul>

<p>Cho địa chỉ IP bắt đầu <code>ip</code> và số lượng địa chỉ IP cần bao phủ <code>n</code>. Mục tiêu là dùng <strong>ít CIDR block nhất có thể</strong> để bao phủ chính xác toàn bộ các địa chỉ trong phạm vi <strong>bao gồm hai đầu mút</strong> <code>[ip, ip + n - 1]</code>. Không được bao phủ bất kỳ địa chỉ IP nào nằm ngoài phạm vi này.</p>

<p>Trả về <em>danh sách <strong>ngắn nhất</strong> gồm các <strong>CIDR block</strong> bao phủ phạm vi địa chỉ IP. Nếu có nhiều đáp án, trả về <strong>bất kỳ đáp án nào</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ip = &quot;255.0.0.7&quot;, n = 10
<strong>Đầu ra:</strong> [&quot;255.0.0.7/32&quot;,&quot;255.0.0.8/29&quot;,&quot;255.0.0.16/32&quot;]
<strong>Giải thích:</strong>
Các địa chỉ IP cần bao phủ là:
- 255.0.0.7  -&gt; 11111111 00000000 00000000 00000111
- 255.0.0.8  -&gt; 11111111 00000000 00000000 00001000
- 255.0.0.9  -&gt; 11111111 00000000 00000000 00001001
- 255.0.0.10 -&gt; 11111111 00000000 00000000 00001010
- 255.0.0.11 -&gt; 11111111 00000000 00000000 00001011
- 255.0.0.12 -&gt; 11111111 00000000 00000000 00001100
- 255.0.0.13 -&gt; 11111111 00000000 00000000 00001101
- 255.0.0.14 -&gt; 11111111 00000000 00000000 00001110
- 255.0.0.15 -&gt; 11111111 00000000 00000000 00001111
- 255.0.0.16 -&gt; 11111111 00000000 00000000 00010000
CIDR block &quot;255.0.0.7/32&quot; bao phủ địa chỉ đầu tiên.
CIDR block &quot;255.0.0.8/29&quot; bao phủ 8 địa chỉ ở giữa (biểu diễn nhị phân là 11111111 00000000 00000000 00001xxx).
CIDR block &quot;255.0.0.16/32&quot; bao phủ địa chỉ cuối cùng.
Lưu ý, dù CIDR block &quot;255.0.0.0/28&quot; bao phủ tất cả địa chỉ này, nó cũng bao gồm các địa chỉ nằm ngoài phạm vi nên không thể dùng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ip = &quot;117.145.102.62&quot;, n = 8
<strong>Đầu ra:</strong> [&quot;117.145.102.62/31&quot;,&quot;117.145.102.64/30&quot;,&quot;117.145.102.68/31&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>7 &lt;= ip.length &lt;= 15</code></li>
	<li><code>ip</code> là địa chỉ <strong>IPv4</strong> hợp lệ có dạng <code>&quot;a.b.c.d&quot;</code>, trong đó <code>a</code>, <code>b</code>, <code>c</code> và <code>d</code> là các số nguyên trong khoảng <code>[0, 255]</code>.</li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li>Mọi địa chỉ suy ra <code>ip + x</code> (với <code>x &lt; n</code>) đều là địa chỉ IPv4 hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bao phủ $n$ địa chỉ IPv4 liên tiếp bằng số CIDR block ít nhất có thể. Các tab ngôn ngữ trong repo này đang để trống; cách làm greedy thông thường là chọn prefix dài nhất.
>
> Từ địa chỉ bắt đầu hiện tại, block lớn nhất được giới hạn bởi số bit 0 ở cuối địa chỉ và số IP còn lại. Xuất block đó, tăng địa chỉ bắt đầu và giảm $n$ tương ứng.
>
> Chuyển IP thành số nguyên; block có mask $m$ bao phủ $2^{32-m}$ địa chỉ và không vượt ra ngoài phạm vi cần bao phủ.

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
