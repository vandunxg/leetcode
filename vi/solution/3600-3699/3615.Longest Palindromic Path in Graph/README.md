---
comments: true
difficulty: Hard
rating: 2463
source: Weekly Contest 458 Q4
tags:
    - Bit Manipulation
    - Graph
    - String
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [3615. Longest Palindromic Path in Graph](https://leetcode.com/problems/longest-palindromic-path-in-graph)

[中文文档](/solution/3600-3699/3615.Longest%20Palindromic%20Path%20in%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một đồ thị <strong>vô hướng</strong> gồm <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>, cùng một mảng 2D <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một chuỗi <code>label</code> có độ dài <code>n</code>, trong đó <code>label[i]</code> là ký tự gắn với node <code>i</code>.</p>

<p>Bạn có thể bắt đầu từ bất kỳ node nào và di chuyển đến một node kề, mỗi node được thăm <strong>nhiều nhất</strong> một lần.</p>

<p>Trả về độ dài <strong>lớn nhất</strong> của một <strong><span data-keyword="palindrome-string">palindrome</span></strong> có thể tạo thành bằng cách đi qua một tập các node <strong>duy nhất</strong> theo một đường đi hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], label = &quot;aba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải</strong><strong>thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3615.Longest%20Palindromic%20Path%20in%20Graph/images/screenshot-2025-06-13-at-230714.png" style="width: 250px; height: 85px;" /></p>

<ul>
    <li>Đường đi palindrome dài nhất đi từ node 0 đến node 2 qua node 1, theo đường đi <code>0 &rarr; 1 &rarr; 2</code> để tạo thành chuỗi <code>&quot;aba&quot;</code>.</li>
    <li>Đây là một palindrome hợp lệ có độ dài 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[0,2]], label = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3615.Longest%20Palindromic%20Path%20in%20Graph/images/screenshot-2025-06-13-at-230017.png" style="width: 200px; height: 150px;" /></p>

<ul>
    <li>Không có đường đi nào có nhiều hơn một node tạo thành palindrome.</li>
    <li>Lựa chọn tốt nhất là một node bất kỳ, tạo thành palindrome có độ dài 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,2],[0,3],[3,1]], label = &quot;bbac&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3615.Longest%20Palindromic%20Path%20in%20Graph/images/screenshot-2025-06-13-at-230508.png" style="width: 200px; height: 200px;" /></p>

<ul>
    <li>Đường đi palindrome dài nhất đi từ node 0 đến node 1, theo đường đi <code>0 &rarr; 3 &rarr; 1</code>, tạo thành chuỗi <code>&quot;bcb&quot;</code>.</li>
    <li>Đây là một palindrome hợp lệ có độ dài 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 14</code></li>
    <li><code>n - 1 &lt;= edges.length &lt;= n * (n - 1) / 2</code></li>
    <li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
    <li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
    <li><code>label.length == n</code></li>
    <li><code>label</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
    <li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần một đường đi đơn có nhãn các đỉnh tạo thành palindrome. $n$ nhỏ nên có thể biểu diễn tập các đỉnh đã dùng bằng bitmask.
>
> Palindrome phát triển từ hai đầu: khi hai đầu hiện tại có cùng nhãn, mỗi đầu đi tới một node kề chưa dùng có cùng nhãn đó. Palindrome lẻ và chẵn bắt đầu từ một đỉnh hoặc một cặp đỉnh kề nhau có nhãn bằng nhau.
>
> Gọi $f[S][i][j]$ là một đường đi palindrome trên tập đỉnh $S$ với hai đầu là $i,j$. Chuyển trạng thái qua các node kề chưa dùng của $i$ và $j$ có nhãn bằng nhau. Đáp án là $|S|$ lớn nhất trong các trạng thái có thể đạt được.

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
