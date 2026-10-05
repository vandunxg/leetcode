---
comments: true
difficulty: Medium
rating: 1759
source: Biweekly Contest 182 Q2
tags:
    - String
---

<!-- problem:start -->

# [3922. Minimum Flips to Make Binary String Coherent](https://leetcode.com/problems/minimum-flips-to-make-binary-string-coherent)

[中文文档](/solution/3900-3999/3922.Minimum%20Flips%20to%20Make%20Binary%20String%20Coherent/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code>.</p>

<p>Một chuỗi được xem là <strong>nhất quán</strong> nếu nó <strong>không chứa</strong> <code>&quot;011&quot;</code> hoặc <code>&quot;110&quot;</code> dưới dạng <span data-keyword="subsequence-string">dãy con</span>.</p>

<p>Trong một thao tác, bạn có thể <strong>đảo</strong> bất kỳ ký tự nào trong <code>s</code> (<code>&#39;0&#39;</code> thành <code>&#39;1&#39;</code> hoặc <code>&#39;1&#39;</code> thành <code>&#39;0&#39;</code>).</p>

<p>Trả về một số nguyên biểu thị số thao tác <strong>nhỏ nhất</strong> cần thực hiện để làm cho <code>s</code> nhất quán.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1010&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đảo <code>s[0]</code> để nhận được <code>&quot;0010&quot;</code>, chuỗi này không chứa dãy con <code>&quot;011&quot;</code> hoặc <code>&quot;110&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0110&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đảo <code>s[1]</code> để nhận được <code>&quot;0010&quot;</code>, loại bỏ mọi dãy con bị cấm <code>&quot;011&quot;</code> và <code>&quot;110&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1000&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi không chứa dãy con <code>&quot;011&quot;</code> hoặc <code>&quot;110&quot;</code>, nên không cần thực hiện phép đảo nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 10^5$, ta không thể liệt kê các tập hợp ký tự cần đảo. Việc cấm các dãy con `011` và `110` có nghĩa là chuỗi không thể chứa “một $0$ rồi sau đó là hai $1$” hoặc “hai $1$ rồi sau đó là một $0$”.
>
> Do đó, các chuỗi nhất quán bị giới hạn trong một số dạng: toàn số 0, toàn số 1, các số 1 đứng trước các số 0, và một vài dạng không bao giờ tách các số 1 thành hai phần quanh một số 0. Số phép đảo nhỏ nhất là khoảng cách Hamming nhỏ nhất đến một trong các dạng đó.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng lại ở bước phân loại này.

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
