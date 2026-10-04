---
comments: true
difficulty: Hard
rating: 2697
source: Weekly Contest 452 Q4
tags:
    - Segment Tree
    - Array
    - Hash Table
    - Math
    - Number Theory
    - Ordered Set
---

<!-- problem:start -->

# [3569. Maximize Count of Distinct Primes After Split](https://leetcode.com/problems/maximize-count-of-distinct-primes-after-split)

[中文文档](/solution/3500-3599/3569.Maximize%20Count%20of%20Distinct%20Primes%20After%20Split/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng số nguyên 2D <code>queries</code>, trong đó <code>queries[i] = [idx, val]</code>.</p>

<p>Với mỗi truy vấn:</p>

<ol>
    <li>Cập nhật <code>nums[idx] = val</code>.</li>
    <li>Chọn một số nguyên <code>k</code> với <code>1 &lt;= k &lt; n</code> để chia mảng thành prefix không rỗng <code>nums[0..k-1]</code> và suffix <code>nums[k..n-1]</code>, sao cho tổng số lượng các giá trị <span data-keyword="prime-number">số nguyên tố</span> <strong>phân biệt</strong> trong mỗi phần là <strong>lớn nhất</strong>.</li>
</ol>

<p><strong data-end="513" data-start="504">Lưu ý:</strong> Các thay đổi được thực hiện trên mảng trong một truy vấn sẽ được giữ lại cho truy vấn tiếp theo.</p>

<p>Trả về một mảng chứa kết quả của mỗi truy vấn theo đúng thứ tự được cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3,1,2], queries = [[1,2],[3,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Ban đầu, <code>nums = [2, 1, 3, 1, 2]</code>.</li>
    <li>Sau truy vấn thứ 1<sup>st</sup>, <code>nums = [2, 2, 3, 1, 2]</code>. Chia <code>nums</code> thành <code>[2]</code> và <code>[2, 3, 1, 2]</code>. <code>[2]</code> chứa 1 số nguyên tố phân biệt và <code>[2, 3, 1, 2]</code> chứa 2 số nguyên tố phân biệt. Do đó, đáp án cho truy vấn này là <code>1 + 2 = 3</code>.</li>
    <li>Sau truy vấn thứ 2<sup>nd</sup>, <code>nums = [2, 2, 3, 3, 2]</code>. Chia <code>nums</code> thành <code>[2, 2, 3]</code> và <code>[3, 2]</code>, đáp án là <code>2 + 2 = 4</code>.</li>
    <li>Kết quả là <code>[3, 4]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,4], queries = [[0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Ban đầu, <code>nums = [2, 1, 4]</code>.</li>
    <li>Sau truy vấn thứ 1<sup>st</sup>, <code>nums = [1, 1, 4]</code>. <code>nums</code> không có số nguyên tố nào, nên đáp án là 0.</li>
    <li>Kết quả là <code>[0]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n == nums.length &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= queries[i][0] &lt; nums.length</code></li>
    <li><code>1 &lt;= queries[i][1] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau mỗi lần cập nhật, ta muốn tối đa hóa số lượng số nguyên tố phân biệt trong prefix cộng với số lượng trong suffix tương ứng. Một số nguyên tố xuất hiện ở cả hai phía được tính hai lần; nếu chỉ xuất hiện ở một phía thì được tính một lần.
>
> Vì $n,q \le 5 \cdot 10^4$, ta lưu chỉ số ngoài cùng bên trái và bên phải của mỗi số nguyên tố. Lợi ích của một vị trí chia $k$ phụ thuộc vào việc hai đầu mút này có nằm về hai phía của $k$ hay không, và segment tree (hoặc cấu trúc chia để trị) có thể lưu thông tin đó. Mỗi lần cập nhật làm mới các đầu mút của một số nguyên tố và truy vấn giá trị lớn nhất trên toàn bộ mảng.

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
