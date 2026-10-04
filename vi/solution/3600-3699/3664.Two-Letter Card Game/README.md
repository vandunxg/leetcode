---
comments: true
difficulty: Medium
rating: 2157
source: Biweekly Contest 164 Q2
tags:
    - Array
    - Hash Table
    - String
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [3664. Two-Letter Card Game](https://leetcode.com/problems/two-letter-card-game)

[中文文档](/solution/3600-3699/3664.Two-Letter%20Card%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một bộ bài được biểu diễn bằng mảng chuỗi <code>cards</code>, trong đó mỗi lá bài hiển thị hai chữ cái thường.</p>

<p>Bạn cũng được cho một chữ cái <code>x</code>. Bạn chơi một trò chơi theo các quy tắc sau:</p>

<ul>
	<li>Bắt đầu với 0 điểm.</li>
	<li>Trong mỗi lượt, bạn phải tìm hai lá bài <strong>tương thích</strong> trong bộ bài, cả hai đều chứa chữ cái <code>x</code> ở một vị trí bất kỳ.</li>
	<li>Loại bỏ cặp lá bài đó và nhận được <strong>1 điểm</strong>.</li>
	<li>Trò chơi kết thúc khi bạn không thể tìm thấy cặp lá bài tương thích nào nữa.</li>
</ul>

<p>Trả về số điểm <strong>lớn nhất</strong> bạn có thể đạt được nếu chơi tối ưu.</p>

<p>Hai lá bài <strong>tương thích</strong> nếu các chuỗi khác nhau ở <strong>đúng</strong> 1 vị trí.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">cards = [&quot;aa&quot;,&quot;ab&quot;,&quot;ba&quot;,&quot;ac&quot;], x = &quot;a&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ở lượt đầu tiên, chọn và loại bỏ hai lá bài <code>&quot;ab&quot;</code> và <code>&quot;ac&quot;</code>. Chúng tương thích vì chỉ khác nhau ở chỉ số 1.</li>
	<li>Ở lượt thứ hai, chọn và loại bỏ hai lá bài <code>&quot;aa&quot;</code> và <code>&quot;ba&quot;</code>. Chúng tương thích vì chỉ khác nhau ở chỉ số 0.</li>
</ul>

<p>Vì không còn cặp lá bài tương thích nào, tổng điểm là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">cards = [&quot;aa&quot;,&quot;ab&quot;,&quot;ba&quot;], x = &quot;a&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ở lượt đầu tiên, chọn và loại bỏ hai lá bài <code>&quot;aa&quot;</code> và <code>&quot;ba&quot;</code>.</li>
</ul>

<p>Vì không còn cặp lá bài tương thích nào, tổng điểm là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">cards = [&quot;aa&quot;,&quot;ab&quot;,&quot;ba&quot;,&quot;ac&quot;], x = &quot;b&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các lá bài duy nhất chứa ký tự <code>&#39;b&#39;</code> là <code>&quot;ab&quot;</code> và <code>&quot;ba&quot;</code>. Tuy nhiên, chúng khác nhau ở cả hai chỉ số nên không tương thích. Do đó, đầu ra là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= cards.length &lt;= 10<sup>5</sup></code></li>
	<li><code>cards[i].length == 2</code></li>
	<li>Mỗi <code>cards[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường từ <code>&#39;a&#39;</code> đến <code>&#39;j&#39;</code>.</li>
	<li><code>x</code> là một chữ cái tiếng Anh viết thường từ <code>&#39;a&#39;</code> đến <code>&#39;j&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lá bài gồm hai chữ cái. Mỗi lượt sử dụng hai lá bài cùng chứa đúng chữ cái $x$ đã cho. Hãy đếm các lá bài dựa trên việc chúng có chứa x hay không và dựa trên chữ cái còn lại.
>
> Các lá bài chứa $x$ có dạng $x?$ hoặc $?x$. Hai lá bài như vậy ghép được với nhau khi chữ cái còn lại hoặc vị trí của $x$ khác nhau. Ghép các nhóm đó theo cách tham lam; các lá bài không chứa $x$ không bao giờ ghép được.
>
> Thử từng lựa chọn trong $26$ chữ cái của $x$, ghép các nhóm đã đếm theo giới hạn tương ứng, rồi giữ lại số lượt lớn nhất.

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
