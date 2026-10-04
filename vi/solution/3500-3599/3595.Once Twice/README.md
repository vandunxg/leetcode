---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [3595. Once Twice 🔒](https://leetcode.com/problems/once-twice)

[中文文档](/solution/3500-3599/3595.Once%20Twice/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code>. Trong mảng này:</p>

<ul>
	<li>
	<p>Chính xác một phần tử xuất hiện <strong>một lần</strong>.</p>
	</li>
	<li>
	<p>Chính xác một phần tử xuất hiện <strong>hai lần</strong>.</p>
	</li>
	<li>
	<p>Tất cả các phần tử còn lại xuất hiện <strong>chính xác ba lần</strong>.</p>
	</li>
</ul>

<p>Trả về một mảng số nguyên có độ dài 2, trong đó phần tử đầu tiên là phần tử xuất hiện <strong>một lần</strong>, còn phần tử thứ hai là phần tử xuất hiện <strong>hai lần</strong>.</p>

<p>Lời giải của bạn phải chạy trong thời gian <strong>O(n)</strong> và sử dụng <strong>O(1)</strong> không gian.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,3,2,5,5,5,7,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,7]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phần tử 3 xuất hiện <b>một lần</b>, còn phần tử 7 xuất hiện <b>hai lần</b>. Các phần tử còn lại đều xuất hiện <b>ba lần</b>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4,6,4,9,9,9,6,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[8,6]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phần tử 8 xuất hiện <b>một lần</b>, còn phần tử 6 xuất hiện <b>hai lần</b>. Các phần tử còn lại đều xuất hiện <b>ba lần</b>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>nums.length</code> là bội số của 3.</li>
	<li>Chính xác một phần tử xuất hiện một lần, một phần tử xuất hiện hai lần, và tất cả các phần tử còn lại xuất hiện ba lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tất cả các giá trị đều xuất hiện ba lần, ngoại trừ một giá trị xuất hiện một lần và một giá trị xuất hiện hai lần, trong khi lời giải phải có thời gian tuyến tính và không gian phụ hằng số, nên không thể dùng hash map. Các bit theo modulo $3$ sẽ phân tách hai giá trị đặc biệt.
>
> Hai mask tích lũy các bit xuất hiện $1 \bmod 3$ và $2 \bmod 3$. Sau khi duyệt xong, chúng chính là hai đáp án. Biểu diễn bù hai xử lý được số âm.

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
