---
comments: true
difficulty: Hard
tags:
    - JavaScript
---

<!-- problem:start -->

# [2756. Query Batching 🔒](https://leetcode.com/problems/query-batching)

[中文文档](/solution/2700-2799/2756.Query%20Batching/README.md)

## Mô tả

<!-- description:start -->

<p>Gom nhiều query nhỏ thành một query lớn có thể là một tối ưu hóa hữu ích. Hãy viết một class&nbsp;<code>QueryBatcher</code>&nbsp;để triển khai chức năng này.</p>

<p>Constructor cần nhận hai tham số:</p>

<ul>
	<li>Một hàm bất đồng bộ&nbsp;<code>queryMultiple</code>&nbsp;nhận một mảng các key chuỗi <code>input</code>. Hàm sẽ resolve với một mảng các value có cùng độ dài với mảng đầu vào. Mỗi chỉ số tương ứng với value gắn với&nbsp;<code>input[i]</code>. Bạn có thể giả định promise sẽ không bao giờ bị reject.</li>
	<li>Một khoảng thời gian throttle tính bằng mili giây&nbsp;<code>t</code>.</li>
</ul>

<p>Class có một method duy nhất.</p>

<ul>
	<li><code>async getValue(key)</code>. Nhận một key chuỗi và resolve với một value chuỗi. Các key truyền cho hàm này cuối cùng phải được truyền vào hàm&nbsp;<code>queryMultiple</code>. Không được gọi&nbsp;<code>queryMultiple</code>&nbsp;liên tiếp trong vòng&nbsp;<code>t</code>&nbsp;mili giây. Lần đầu tiên&nbsp;<code>getValue</code>&nbsp;được gọi, cần gọi ngay&nbsp;<code>queryMultiple</code>&nbsp;với key đó. Nếu sau&nbsp;<code>t</code>&nbsp;mili giây,&nbsp;<code>getValue</code>&nbsp;được gọi lại, tất cả key đã truyền phải được truyền vào&nbsp;<code>queryMultiple</code>&nbsp;và cuối cùng được trả về. Bạn có thể giả định mọi key truyền vào method này đều là duy nhất.</li>
</ul>

<p>Sơ đồ dưới đây minh họa cách thuật toán throttle hoạt động. Mỗi hình chữ nhật biểu thị 100ms. Thời gian throttle là 400ms.</p>

