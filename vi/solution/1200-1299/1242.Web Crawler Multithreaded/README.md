---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Concurrency
---

<!-- problem:start -->

# [1242. Web Crawler Multithreaded 🔒](https://leetcode.com/problems/web-crawler-multithreaded)

[中文文档](/solution/1200-1299/1242.Web%20Crawler%20Multithreaded/README.md)

## Mô tả

<!-- description:start -->

<p>Cho URL <code>startUrl</code> và interface <code>HtmlParser</code>, hãy triển khai <strong>web crawler đa luồng</strong> để thu thập mọi liên kết có <strong>cùng hostname</strong> với <code>startUrl</code>.</p>

<p>Trả về tất cả URL mà web crawler tìm được theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>Crawler cần thực hiện các việc sau:</p>

<ul>
	<li>Bắt đầu từ trang: <code>startUrl</code></li>
	<li>Gọi <code>HtmlParser.getUrls(url)</code> để lấy tất cả URL từ trang web tương ứng với URL đã cho.</li>
	<li>Không crawl cùng một liên kết hai lần.</li>
	<li>Chỉ duyệt các liên kết có <strong>cùng hostname</strong> với <code>startUrl</code>.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1242.Web%20Crawler%20Multithreaded/images/urlhostname.png" style="width: 500px; height: 136px;" />
<p>Như ví dụ URL ở trên, hostname là <code>example.org</code>. Để đơn giản, có thể giả sử mọi URL đều dùng <strong>giao thức HTTP</strong> và không chỉ định <strong>port</strong>. Ví dụ, <code>http://leetcode.com/problems</code> và <code>http://leetcode.com/contest</code> có cùng hostname, còn <code>http://example.org/test</code> và <code>http://example.com/abc</code> thì không.</p>

<p>Interface <code>HtmlParser</code> được định nghĩa như sau:</p>

<pre>
interface HtmlParser {
  // Return a list of all urls from a webpage of given <em>url</em>.
  // This is a blocking call, that means it will do HTTP request and return when this request is finished.
  public List&lt;String&gt; getUrls(String url);
}
</pre>

<p>Lưu ý, <code>getUrls(String url)</code> mô phỏng một HTTP request. Có thể xem đây là lời gọi hàm blocking, chờ cho đến khi HTTP request hoàn tất. Đảm bảo <code>getUrls(String url)</code> trả về các URL trong vòng <strong>15ms. </strong> Lời giải đơn luồng sẽ vượt quá thời gian giới hạn; liệu web crawler đa luồng có thể chạy nhanh hơn không?</p>

<p>Dưới đây là hai ví dụ minh họa chức năng của bài toán. Khi custom test, bạn sẽ có ba biến <code>urls</code>, <code>edges</code> và <code>startUrl</code>. Lưu ý, trong code bạn chỉ truy cập được <code>startUrl</code>; <code>urls</code> và <code>edges</code> không thể được truy cập trực tiếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1242.Web%20Crawler%20Multithreaded/images/sample_2_1497.png" style="width: 610px; height: 300px;" /></p>

<pre>
<strong>Đầu vào:
</strong>urls = [
&nbsp; &quot;http://news.yahoo.com&quot;,
&nbsp; &quot;http://news.yahoo.com/news&quot;,
&nbsp; &quot;http://news.yahoo.com/news/topics/&quot;,
&nbsp; &quot;http://news.google.com&quot;,
&nbsp; &quot;http://news.yahoo.com/us&quot;
]
edges = [[2,0],[2,1],[3,2],[3,1],[0,4]]
startUrl = &quot;http://news.yahoo.com/news/topics/&quot;
<strong>Đầu ra:</strong> [
&nbsp; &quot;http://news.yahoo.com&quot;,
&nbsp; &quot;http://news.yahoo.com/news&quot;,
&nbsp; &quot;http://news.yahoo.com/news/topics/&quot;,
&nbsp; &quot;http://news.yahoo.com/us&quot;
]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1242.Web%20Crawler%20Multithreaded/images/sample_3_1497.png" style="width: 540px; height: 270px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> 
urls = [
&nbsp; &quot;http://news.yahoo.com&quot;,
&nbsp; &quot;http://news.yahoo.com/news&quot;,
&nbsp; &quot;http://news.yahoo.com/news/topics/&quot;,
&nbsp; &quot;http://news.google.com&quot;
]
edges = [[0,2],[2,1],[3,2],[3,1],[3,0]]
startUrl = &quot;http://news.google.com&quot;
<strong>Đầu ra:</strong> [&quot;http://news.google.com&quot;]
<strong>Giải thích: </strong>startUrl liên kết đến tất cả các trang khác không cùng hostname.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= urls.length &lt;= 1000</code></li>
	<li><code>1 &lt;= urls[i].length &lt;= 300</code></li>
	<li><code>startUrl</code> là một trong các URL trong <code>urls</code>.</li>
	<li>Nhãn hostname dài từ <code>1</code> đến <code>63</code> ký tự, tính cả dấu chấm; chỉ được chứa chữ cái ASCII từ <code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code>, chữ số từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code> và dấu gạch nối (<code>&#39;-&#39;</code>).</li>
	<li>Hostname không được bắt đầu hoặc kết thúc bằng dấu gạch nối (&#39;-&#39;).</li>
	<li>Xem thêm:&nbsp;&nbsp;<a href="https://en.wikipedia.org/wiki/Hostname#Restrictions_on_valid_hostnames" target="_blank">https://en.wikipedia.org/wiki/Hostname#Restrictions_on_valid_hostnames</a></li>
	<li>Có thể giả sử thư viện URL không chứa URL trùng lặp.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ol>
	<li>Giả sử có 10.000 node và 1 tỷ URL cần crawl. Ta triển khai cùng một phần mềm trên mỗi node và phần mềm có thể biết tất cả các node. Cần giảm thiểu giao tiếp giữa các máy và đảm bảo mỗi node xử lý lượng công việc như nhau. Bạn sẽ thay đổi thiết kế web crawler thế nào?</li>
	<li>Nếu một node gặp lỗi hoặc ngừng hoạt động thì sao?</li>
	<li>Làm thế nào để biết crawler đã hoàn tất?</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tương tự crawler đơn luồng, ta chỉ thu thập URL cùng host. Có thể fetch song song; tập các URL đã thăm cần được bảo vệ bằng mutex.
>
> Set đó phải thread-safe: sau khi worker phân tích một liên kết, nó chỉ chuyển URL cho các worker khác nếu host khớp và thao tác thêm vào set thành công. Queue cùng set đảm bảo mỗi trang chỉ được fetch tối đa một lần; bộ lọc host giữ crawler trong cùng một domain.

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
