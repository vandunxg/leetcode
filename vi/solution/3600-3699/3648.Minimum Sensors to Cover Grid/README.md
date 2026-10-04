---
comments: true
difficulty: Medium
rating: 1395
source: Biweekly Contest 163 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3648. Minimum Sensors to Cover Grid](https://leetcode.com/problems/minimum-sensors-to-cover-grid)

[Tài liệu tiếng Trung](/solution/3600-3699/3648.Minimum%20Sensors%20to%20Cover%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một lưới <code>n &times; m</code> và một số nguyên <code>k</code>.</p>

<p>Một sensor được đặt tại ô <code>(r, c)</code> sẽ phủ tất cả các ô có <strong>khoảng cách Chebyshev</strong> đến <code>(r, c)</code> <strong>không lớn hơn</strong> <code>k</code>.</p>

<p><strong>Khoảng cách Chebyshev</strong> giữa hai ô <code>(r<sub>1</sub>, c<sub>1</sub>)</code> và <code>(r<sub>2</sub>, c<sub>2</sub>)</code> là <code>max(|r<sub>1</sub> &minus; r<sub>2</sub>|,|c<sub>1</sub> &minus; c<sub>2</sub>|)</code>.</p>

<p>Nhiệm vụ của bạn là trả về số lượng sensor <strong>nhỏ nhất</strong> cần thiết để phủ mọi ô trong lưới.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, m = 5, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đặt các sensor tại các vị trí <code>(0, 3)</code>, <code>(1, 0)</code>, <code>(3, 3)</code> và <code>(4, 1)</code> sẽ đảm bảo mọi ô trong lưới đều được phủ. Vì vậy, đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, m = 2, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với <code>k = 2</code>, một sensor duy nhất có thể phủ toàn bộ lưới <code>2 * 2</code> bất kể vị trí của nó. Vì vậy, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>3</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một sensor phủ một hình cầu Chebyshev bán kính $k$. Phủ một lưới $n\times m$ tương đương với việc lát lưới bằng các hình vuông có cạnh $2k+1$.
>
> Một sensor phủ được $2k+1$ hàng liên tiếp và cùng số cột liên tiếp, được giới hạn lại khi chạm biên.
>
> Số lượng sensor là $\lceil n/(2k+1)\rceil\cdot\lceil m/(2k+1)\rceil$. Khi $k=0$, mỗi ô cần một sensor riêng.

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
