---
comments: true
difficulty: Hard
tags:
    - Minimax
    - Array
    - Math
    - String
    - Game Theory
    - Interactive
---

<!-- problem:start -->

# [843. Guess the Word](https://leetcode.com/problems/guess-the-word)

[中文文档](/solution/0800-0899/0843.Guess%20the%20Word/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các chuỗi khác nhau <code>words</code>, trong đó mỗi <code>words[i]</code> dài sáu ký tự. Một từ trong <code>words</code> được chọn làm từ bí mật.</p>

<p>Bạn cũng được cung cấp object hỗ trợ <code>Master</code>. Bạn có thể gọi <code>Master.guess(word)</code>, trong đó <code>word</code> là chuỗi dài sáu ký tự và phải thuộc <code>words</code>. <code>Master.guess(word)</code> trả về:</p>

<ul>
	<li><code>-1</code> nếu <code>word</code> không thuộc <code>words</code>, hoặc</li>
	<li>một số nguyên biểu thị số ký tự trong từ bạn đoán khớp chính xác với từ bí mật cả về giá trị lẫn vị trí.</li>
</ul>

<p>Mỗi test case có tham số <code>allowedGuesses</code>, là số lần tối đa bạn được gọi <code>Master.guess(word)</code>.</p>

<p>Với mỗi test case, bạn cần gọi <code>Master.guess</code> với từ bí mật mà không vượt quá số lần đoán cho phép. Kết quả sẽ là:</p>

<ul>
	<li><strong><code>&quot;Either you took too many guesses, or you did not find the secret word.&quot;</code></strong> nếu bạn gọi <code>Master.guess</code> quá <code>allowedGuesses</code> lần hoặc không gọi <code>Master.guess</code> với từ bí mật; hoặc</li>
	<li><strong><code>&quot;You guessed the secret word correctly.&quot;</code></strong> nếu bạn gọi <code>Master.guess</code> với từ bí mật và tổng số lần gọi <code>Master.guess</code> không vượt quá <code>allowedGuesses</code>.</li>
</ul>

<p>Các test case được tạo sao cho bạn có thể tìm ra từ bí mật bằng một chiến lược hợp lý (không dùng brute force).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> secret = &quot;acckzz&quot;, words = [&quot;acckzz&quot;,&quot;ccbazz&quot;,&quot;eiowzz&quot;,&quot;abcczz&quot;], allowedGuesses = 10
<strong>Output:</strong> You guessed the secret word correctly.
<strong>Giải thích:</strong>
master.guess(&quot;aaaaaa&quot;) trả về -1 vì &quot;aaaaaa&quot; không có trong words.
master.guess(&quot;acckzz&quot;) trả về 6 vì &quot;acckzz&quot; là từ bí mật và khớp cả 6 ký tự.
master.guess(&quot;ccbazz&quot;) trả về 3 vì &quot;ccbazz&quot; có 3 ký tự khớp.
master.guess(&quot;eiowzz&quot;) trả về 2 vì &quot;eiowzz&quot; có 2 ký tự khớp.
master.guess(&quot;abcczz&quot;) trả về 4 vì &quot;abcczz&quot; có 4 ký tự khớp.
Ta đã gọi master.guess 5 lần, trong đó có một lần đoán đúng từ bí mật, nên test case này đạt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> secret = &quot;hamada&quot;, words = [&quot;hamada&quot;,&quot;khaled&quot;], allowedGuesses = 10
<strong>Output:</strong> You guessed the secret word correctly.
<strong>Giải thích:</strong> Vì chỉ có hai từ nên bạn có thể đoán cả hai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>words[i].length == 6</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả chuỗi trong <code>words</code> đều <strong>khác nhau</strong>.</li>
	<li><code>secret</code> có trong <code>words</code>.</li>
	<li><code>10 &lt;= allowedGuesses &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm từ bí mật trong tối đa $30$ lượt đoán; mỗi lượt trả về số ký tự khớp. Chỉ có tối đa $100$ từ dài $6$ ký tự, nhưng đoán ngẫu nhiên chưa chắc làm giảm tập ứng viên.
>
> Giữ lại những từ vẫn phù hợp với tất cả câu trả lời, rồi lọc theo số ký tự khớp sau mỗi lần đoán. Dù ở đây không có code triển khai, ý chính là dùng số lượng khớp đó để thu hẹp phạm vi tìm kiếm xuống mức có thể xử lý.

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
