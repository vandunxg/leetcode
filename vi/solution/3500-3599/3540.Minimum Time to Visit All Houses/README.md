---
comments: true
difficulty: Medium
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3540. Minimum Time to Visit All Houses 🔒](https://leetcode.com/problems/minimum-time-to-visit-all-houses)

[Tài liệu tiếng Trung](/solution/3500-3599/3540.Minimum%20Time%20to%20Visit%20All%20Houses/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>forward</code> và <code>backward</code>, đều có kích thước <code>n</code>. Bạn cũng được cho một mảng số nguyên khác là <code>queries</code>.</p>

<p>Có <code>n</code> ngôi nhà <em>được sắp xếp theo một vòng tròn</em>. Các ngôi nhà được nối với nhau bằng những con đường theo cách đặc biệt:</p>

<ul>
    <li>Với mọi <code>0 &lt;= i &lt;= n - 2</code>, ngôi nhà <code>i</code> được nối với ngôi nhà <code>i + 1</code> bằng một con đường có độ dài <code>forward[i]</code> mét. Ngoài ra, ngôi nhà <code>n - 1</code> được nối ngược về ngôi nhà 0 bằng một con đường có độ dài <code>forward[n - 1]</code> mét, tạo thành một vòng tròn.</li>
    <li>Với mọi <code>1 &lt;= i &lt;= n - 1</code>, ngôi nhà <code>i</code> được nối với ngôi nhà <code>i - 1</code> bằng một con đường có độ dài <code>backward[i]</code> mét. Ngoài ra, ngôi nhà 0 được nối ngược về ngôi nhà <code>n - 1</code> bằng một con đường có độ dài <code>backward[0]</code> mét, tạo thành một vòng tròn.</li>
</ul>

<p>Bạn có thể đi bộ với tốc độ <strong>một</strong> mét mỗi giây. Bắt đầu từ ngôi nhà 0, hãy tìm <strong>thời gian tối thiểu</strong> cần để ghé thăm từng ngôi nhà theo thứ tự được chỉ định bởi <code>queries</code>.</p>

<p>Trả về <strong>tổng thời gian tối thiểu</strong> cần để ghé thăm các ngôi nhà.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">forward = [1,4,4], backward = [4,1,2], queries = [1,2,0,2]</span></p>

<p><strong>Đầu ra:</strong> 12</p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi được thực hiện là <code><u>0</u><sup>(0)</sup> &rarr; <u>1</u><sup>(1)</sup> &rarr;​​​​​​​ <u>2</u><sup>(5)</sup> <u>&rarr;</u> 1<sup>(7)</sup> <u>&rarr;</u>​​​​​​​ <u>0</u><sup>(8)</sup> <u>&rarr;</u> <u>2</u><sup>(12)</sup></code>.</p>

<p><strong>Lưu ý:</strong> Ký hiệu được sử dụng là <code>node<sup>(total time)</sup></code>, <code>&rarr;</code> biểu diễn con đường theo hướng tiến, còn <code><u>&rarr;</u></code> biểu diễn con đường theo hướng lùi.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">forward = [1,1,1,1], backward = [2,2,2,2], queries = [1,2,3,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi là <code><u>0</u> &rarr;​​​​​​​ <u>1</u> &rarr;​​​​​​​ <u>2</u> &rarr;​​​​​​​ <u>3</u> &rarr; <u>0</u></code>. Mỗi bước đều đi theo hướng tiến và cần 1 giây.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>n == forward.length == backward.length</code></li>
    <li><code>1 &lt;= forward[i], backward[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= queries[i] &lt; n</code></li>
    <li><code>queries[i] != queries[i + 1]</code></li>
    <li><code>queries[0]</code> không phải là 0.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các ngôi nhà nằm trên một chu kỳ với trọng số khác nhau theo hướng tiến và lùi, đồng thời phải ghé thăm theo thứ tự trong $\textit{queries}$. Với $n,q \le 10^5$, việc tìm kiếm trên chu kỳ cho mỗi lần là không thể.
>
> Prefix sum của hai hướng giúp tính cung ngắn hơn giữa mọi cặp ngôi nhà. Cộng độ dài các cung đó theo chuỗi truy vấn.

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
