---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2694. Event Emitter](https://leetcode.com/problems/event-emitter)

[Tài liệu tiếng Trung](/solution/2600-2699/2694.Event%20Emitter/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một class <code>EventEmitter</code>. Giao diện này tương tự (nhưng có một số khác biệt) so với giao diện được tìm thấy trong Node.js hoặc Event Target của DOM. <code>EventEmitter</code> phải cho phép đăng ký và phát các sự kiện.</p>

<p>Class <code>EventEmitter</code> của bạn cần có hai phương thức sau:</p>

<ul>
	<li><strong>subscribe</strong> - Phương thức này nhận hai đối số: tên của một event dưới dạng chuỗi và một hàm callback. Hàm callback này sẽ được gọi khi event được phát sau đó.<br />
	Một event có thể có nhiều listener cho cùng một event. Khi phát một event có nhiều callback, mỗi callback phải được gọi theo thứ tự mà chúng được đăng ký. Cần trả về một mảng kết quả. Bạn có thể giả sử rằng không có callback nào được truyền vào <code>subscribe</code> có cùng tham chiếu.<br />
	Phương thức <code>subscribe</code> cũng phải trả về một object có phương thức <code>unsubscribe</code> để người dùng hủy đăng ký. Khi được gọi, callback phải được xóa khỏi danh sách đăng ký và phải trả về <code>undefined</code>.</li>
	<li><strong>emit</strong> - Phương thức này nhận hai đối số: tên của một event dưới dạng chuỗi và một mảng đối số tùy chọn sẽ được truyền vào callback. Nếu không có callback nào được đăng ký cho event đã cho, hãy trả về một mảng rỗng. Nếu không, hãy trả về một mảng chứa kết quả của tất cả các lần gọi callback theo thứ tự chúng được đăng ký.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;EventEmitter&quot;, &quot;emit&quot;, &quot;subscribe&quot;, &quot;subscribe&quot;, &quot;emit&quot;],
values = [[], [&quot;firstEvent&quot;], [&quot;firstEvent&quot;, &quot;function cb1() { return 5; }&quot;],&nbsp; [&quot;firstEvent&quot;, &quot;function cb1() { return 6; }&quot;], [&quot;firstEvent&quot;]]
<strong>Đầu ra:</strong> [[],[&quot;emitted&quot;,[]],[&quot;subscribed&quot;],[&quot;subscribed&quot;],[&quot;emitted&quot;,[5,6]]]
<strong>Giải thích:</strong>
const emitter = new EventEmitter();
emitter.emit(&quot;firstEvent&quot;); // [], no callback are subscribed yet
emitter.subscribe(&quot;firstEvent&quot;, function cb1() { return 5; });
emitter.subscribe(&quot;firstEvent&quot;, function cb2() { return 6; });
emitter.emit(&quot;firstEvent&quot;); // [5, 6], returns the output of cb1 and cb2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;EventEmitter&quot;, &quot;subscribe&quot;, &quot;emit&quot;, &quot;emit&quot;],
values = [[], [&quot;firstEvent&quot;, &quot;function cb1(...args) { return args.join(&#39;,&#39;); }&quot;], [&quot;firstEvent&quot;, [1,2,3]], [&quot;firstEvent&quot;, [3,4,6]]]
<strong>Đầu ra:</strong> [[],[&quot;subscribed&quot;],[&quot;emitted&quot;,[&quot;1,2,3&quot;]],[&quot;emitted&quot;,[&quot;3,4,6&quot;]]]
<strong>Giải thích: </strong>Lưu ý rằng phương thức emit phải có khả năng nhận một mảng đối số TÙY CHỌN.

const emitter = new EventEmitter();
emitter.subscribe(&quot;firstEvent, function cb1(...args) { return args.join(&#39;,&#39;); });
emitter.emit(&quot;firstEvent&quot;, [1, 2, 3]); // [&quot;1,2,3&quot;]
emitter.emit(&quot;firstEvent&quot;, [3, 4, 6]); // [&quot;3,4,6&quot;]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;EventEmitter&quot;, &quot;subscribe&quot;, &quot;emit&quot;, &quot;unsubscribe&quot;, &quot;emit&quot;],
values = [[], [&quot;firstEvent&quot;, &quot;(...args) =&gt; args.join(&#39;,&#39;)&quot;], [&quot;firstEvent&quot;, [1,2,3]], [0], [&quot;firstEvent&quot;, [4,5,6]]]
<strong>Đầu ra:</strong> [[],[&quot;subscribed&quot;],[&quot;emitted&quot;,[&quot;1,2,3&quot;]],[&quot;unsubscribed&quot;,0],[&quot;emitted&quot;,[]]]
<strong>Giải thích:</strong>
const emitter = new EventEmitter();
const sub = emitter.subscribe(&quot;firstEvent&quot;, (...args) =&gt; args.join(&#39;,&#39;));
emitter.emit(&quot;firstEvent&quot;, [1, 2, 3]); // [&quot;1,2,3&quot;]
sub.unsubscribe(); // undefined
emitter.emit(&quot;firstEvent&quot;, [4, 5, 6]); // [], there are no subscriptions
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;EventEmitter&quot;, &quot;subscribe&quot;, &quot;subscribe&quot;, &quot;unsubscribe&quot;, &quot;emit&quot;],
values = [[], [&quot;firstEvent&quot;, &quot;x =&gt; x + 1&quot;], [&quot;firstEvent&quot;, &quot;x =&gt; x + 2&quot;], [0], [&quot;firstEvent&quot;, [5]]]
<strong>Đầu ra:</strong> [[],[&quot;subscribed&quot;],[&quot;subscribed&quot;],[&quot;unsubscribed&quot;,0],[&quot;emitted&quot;,[7]]]
<strong>Giải thích:</strong>
const emitter = new EventEmitter();
const sub1 = emitter.subscribe(&quot;firstEvent&quot;, x =&gt; x + 1);
const sub2 = emitter.subscribe(&quot;firstEvent&quot;, x =&gt; x + 2);
sub1.unsubscribe(); // undefined
emitter.emit(&quot;firstEvent&quot;, [5]); // [7]</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= actions.length &lt;= 10</code></li>
	<li><code>values.length === actions.length</code></li>
	<li>Tất cả test case đều hợp lệ, ví dụ: bạn không cần xử lý trường hợp hủy đăng ký một subscription không tồn tại.</li>
	<li>Chỉ có 4 action khác nhau: <code>EventEmitter</code>, <code>emit</code>, <code>subscribe</code> và <code>unsubscribe</code>.</li>
	<li>Action <code>EventEmitter</code> không nhận đối số nào.</li>
	<li>Action <code>emit</code> nhận từ 1 đến 2 đối số. Đối số thứ nhất là tên của event mà chúng ta muốn phát, và đối số thứ hai được truyền vào các hàm callback.</li>
	<li>Action <code>subscribe</code> nhận 2 đối số, trong đó đối số đầu tiên là tên event và đối số thứ hai là hàm callback.</li>
	<li>Action <code>unsubscribe</code> nhận một đối số, là thứ tự bắt đầu từ 0 của subscription được tạo trước đó.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một event có thể có nhiều callback và thao tác unsubscribe phải chỉ xóa chính callback đó. Nếu xóa tuyến tính khỏi một array, ta cần tìm chỉ số. `Map<string, Set<Callback>>` cho phép thêm khi subscribe, xóa khi unsubscribe, còn `emit` gọi các phần tử trong set theo thứ tự chèn và thu thập các giá trị trả về.
>
> Nếu event không tồn tại, trả về một mảng rỗng.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type Callback = (...args: any[]) => any;
type Subscription = {
    unsubscribe: () => void;
};

class EventEmitter {
    private d: Map<string, Set<Callback>> = new Map();

    subscribe(eventName: string, callback: Callback): Subscription {
        this.d.set(eventName, (this.d.get(eventName) || new Set()).add(callback));
        return {
            unsubscribe: () => {
                this.d.get(eventName)?.delete(callback);
            },
        };
    }

    emit(eventName: string, args: any[] = []): any {
        const callbacks = this.d.get(eventName);
        if (!callbacks) {
            return [];
        }
        return [...callbacks].map(callback => callback(...args));
    }
}

/**
 * const emitter = new EventEmitter();
 *
 * // Subscribe to the onClick event with onClickCallback
 * function onClickCallback() { return 99 }
 * const sub = emitter.subscribe('onClick', onClickCallback);
 *
 * emitter.emit('onClick'); // [99]
 * sub.unsubscribe(); // undefined
 * emitter.emit('onClick'); // []
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
