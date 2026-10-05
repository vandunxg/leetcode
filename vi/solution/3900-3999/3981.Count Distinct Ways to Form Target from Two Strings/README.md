---
comments: true
difficulty: Hard
rating: 2128
source: Biweekly Contest 186 Q4
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3981. Count Distinct Ways to Form Target from Two Strings](https://leetcode.com/problems/count-distinct-ways-to-form-target-from-two-strings)

[中文文档](/solution/3900-3999/3981.Count%20Distinct%20Ways%20to%20Form%20Target%20from%20Two%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba chuỗi <code>word1</code>, <code>word2</code> và <code>target</code>.</p>

<p>Nhiệm vụ của bạn là đếm số cách tạo thành <code>target</code> bằng cách chọn các ký tự từ <code>word1</code> và <code>word2</code> với các điều kiện sau:</p>

<ul>
	<li>Với mỗi ký tự của <code>target</code>, chọn một ký tự trùng khớp từ <code>word1</code> hoặc <code>word2</code>.</li>
	<li>Các chỉ số được chọn từ <code>word1</code> phải tăng <strong>nghiêm ngặt</strong>.</li>
	<li>Các chỉ số được chọn từ <code>word2</code> phải tăng <strong>nghiêm ngặt</strong>.</li>
	<li>Phải chọn <strong>ít nhất</strong> một ký tự từ <strong>cả</strong> <code>word1</code> và <code>word2</code>.</li>
</ul>

<p>Hai cách được xem là khác nhau nếu tại <strong>ít nhất</strong> một vị trí trong <code>target</code>, ký tự được chọn đến từ chuỗi khác hoặc chỉ số khác.</p>

<p>Trả về số cách. Vì đáp án có thể rất lớn, hãy trả về phần dư khi chia <strong>modulo</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;abc&quot;, word2 = &quot;bac&quot;, target = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 5 cách tạo thành <code>target</code>:</p>

<ul>
	<li><code>word1[0] = &#39;a&#39;</code>, <code>word1[1] = &#39;b&#39;</code>, <code>word2[2] = &#39;c&#39;</code></li>
	<li><code>word1[0] = &#39;a&#39;</code>, <code>word2[0] = &#39;b&#39;</code>, <code>word1[2] = &#39;c&#39;</code></li>
	<li><code>word1[0] = &#39;a&#39;</code>, <code>word2[0] = &#39;b&#39;</code>, <code>word2[2] = &#39;c&#39;</code></li>
	<li><code>word2[1] = &#39;a&#39;</code>, <code>word1[1] = &#39;b&#39;</code>, <code>word1[2] = &#39;c&#39;</code></li>
	<li><code>word2[1] = &#39;a&#39;</code>, <code>word1[1] = &#39;b&#39;</code>, <code>word2[2] = &#39;c&#39;</code></li>
</ul>

<p>Tất cả các cách đều giữ thứ tự chỉ số tăng dần trong mỗi chuỗi và chọn ít nhất một ký tự từ mỗi chuỗi.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;cd&quot;, word2 = &quot;cd&quot;, target = &quot;ccd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 4 cách tạo thành <code>target</code>:</p>

<ul>
	<li><code>word1[0] = &#39;c&#39;</code>, <code>word2[0] = &#39;c&#39;</code>, <code>word1[1] = &#39;d&#39;</code></li>
	<li><code>word1[0] = &#39;c&#39;</code>, <code>word2[0] = &#39;c&#39;</code>, <code>word2[1] = &#39;d&#39;</code></li>
	<li><code>word2[0] = &#39;c&#39;</code>, <code>word1[0] = &#39;c&#39;</code>, <code>word1[1] = &#39;d&#39;</code></li>
	<li><code>word2[0] = &#39;c&#39;</code>, <code>word1[0] = &#39;c&#39;</code>, <code>word2[1] = &#39;d&#39;</code></li>
</ul>

<p>Hai ký tự <code>&#39;c&#39;</code> đầu tiên trong <code>target</code> phải lần lượt đến từ mỗi chuỗi. Ký tự <code>&#39;d&#39;</code> cuối cùng có thể được chọn từ một trong hai chuỗi.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;xy&quot;, word2 = &quot;xy&quot;, target = &quot;xyxy&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 2 cách tạo thành <code>target</code>:</p>

<ul>
	<li><code>word1[0] = &#39;x&#39;</code>, <code>word1[1] = &#39;y&#39;</code>, <code>word2[0] = &#39;x&#39;</code>, <code>word2[1] = &#39;y&#39;</code></li>
	<li><code>word2[0] = &#39;x&#39;</code>, <code>word2[1] = &#39;y&#39;</code>, <code>word1[0] = &#39;x&#39;</code>, <code>word1[1] = &#39;y&#39;</code></li>
</ul>

<p>Mỗi phần <code>&quot;xy&quot;</code> trong <code>target</code> hoàn toàn đến từ một chuỗi.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;ab&quot;, word2 = &quot;cde&quot;, target = &quot;ace&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách duy nhất là chọn <code>word1[0] = &#39;a&#39;</code>, <code>word2[0] = &#39;c&#39;</code> và <code>word2[2] = &#39;e&#39;</code>. Vì vậy, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length, word2.length, target.length &lt;= 100</code></li>
	<li><code>word1</code>, <code>word2</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta đan xen các chỉ số tăng dần từ hai chuỗi để tạo thành $\textit{target}$, đồng thời phải sử dụng cả hai chuỗi ít nhất một lần. Trạng thái của subsequence DP là tiền tố của $\textit{target}$ cùng với số ký tự đã dùng ở mỗi chuỗi, kèm cờ cho biết mỗi phía đã được sử dụng.
>
> Việc có thể dùng rolling array ba chiều hay không phụ thuộc vào tích độ dài hai chuỗi. Mỗi chuyển trạng thái chọn chuỗi cung cấp ký tự tiếp theo rồi nhảy đến lần xuất hiện khớp tiếp theo trong chuỗi đó.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng ở subsequence DP trên hai chuỗi.

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
