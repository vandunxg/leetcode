---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Trie
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3632. Subarrays with XOR at Least K 🔒](https://leetcode.com/problems/subarrays-with-xor-at-least-k)

[中文文档](/solution/3600-3699/3632.Subarrays%20with%20XOR%20at%20Least%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên dương <code data-end="114" data-start="109">nums</code> có độ dài <code data-end="128" data-start="125">n</code> và một số nguyên không âm <code data-end="159" data-start="156">k</code>.</p>

<p>Hãy trả về số lượng <strong><span data-keyword="subarray">mảng con</span> liên tiếp</strong> có XOR bitwise của tất cả các phần tử <strong>lớn hơn</strong> hoặc <strong>bằng</strong> <code data-end="268" data-start="265">k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ với <code>XOR &gt;= 2</code> là <code>[3]</code> tại chỉ số 0, <code>[3, 1]</code> tại các chỉ số 0 - 1, <code>[3, 1, 2, 3]</code> tại các chỉ số 0 - 3, <code>[1, 2]</code> tại các chỉ số 1 - 2, <code>[2]</code> tại chỉ số 2 và <code>[3]</code> tại chỉ số 3; tổng cộng có 6 mảng con.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,0,0], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi mảng con liên tiếp đều cho <code>XOR = 0</code>, thỏa mãn <code>k = 0</code>. Có tổng cộng 6 mảng con như vậy.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="49" data-start="21"><code data-end="47" data-start="21">1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li data-end="76" data-start="52"><code data-end="74" data-start="52">0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li data-end="97" data-start="79"><code data-end="95" data-start="79">0 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> XOR của một mảng con là hiệu của các XOR tiền tố. Duyệt mọi điểm bắt đầu trái cho mỗi điểm kết thúc phải quá chậm. Ta cần đếm số tiền tố $p$ thỏa mãn $p\oplus \textit{pre}[r]\ge k$.
>
> Xét từng bit, $x\oplus y\ge k$ được quyết định trên một binary trie: một số nhánh được đếm toàn bộ, các nhánh khác được đi xuống, tùy theo các bit của $k$.
>
> Chèn các XOR tiền tố từ trái sang phải. Trước mỗi lần chèn, truy vấn 01-trie để đếm số tiền tố đã lưu có XOR với giá trị hiện tại lớn hơn hoặc bằng $k$. Mỗi truy vấn có độ phức tạp $O(\log A)$.

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