<p><img alt="Throttle info" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2756.Query%20Batching/images/throttle.png" style="width: 622px; height: 200px;" /></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
queryMultiple = async function(keys) {
&nbsp; return keys.map(key =&gt; key + &#39;!&#39;);
}
t = 100
calls = [
&nbsp;{&quot;key&quot;: &quot;a&quot;, &quot;time&quot;: 10},
&nbsp;{&quot;key&quot;: &quot;b&quot;, &quot;time&quot;: 20},
&nbsp;{&quot;key&quot;: &quot;c&quot;, &quot;time&quot;: 30}
]
<strong>Đầu ra:</strong> [
&nbsp;{&quot;resolved&quot;: &quot;a!&quot;, &quot;time&quot;: 10},
&nbsp;{&quot;resolved&quot;: &quot;b!&quot;, &quot;time&quot;: 110},
&nbsp;{&quot;resolved&quot;: &quot;c!&quot;, &quot;time&quot;: 110}
]
<strong>Giải thích:</strong>
const batcher = new QueryBatcher(queryMultiple, 100);
setTimeout(() =&gt; batcher.getValue(&#39;a&#39;), 10); // &quot;a!&quot; at t=10ms
setTimeout(() =&gt; batcher.getValue(&#39;b&#39;), 20); // &quot;b!&quot; at t=110ms
setTimeout(() =&gt; batcher.getValue(&#39;c&#39;), 30); // &quot;c!&quot; at t=110ms

queryMultiple chỉ thêm &quot;!&quot; vào key
Tại t=10ms, getValue(&#39;a&#39;) được gọi, queryMultiple([&#39;a&#39;]) được gọi ngay lập tức và kết quả được trả về ngay.
Tại t=20ms, getValue(&#39;b&#39;) được gọi nhưng query được đưa vào hàng đợi
Tại t=30ms, getValue(&#39;c&#39;) được gọi nhưng query được đưa vào hàng đợi.
Tại t=110ms, queryMultiple([&#39;a&#39;, &#39;b&#39;]) được gọi và kết quả được trả về ngay.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
queryMultiple = async function(keys) {
&nbsp; await new Promise(res =&gt; setTimeout(res, 100));
&nbsp; return keys.map(key =&gt; key + &#39;!&#39;);
}
t = 100
calls = [
&nbsp;{&quot;key&quot;: &quot;a&quot;, &quot;time&quot;: 10},
&nbsp;{&quot;key&quot;: &quot;b&quot;, &quot;time&quot;: 20},
&nbsp;{&quot;key&quot;: &quot;c&quot;, &quot;time&quot;: 30}
]
<strong>Đầu ra:</strong> [
&nbsp; {&quot;resolved&quot;: &quot;a!&quot;, &quot;time&quot;: 110},
&nbsp; {&quot;resolved&quot;: &quot;b!&quot;, &quot;time&quot;: 210},
&nbsp; {&quot;resolved&quot;: &quot;c!&quot;, &quot;time&quot;: 210}
]
<strong>Giải thích:</strong>
Ví dụ này giống ví dụ 1, ngoại trừ việc có độ trễ 100ms trong queryMultiple. Kết quả giống nhau, chỉ khác là các promise resolve muộn hơn 100ms.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
queryMultiple = async function(keys) {
&nbsp; await new Promise(res =&gt; setTimeout(res, keys.length * 100));
&nbsp; return keys.map(key =&gt; key + &#39;!&#39;);
}
t = 100
calls = [
&nbsp; {&quot;key&quot;: &quot;a&quot;, &quot;time&quot;: 10},
  {&quot;key&quot;: &quot;b&quot;, &quot;time&quot;: 20},
&nbsp; {&quot;key&quot;: &quot;c&quot;, &quot;time&quot;: 30},
  {&quot;key&quot;: &quot;d&quot;, &quot;time&quot;: 40},
&nbsp; {&quot;key&quot;: &quot;e&quot;, &quot;time&quot;: 250}
&nbsp; {&quot;key&quot;: &quot;f&quot;, &quot;time&quot;: 300}
]
<strong>Đầu ra:</strong> [
&nbsp; {&quot;resolved&quot;:&quot;a!&quot;,&quot;time&quot;:110},
&nbsp; {&quot;resolved&quot;:&quot;e!&quot;,&quot;time&quot;:350},
&nbsp; {&quot;resolved&quot;:&quot;b!&quot;,&quot;time&quot;:410},
&nbsp; {&quot;resolved&quot;:&quot;c!&quot;,&quot;time&quot;:410},
&nbsp; {&quot;resolved&quot;:&quot;d!&quot;,&quot;time&quot;:410},
  {&quot;resolved&quot;:&quot;f!&quot;,&quot;time&quot;:450}
]
<strong>Giải thích:
</strong>queryMultiple([&#39;a&#39;]) được gọi tại t=10ms và resolve tại t=110ms
queryMultiple([&#39;b&#39;, &#39;c&#39;, &#39;d&#39;]) được gọi tại t=110ms và resolve tại t=410ms
queryMultiple([&#39;e&#39;]) được gọi tại t=250ms và resolve tại t=350ms
queryMultiple([&#39;f&#39;]) được gọi tại t=350ms và resolve tại t=450ms
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= t &lt;= 1000</code></li>
	<li><code>0 &lt;= calls.length &lt;= 10</code></li>
	<li><code>1 &lt;= key.length&nbsp;&lt;= 100</code></li>
	<li>Tất cả key đều là duy nhất</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các lời gọi đến $getValue$ trong cùng một cửa sổ throttle $t$ phải dùng chung một $queryMultiple$, còn lời gọi đầu tiên được gửi đi ngay lập tức. Gửi một request cho mỗi key là đúng nhưng lãng phí; nếu không có throttle, các key đồng thời sẽ tạo ra nhiều round trip.
>
> Hãy ghi nhớ thời điểm flush gần nhất. Nếu đã trôi qua ít nhất $t$, gửi key hiện tại ngay lập tức; nếu không, đưa key và $resolve$ của nó vào hàng đợi, rồi gửi toàn bộ batch khi cửa sổ kết thúc. Mỗi Promise được fulfill bằng value ở chỉ số tương ứng.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
