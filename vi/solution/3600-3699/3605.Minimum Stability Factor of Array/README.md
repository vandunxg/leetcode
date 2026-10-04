---
comments: true
difficulty: Hard
rating: 2409
source: Biweekly Contest 160 Q4
tags:
    - Greedy
    - Segment Tree
    - Array
    - Math
    - Binary Search
    - Number Theory
---

<!-- problem:start -->

# [3605. Minimum Stability Factor of Array](https://leetcode.com/problems/minimum-stability-factor-of-array)

[中文文档](/solution/3600-3699/3605.Minimum%20Stability%20Factor%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>maxC</code>.</p>

<p>Một <strong><span data-keyword="subarray">mảng con</span></strong> được gọi là <strong>ổn định</strong> nếu <em>ước chung lớn nhất (HCF)</em> của tất cả các phần tử trong mảng con <strong>lớn hơn hoặc bằng</strong> 2.</p>

<p><strong>Hệ số ổn định</strong> của một mảng được định nghĩa là độ dài của <strong>mảng con ổn định dài nhất</strong>.</p>

<p>Bạn có thể thay đổi <strong>nhiều nhất</strong> <code>maxC</code> phần tử của mảng thành một số nguyên bất kỳ.</p>

<p>Trả về <strong>hệ số ổn định nhỏ nhất</strong> có thể đạt được sau nhiều nhất <code>maxC</code> lần thay đổi. Nếu không còn mảng con ổn định nào, trả về 0.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
    <li><strong>Ước chung lớn nhất (HCF)</strong> của một mảng là số nguyên lớn nhất chia hết cho tất cả các phần tử trong mảng.</li>
    <li>Một <strong>mảng con</strong> có độ dài 1 là ổn định nếu phần tử duy nhất của nó lớn hơn hoặc bằng 2, vì <code>HCF([x]) = x</code>.</li>
</ul>

<div class="notranslate" style="all: initial;"> </div>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5,10], maxC = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Mảng con ổn định <code>[5, 10]</code> có <code>HCF = 5</code>, nên có hệ số ổn định bằng 2.</li>
    <li>Vì <code>maxC = 1</code>, một chiến lược tối ưu là thay đổi <code>nums[1]</code> thành <code>7</code>, thu được <code>nums = [3, 7, 10]</code>.</li>
    <li>Lúc này, không có mảng con nào có độ dài lớn hơn 1 có <code>HCF &gt;= 2</code>. Do đó, hệ số ổn định nhỏ nhất có thể đạt được là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,6,8], maxC = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Mảng con <code>[2, 6, 8]</code> có <code>HCF = 2</code>, nên có hệ số ổn định bằng 3.</li>
    <li>Vì <code>maxC = 2</code>, một chiến lược tối ưu là thay đổi <code>nums[1]</code> thành 3 và <code>nums[2]</code> thành 5, thu được <code>nums = [2, 3, 5]</code>.</li>
    <li>Lúc này, không có mảng con nào có độ dài lớn hơn 1 có <code>HCF &gt;= 2</code>. Do đó, hệ số ổn định nhỏ nhất có thể đạt được là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,9,6], maxC = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các mảng con ổn định là:
    <ul>
        <li><code>[2, 4]</code> có <code>HCF = 2</code> và hệ số ổn định bằng 2.</li>
        <li><code>[9, 6]</code> có <code>HCF = 3</code> và hệ số ổn định bằng 2.</li>
    </ul>
    </li>
    <li>Vì <code>maxC = 1</code>, không thể giảm hệ số ổn định bằng 2 do có hai mảng con ổn định riêng biệt. Do đó, hệ số ổn định nhỏ nhất có thể đạt được là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>0 &lt;= maxC &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hệ số ổn định là độ dài của mảng con dài nhất có $\gcd$ ít nhất bằng $2$. Sau nhiều nhất $\textit{maxC}$ lần thay đổi, ta muốn giảm độ dài đó xuống nhỏ nhất. Việc liệt kê các lần thay đổi là không thể thực hiện được.
>
> Nếu có thể buộc một độ dài $L$ sao cho mọi cửa sổ có độ dài $L+1$ đều không ổn định bằng nhiều nhất $\textit{maxC}$ lần thay đổi, thì mọi mục tiêu nhỏ hơn đều khả thi, do đó có thể dùng tìm kiếm nhị phân để tìm đáp án.
>
> Có thể tính $\gcd$ trên một đoạn trong $O(1)$ bằng sparse table. Với một $\textit{mid}$ được chọn, ta duyệt các cửa sổ có độ dài $\textit{mid}+1$ và thay đổi một vị trí trong mỗi cửa sổ có $\gcd$ ít nhất bằng $2$. Đặt vị trí thay đổi càng xa về bên phải càng tốt để bao phủ các cửa sổ chồng lấn về sau, rồi so sánh số lần thay đổi với $\textit{maxC}$.

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
