---
comments: true
difficulty: Hard
tags:
    - JavaScript
---

<!-- problem:start -->

# [2691. Immutability Helper 🔒](https://leetcode.com/problems/immutability-helper)

[中文文档](/solution/2600-2699/2691.Immutability%20Helper/README.md)

## Mô tả

<!-- description:start -->

<p>Việc tạo bản sao của các object bất biến với một vài thay đổi nhỏ có thể khá mất thời gian. Hãy viết một class&nbsp;<code>ImmutableHelper</code>&nbsp;để hỗ trợ yêu cầu này. Constructor nhận một object bất biến&nbsp;<code>obj</code>&nbsp;là một JSON object hoặc mảng.</p>

<p>Class này có một method duy nhất là&nbsp;<code>produce</code>, nhận một&nbsp;function&nbsp;<code>mutator</code>. Function này trả về một object mới tương tự object ban đầu, nhưng đã áp dụng các mutation đó.</p>

<p><code>mutator</code>&nbsp;nhận một phiên bản của&nbsp;<code>obj</code>&nbsp;được bọc bằng <strong>proxy</strong>. Người dùng có thể (trông như) thay đổi object này, nhưng object gốc&nbsp;<code>obj</code>&nbsp;thực tế không được thay đổi.</p>

<p>Ví dụ, người dùng có thể viết như sau:</p>

<pre>
const originalObj = {&quot;x&quot;: 5};
const helper = new ImmutableHelper(originalObj);
const newObj = helper.produce((proxy) =&gt; {
  proxy.x = proxy.x + 1;
});
console.log(originalObj); // {&quot;x&quot;: 5}
console.log(newObj); // {&quot;x&quot;: 6}</pre>

<p>Các tính chất của function&nbsp;<code>mutator</code>:</p>

<ul>
	<li>Luôn trả về <code>undefined</code>.</li>
	<li>Không bao giờ truy cập các key không tồn tại.</li>
	<li>Không bao giờ xóa key (<code>delete obj.key</code>).</li>
	<li>Không bao giờ gọi method trên một object được bọc bằng proxy (<code>push</code>, <code>shift</code>, v.v.).</li>
	<li>Không bao giờ gán key thành object (<code>proxy.x = {}</code>).</li>
</ul>

<p><strong>Lưu ý về cách kiểm thử lời giải:</strong>&nbsp;bộ kiểm tra lời giải chỉ phân tích sự khác biệt giữa giá trị được trả về và object gốc&nbsp;<code>obj</code>. Việc so sánh toàn bộ sẽ tốn quá nhiều tài nguyên tính toán. Ngoài ra, mọi thay đổi đối với object gốc đều dẫn đến đáp án sai.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = {&quot;val&quot;: 10},
mutators = [
&nbsp; proxy =&gt; { proxy.val += 1; },
&nbsp; proxy =&gt; { proxy.val -= 1; }
]
<strong>Đầu ra:</strong>
[
  {&quot;val&quot;: 11},
&nbsp; {&quot;val&quot;: 9}
]
<strong>Giải thích:</strong>
const helper = new ImmutableHelper({val: 10});
helper.produce(proxy =&gt; { proxy.val += 1; }); // { &quot;val&quot;: 11 }
helper.produce(proxy =&gt; { proxy.val -= 1; }); // { &quot;val&quot;: 9 }
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = {&quot;arr&quot;: [1, 2, 3]}
mutators = [
&nbsp;proxy =&gt; {
&nbsp;  proxy.arr[0] = 5;
&nbsp;  proxy.newVal = proxy.arr[0] + proxy.arr[1];
  }
]
<strong>Đầu ra:</strong>
[
  {&quot;arr&quot;: [5, 2, 3], &quot;newVal&quot;: 7 }
]
<strong>Giải thích: </strong>Hai thay đổi đã được thực hiện trên mảng ban đầu. Phần tử đầu tiên của mảng được đặt thành 5. Sau đó, một key mới được thêm vào với giá trị 7.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = {&quot;obj&quot;: {&quot;val&quot;: {&quot;x&quot;: 10, &quot;y&quot;: 20}}}
mutators = [
&nbsp; proxy =&gt; {
&nbsp;   let data = proxy.obj.val;
&nbsp;   let temp = data.x;
&nbsp;   data.x = data.y;
&nbsp;   data.y = temp;
&nbsp; }
]
<strong>Đầu ra:</strong>
[
  {&quot;obj&quot;: {&quot;val&quot;: {&quot;x&quot;: 20, &quot;y&quot;: 10}}}
]
<strong>Giải thích:</strong> Giá trị của &quot;x&quot; và &quot;y&quot; đã được hoán đổi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= JSON.stringify(obj).length &lt;= 4 * 10<sup>5</sup></code></li>
	<li><code>mutators</code> là một mảng các function</li>
	<li><code><font face="monospace">total calls to produce() &lt; 10<sup>5</sup></font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các mutation cần trông như được thực hiện trực tiếp trên object nhưng vẫn phải tạo ra một object mới mà không chạm vào object gốc. Deep-clone toàn bộ cây ở mỗi lần `produce` sẽ lãng phí các nhánh không thay đổi khi có tới $10^5$ lần gọi và dữ liệu lớn.
>
> Proxy ghi nhận các path được ghi và chỉ copy-on-write các nhánh trên đường dẫn đó, đồng thời dùng chung các subtree không bị chạm tới. mutator không xóa key, gọi method hay gán object, nên proxy chỉ cần xử lý các thao tác đọc và ghi.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
