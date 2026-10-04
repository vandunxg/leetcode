---
comments: true
difficulty: Hard
rating: 2584
source: Weekly Contest 451 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3563. Lexicographically Smallest String After Adjacent Removals](https://leetcode.com/problems/lexicographically-smallest-string-after-adjacent-removals)

[中文文档](/solution/3500-3599/3563.Lexicographically%20Smallest%20String%20After%20Adjacent%20Removals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn có thể thực hiện thao tác sau một số lần tùy ý, kể cả không lần nào:</p>

<ul>
    <li>Xóa <strong>bất kỳ</strong> cặp ký tự <strong>kề nhau</strong> nào trong chuỗi mà <strong>liên tiếp</strong> trong bảng chữ cái, theo một trong hai thứ tự (ví dụ: <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code>, hoặc <code>&#39;b&#39;</code> và <code>&#39;a&#39;</code>).</li>
    <li>Dịch các ký tự còn lại sang trái để lấp đầy khoảng trống.</li>
</ul>

<p>Trả về chuỗi <strong><span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span></strong> có thể thu được sau khi thực hiện các thao tác một cách tối ưu.</p>

<p><strong>Lưu ý:</strong> Hãy xem bảng chữ cái là vòng tròn, do đó <code>&#39;a&#39;</code> và <code>&#39;z&#39;</code> là hai ký tự liên tiếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;a&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>&quot;bc&quot;</code> khỏi chuỗi, còn lại <code>&quot;a&quot;</code>.</li>
    <li>Không thể thực hiện thêm thao tác nào. Vì vậy, chuỗi nhỏ nhất theo thứ tự từ điển sau khi thực hiện tất cả các thao tác xóa có thể là <code>&quot;a&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;bcda&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>​​​​​​​</strong>Xóa <code>&quot;cd&quot;</code> khỏi chuỗi, còn lại <code>&quot;ba&quot;</code>.</li>
    <li>Xóa <code>&quot;ba&quot;</code> khỏi chuỗi, còn lại <code>&quot;&quot;</code>.</li>
    <li>Không thể thực hiện thêm thao tác nào. Vì vậy, chuỗi nhỏ nhất theo thứ tự từ điển sau khi thực hiện tất cả các thao tác xóa có thể là <code>&quot;&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zdce&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;zdce&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>&quot;dc&quot;</code> khỏi chuỗi, còn lại <code>&quot;ze&quot;</code>.</li>
    <li>Không thể thực hiện thêm thao tác nào trên <code>&quot;ze&quot;</code>.</li>
    <li>Tuy nhiên, vì <code>&quot;zdce&quot;</code> nhỏ hơn <code>&quot;ze&quot;</code> theo thứ tự từ điển, chuỗi nhỏ nhất sau khi thực hiện tất cả các thao tác xóa có thể là <code>&quot;zdce&quot;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 250</code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái liên tiếp kề nhau có thể bị xóa, và ta cần tìm chuỗi còn lại nhỏ nhất theo thứ tự từ điển, chứ không phải một chuỗi bất kỳ. Với $n \le 250$, ta có thể dùng quy hoạch động trên đoạn để kiểm tra một đoạn có thể bị xóa hoàn toàn hay không, sau đó dựng lại chuỗi nhỏ nhất.
>
> $g[i][j]$ cho biết $s[i..j]$ có thể bị xóa hay không, bằng cách ghép $s[i]$ với một ký tự phù hợp ở phía sau. $f[i]$ là chuỗi nhỏ nhất tạo được từ $s[i..]$: giữ lại $s[i]$ rồi nối với $f[i+1]$, hoặc bỏ qua một đoạn $[i,k]$ có thể xóa hoàn toàn để chuyển sang $f[k+1]$, sau đó lấy giá trị nhỏ hơn theo thứ tự từ điển.

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
