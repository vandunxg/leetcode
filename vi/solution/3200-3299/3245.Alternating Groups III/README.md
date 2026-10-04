---
comments: true
difficulty: Hard
rating: 3112
source: Weekly Contest 409 Q4
tags:
    - Binary Indexed Tree
    - Array
    - Ordered Set
---

<!-- problem:start -->

# [3245. Alternating Groups III](https://leetcode.com/problems/alternating-groups-iii)

[中文文档](/solution/3200-3299/3245.Alternating%20Groups%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số ô màu đỏ và xanh dương được sắp xếp thành vòng tròn. Cho một mảng số nguyên <code>colors</code> và một mảng số nguyên 2 chiều <code>queries</code>.</p>

<p>Màu của ô <code>i</code> được biểu diễn bởi <code>colors[i]</code>:</p>

<ul>
    <li><code>colors[i] == 0</code> nghĩa là ô <code>i</code> có màu <strong>đỏ</strong>.</li>
    <li><code>colors[i] == 1</code> nghĩa là ô <code>i</code> có màu <strong>xanh dương</strong>.</li>
</ul>

<p>Một nhóm <strong>xen kẽ</strong> là một tập hợp liên tiếp các ô trong vòng tròn có màu <strong>xen kẽ</strong> (mỗi ô trong nhóm, ngoại trừ ô đầu tiên và ô cuối cùng, có màu khác với các ô <b>kề bên</b> trong nhóm).</p>

<p>Cần xử lý hai loại truy vấn:</p>

<ul>
    <li><code>queries[i] = [1, size<sub>i</sub>]</code>, xác định số lượng nhóm <strong>xen kẽ</strong> có kích thước <code>size<sub>i</sub></code>.</li>
    <li><code>queries[i] = [2, index<sub>i</sub>, color<sub>i</sub>]</code>, thay đổi <code>colors[index<sub>i</sub>]</code> thành <code>color<font face="monospace"><sub>i</sub></font></code>.</li>
</ul>

<p>Trả về một mảng <code>answer</code> chứa kết quả của các truy vấn loại thứ nhất <em>theo đúng thứ tự</em>.</p>

<p><strong>Lưu ý</strong> rằng vì <code>colors</code> biểu diễn một <strong>vòng tròn</strong>, ô <strong>đầu tiên</strong> và ô <strong>cuối cùng</strong> được xem là nằm cạnh nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">colors = [0,1,1,0,1], queries = [[2,1,0],[1,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3245.Alternating%20Groups%20III/images/screenshot-from-2024-06-03-20-14-44.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></strong></p>

<p>Truy vấn thứ nhất:</p>

<p>Thay đổi <code>colors[1]</code> thành 0.</p>

<p><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3245.Alternating%20Groups%20III/images/screenshot-from-2024-06-03-20-20-25.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></p>

<p>Truy vấn thứ hai:</p>

<p>Đếm các nhóm xen kẽ có kích thước 4:</p>

<p><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3245.Alternating%20Groups%20III/images/screenshot-from-2024-06-03-20-25-02-2.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3245.Alternating%20Groups%20III/images/screenshot-from-2024-06-03-20-24-12.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">colors = [0,0,1,0,1,1], queries = [[1,3],[2,3,0],[1,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,0]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3245.Alternating%20Groups%20III/images/screenshot-from-2024-06-03-20-35-50.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></p>

<p>Truy vấn thứ nhất:</p>

<p>Đếm các nhóm xen kẽ có kích thước 3:</p>

<p><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3245.Alternating%20Groups%20III/images/screenshot-from-2024-06-03-20-37-13.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3245.Alternating%20Groups%20III/images/screenshot-from-2024-06-03-20-36-40.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></p>

<p>Truy vấn thứ hai: <code>colors</code> không thay đổi.</p>

<p>Truy vấn thứ ba: Không có nhóm xen kẽ nào có kích thước 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>4 &lt;= colors.length &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>0 &lt;= colors[i] &lt;= 1</code></li>
    <li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>queries[i][0] == 1</code> hoặc <code>queries[i][0] == 2</code></li>
    <li>Với mọi <code>i</code> thỏa mãn:
    <ul>
        <li><code>queries[i][0] == 1</code>: <code>queries[i].length == 2</code>, <code>3 &lt;= queries[i][1] &lt;= colors.length - 1</code></li>
        <li><code>queries[i][0] == 2</code>: <code>queries[i].length == 3</code>, <code>0 &lt;= queries[i][1] &lt;= colors.length - 1</code>, <code>0 &lt;= queries[i][2] &lt;= 1</code></li>
    </ul>
    </li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trên một vòng tròn, ta đổi màu các ô và đếm các nhóm xen kẽ có kích thước cho trước. Với $n,q\le 5\times 10^4$, việc quét tuyến tính cho mỗi truy vấn như ở I/II sẽ quá chậm. Các nhóm được xác định bởi các đoạn xen kẽ cực đại; mỗi lần đổi màu chỉ tách hoặc gộp các đoạn lân cận.
>
> Một ordered set lưu các đoạn, còn cây Fenwick đếm số đoạn có từng độ dài. Với truy vấn kích thước $s$, ta tính tổng $\max(\ell-s+1,0)$ theo các độ dài đoạn; một prefix có thể trả lời tổng này. Hiện chưa có phần cài đặt trong cây; phần tư duy này chỉ bám theo hướng đoạn kết hợp với Fenwick.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python

```

#### Java

```java

```

#### C++

```cpp

```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
