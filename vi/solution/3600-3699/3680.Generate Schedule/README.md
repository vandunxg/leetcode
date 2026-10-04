---
comments: true
difficulty: Medium
rating: 2377
source: Biweekly Contest 165 Q3
tags:
    - Greedy
    - Array
    - Math
---

<!-- problem:start -->

# [3680. Generate Schedule](https://leetcode.com/problems/generate-schedule)

[中文文档](/solution/3600-3699/3680.Generate%20Schedule/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> biểu thị <code>n</code> đội. Hãy tạo một lịch thi đấu sao cho:</p>

<ul>
	<li>Mỗi đội đấu với mọi đội khác <strong>đúng hai lần</strong>: một lần trên sân nhà và một lần trên sân khách.</li>
	<li>Có <strong>đúng một</strong> trận đấu mỗi ngày; lịch thi đấu là một danh sách các ngày <strong>liên tiếp</strong> và <code>schedule[i]</code> là trận đấu vào ngày <code>i</code>.</li>
	<li>Không đội nào thi đấu trong hai ngày <strong>liên tiếp</strong>.</li>
</ul>

<p>Trả về một mảng số nguyên 2 chiều <code>schedule</code>, trong đó <code>schedule[i][0]</code> là đội chủ nhà và <code>schedule[i][1]</code> là đội khách. Nếu có nhiều lịch thi đấu thỏa mãn các điều kiện, hãy trả về <strong>bất kỳ</strong> lịch nào trong số đó.</p>

<p>Nếu không tồn tại lịch thi đấu nào thỏa mãn các điều kiện, hãy trả về một mảng rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>​​​​​​​Vì mỗi đội đấu với mọi đội khác đúng hai lần, tổng cộng cần thi đấu 6 trận: <code>[0,1],[0,2],[1,2],[1,0],[2,0],[2,1]</code>.</p>

<p>Không thể tạo lịch thi đấu mà không có ít nhất một đội thi đấu trong hai ngày liên tiếp.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[0,1],[2,3],[0,4],[1,2],[3,4],[0,2],[1,3],[2,4],[0,3],[1,4],[2,0],[3,1],[4,0],[2,1],[4,3],[1,0],[3,2],[4,1],[3,0],[4,2]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì mỗi đội đấu với mọi đội khác đúng hai lần, tổng cộng cần thi đấu 20 trận.</p>

<p>Kết quả cho thấy một lịch thi đấu thỏa mãn các điều kiện. Không đội nào thi đấu trong hai ngày liên tiếp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 50</code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xây dựng một vòng tròn thi đấu một lượt cho $n$ đội sao cho hai trận liên tiếp không có đội nào trùng nhau. $n\le 50$ cho phép dùng phương pháp vòng tròn kết hợp với việc xáo trộn các vòng đấu.
>
> Liệt kê mọi cặp $(i,j)$, nhóm chúng thành các vòng đấu có các đội không trùng nhau, sau đó nối các vòng đấu với một phép xoay để ở ranh giới giữa hai vòng không lặp lại đội nào.
>
> Với $n$ nhỏ, có thể không tồn tại đáp án nên trả về mảng rỗng. Cách xây dựng này ghép mỗi cặp đúng một lần và giữ cho các trận liền kề không có đội chung.

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
