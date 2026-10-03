---
comments: true
difficulty: Medium
tags:
    - Tree
    - Binary Search Tree
    - Array
    - Binary Tree
    - Ordered Set
---

<!-- problem:start -->

# [1902. Depth of BST Given Insertion Order 🔒](https://leetcode.com/problems/depth-of-bst-given-insertion-order)

[中文文档](/solution/1900-1999/1902.Depth%20of%20BST%20Given%20Insertion%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>order</code> được <strong>đánh chỉ số từ 0</strong> có độ dài <code>n</code>, là một <strong>hoán vị</strong> của các số nguyên từ <code>1</code> đến <code>n</code>, biểu diễn <strong>thứ tự</strong> chèn vào một <strong>cây tìm kiếm nhị phân</strong>.</p>

<p>Cây tìm kiếm nhị phân được định nghĩa như sau:</p>

<ul>
	<li>Cây con trái của một nút chỉ chứa các nút có khóa <strong>nhỏ hơn</strong> khóa của nút đó.</li>
	<li>Cây con phải của một nút chỉ chứa các nút có khóa <strong>lớn hơn</strong> khóa của nút đó.</li>
	<li>Cả cây con trái và cây con phải cũng phải là các cây tìm kiếm nhị phân.</li>
</ul>

<p>Cây tìm kiếm nhị phân được xây dựng như sau:</p>

<ul>
	<li><code>order[0]</code> là <strong>gốc</strong> của cây tìm kiếm nhị phân.</li>
	<li>Tất cả phần tử tiếp theo được chèn làm <strong>con</strong> của <strong>bất kỳ</strong> nút hiện có nào sao cho các tính chất của cây tìm kiếm nhị phân vẫn được giữ nguyên.</li>
</ul>

<p>Trả về <em><strong>độ sâu</strong> của cây tìm kiếm nhị phân</em>.</p>

<p><strong>Độ sâu</strong> của một cây nhị phân là số lượng <strong>nút</strong> trên <strong>đường đi dài nhất</strong> từ nút gốc xuống nút lá xa nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1902.Depth%20of%20BST%20Given%20Insertion%20Order/images/1.png" style="width: 624px; height: 154px;" />
<pre>
<strong>Đầu vào:</strong> order = [2,1,4,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Cây tìm kiếm nhị phân có độ sâu bằng 3 với đường đi 2-&gt;3-&gt;4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1902.Depth%20of%20BST%20Given%20Insertion%20Order/images/2.png" style="width: 624px; height: 146px;" />
<pre>
<strong>Đầu vào:</strong> order = [2,1,3,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Cây tìm kiếm nhị phân có độ sâu bằng 3 với đường đi 2-&gt;3-&gt;4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1902.Depth%20of%20BST%20Given%20Insertion%20Order/images/3.png" style="width: 624px; height: 225px;" />
<pre>
<strong>Đầu vào:</strong> order = [1,2,3,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Cây tìm kiếm nhị phân có độ sâu bằng 4 với đường đi 1-&gt;2-&gt;3-&gt;4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == order.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>order</code> là một hoán vị của các số nguyên từ <code>1</code> đến <code>n</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần chèn một giá trị bằng cách đi từ gốc sẽ tốn $O(n)$, dẫn đến tổng cộng $O(n^2)$. Điều này không đáp ứng được $n\le 10^5$.
>
> Một khóa mới được gắn dưới nút đứng ngay trước hoặc ngay sau nó trong số các khóa đã được chèn, nên độ sâu của nó bằng một cộng với giá trị lớn hơn trong độ sâu của hai nút đó. Không cần dùng con trỏ nếu ta có thể truy vấn độ sâu của các nút lân cận trong một tập hợp đã sắp xếp.
>
> Một map đã sắp xếp lưu các giá trị đã chèn cùng độ sâu của chúng, với các giá trị lính canh $0$ và $+\infty$. Với mỗi $v$, ta tìm hai nút lân cận, ghi lại độ sâu mới rồi cập nhật đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDepthBST(self, order: List[int]) -> int:
        sd = SortedDict({0: 0, inf: 0, order[0]: 1})
        ans = 1
        for v in order[1:]:
            lower = sd.bisect_left(v) - 1
            higher = lower + 1
            depth = 1 + max(sd.values()[lower], sd.values()[higher])
            ans = max(ans, depth)
            sd[v] = depth
        return ans
```

#### Java

```java
class Solution {
    public int maxDepthBST(int[] order) {
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        tm.put(0, 0);
        tm.put(Integer.MAX_VALUE, 0);
        tm.put(order[0], 1);
        int ans = 1;
        for (int i = 1; i < order.length; ++i) {
            int v = order[i];
            Map.Entry<Integer, Integer> lower = tm.lowerEntry(v);
            Map.Entry<Integer, Integer> higher = tm.higherEntry(v);
            int depth = 1 + Math.max(lower.getValue(), higher.getValue());
            ans = Math.max(ans, depth);
            tm.put(v, depth);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
