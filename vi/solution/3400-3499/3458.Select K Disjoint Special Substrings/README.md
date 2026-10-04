---
comments: true
difficulty: Medium
rating: 2220
source: Weekly Contest 437 Q3
tags:
    - Greedy
    - Hash Table
    - String
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3458. Select K Disjoint Special Substrings](https://leetcode.com/problems/select-k-disjoint-special-substrings)

[中文文档](/solution/3400-3499/3458.Select%20K%20Disjoint%20Special%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> có độ dài <code>n</code> và số nguyên <code>k</code>, hãy xác định liệu có thể chọn được <code>k</code> <strong>chuỗi con đặc biệt</strong> hay không.</p>

<p>Một <strong>chuỗi con đặc biệt</strong> là một <span data-keyword="substring-nonempty">chuỗi con</span> thỏa mãn:</p>

<ul>
	<li>Mọi ký tự xuất hiện bên trong chuỗi con không được xuất hiện bên ngoài nó trong chuỗi.</li>
	<li>Chuỗi con không phải là toàn bộ chuỗi <code>s</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng tất cả <code>k</code> chuỗi con phải không giao nhau, nghĩa là chúng không được chồng lấn lên nhau.</p>

<p>Trả về <code>true</code> nếu có thể chọn được <code>k</code> chuỗi con đặc biệt không giao nhau như vậy; ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcdbaefab&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta có thể chọn hai chuỗi con đặc biệt không giao nhau: <code>&quot;cd&quot;</code> và <code>&quot;ef&quot;</code>.</li>
	<li><code>&quot;cd&quot;</code> chứa các ký tự <code>&#39;c&#39;</code> và <code>&#39;d&#39;</code>, không xuất hiện ở nơi khác trong <code>s</code>.</li>
	<li><code>&quot;ef&quot;</code> chứa các ký tự <code>&#39;e&#39;</code> và <code>&#39;f&#39;</code>, không xuất hiện ở nơi khác trong <code>s</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cdefdc&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Nhiều nhất chỉ có thể có 2 chuỗi con đặc biệt không giao nhau: <code>&quot;e&quot;</code> và <code>&quot;f&quot;</code>. Vì <code>k = 3</code>, kết quả là <code>false</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abeabe&quot;, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt;= 26</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con đặc biệt là đoạn từ lần xuất hiện đầu tiên đến lần xuất hiện cuối cùng của một chữ cái, và mọi chữ cái bên trong đoạn đó đều phải nằm trọn trong đoạn. $n\le 5\times 10^4$, $k\le 26$.
>
> Mỗi chữ cái tạo ra nhiều nhất một khoảng ứng viên. Việc chọn $k$ khoảng không giao nhau là bài toán tập độc lập trên interval graph, có thể giải bằng cách sắp xếp theo điểm kết thúc.
>
> Tính $[\textit{first},\textit{last}]$ cho $26$ chữ cái, mở rộng mỗi khoảng để bao phủ các chữ cái nằm bên trong, sắp xếp theo điểm kết thúc, rồi chọn tham lam. Có thể thực hiện được nếu chọn được ít nhất $k$ khoảng.

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
