---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2757. Generate Circular Array Values 🔒](https://leetcode.com/problems/generate-circular-array-values)

[中文文档](/solution/2700-2799/2757.Generate%20Circular%20Array%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <strong>vòng</strong> <code>arr</code> và một số nguyên&nbsp;<code>startIndex</code>, hãy trả về một đối tượng generator&nbsp;<code>gen</code> sinh ra các giá trị từ <code>arr</code>.</p>

<p>Lần đầu tiên gọi <code>gen.next()</code> trên generator, nó phải sinh ra&nbsp;<code>arr[startIndex]</code>.</p>

<p>Mỗi lần gọi <code>gen.next()</code>&nbsp;sau đó, một số nguyên <code>jump</code>&nbsp;sẽ được truyền vào hàm (Ví dụ: <code>gen.next(-3)</code>).</p>

<ul>
	<li>Nếu&nbsp;<code>jump</code>&nbsp;là số dương, chỉ số sẽ tăng thêm giá trị đó. Tuy nhiên, nếu chỉ số hiện tại là chỉ số cuối cùng, nó sẽ chuyển sang chỉ số đầu tiên.</li>
	<li>Nếu&nbsp;<code>jump</code>&nbsp;là số âm, chỉ số sẽ giảm đi độ lớn của giá trị đó. Tuy nhiên, nếu chỉ số hiện tại là chỉ số đầu tiên, nó sẽ chuyển sang chỉ số cuối cùng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3,4,5], steps = [1,2,6], startIndex = 0
<strong>Đầu ra:</strong> [1,2,4,5]
<strong>Giải thích:</strong> &nbsp;
&nbsp;const gen = cycleGenerator(arr, startIndex);
&nbsp;gen.next().value; &nbsp;// 1, index = startIndex = 0
&nbsp;gen.next(1).value; // 2, index = 1, 0 -&gt; 1
&nbsp;gen.next(2).value; // 4, index = 3, 1 -&gt; 2 -&gt; 3
&nbsp;gen.next(6).value; // 5, index = 4, 3 -&gt; 4 -&gt; 0 -&gt; 1 -&gt; 2 -&gt; 3 -&gt; 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [10,11,12,13,14,15], steps = [1,4,0,-1,-3], startIndex = 1
<strong>Đầu ra:</strong> [11,12,10,10,15,12]
<strong>Giải thích:</strong>
&nbsp;const gen = cycleGenerator(arr, startIndex);
&nbsp;gen.next().value; &nbsp; // 11, index = 1
&nbsp;gen.next(1).value;  // 12, index = 2
&nbsp;gen.next(4).value;  // 10, index = 0
&nbsp;gen.next(0).value;  // 10, index = 0
&nbsp;gen.next(-1).value; // 15, index = 5
&nbsp;gen.next(-3).value; // 12, index = 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,4,6,7,8,10], steps = [-4,5,-3,10], startIndex = 3
<strong>Đầu ra:</strong> [7,10,8,4,10]
<strong>Giải thích:</strong> &nbsp;
&nbsp;const gen = cycleGenerator(arr, startIndex);
&nbsp;gen.next().value &nbsp; // 7,  index = 3
&nbsp;gen.next(-4).value // 10, index = 5
&nbsp;gen.next(5).value  // 8,  index = 4
&nbsp;gen.next(-3).value // 4,  index = 1 &nbsp;
&nbsp;gen.next(10).value // 10, index = 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= steps.length &lt;= 100</code></li>
	<li><code>-10<sup>4</sup>&nbsp;&lt;= steps[i],&nbsp;arr[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= startIndex &lt;&nbsp;arr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Generator cần di chuyển trong mảng vòng theo bước nhảy nhận được và sinh ra giá trị hiện tại. Không cần sao chép mảng; chỉ số modulo $n$ là đủ.
>
> $yield$ $arr[startIndex]$, sau đó cộng $jump$ tiếp theo vào chỉ số và đưa kết quả về miền dương theo modulo $n$ để các bước nhảy âm vẫn hợp lệ.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function* cycleGenerator(arr: number[], startIndex: number): Generator<number, void, number> {
    const n = arr.length;
    while (true) {
        const jump = yield arr[startIndex];
        startIndex = (((startIndex + jump) % n) + n) % n;
    }
}
/**
 *  const gen = cycleGenerator([1,2,3,4,5], 0);
 *  gen.next().value  // 1
 *  gen.next(1).value // 2
 *  gen.next(2).value // 4
 *  gen.next(6).value // 5
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
