---
comments: true
difficulty: Easy
rating: 1162
source: Weekly Contest 413 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [3274. Check if Two Chessboard Squares Have the Same Color](https://leetcode.com/problems/check-if-two-chessboard-squares-have-the-same-color)

[中文文档](/solution/3200-3299/3274.Check%20if%20Two%20Chessboard%20Squares%20Have%20the%20Same%20Color/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>coordinate1</code> và <code>coordinate2</code>, lần lượt biểu diễn tọa độ của một ô trên bàn cờ vua <code>8 x 8</code>.</p>

<p>Dưới đây là hình bàn cờ để tham khảo.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3274.Check%20if%20Two%20Chessboard%20Squares%20Have%20the%20Same%20Color/images/screenshot-2021-02-20-at-22159-pm.png" style="width: 400px; height: 396px;" /></p>

<p>Trả về <code>true</code> nếu hai ô này có cùng màu và <code>false</code> nếu ngược lại.</p>

<p>Tọa độ luôn biểu diễn một ô hợp lệ trên bàn cờ. Tọa độ luôn có chữ cái đứng trước (biểu thị cột), sau đó là chữ số (biểu thị hàng).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coordinate1 = &quot;a1&quot;, coordinate2 = &quot;c3&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cả hai ô đều màu đen.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coordinate1 = &quot;a1&quot;, coordinate2 = &quot;h3&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ô <code>&quot;a1&quot;</code> màu đen còn ô <code>&quot;h3&quot;</code> màu trắng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>coordinate1.length == coordinate2.length == 2</code></li>
	<li><code>&#39;a&#39; &lt;= coordinate1[0], coordinate2[0] &lt;= &#39;h&#39;</code></li>
	<li><code>&#39;1&#39; &lt;= coordinate1[1], coordinate2[1] &lt;= &#39;8&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Bàn cờ xen kẽ các màu; màu của $(c,r)$ được xác định bởi tính chẵn lẻ của tổng cột và hàng. Hai ô có cùng màu khi tính chẵn lẻ này giống nhau.
>
> Nếu tổng độ chênh lệch của cột và hàng là số chẵn, hai ô sẽ có cùng màu. Chỉ cần lấy hiệu của hai ký tự rồi tính modulo $2$, nên thời gian thực hiện là hằng số.

<!-- thinking:end -->

Ta tính độ chênh lệch tọa độ x và tọa độ y của hai điểm. Nếu tổng hai độ chênh lệch này là số chẵn, hai ô tại hai tọa độ tương ứng sẽ có cùng màu; nếu không, chúng có màu khác nhau.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkTwoChessboards(self, coordinate1: str, coordinate2: str) -> bool:
        x = ord(coordinate1[0]) - ord(coordinate2[0])
        y = int(coordinate1[1]) - int(coordinate2[1])
        return (x + y) % 2 == 0
```

#### Java

```java
class Solution {
    public boolean checkTwoChessboards(String coordinate1, String coordinate2) {
        int x = coordinate1.charAt(0) - coordinate2.charAt(0);
        int y = coordinate1.charAt(1) - coordinate2.charAt(1);
        return (x + y) % 2 == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkTwoChessboards(string coordinate1, string coordinate2) {
        int x = coordinate1[0] - coordinate2[0];
        int y = coordinate1[1] - coordinate2[1];
        return (x + y) % 2 == 0;
    }
};
```

#### Go

```go
func checkTwoChessboards(coordinate1 string, coordinate2 string) bool {
	x := coordinate1[0] - coordinate2[0]
	y := coordinate1[1] - coordinate2[1]
	return (x+y)%2 == 0
}
```

#### TypeScript

```ts
function checkTwoChessboards(coordinate1: string, coordinate2: string): boolean {
    const x = coordinate1.charCodeAt(0) - coordinate2.charCodeAt(0);
    const y = coordinate1.charCodeAt(1) - coordinate2.charCodeAt(1);
    return (x + y) % 2 === 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
