---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2805. Custom Interval 🔒](https://leetcode.com/problems/custom-interval)

[中文文档](/solution/2800-2899/2805.Custom%20Interval/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Hàm&nbsp;</strong><code>customInterval</code></p>

<p>Cho một hàm <code>fn</code>, một số <code>delay</code> và một số <code>period</code>, hãy trả về một số <code>id</code>.</p>

<p><code>customInterval</code>&nbsp;là một hàm sẽ thực thi hàm được cung cấp <code>fn</code> theo các khoảng thời gian dựa trên một mẫu tuyến tính được xác định bởi công thức <code>delay&nbsp;+ period&nbsp;* count</code>.&nbsp;</p>

<p><code>count</code> trong công thức biểu thị số lần interval đã được thực thi, bắt đầu từ giá trị ban đầu là <code>0</code>.</p>

<p><strong>Hàm </strong><code>customClearInterval</code>&nbsp;</p>

<p>Cho <code>id</code>. <code>id</code>&nbsp;là giá trị được trả về từ hàm <code>customInterval</code>.</p>

<p><code>customClearInterval</code>&nbsp;sẽ dừng việc thực thi hàm được cung cấp <code>fn</code> theo các khoảng thời gian.</p>

<p><strong>Lưu ý:</strong> Trong Node.js, các hàm <code>setTimeout</code> và <code>setInterval</code> trả về một object, không phải một số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> delay = 50, period = 20, cancelTime = 225
<strong>Đầu ra:</strong> [50,120,210]
<strong>Giải thích:</strong>
const t = performance.now()&nbsp;&nbsp;
const result = []
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;
const fn = () =&gt; {
    result.push(Math.floor(performance.now() - t))
}
const id = customInterval(fn, delay, period)

setTimeout(() =&gt; {
    customClearInterval(id)
}, 225)

50 + 20 * 0 = 50 // 50ms - 1st function call
50 + 20&nbsp;* 1 = 70 // 50ms + 70ms = 120ms - 2nd function call
50 + 20 * 2 = 90 // 50ms + 70ms + 90ms = 210ms - 3rd function call
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> delay = 20, period = 20, cancelTime = 150
<strong>Đầu ra:</strong> [20,60,120]
<strong>Giải thích:</strong>
20 + 20 * 0 = 20 // 20ms - 1st function call
20 + 20&nbsp;* 1 = 40 // 20ms + 40ms = 60ms - 2nd function call
20 + 20 * 2 = 60 // 20ms + 40ms + 60ms = 120ms - 3rd function call
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> delay = 100, period = 200, cancelTime = 500
<strong>Đầu ra:</strong> [100,400]
<strong>Giải thích:</strong>
100 + 200 * 0 = 100 // 100ms - 1st function call
100 + 200&nbsp;* 1 = 300 // 100ms + 300ms = 400ms - 2nd function call
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>20 &lt;= delay, period &lt;= 250</code></li>
	<li><code>20 &lt;= cancelTime &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thời gian chờ trước lần thực thi thứ $count$ là $delay+period\cdot count$, vì vậy `setInterval` với chu kỳ cố định không phù hợp. Ta đệ quy lên lịch `setTimeout` với thời gian chờ đó, lưu handle vào một map theo một mã định danh, rồi để `customClearInterval` hủy timeout đang chờ.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
const intervalMap = new Map<number, NodeJS.Timeout>();

function customInterval(fn: Function, delay: number, period: number): number {
    let count = 0;
    function recursiveTimeout() {
        intervalMap.set(
            id,
            setTimeout(
                () => {
                    fn();
                    count++;
                    recursiveTimeout();
                },
                delay + period * count,
            ),
        );
    }

    const id = Date.now();
    recursiveTimeout();
    return id;
}

function customClearInterval(id: number) {
    if (intervalMap.has(id)) {
        clearTimeout(intervalMap.get(id)!);
        intervalMap.delete(id);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
