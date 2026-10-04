---
comments: true
difficulty: Hard
tags:
    - JavaScript
---

<!-- problem:start -->

# [2630. Memoize II](https://leetcode.com/problems/memoize-ii)

[中文文档](/solution/2600-2699/2630.Memoize%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm <code>fn</code>, hãy trả về phiên bản <strong>memoized</strong> của hàm đó.</p>

<p>Hàm <strong>memoized</strong> là một hàm sẽ không bao giờ được gọi hai lần với cùng một đầu vào. Thay vào đó, hàm sẽ trả về giá trị đã lưu trong cache.</p>

<p><code>fn</code> có thể là bất kỳ hàm nào và không có giới hạn về kiểu giá trị mà hàm đó nhận vào. Hai đầu vào được xem là giống nhau nếu chúng <code>===</code> nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
getInputs = () =&gt; [[2,2],[2,2],[1,2]]
fn = function (a, b) { return a + b; }
<strong>Đầu ra:</strong> [{&quot;val&quot;:4,&quot;calls&quot;:1},{&quot;val&quot;:4,&quot;calls&quot;:1},{&quot;val&quot;:3,&quot;calls&quot;:2}]
<strong>Giải thích:</strong>
const inputs = getInputs();
const memoized = memoize(fn);
for (const arr of inputs) {
  memoized(...arr);
}

Với đầu vào (2, 2): 2 + 2 = 4 và cần gọi fn() một lần.
Với đầu vào (2, 2): 2 + 2 = 4, nhưng vì đầu vào này đã xuất hiện trước đó nên không cần gọi fn().
Với đầu vào (1, 2): 1 + 2 = 3 và cần gọi fn() thêm một lần, tổng cộng là 2 lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
getInputs = () =&gt; [[{},{}],[{},{}],[{},{}]]
fn = function (a, b) { return ({...a, ...b}); }
<strong>Đầu ra:</strong> [{&quot;val&quot;:{},&quot;calls&quot;:1},{&quot;val&quot;:{},&quot;calls&quot;:2},{&quot;val&quot;:{},&quot;calls&quot;:3}]
<strong>Giải thích:</strong>
Việc gộp hai object rỗng luôn cho ra một object rỗng. Có thể bạn nghĩ rằng chỉ cần 1 lần gọi fn() vì có cache hit, nhưng không object nào trong số đó === với object khác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
getInputs = () =&gt; { const o = {}; return [[o,o],[o,o],[o,o]]; }
fn = function (a, b) { return ({...a, ...b}); }
<strong>Đầu ra:</strong> [{&quot;val&quot;:{},&quot;calls&quot;:1},{&quot;val&quot;:{},&quot;calls&quot;:1},{&quot;val&quot;:{},&quot;calls&quot;:1}]
<strong>Giải thích:</strong>
Việc gộp hai object rỗng luôn cho ra một object rỗng. Lần gọi hàm thứ hai và thứ ba đều là cache hit. Đó là vì mọi object được truyền vào đều giống hệt nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= inputs.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= inputs.flat().length &lt;= 10<sup>5</sup></code></li>
	<li><code>inputs[i][j] != NaN</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các đối số có thể là những object bất kỳ và phải được phân biệt bằng reference. `JSON.stringify` sẽ gộp các object khác nhau nhưng có cùng nội dung.
>
> Gán cho mỗi giá trị đã gặp một id tăng dần rồi nối các id đó thành cache key, để phép so sánh dựa trên identity thay vì cấu trúc.
>
> Một map lưu quan hệ từ value đến id, map còn lại lưu kết quả theo id-string.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type Fn = (...params: any) => any;

function memoize(fn: Fn): Fn {
    const idxMap: Map<string, number> = new Map();
    const cache: Map<string, any> = new Map();

    const getIdx = (obj: any): number => {
        if (!idxMap.has(obj)) {
            idxMap.set(obj, idxMap.size);
        }
        return idxMap.get(obj)!;
    };

    return function (...params: any) {
        const key = params.map(getIdx).join(',');
        if (!cache.has(key)) {
            cache.set(key, fn(...params));
        }
        return cache.get(key)!;
    };
}

/**
 * let callCount = 0;
 * const memoizedFn = memoize(function (a, b) {
 *	 callCount += 1;
 *   return a + b;
 * })
 * memoizedFn(2, 3) // 5
 * memoizedFn(2, 3) // 5
 * console.log(callCount) // 1
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
