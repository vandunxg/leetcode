---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [4029. Elevator Requests IV 🔒](https://leetcode.com/problems/elevator-requests-iv)

[Tài liệu tiếng Trung](/solution/4000-4099/4029.Elevator%20Requests%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> biểu thị số tầng của một tòa nhà, trong đó các tầng được đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Cho một số nguyên <code>start</code> và một mảng số nguyên 2D <code>requests</code>, trong đó <code>requests[i] = [arrival<sub>i</sub>, floor<sub>i</sub>]</code> cho biết yêu cầu đến tầng <code>floor<sub>i</sub></code> được tạo ra tại thời điểm <code>arrival<sub>i</sub></code>.</p>

<p>Tại thời điểm 0, thang máy đang ở tầng <code>start</code>.</p>

<p>Mỗi giây, thang máy có thể đi <strong>lên</strong> 1 tầng, đi <strong>xuống</strong> 1 tầng hoặc <strong>đứng yên</strong> tại tầng hiện tại.</p>

<p>Một yêu cầu chỉ có thể được đáp ứng <strong>tại hoặc sau</strong> thời điểm yêu cầu đến; yêu cầu được đáp ứng <strong>ngay lập tức</strong> khi thang máy ở tầng được yêu cầu tại bất kỳ thời điểm nào từ thời điểm yêu cầu đến trở đi.</p>

<p>Trả về thời gian <strong>nhỏ nhất</strong> cần để đáp ứng tất cả các yêu cầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 9, start = 0, requests = [[0,8],[6,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Di chuyển từ tầng 0 (<code>start</code>) đến tầng 5 (<code>requests[1][1]</code>) trong 5 giây, đến nơi tại thời điểm 5. Vì <code>requests[1][0] = 6</code>, chờ đến thời điểm 6 để đáp ứng yêu cầu.</li>
	<li>Di chuyển từ tầng 5 đến tầng 8 (<code>requests[0][1]</code>) trong 3 giây, đáp ứng yêu cầu tại thời điểm 9.</li>
</ul>

<p>Vậy tất cả các yêu cầu được đáp ứng trước hoặc tại thời điểm 9.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 8, start = 5, requests = [[1,7],[7,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Di chuyển từ tầng 5 (<code>start</code>) đến tầng 7 (<code>requests[0][1]</code>) trong 2 giây, đến nơi tại thời điểm 2. Vì <code>requests[0][0] = 1</code> đã trôi qua, yêu cầu ở tầng 7 được đáp ứng tại thời điểm 2.</li>
	<li>Di chuyển từ tầng 7 đến tầng 3 (<code>requests[1][1]</code>) trong 4 giây, đến nơi tại thời điểm 6. Vì <code>requests[1][0] = 7</code>, chờ đến thời điểm 7.</li>
</ul>

<p>Vậy tất cả các yêu cầu được đáp ứng trước hoặc tại thời điểm 7.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7, start = 3, requests = [[0,5],[0,1],[6,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Di chuyển từ tầng 3 (<code>start</code>) đến tầng 5 (<code>requests[0][1]</code>) trong 2 giây, đáp ứng yêu cầu tại thời điểm 2.</li>
	<li>Di chuyển từ tầng 5 đến tầng 1 (<code>requests[1][1]</code>) trong 4 giây, đáp ứng yêu cầu tại thời điểm 6.</li>
	<li>Di chuyển từ tầng 1 đến tầng 3 (<code>requests[2][1]</code>) trong 2 giây, đến nơi tại thời điểm 8. Yêu cầu của tầng này đến tại <code>requests[2][0] = 6</code>, nên tầng 3 được đáp ứng tại thời điểm 8.</li>
</ul>

<p>Vậy tất cả các yêu cầu được đáp ứng trước hoặc tại thời điểm 8.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= requests.length &lt;= 500</code></li>
	<li><code>requests[i] == [arrival<sub>i</sub>, floor<sub>i</sub>]</code></li>
	<li><code>0 &lt;= arrival<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= start, floor<sub>i</sub> &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các yêu cầu có thời điểm đến và thang máy có thể phải chờ. Vì $m\le 500$, không thể dùng subset DP với $2^m$. Một yêu cầu được hoàn thành tại thời điểm $\max(\text{time of arrival at that floor},\textit{arrival})$, và ta cần tìm thời điểm yêu cầu cuối cùng được hoàn thành.
>
> Các tầng vẫn nằm trên một đường thẳng, nên một thứ tự xử lý là một chuỗi các lần di chuyển xen kẽ với những khoảng chờ bắt buộc. Sau khi sắp xếp các yêu cầu, có thể dùng DP $O(m^2)$ lưu các đầu mút đã xử lý (hoặc một tiền tố đã xử lý và tầng hiện tại), đồng thời tính chi phí di chuyển và chờ trong mỗi chuyển tiếp.
>
> Không cần đưa chỉ số tầng thô vào trạng thái; chỉ cần khoảng cách giữa các yêu cầu.

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
