---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2628. JSON Deep Equal 🔒](https://leetcode.com/problems/json-deep-equal)

[中文文档](/solution/2600-2699/2628.JSON%20Deep%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai giá trị <code>o1</code> và <code>o2</code>, hãy trả về một giá trị boolean cho biết hai giá trị <code>o1</code> và <code>o2</code> có <strong>bằng nhau sâu</strong> hay không.</p>

<p>Hai giá trị được coi là <strong>bằng nhau sâu</strong> khi thỏa mãn các điều kiện sau:</p>

<ul>
	<li>
	<p>Nếu cả hai giá trị đều là kiểu nguyên thủy, chúng <strong>bằng nhau sâu</strong> nếu phép so sánh <code>===</code> trả về true.</p>
	</li>
	<li>
	<p>Nếu cả hai giá trị đều là mảng, chúng <strong>bằng nhau sâu</strong> nếu có cùng các phần tử theo cùng thứ tự, và mỗi phần tử cũng <strong>bằng nhau sâu</strong> theo các điều kiện này.</p>
	</li>
	<li>
	<p>Nếu cả hai giá trị đều là object, chúng <strong>bằng nhau sâu</strong> nếu có cùng các key, và giá trị tương ứng với mỗi key cũng <strong>bằng nhau sâu</strong> theo các điều kiện này.</p>
	</li>
</ul>

<p>Bạn có thể giả định rằng cả hai giá trị đều là kết quả của <code>JSON.parse</code>. Nói cách khác, chúng là JSON hợp lệ.</p>

<p>Hãy giải bài toán mà không sử dụng hàm <code>_.isEqual()</code> của lodash</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> o1 = {&quot;x&quot;:1,&quot;y&quot;:2}, o2 = {&quot;x&quot;:1,&quot;y&quot;:2}
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các key và giá trị khớp chính xác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> o1 = {&quot;y&quot;:2,&quot;x&quot;:1}, o2 = {&quot;x&quot;:1,&quot;y&quot;:2}
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Mặc dù các key có thứ tự khác nhau, chúng vẫn khớp chính xác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> o1 = {&quot;x&quot;:null,&quot;L&quot;:[1,2,3]}, o2 = {&quot;x&quot;:null,&quot;L&quot;:[&quot;1&quot;,&quot;2&quot;,&quot;3&quot;]}
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Mảng các số khác với mảng các chuỗi.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong> o1 = true, o2 = false
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> true !== false</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= JSON.stringify(o1).length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= JSON.stringify(o2).length &lt;= 10<sup>5</sup></code></li>
	<li><code>maxNestingDepth &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Không thể chỉ dùng `===` để so sánh các giá trị JSON tùy ý; object và mảng cần được xử lý đệ quy. Mảng không được bằng object thông thường, và các tập key phải khớp nhau.
>
> Trước tiên xử lý `null` và các giá trị không phải object; từ chối trường hợp kiểu dữ liệu hoặc mảng/object không khớp. Duyệt đệ quy theo chỉ số đối với mảng và theo key đối với object, trả về false ngay khi độ dài hoặc số lượng key khác nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function areDeeplyEqual(o1: any, o2: any): boolean {
    if (o1 === null || typeof o1 !== 'object') {
        return o1 === o2;
    }
    if (typeof o1 !== typeof o2) {
        return false;
    }
    if (Array.isArray(o1) !== Array.isArray(o2)) {
        return false;
    }
    if (Array.isArray(o1)) {
        if (o1.length !== o2.length) {
            return false;
        }
        for (let i = 0; i < o1.length; i++) {
            if (!areDeeplyEqual(o1[i], o2[i])) {
                return false;
            }
        }
        return true;
    } else {
        const keys1 = Object.keys(o1);
        const keys2 = Object.keys(o2);
        if (keys1.length !== keys2.length) {
            return false;
        }
        for (const key of keys1) {
            if (!areDeeplyEqual(o1[key], o2[key])) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
