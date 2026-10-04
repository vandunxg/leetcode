---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2823. Deep Object Filter 🔒](https://leetcode.com/problems/deep-object-filter)

[中文文档](/solution/2800-2899/2823.Deep%20Object%20Filter/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một object hoặc một mảng&nbsp;<code>obj</code> và một hàm <code>fn</code>, hãy trả về một object hoặc mảng đã được lọc&nbsp;<code>filteredObject</code>.&nbsp;</p>

<p>Hàm <code>deepFilter</code>&nbsp;cần thực hiện thao tác lọc sâu trên&nbsp;<code>obj</code>. Thao tác lọc sâu phải loại bỏ các thuộc tính mà kết quả của hàm lọc <code>fn</code> là <code>false</code>, cũng như mọi object hoặc mảng rỗng còn lại sau khi các key bị loại bỏ.</p>

<p>Nếu thao tác lọc sâu tạo ra một object hoặc mảng rỗng, không còn thuộc tính nào, <code>deepFilter</code> phải trả về <code>undefined</code> để cho biết rằng không còn dữ liệu hợp lệ nào trong <code>filteredObject</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = [-5, -4, -3, -2, -1, 0, 1],
fn = (x) =&gt; x &gt; 0
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong> Tất cả các giá trị không lớn hơn 0 đều bị loại bỏ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = {&quot;a&quot;: 1, &quot;b&quot;: &quot;2&quot;, &quot;c&quot;: 3, &quot;d&quot;: &quot;4&quot;, &quot;e&quot;: 5, &quot;f&quot;: 6, &quot;g&quot;: {&quot;a&quot;: 1}},
fn = (x) =&gt; typeof x === &quot;string&quot;
<strong>Đầu ra:</strong> {&quot;b&quot;:&quot;2&quot;,&quot;d&quot;:&quot;4&quot;}
<strong>Giải thích:</strong> Tất cả các key có giá trị không phải là chuỗi đều bị loại bỏ. Khi các key của object bị loại bỏ trong quá trình lọc, mọi object rỗng tạo ra cũng bị loại bỏ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = [-1, [-1, -1, 5, -1, 10], -1, [-1], [-5]],
fn = (x) =&gt; x &gt; 0
<strong>Đầu ra:</strong> [[5,10]]
<strong>Giải thích:</strong> Tất cả các giá trị không lớn hơn 0 đều bị loại bỏ. Khi các giá trị bị loại bỏ trong quá trình lọc, mọi mảng rỗng tạo ra cũng bị loại bỏ.</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = [[[[5]]]],
fn = (x) =&gt; Array.isArray(x)
<strong>Đầu ra:</strong> undefined
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>fn</code> là một hàm trả về giá trị boolean</li>
	<li><code>obj</code> là một object hoặc mảng JSON hợp lệ</li>
	<li><code>2 &lt;= JSON.stringify(obj).length &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Việc lọc phải đệ quy qua các mảng và object lồng nhau, trong đó $fn$ quyết định với từng giá trị lá. Loại bỏ các phần tử con trả về `undefined`; nếu mảng hoặc object trở nên rỗng thì chính nó cũng bị loại bỏ.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function deepFilter(obj: Record<string, any>, fn: Function): Record<string, any> | undefined {
    const dfs = (data: any): any => {
        if (Array.isArray(data)) {
            const res = data.map(dfs).filter((item: any) => item !== undefined);
            return res.length > 0 ? res : undefined;
        }
        if (typeof data === 'object' && data !== null) {
            const res: Record<string, any> = {};
            for (const key in data) {
                if (data.hasOwnProperty(key)) {
                    const filteredValue = dfs(data[key]);
                    if (filteredValue !== undefined) {
                        res[key] = filteredValue;
                    }
                }
            }
            return Object.keys(res).length > 0 ? res : undefined;
        }
        return fn(data) ? data : undefined;
    };

    return dfs(obj);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
