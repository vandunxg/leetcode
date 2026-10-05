---
comments: true
difficulty: Hard
rating: 2409
source: Weekly Contest 503 Q4
tags:
    - Array
    - Hash Table
    - Divide and Conquer
    - Counting
---

<!-- problem:start -->

# [3943. Number of Pairs After Increment](https://leetcode.com/problems/number-of-pairs-after-increment)

[中文文档](/solution/3900-3999/3943.Number%20of%20Pairs%20After%20Increment/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, cùng một mảng số nguyên 2D <code>queries</code>.</p>

<p>Mỗi <code>queries[i]</code> thuộc một trong các loại sau:</p>

<ul>
	<li><code>[1, x, y, val]</code> &ndash; <strong>Cộng</strong> <code>val</code> vào mọi phần tử trong <code>nums2[x..y]</code>.</li>
	<li><code>[2, tot]</code> &ndash; <strong>Tính</strong> số cặp <code>(j, k)</code> sao cho <code>nums1[j] + nums2[k] == tot</code>.</li>
</ul>

<p>Trả về một mảng số nguyên <code>answer</code>, trong đó <code>answer[j]</code> là số cặp của truy vấn loại 2 thứ <code>j<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,2], nums2 = [3,4], queries = [[2,5],[1,0,0,2],[2,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>queries[0] = [2, 5]</code>: Các cặp hợp lệ là <code>nums1[0] + nums2[1] = 1 + 4 = 5</code> và <code>nums1[1] + nums2[0] = 2 + 3 = 5</code>.</li>
	<li><code>queries[1] = [1, 0, 0, 2]</code>: Cộng 2 vào <code>nums2[0]</code>, khi đó <code>nums2 = [5, 4]</code>.</li>
	<li><code>queries[2] = [2, 5]</code>: Cặp hợp lệ là <code>nums1[0] + nums2[1] = 1 + 4 = 5</code>.</li>
	<li>Do đó, <code>answer = [2, 1]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,1], nums2 = [2,2,3], queries = [[2,4],[1,0,1,1],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,6]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>queries[0] = [2, 4]</code>: Các cặp hợp lệ là <code>nums1[0] + nums2[2] = 1 + 3</code> và <code>nums1[1] + nums2[2] = 1 + 3</code>.</li>
	<li><code>queries[1] = [1, 0, 1, 1]</code>: Cộng 1 vào <code>nums2[0]</code> và <code>nums2[1]</code>, khi đó <code>nums2 = [3, 3, 3]</code>.</li>
	<li><code>queries[2] = [2, 4]</code>: Mỗi phần tử của <code>nums1 = [1, 1]</code> ghép với mọi phần tử của <code>nums2 = [3, 3, 3]</code> vì <code>1 + 3 = 4</code>. Tổng cộng có <code>2 &times; 3 = 6</code> cặp.</li>
	<li>Do đó, <code>answer = [2, 6]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [2,5,8,4], nums2 = [1,3,8], queries = [[2,9],[1,1,2,1],[2,10]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>queries[0] = [2, 9]</code>: Cặp hợp lệ duy nhất là <code>nums1[2] + nums2[0] = 8 + 1 = 9</code>.</li>
	<li><code>queries[1] = [1, 1, 2, 1]</code>: Cộng 1 vào <code>nums2[1]</code> và <code>nums2[2]</code>, khi đó <code>nums2 = [1, 4, 9]</code>.</li>
	<li><code>queries[2] = [2, 10]</code>: Không có cặp nào có tổng bằng <code>10</code>.</li>
	<li>Do đó, <code>answer = [1, 0]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length &lt;= 5</code></li>
	<li><code>1 &lt;= nums2.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>queries[i].length == 2 or 4</code>
	<ul>
		<li><code>queries[i] == [1, x, y, val], or</code></li>
		<li><code>queries[i] == [2, tot]</code></li>
		<li><code>0 &lt;= x &lt;= y &lt; nums2.length</code></li>
		<li><code>1 &lt;= val &lt;= 10<sup>5</sup></code></li>
		<li><code>1 &lt;= tot &lt;= 10<sup>9</sup>​​​​​​​</code></li>
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
> $\textit{nums1}$ có độ dài nhiều nhất là $5$, nhưng có thể có $5\times 10^4$ truy vấn cộng trên đoạn và truy vấn đếm cặp. Việc duyệt $\textit{nums1}$ cho mỗi truy vấn loại-$2$ là chấp nhận được; ta vẫn cần biết số phần tử của $\textit{nums2}$ bằng $tot-a$ sau các phép tăng trên đoạn.
>
> Duy trì một cấu trúc tần suất trên $\textit{nums2}$ (Fenwick hoặc segment tree) hỗ trợ cộng trên đoạn — bằng các lazy tag hoặc một cơ chế dịch dựa trên hiệu. Với mỗi truy vấn, ta duyệt mảng ngắn và tra cứu $tot-a$.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần walkthrough dừng ở “duyệt mảng ngắn kết hợp với map tần suất hỗ trợ cộng trên đoạn”.

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
