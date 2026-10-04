---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2622. Cache With Time Limit](https://leetcode.com/problems/cache-with-time-limit)

[中文文档](/solution/2600-2699/2622.Cache%20With%20Time%20Limit/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một class cho phép lấy và thiết lập các cặp key-value, trong đó mỗi key đi kèm với một <strong>thời gian đến khi hết hạn</strong>.</p>

<p>Class có ba phương thức public:</p>

<p><code>set(key, value, duration)</code>: nhận một <code>key</code> kiểu số nguyên, một <code>value</code> kiểu số nguyên và một <code>duration</code> tính bằng mili giây. Sau khi <code>duration</code> trôi qua, không thể truy cập key đó nữa. Phương thức trả về <code>true</code> nếu key đó đã tồn tại và chưa hết hạn, và <code>false</code> trong các trường hợp còn lại. Nếu key đã tồn tại, cả value và duration đều phải được ghi đè.</p>

<p><code>get(key)</code>: nếu tồn tại một key chưa hết hạn, trả về value tương ứng. Nếu không, trả về <code>-1</code>.</p>

<p><code>count()</code>: trả về số lượng key chưa hết hạn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;TimeLimitedCache&quot;, &quot;set&quot;, &quot;get&quot;, &quot;count&quot;, &quot;get&quot;]
values = [[], [1, 42, 100], [1], [], [1]]
timeDelays = [0, 0, 50, 50, 150]
<strong>Đầu ra:</strong> [null, false, 42, 1, -1]
<strong>Giải thích:</strong>
Tại t=0, cache được khởi tạo.
Tại t=0, một cặp key-value (1: 42) được thêm vào với giới hạn thời gian là 100ms. Value chưa tồn tại nên trả về false.
Tại t=50, yêu cầu key=1 được gửi và value 42 được trả về.
Tại t=50, count() được gọi và cache có một key đang hoạt động.
Tại t=100, key=1 hết hạn.
Tại t=150, get(1) được gọi nhưng trả về -1 vì cache trống.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;TimeLimitedCache&quot;, &quot;set&quot;, &quot;set&quot;, &quot;get&quot;, &quot;get&quot;, &quot;get&quot;, &quot;count&quot;]
values = [[], [1, 42, 50], [1, 50, 100], [1], [1], [1], []]
timeDelays = [0, 0, 40, 50, 120, 200, 250]
<strong>Đầu ra:</strong> [null, false, true, 50, 50, -1, 0]
<strong>Giải thích:</strong>
Tại t=0, cache được khởi tạo.
Tại t=0, một cặp key-value (1: 42) được thêm vào với giới hạn thời gian là 50ms. Value chưa tồn tại nên trả về false.
Tại t=40, một cặp key-value (1: 50) được thêm vào với giới hạn thời gian là 100ms. Một value chưa hết hạn đã tồn tại nên trả về true và value cũ bị ghi đè.
Tại t=50, get(1) được gọi và trả về 50.
Tại t=120, get(1) được gọi và trả về 50.
Tại t=140, key=1 hết hạn.
Tại t=200, get(1) được gọi nhưng cache trống nên trả về -1.
Tại t=250, count() trả về 0 vì cache trống.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= key, value &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= duration &lt;= 1000</code></li>
	<li><code>1 &lt;= actions.length &lt;= 100</code></li>
	<li><code>actions.length === values.length</code></li>
	<li><code>actions.length === timeDelays.length</code></li>
	<li><code>0 &lt;= timeDelays[i] &lt;= 1450</code></li>
	<li><code>actions[i]</code>&nbsp;là một trong các giá trị &quot;TimeLimitedCache&quot;, &quot;set&quot;, &quot;get&quot; và&nbsp;&quot;count&quot;</li>
	<li>Hành động đầu tiên luôn là &quot;TimeLimitedCache&quot; và phải được thực thi ngay lập tức, với độ trễ 0 mili giây</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các entry sẽ hết hạn; thao tác đọc và đếm phải bỏ qua các key đã cũ. Với kích thước này, việc quét toàn bộ map trong mỗi lần gọi là chấp nhận được, nhưng việc kiểm tra hết hạn chỉ đơn giản là so sánh timestamp.
>
> Lưu $[value, expire]$ và so sánh với `Date.now()` khi truy cập. `set` ghi đè key, cập nhật thời hạn và cho biết key đó đã tồn tại hay chưa.
>
> `count` lọc các entry vẫn còn hiệu lực; không cần chủ động xóa các entry hết hạn.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
class TimeLimitedCache {
    #cache: Map<number, [value: number, expire: number]> = new Map();

    set(key: number, value: number, duration: number): boolean {
        const isExist = this.#cache.has(key);

        if (!this.#isExpired(key)) {
            this.#cache.set(key, [value, Date.now() + duration]);
        }

        return isExist;
    }

    get(key: number): number {
        if (this.#isExpired(key)) return -1;
        const res = this.#cache.get(key)?.[0] ?? -1;
        return res;
    }

    count(): number {
        const xs = Array.from(this.#cache).filter(([key]) => !this.#isExpired(key));
        return xs.length;
    }

    #isExpired = (key: number) =>
        this.#cache.has(key) &&
        (this.#cache.get(key)?.[1] ?? Number.NEGATIVE_INFINITY) < Date.now();
}

/**
 * Your TimeLimitedCache object will be instantiated and called as such:
 * var obj = new TimeLimitedCache()
 * obj.set(1, 42, 1000); // false
 * obj.get(1) // 42
 * obj.count() // 1
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
