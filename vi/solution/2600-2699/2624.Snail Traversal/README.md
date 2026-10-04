---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2624. Snail Traversal](https://leetcode.com/problems/snail-traversal)

[中文文档](/solution/2600-2699/2624.Snail%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Viết code để mở rộng mọi mảng, cho phép gọi phương thức <code>snail(rowsCount, colsCount)</code> nhằm chuyển mảng 1D thành mảng 2D được sắp xếp theo mẫu được gọi là <strong>thứ tự duyệt hình xoắn ốc</strong>. Các giá trị đầu vào không hợp lệ phải trả về một mảng rỗng. Nếu <code>rowsCount * colsCount !== nums.length</code>, đầu vào được xem là không hợp lệ.</p>

<p><strong>Thứ tự duyệt hình xoắn ốc</strong><em>&nbsp;</em>bắt đầu tại ô trên cùng bên trái với giá trị đầu tiên của mảng hiện tại. Sau đó đi qua toàn bộ cột đầu tiên từ trên xuống dưới, rồi chuyển sang cột kế tiếp bên phải và duyệt cột đó từ dưới lên trên. Mẫu này tiếp tục, luân phiên hướng duyệt ở mỗi cột, cho đến khi duyệt hết mảng hiện tại. Ví dụ, với mảng đầu vào <code>[19, 10, 3, 7, 9, 8, 5, 2, 1, 17, 16, 14, 12, 18, 6, 13, 11, 20, 4, 15]</code>, <code>rowsCount = 5</code> và <code>colsCount = 4</code>, ma trận đầu ra mong muốn được hiển thị bên dưới. Lưu ý rằng việc duyệt ma trận theo các mũi tên sẽ tương ứng với thứ tự các số trong mảng ban đầu.</p>

<p>&nbsp;</p>

<p><img alt="Traversal Diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2624.Snail%20Traversal/images/screen-shot-2023-04-10-at-100006-pm.png" style="width: 275px; height: 343px;" /></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
nums = [19, 10, 3, 7, 9, 8, 5, 2, 1, 17, 16, 14, 12, 18, 6, 13, 11, 20, 4, 15]
rowsCount = 5
colsCount = 4
<strong>Đầu ra:</strong>
[
 [19,17,16,15],
 &nbsp;[10,1,14,4],
 &nbsp;[3,2,12,20],
 &nbsp;[7,5,18,11],
 &nbsp;[9,8,6,13]
]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
nums = [1,2,3,4]
rowsCount = 1
colsCount = 4
<strong>Đầu ra:</strong> [[1, 2, 3, 4]]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
nums = [1,3]
rowsCount = 2
colsCount = 2
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> 2 nhân với 2 bằng 4, còn mảng ban đầu [1,3] có độ dài là 2; do đó, đầu vào không hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>0 &lt;= nums.length &lt;= 250</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
    <li><code>1 &lt;= rowsCount &lt;= 250</code></li>
    <li><code>1 &lt;= colsCount &lt;= 250</code></li>
</ul>

<p>&nbsp;</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng một chiều phải được điền vào ma trận theo từng cột, luân phiên từ trên xuống dưới và từ dưới lên trên. Nếu độ dài không bằng $rows \times cols$, đầu vào không hợp lệ. Một lượt duyệt có thể ghi toàn bộ ô.
>
> Chỉ cần giữ bước dọc $k=\pm 1$; khi chạm một trong hai biên, đảo hướng rồi chuyển sang cột kế tiếp, đồng thời ghi các phần tử theo đường đi đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
declare global {
    interface Array<T> {
        snail(rowsCount: number, colsCount: number): number[][];
    }
}

Array.prototype.snail = function (rowsCount: number, colsCount: number): number[][] {
    if (rowsCount * colsCount !== this.length) {
        return [];
    }
    const ans: number[][] = Array.from({ length: rowsCount }, () => Array(colsCount));
    for (let h = 0, i = 0, j = 0, k = 1; h < this.length; ++h) {
        ans[i][j] = this[h];
        i += k;
        if (i === rowsCount || i === -1) {
            i -= k;
            k = -k;
            ++j;
        }
    }
    return ans;
};

/**
 * const arr = [1,2,3,4];
 * arr.snail(1,4); // [[1,2,3,4]]
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
