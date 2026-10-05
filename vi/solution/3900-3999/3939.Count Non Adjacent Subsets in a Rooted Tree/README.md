---
comments: true
difficulty: Hard
rating: 2354
source: Biweekly Contest 183 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3939. Count Non Adjacent Subsets in a Rooted Tree](https://leetcode.com/problems/count-non-adjacent-subsets-in-a-rooted-tree)

[中文文档](/solution/3900-3999/3939.Count%20Non%20Adjacent%20Subsets%20in%20a%20Rooted%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p data-end="186" data-start="43">Bạn được cho một cây có gốc gồm <code data-end="79" data-start="76">n</code> nút được đánh số từ 0 đến <code data-end="113" data-start="106">n - 1</code>, được biểu diễn bằng một mảng số nguyên <code data-end="164" data-start="156">parent</code> có độ dài <code data-end="178" data-start="175">n</code>, trong đó:</p>

<ul>
	<li data-end="227" data-start="190"><code data-end="206" data-start="190">parent[0] = -1</code> (nút 0 là nút gốc).</li>
	<li data-end="311" data-start="230">Với mỗi <code data-end="250" data-start="239">1 &lt;= i &lt; n</code>, <code data-end="263" data-start="252">parent[i]</code> là cha của nút <code data-end="289" data-start="286">i</code> (<code data-end="310" data-start="291">0 &lt;= parent[i] &lt; i</code>).</li>
</ul>

<p data-end="439" data-start="313">Bạn cũng được cho một mảng số nguyên <font face="monospace">nums</font> có độ dài <code data-end="377" data-start="374">n</code>, trong đó <code><font face="monospace">nums[i]</font></code> là giá trị của nút <code data-end="418" data-start="415">i</code>, và một số nguyên <code data-end="438" data-start="435">k</code>.</p>

<p data-end="488" data-start="441">Một tập con không rỗng của các nút được gọi là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li data-end="555" data-start="491"><strong>Tổng</strong> các giá trị của các nút được chọn <strong>chia hết</strong> cho <code data-end="554" data-start="551">k</code>.</li>
	<li data-end="669" data-start="558">Không có <strong>hai</strong> nút được chọn nào <strong>kề nhau</strong> trong cây (không đồng thời chọn một nút và cha trực tiếp của nó).</li>
</ul>

<p data-end="721" data-start="671">Trả về số tập con hợp lệ modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">parent = [-1,0,1], nums = [1,2,3], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3939.Count%20Non%20Adjacent%20Subsets%20in%20a%20Rooted%20Tree/images/image1.png" style="width: 230px; height: 75px;" />​​​​​​​</strong></p>

<p>Tập con hợp lệ duy nhất là <code>{2}</code>. Tập con này chứa nút 2 có giá trị 3, chia hết cho 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">parent = [-1,0,0,0], nums = [2,1,2,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3939.Count%20Non%20Adjacent%20Subsets%20in%20a%20Rooted%20Tree/images/image2.png" style="width: 250px; height: 180px;" />​​​​​​​</strong>​​​​​​​</p>

<p>Các tập con hợp lệ là:</p>

<ul>
	<li><code>{1, 2}</code>: Các nút 1 và 2 đều là con của nút 0 và không nối trực tiếp với nhau. Tổng giá trị của chúng là <code>1 + 2 = 3</code>, chia hết cho 3.</li>
	<li><code>{2, 3}</code>: Các nút 2 và 3 cũng không kề nhau. Tổng giá trị của chúng là <code>2 + 1 = 3</code>, chia hết cho 3.</li>
</ul>

<p>Không có tập con nào khác thỏa mãn cả hai điều kiện. Vì vậy, đáp án là 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li data-end="57" data-start="20"><code data-end="55" data-start="20">n == parent.length == nums.length</code></li>
	<li data-end="78" data-start="60"><code data-end="76" data-start="60">1 &lt;= n &lt;= 1000</code></li>
	<li data-end="100" data-start="81"><code data-end="98" data-start="81">parent[0] == -1</code></li>
	<li data-end="147" data-start="103">Với mọi <code data-end="123" data-start="111">1 &lt;= i &lt; n</code>:
	<ul>
		<li data-end="147" data-start="103"><code data-end="145" data-start="125">0 &lt;= parent[i] &lt; i</code></li>
	</ul>
	</li>
	<li data-end="174" data-start="150"><code data-end="172" data-start="150">1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li data-end="195" data-start="177"><code data-end="193" data-start="177">1 &lt;= k &lt;= 100</code>​​​​​​​​​​​​​​<code data-end="193" data-start="177">​​​​​​​</code></li>
	<li data-end="195" data-start="177"><code data-end="206" data-start="198">parent</code> mô tả một cây có gốc hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 1000$, việc liệt kê các tập con là bất khả thi. Đếm các tập độc lập (không có cặp nút cha–con) theo phần dư modulo $k$ là một bài toán knapsack trên cây: mỗi cây con lưu số cách chọn hoặc bỏ qua gốc của nó với từng tổng modulo $k$.
>
> Các cây con được gộp bằng phép chập. Khi bỏ qua gốc, mỗi cây con có thể dùng bất kỳ trạng thái hợp lệ nào; khi chọn gốc, mọi cây con phải ở trạng thái “bỏ qua gốc của cây con đó”. Cuối cùng, lấy modulo $10^9+7$.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần hướng dẫn dừng ở bài toán knapsack trên cây đó.

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
