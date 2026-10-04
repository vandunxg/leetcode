---
comments: true
difficulty: Hard
rating: 2231
source: Biweekly Contest 124 Q4
tags:
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3041. Maximize Consecutive Elements in an Array After Modification](https://leetcode.com/problems/maximize-consecutive-elements-in-an-array-after-modification)

[中文文档](/solution/3000-3099/3041.Maximize%20Consecutive%20Elements%20in%20an%20Array%20After%20Modification/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>dương</strong> <code>nums</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Ban đầu, bạn có thể tăng giá trị của <strong>bất kỳ</strong> phần tử nào trong mảng <strong>nhiều nhất</strong> <code>1</code>.</p>

<p>Sau đó, bạn cần chọn <strong>một hoặc nhiều</strong> phần tử từ mảng cuối cùng sao cho các phần tử đó <strong>liên tiếp</strong> khi được sắp xếp theo thứ tự tăng dần. Ví dụ, các phần tử <code>[3, 4, 5]</code> là liên tiếp, còn <code>[3, 4, 6]</code> và <code>[1, 1, 2, 3]</code> thì không.<!-- notionvc: 312f8c5d-40d0-4cd1-96cc-9e96a846735b --></p>

<p>Trả về <em>số lượng phần tử <strong>lớn nhất</strong> có thể chọn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,5,1,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể tăng các phần tử tại chỉ số 0 và 3. Mảng sau khi biến đổi là nums = [3,1,5,2,1].
Ta chọn các phần tử [<u><strong>3</strong></u>,<u><strong>1</strong></u>,5,<u><strong>2</strong></u>,1] rồi sắp xếp chúng thành [1,2,3], là các phần tử liên tiếp.
Có thể chứng minh rằng ta không thể chọn nhiều hơn 3 phần tử liên tiếp.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,7,10]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Số lượng phần tử liên tiếp lớn nhất mà ta có thể chọn là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phần tử có thể tăng nhiều nhất một đơn vị, sau đó ta chọn dãy con dài nhất có thể trở thành dãy liên tiếp. $n \le 10^5$ và các giá trị có thể đạt tới $10^6$.
>
> Sau khi sắp xếp, hai phần tử kề nhau có thể bằng nhau, chênh nhau một đơn vị hoặc cách nhau xa hơn. Phép cộng một đơn vị ánh xạ một giá trị thành $x$ hoặc $x+1$, nên DP chỉ cần biết phần tử cuối hiện tại vẫn là $x$ hay đã trở thành $x+1$.
>
> Ta duyệt các giá trị theo thứ tự và lưu độ dài dãy liên tiếp tốt nhất kết thúc tại $x$ và tại $x+1$, cập nhật dựa trên việc khoảng cách là $0$, $1$ hay ít nhất là $2$.

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
