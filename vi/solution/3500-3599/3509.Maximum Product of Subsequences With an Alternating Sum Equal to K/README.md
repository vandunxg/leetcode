---
comments: true
difficulty: Hard
rating: 2702
source: Weekly Contest 444 Q3
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [3509. Maximum Product of Subsequences With an Alternating Sum Equal to K](https://leetcode.com/problems/maximum-product-of-subsequences-with-an-alternating-sum-equal-to-k)

[中文文档](/solution/3500-3599/3509.Maximum%20Product%20of%20Subsequences%20With%20an%20Alternating%20Sum%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> cùng hai số nguyên <code>k</code> và <code>limit</code>. Nhiệm vụ của bạn là tìm một <strong><span data-keyword="subsequence-array">dãy con</span></strong> không rỗng của <code>nums</code> thỏa mãn:</p>

<ul>
    <li>Có <strong>tổng luân phiên</strong> bằng <code>k</code>.</li>
    <li><strong>Tối đa hóa</strong> tích của tất cả các số trong dãy con nhưng <em>không để tích vượt quá</em> <code>limit</code>.</li>
</ul>

<p>Trả về <em>tích</em> của các số trong dãy con đó. Nếu không có dãy con nào thỏa mãn yêu cầu, trả về -1.</p>

<p><strong>Tổng luân phiên</strong> của một mảng <strong>0-indexed</strong> được định nghĩa là <strong>tổng</strong> các phần tử ở chỉ số <strong>chẵn</strong> <strong>trừ</strong> <strong>tổng</strong> các phần tử ở chỉ số <strong>lẻ</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 2, limit = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con có tổng luân phiên bằng 2 là:</p>

<ul>
    <li><code>[1, 2, 3]</code>

    <ul>
        <li>Tổng luân phiên: <code>1 - 2 + 3 = 2</code></li>
        <li>Tích: <code>1 * 2 * 3 = 6</code></li>
    </ul>
    </li>
    <li><code>[2]</code>
    <ul>
        <li>Tổng luân phiên: 2</li>
        <li>Tích: 2</li>
    </ul>
    </li>

</ul>

<p>Tích lớn nhất không vượt quá giới hạn là 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,2,3], k = -5, limit = 12</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại dãy con nào có tổng luân phiên chính xác bằng -5.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,3,3], k = 0, limit = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con có tổng luân phiên bằng 0 là:</p>

<ul>
    <li><code>[2, 2]</code>

    <ul>
        <li>Tổng luân phiên: <code>2 - 2 = 0</code></li>
        <li>Tích: <code>2 * 2 = 4</code></li>
    </ul>
    </li>
    <li><code>[3, 3]</code>
    <ul>
        <li>Tổng luân phiên: <code>3 - 3 = 0</code></li>
        <li>Tích: <code>3 * 3 = 9</code></li>
    </ul>
    </li>
    <li><code>[2, 2, 3, 3]</code>
    <ul>
        <li>Tổng luân phiên: <code>2 - 2 + 3 - 3 = 0</code></li>
        <li>Tích: <code>2 * 2 * 3 * 3 = 36</code></li>
    </ul>
    </li>

</ul>

<p>Dãy con <code>[2, 2, 3, 3]</code> có tích lớn nhất với tổng luân phiên bằng <code>k</code>, nhưng <code>36 &gt; 9</code>. Tích lớn nhất tiếp theo là 9, và giá trị này nằm trong giới hạn.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 150</code></li>
    <li><code>0 &lt;= nums[i] &lt;= 12</code></li>
    <li><code>-10<sup>5</sup> &lt;= k &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= limit &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có $2^n$ dãy con và $n \le 150$. Cần theo dõi đồng thời tổng luân phiên và tích; miền giá trị của $k$ rộng, nhưng $\textit{limit} \le 5000$ và $nums[i] \le 12$, nên có thể loại bỏ các tích lớn hơn $\textit{limit}$.
>
> Dùng DP theo chỉ số, tính chẵn lẻ của độ dài đã chọn, tổng luân phiên hiện tại và tích bị chặn. Một số 0 trong $\textit{nums}$ cần được xử lý riêng cho trường hợp tích bằng 0.

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
