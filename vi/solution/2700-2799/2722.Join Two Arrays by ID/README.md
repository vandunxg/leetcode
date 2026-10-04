---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2722. Join Two Arrays by ID](https://leetcode.com/problems/join-two-arrays-by-id)

[中文文档](/solution/2700-2799/2722.Join%20Two%20Arrays%20by%20ID/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng <code>arr1</code> và <code>arr2</code>, hãy trả về một mảng mới&nbsp;<code>joinedArray</code>. Tất cả object trong mỗi mảng đầu vào đều chứa trường&nbsp;<code>id</code>&nbsp;có giá trị là một số nguyên.&nbsp;</p>

<p><code>joinedArray</code>&nbsp;là mảng được tạo bằng cách hợp nhất&nbsp;<code>arr1</code> và <code>arr2</code> dựa trên key&nbsp;<code>id</code>&nbsp;của chúng. Độ dài của&nbsp;<code>joinedArray</code>&nbsp;phải bằng số giá trị duy nhất của <code>id</code>. Mảng trả về phải được sắp xếp theo thứ tự <strong>tăng dần</strong> dựa trên key <code>id</code>.</p>

<p>Nếu một&nbsp;<code>id</code>&nbsp;chỉ tồn tại trong một mảng mà không tồn tại trong mảng còn lại, object duy nhất có&nbsp;<code>id</code>&nbsp;đó phải được đưa vào mảng kết quả mà không thay đổi.</p>

<p>Nếu hai object có cùng một <code>id</code>, các thuộc tính của chúng phải được hợp nhất thành một&nbsp;object:</p>

<ul>
	<li>Nếu một key chỉ tồn tại trong một object, cặp key-value duy nhất đó được đưa vào object.</li>
	<li>Nếu một key xuất hiện trong cả hai object, giá trị trong object từ <code>arr2</code>&nbsp;phải ghi đè giá trị từ <code>arr1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr1 = [
&nbsp;   {&quot;id&quot;: 1, &quot;x&quot;: 1},
&nbsp;   {&quot;id&quot;: 2, &quot;x&quot;: 9}
],
arr2 = [
    {&quot;id&quot;: 3, &quot;x&quot;: 5}
]
<strong>Đầu ra:</strong>
[
&nbsp;   {&quot;id&quot;: 1, &quot;x&quot;: 1},
&nbsp;   {&quot;id&quot;: 2, &quot;x&quot;: 9},
    {&quot;id&quot;: 3, &quot;x&quot;: 5}
]
<strong>Giải thích:</strong> Không có id trùng lặp nên arr1 chỉ đơn giản được nối với arr2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr1 = [
    {&quot;id&quot;: 1, &quot;x&quot;: 2, &quot;y&quot;: 3},
    {&quot;id&quot;: 2, &quot;x&quot;: 3, &quot;y&quot;: 6}
],
arr2 = [
    {&quot;id&quot;: 2, &quot;x&quot;: 10, &quot;y&quot;: 20},
    {&quot;id&quot;: 3, &quot;x&quot;: 0, &quot;y&quot;: 0}
]
<strong>Đầu ra:</strong>
[
    {&quot;id&quot;: 1, &quot;x&quot;: 2, &quot;y&quot;: 3},
    {&quot;id&quot;: 2, &quot;x&quot;: 10, &quot;y&quot;: 20},
&nbsp;   {&quot;id&quot;: 3, &quot;x&quot;: 0, &quot;y&quot;: 0}
]
<strong>Giải thích:</strong> Hai object có id=1 và id=3 được đưa vào mảng kết quả mà không thay đổi. Hai object có id=2 được hợp nhất với nhau. Các key từ arr2 ghi đè các giá trị trong arr1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr1 = [
    {&quot;id&quot;: 1, &quot;b&quot;: {&quot;b&quot;: 94},&quot;v&quot;: [4, 3], &quot;y&quot;: 48}
]
arr2 = [
    {&quot;id&quot;: 1, &quot;b&quot;: {&quot;c&quot;: 84}, &quot;v&quot;: [1, 3]}
]
<strong>Đầu ra:</strong> [
    {&quot;id&quot;: 1, &quot;b&quot;: {&quot;c&quot;: 84}, &quot;v&quot;: [1, 3], &quot;y&quot;: 48}
]
<strong>Giải thích:</strong> Hai object có id=1 được hợp nhất với nhau. Đối với các key &quot;b&quot; và &quot;v&quot;, các giá trị từ arr2 được sử dụng. Vì key &quot;y&quot; chỉ tồn tại trong arr1 nên giá trị đó được lấy từ arr1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>arr1</code> và <code>arr2</code> là các mảng JSON hợp lệ</li>
	<li>Mỗi object trong <code>arr1</code> và <code>arr2</code> có một key <code>id</code> kiểu số nguyên duy nhất</li>
	<li><code>2 &lt;= JSON.stringify(arr1).length &lt;= 10<sup>6</sup></code></li>
	<li><code>2 &lt;= JSON.stringify(arr2).length &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hãy hợp nhất hai mảng object theo $id$, trong đó giá trị từ $arr2$ sẽ thắng khi có xung đột. Duyệt lồng nhau sẽ chậm với input lớn và vẫn cần thêm một bước sort.
>
> Hãy index $arr1$ theo $id$, sau đó duyệt $arr2$: dùng $Object.assign$ để merge vào record đã tồn tại hoặc chèn một record mới. $Object.values$ lấy ra các record; các key số nguyên giúp chúng giữ đúng thứ tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function join(arr1: ArrayType[], arr2: ArrayType[]): ArrayType[] {
    const r = (acc: Obj, x: ArrayType): Obj => ((acc[x.id] = x), acc);
    const d = arr1.reduce(r, {});

    arr2.forEach(x => {
        if (d[x.id]) {
            Object.assign(d[x.id], x);
        } else {
            d[x.id] = x;
        }
    });
    return Object.values(d);
}

type JSONValue = null | boolean | number | string | JSONValue[] | { [key: string]: JSONValue };
type ArrayType = { id: number } & Record<string, JSONValue>;

type Obj = Record<number, ArrayType>;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
