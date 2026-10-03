---
comments: true
difficulty: Hard
rating: 2502
source: Weekly Contest 250 Q4
tags:
    - Bit Manipulation
    - Depth-First Search
    - Trie
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1938. Maximum Genetic Difference Query](https://leetcode.com/problems/maximum-genetic-difference-query)

[中文文档](/solution/1900-1999/1938.Maximum%20Genetic%20Difference%20Query/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây có gốc gồm <code>n</code> node, được đánh số từ <code>0</code> đến <code>n - 1</code>. Số của mỗi node biểu thị <strong>giá trị di truyền duy nhất</strong> của node đó (tức là giá trị di truyền của node <code>x</code> là <code>x</code>). <strong>Độ khác biệt di truyền</strong> giữa hai giá trị di truyền được định nghĩa là <strong>phép XOR</strong><strong> theo bit</strong> của hai giá trị đó. Bạn được cho mảng số nguyên <code>parents</code>, trong đó <code>parents[i]</code> là node cha của node <code>i</code>. Nếu node <code>x</code> là <strong>root</strong> của cây thì <code>parents[x] == -1</code>.</p>

<p>Bạn cũng được cho mảng <code>queries</code>, trong đó <code>queries[i] = [node<sub>i</sub>, val<sub>i</sub>]</code>. Với mỗi query <code>i</code>, hãy tìm <strong>độ khác biệt di truyền lớn nhất</strong> giữa <code>val<sub>i</sub></code> và <code>p<sub>i</sub></code>, trong đó <code>p<sub>i</sub></code> là giá trị di truyền của một node bất kỳ nằm trên đường đi từ <code>node<sub>i</sub></code> đến root (bao gồm cả <code>node<sub>i</sub></code> và root). Nói cách khác, cần tối đa hóa <code>val<sub>i</sub> XOR p<sub>i</sub></code>.</p>

<p>Trả về <em>một mảng </em><code>ans</code><em>, trong đó </em><code>ans[i]</code><em> là đáp án của </em><code>i<sup>th</sup></code><em> query</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1938.Maximum%20Genetic%20Difference%20Query/images/c1.png" style="width: 118px; height: 163px;" />
<pre>
<strong>Đầu vào:</strong> parents = [-1,0,1,1], queries = [[0,2],[3,2],[2,5]]
<strong>Đầu ra:</strong> [2,3,7]
<strong>Giải thích: </strong>Các query được xử lý như sau:
- [0,2]: Node có độ khác biệt di truyền lớn nhất là 0, với độ khác biệt là 2 XOR 0 = 2.
- [3,2]: Node có độ khác biệt di truyền lớn nhất là 1, với độ khác biệt là 2 XOR 1 = 3.
- [2,5]: Node có độ khác biệt di truyền lớn nhất là 2, với độ khác biệt là 5 XOR 2 = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1938.Maximum%20Genetic%20Difference%20Query/images/c2.png" style="width: 256px; height: 221px;" />
<pre>
<strong>Đầu vào:</strong> parents = [3,7,-1,2,0,7,0,2], queries = [[4,6],[1,15],[0,5]]
<strong>Đầu ra:</strong> [6,14,7]
<strong>Giải thích: </strong>Các query được xử lý như sau:
- [4,6]: Node có độ khác biệt di truyền lớn nhất là 0, với độ khác biệt là 6 XOR 0 = 6.
- [1,15]: Node có độ khác biệt di truyền lớn nhất là 1, với độ khác biệt là 15 XOR 1 = 14.
- [0,5]: Node có độ khác biệt di truyền lớn nhất là 2, với độ khác biệt là 5 XOR 2 = 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= parents.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= parents[i] &lt;= parents.length - 1</code> với mọi node <code>i</code> <strong>không</strong> phải root.</li>
	<li><code>parents[root] == -1</code></li>
	<li><code>1 &lt;= queries.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= node<sub>i</sub> &lt;= parents.length - 1</code></li>
	<li><code>0 &lt;= val<sub>i</sub> &lt;= 2 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi query, cần tìm node trên đường đi đến root sao cho XOR với một giá trị cho trước là lớn nhất. Nếu lần lượt đi từ node đó lên root cho từng query thì quá chậm khi $n,q\le 10^5$.
>
> Binary trie có thể trả lời truy vấn XOR lớn nhất. Gắn các query vào node tương ứng; khi DFS đi vào thì chèn giá trị của node, khi đi ra thì xóa nó, để trie luôn chứa đúng đường đi từ root đến node hiện tại.
>
> Ưu tiên bit đối diện ở mỗi mức cho phép trả lời toàn bộ query của node đó trong $O(\log V)$.

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
