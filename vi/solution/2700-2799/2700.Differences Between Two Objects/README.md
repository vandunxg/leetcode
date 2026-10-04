---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2700. Differences Between Two Objects 🔒](https://leetcode.com/problems/differences-between-two-objects)

[中文文档](/solution/2700-2799/2700.Differences%20Between%20Two%20Objects/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm nhận vào hai object hoặc mảng lồng nhau sâu <code>obj1</code> và <code>obj2</code>, sau đó trả về một object mới biểu diễn những điểm khác nhau giữa chúng.</p>

<p>Hàm cần so sánh các thuộc tính của hai object và xác định mọi thay đổi. object được trả về chỉ chứa những key có giá trị khác nhau từ <code>obj1</code> sang <code>obj2</code>.</p>

<p>Với mỗi key đã thay đổi, giá trị cần được biểu diễn dưới dạng một mảng <code>[obj1 value, obj2&nbsp;value]</code>. Những key chỉ tồn tại trong một object mà không tồn tại trong object còn lại không được đưa vào object kết quả. Kết quả cuối cùng là một object lồng nhau sâu, trong đó mỗi giá trị lá là một mảng biểu diễn sự khác biệt.</p>

<p>Khi so sánh hai mảng, các chỉ số của mảng được xem là key của chúng.</p>

<p>Có thể giả sử rằng cả hai object đều là kết quả đầu ra của <code>JSON.parse</code>.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj1 = {}
obj2 = {
&nbsp; &quot;a&quot;: 1,
  &quot;b&quot;: 2
}
<strong>Đầu ra:</strong> {}
<strong>Giải thích:</strong> Không có thay đổi nào được thực hiện trên obj1. Các key mới &quot;a&quot; và &quot;b&quot; xuất hiện trong obj2, nhưng những key được thêm vào hoặc bị xóa sẽ bị bỏ qua.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj1 = {
&nbsp; &quot;a&quot;: 1,
&nbsp; &quot;v&quot;: 3,
&nbsp; &quot;x&quot;: [],
&nbsp; &quot;z&quot;: {
&nbsp; &nbsp; &quot;a&quot;: null
&nbsp; }
}
obj2 = {
&nbsp; &quot;a&quot;: 2,
&nbsp; &quot;v&quot;: 4,
&nbsp; &quot;x&quot;: [],
&nbsp; &quot;z&quot;: {
&nbsp; &nbsp; &quot;a&quot;: 2
&nbsp; }
}
<strong>Đầu ra:</strong>
{
&nbsp; &quot;a&quot;: [1, 2],
  &quot;v&quot;: [3, 4],
&nbsp; &quot;z&quot;: {
&nbsp;   &quot;a&quot;: [null, 2]
&nbsp; }
}
<strong>Giải thích:</strong> Các key &quot;a&quot;, &quot;v&quot; và &quot;z&quot; đều đã có thay đổi. &quot;a&quot; được thay đổi từ 1 thành 2. &quot;v&quot; được thay đổi từ 3 thành 4. &quot;z&quot; có một thay đổi ở object con. &quot;z.a&quot; được thay đổi từ null thành 2.
</pre>

<p><strong>Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj1 = {
&nbsp; &quot;a&quot;: 5,
&nbsp; &quot;v&quot;: 6,
&nbsp; &quot;z&quot;: [1, 2, 4, [2, 5, 7]]
}
obj2 = {
&nbsp; &quot;a&quot;: 5,
&nbsp; &quot;v&quot;: 7,
&nbsp; &quot;z&quot;: [1, 2, 3, [1]]
}
<strong>Đầu ra:</strong>
{
&nbsp; &quot;v&quot;: [6, 7],
&nbsp; &quot;z&quot;: {
&nbsp;   &quot;2&quot;: [4, 3],
&nbsp;   &quot;3&quot;: {
&nbsp;     &quot;0&quot;: [2, 1]
&nbsp;   }
&nbsp; }
}
<strong>Giải thích:</strong> Trong obj1 và obj2, các key &quot;v&quot; và &quot;z&quot; có giá trị được gán khác nhau. &quot;a&quot; bị bỏ qua vì giá trị không thay đổi. Trong key &quot;z&quot; có một mảng lồng nhau. Mảng được xem như object, trong đó các chỉ số là key. Có hai thay đổi trong mảng: z[2] và z[3][0]. z[0] và z[1] không thay đổi nên không được đưa vào. z[3][1] và z[3][2] đã bị xóa nên cũng không được đưa vào.
</pre>

<p><strong>Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj1 = {
&nbsp; &quot;a&quot;: {&quot;b&quot;: 1},
}
obj2 = {
&nbsp; &quot;a&quot;: [5],
}
<strong>Đầu ra:</strong>
{
  &quot;a&quot;: [{&quot;b&quot;: 1}, [5]]
}
<strong>Giải thích:</strong> Key &quot;a&quot; tồn tại trong cả hai object. Vì hai giá trị tương ứng có kiểu khác nhau, chúng được đặt trong mảng biểu diễn sự khác biệt.</pre>

<p><strong>Ví dụ 5:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj1 = {
&nbsp; &quot;a&quot;: [1, 2, {}],
&nbsp; &quot;b&quot;: false
}
obj2 = { &nbsp;
&nbsp; &quot;b&quot;: false,
&nbsp; &quot;a&quot;: [1, 2, {}]
}
<strong>Đầu ra:</strong>
{}
<strong>Giải thích:</strong> Ngoài thứ tự các key khác nhau, hai object giống hệt nhau nên một object rỗng được trả về.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>obj1</code> và <code>obj2</code> là các object hoặc mảng JSON hợp lệ</li>
	<li><code>2 &lt;= JSON.stringify(obj1).length &lt;= 10<sup>4</sup></code></li>
	<li><code>2 &lt;= JSON.stringify(obj2).length &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> So sánh từng key của hai giá trị JSON sau khi lấy hợp các key là khả thi với giới hạn độ dài chuỗi tuần tự hóa là $10^4$, nhưng những key chỉ xuất hiện ở một phía phải bị loại bỏ, còn trường hợp khác kiểu phải trở thành một cặp ở nút lá thay vì tiếp tục duyệt lồng nhau.
>
> Vì vậy, chúng ta chỉ đệ quy trên các key chung. Các giá trị vô hướng được so sánh trực tiếp; object và mảng tạo ra một diff lồng nhau, chỉ được ghi lại khi diff đó không rỗng. Một tag kiểu dữ liệu nội bộ giúp phân biệt mảng với object thông thường.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function objDiff(obj1: any, obj2: any): any {
    if (type(obj1) !== type(obj2)) return [obj1, obj2];
    if (!isObject(obj1)) return obj1 === obj2 ? {} : [obj1, obj2];
    const diff: Record<string, unknown> = {};
    const sameKeys = Object.keys(obj1).filter(key => key in obj2);
    sameKeys.forEach(key => {
        const subDiff = objDiff(obj1[key], obj2[key]);
        if (Object.keys(subDiff).length) diff[key] = subDiff;
    });
    return diff;
}

function type(obj: unknown): string {
    return Object.prototype.toString.call(obj).slice(8, -1);
}

function isObject(obj: unknown): obj is Record<string, unknown> {
    return typeof obj === 'object' && obj !== null;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
