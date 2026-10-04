---
comments: true
difficulty: Hard
tags:
    - JavaScript
---

<!-- problem:start -->

# [2675. Array of Objects to Matrix 🔒](https://leetcode.com/problems/array-of-objects-to-matrix)

[中文文档](/solution/2600-2699/2675.Array%20of%20Objects%20to%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm chuyển một mảng các object&nbsp;<code>arr</code> thành một ma trận <code>m</code>.</p>

<p><code>arr</code>&nbsp;là một mảng gồm các object hoặc array. Mỗi phần tử trong mảng có thể được lồng sâu với các array con và object con. Mảng cũng có thể chứa các giá trị số, chuỗi, boolean và&nbsp;null.</p>

<p>Hàng đầu tiên của <code>m</code>&nbsp;phải là tên các cột. Nếu không có cấu trúc lồng nhau, tên cột là các key duy nhất trong các object. Nếu có cấu trúc lồng nhau, tên cột là các đường dẫn tương ứng trong object, được ngăn cách bởi <code>&quot;.&quot;</code>.</p>

<p>Mỗi hàng còn lại tương ứng với một object trong <code>arr</code>. Mỗi giá trị trong ma trận tương ứng với một giá trị trong object. Nếu một object không chứa giá trị cho một cột nào đó, ô tương ứng phải chứa một chuỗi rỗng <code>&quot;&quot;</code>.</p>

<p>Các cột trong ma trận phải được sắp xếp theo <strong>thứ tự từ điển tăng dần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr = [
&nbsp; {&quot;b&quot;: 1, &quot;a&quot;: 2},
&nbsp; {&quot;b&quot;: 3, &quot;a&quot;: 4}
]
<strong>Đầu ra:</strong>
[
&nbsp; [&quot;a&quot;, &quot;b&quot;],
&nbsp; [2, 1],
&nbsp; [4, 3]
]

<strong>Giải thích:</strong>
Có hai tên cột duy nhất trong hai object: &quot;a&quot; và &quot;b&quot;.
&quot;a&quot; tương ứng với [2, 4].
&quot;b&quot; tương ứng với [1, 3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr = [
&nbsp; {&quot;a&quot;: 1, &quot;b&quot;: 2},
&nbsp; {&quot;c&quot;: 3, &quot;d&quot;: 4},
&nbsp; {}
]
<strong>Đầu ra:</strong>
[
&nbsp; [&quot;a&quot;, &quot;b&quot;, &quot;c&quot;, &quot;d&quot;],
&nbsp; [1, 2, &quot;&quot;, &quot;&quot;],
&nbsp; [&quot;&quot;, &quot;&quot;, 3, 4],
&nbsp; [&quot;&quot;, &quot;&quot;, &quot;&quot;, &quot;&quot;]
]

<strong>Giải thích:</strong>
Có 4 tên cột duy nhất: &quot;a&quot;, &quot;b&quot;, &quot;c&quot;, &quot;d&quot;.
Object đầu tiên có các giá trị tương ứng với &quot;a&quot; và &quot;b&quot;.
Object thứ hai có các giá trị tương ứng với &quot;c&quot; và &quot;d&quot;.
Object thứ ba không có key nào, nên nó chỉ là một hàng gồm các chuỗi rỗng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr = [
&nbsp; {&quot;a&quot;: {&quot;b&quot;: 1, &quot;c&quot;: 2}},
&nbsp; {&quot;a&quot;: {&quot;b&quot;: 3, &quot;d&quot;: 4}}
]
<strong>Đầu ra:</strong>
[
&nbsp; [&quot;a.b&quot;, &quot;a.c&quot;, &quot;a.d&quot;],
&nbsp; [1, 2, &quot;&quot;],
&nbsp; [3, &quot;&quot;, 4]
]

<strong>Giải thích:</strong>
Trong ví dụ này, các object được lồng nhau. Các key biểu diễn đường dẫn đầy đủ đến mỗi giá trị, được ngăn cách bởi dấu chấm.
Có ba đường dẫn: &quot;a.b&quot;, &quot;a.c&quot;, &quot;a.d&quot;.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr = [
&nbsp; [{&quot;a&quot;: null}],
&nbsp; [{&quot;b&quot;: true}],
&nbsp; [{&quot;c&quot;: &quot;x&quot;}]
]
<strong>Đầu ra:</strong>
[
&nbsp; [&quot;0.a&quot;, &quot;0.b&quot;, &quot;0.c&quot;],
&nbsp; [null, &quot;&quot;, &quot;&quot;],
&nbsp; [&quot;&quot;, true, &quot;&quot;],
&nbsp; [&quot;&quot;, &quot;&quot;, &quot;x&quot;]
]

<strong>Giải thích:</strong>
Array cũng được xem là object, với các key là chỉ số của chúng.
Mỗi array có một phần tử nên các key là &quot;0.a&quot;, &quot;0.b&quot; và &quot;0.c&quot;.
</pre>

<p><strong class="example">Ví dụ 5:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr = [
  {},
&nbsp; {},
&nbsp; {},
]
<strong>Đầu ra:</strong>
[
&nbsp; [],
&nbsp; [],
&nbsp; [],
&nbsp; []
]

<strong>Giải thích:</strong>
Không có key nào nên mọi hàng đều là một mảng rỗng.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>arr</code> là một mảng JSON hợp lệ</li>
	<li><code>1 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>unique keys &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng object phải được chuyển thành ma trận có các cột là các đường dẫn dùng dấu chấm, với chuỗi rỗng cho các key bị thiếu. Việc xử lý từng tầng thủ công sẽ bỏ sót các trường lồng nhau. DFS thu thập các lá dưới dạng `{path: value}`, sau đó các path duy nhất được sắp xếp để tạo header.
>
> Mỗi hàng tra cứu các path đó và ghi một chuỗi rỗng nếu không tồn tại.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function jsonToMatrix(arr: any[]): (string | number | boolean | null)[] {
    const dfs = (key: string, obj: any) => {
        if (
            typeof obj === 'number' ||
            typeof obj === 'string' ||
            typeof obj === 'boolean' ||
            obj === null
        ) {
            return { [key]: obj };
        }
        const res: any[] = [];
        for (const [k, v] of Object.entries(obj)) {
            const newKey = key ? `${key}.${k}` : `${k}`;
            res.push(dfs(newKey, v));
        }
        return res.flat();
    };

    const kv = arr.map(obj => dfs('', obj));
    const keys = [
        ...new Set(
            kv
                .flat()
                .map(obj => Object.keys(obj))
                .flat(),
        ),
    ].sort();
    const ans: any[] = [keys];
    for (const row of kv) {
        const newRow: any[] = [];
        for (const key of keys) {
            const v = row.find(r => r.hasOwnProperty(key))?.[key];
            newRow.push(v === undefined ? '' : v);
        }
        ans.push(newRow);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
