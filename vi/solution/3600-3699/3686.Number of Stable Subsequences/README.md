---
comments: true
difficulty: Hard
rating: 1969
source: Weekly Contest 467 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3686. Number of Stable Subsequences](https://leetcode.com/problems/number-of-stable-subsequences)

[中文文档](/solution/3600-3699/3686.Number%20of%20Stable%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong><span data-keyword="subsequence-array-nonempty">subsequence</span></strong> được gọi là <strong>ổn định</strong> nếu không chứa <strong>ba</strong> phần tử <strong>liên tiếp</strong> có cùng tính chẵn lẻ khi đọc subsequence theo <strong>thứ tự</strong> (tức là liên tiếp <strong>trong subsequence</strong>).</p>

<p>Trả về số lượng subsequence ổn định.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về phần dư của đáp án khi chia cho <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các subsequence ổn định là <code>[1]</code>, <code>[3]</code>, <code>[5]</code>, <code>[1, 3]</code>, <code>[1, 5]</code> và <code>[3, 5]</code>.</li>
	<li>Subsequence <code>[1, 3, 5]</code> không ổn định vì chứa ba số lẻ liên tiếp. Do đó, đáp án là 6.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = </span>[2,3,4,2]</p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Subsequence duy nhất không ổn định là <code>[2, 4, 2]</code>, vì chứa ba số chẵn liên tiếp.</li>
	<li>Mọi subsequence khác đều ổn định. Do đó, đáp án là 14.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>​​​​​​​5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một subsequence ổn định không cho phép một đoạn liên tiếp có cùng tính chẵn lẻ kéo dài quá lâu. Với $n\le 10^5$, cần dùng DP tuyến tính.
>
> Ta dùng các trạng thái để phân biệt tính chẵn lẻ cuối cùng và độ dài đoạn hiện tại là $1$ hay $2$; đoạn có độ dài $3$ là không hợp lệ.
>
> Một $x$ mới có thể nối sau một phần tử có tính chẵn lẻ ngược lại, hoặc có cùng tính chẵn lẻ khi đoạn hiện tại vẫn ngắn hơn $2$. Lấy phần dư theo modulo $10^9+7$ và cộng tất cả các trạng thái hợp lệ.

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
