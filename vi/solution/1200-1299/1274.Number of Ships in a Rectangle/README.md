---
comments: true
difficulty: Hard
rating: 1997
source: Biweekly Contest 14 Q4
tags:
    - Array
    - Divide and Conquer
    - Interactive
---

<!-- problem:start -->

# [1274. Number of Ships in a Rectangle 🔒](https://leetcode.com/problems/number-of-ships-in-a-rectangle)

[中文文档](/solution/1200-1299/1274.Number%20of%20Ships%20in%20a%20Rectangle/README.md)

## Mô tả

<!-- description:start -->

<p><em>(Đây là một <strong>bài toán tương tác</strong>.)</em></p>

<p>Mỗi con tàu nằm tại một điểm tọa độ nguyên trên vùng biển được biểu diễn bằng mặt phẳng Cartesian, và mỗi điểm nguyên có thể có nhiều nhất một con tàu.</p>

<p>Bạn có hàm <code>Sea.hasShips(topRight, bottomLeft)</code> nhận hai điểm làm tham số và trả về <code>true</code> nếu hình chữ nhật được xác định bởi hai điểm đó có ít nhất một con tàu, kể cả trên đường biên.</p>

<p>Cho hai điểm là góc trên bên phải và góc dưới bên trái của một hình chữ nhật, hãy trả về số con tàu nằm trong hình chữ nhật đó. Đảm bảo có <strong>không quá 10 con tàu</strong> trong hình chữ nhật.</p>

<p>Bài nộp gọi <code>hasShips</code> <strong>quá 400 lần</strong> sẽ bị chấm <em>Wrong Answer</em>. Mọi giải pháp tìm cách qua mặt bộ chấm cũng sẽ bị loại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1274.Number%20of%20Ships%20in%20a%20Rectangle/images/1445_example_1.png" style="width: 496px; height: 500px;" />
<pre>
<strong>Đầu vào:</strong> 
ships = [[1,1],[2,2],[3,3],[5,5]], topRight = [4,4], bottomLeft = [0,0]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong phạm vi từ [0,0] đến [4,4], có thể đếm được 3 con tàu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ans = [[1,1],[2,2],[3,3]], topRight = [1000,1000], bottomLeft = [0,0]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Đầu vào <code>ships</code> chỉ được dùng để khởi tạo bản đồ nội bộ. Bạn phải giải bài toán mà không biết trước vị trí tàu; nói cách khác, hãy tìm đáp án bằng API <code>hasShips</code> được cung cấp.</li>
	<li><code>0 &lt;= bottomLeft[0] &lt;= topRight[0] &lt;= 1000</code></li>
	<li><code>0 &lt;= bottomLeft[1] &lt;= topRight[1] &lt;= 1000</code></li>
	<li><code>topRight != bottomLeft</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy + Chia để trị

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ có thể gọi $hasShips$, và mỗi hình chữ nhật chứa nhiều nhất $10$ con tàu. Nếu hình chữ nhật không có tàu thì không cần chia tiếp; nếu chỉ còn một ô có tàu thì đếm được $1$. Trường hợp khác, chia hình chữ nhật thành bốn phần tư. Chia để trị chỉ truy vấn tiếp những phần có tàu; số truy vấn phụ thuộc vào số tàu nhân với logarit của đường kính hình chữ nhật.

<!-- thinking:end -->

Vì hình chữ nhật có nhiều nhất $10$ con tàu, ta có thể chia nó thành bốn hình chữ nhật nhỏ, tính số tàu trong từng phần rồi cộng lại. Nếu một phần không có tàu thì không cần chia tiếp.

Độ phức tạp thời gian là $O(C \times \log \max(m, n))$ và độ phức tạp không gian là $O(\log \max(m, n))$, trong đó $C$ là số tàu, còn $m$ và $n$ lần lượt là chiều dài và chiều rộng của hình chữ nhật.

<!-- tabs:start -->

#### Python3

```python
# """
# This is Sea's API interface.
# You should not implement it, or speculate about its implementation
# """
# class Sea:
#    def hasShips(self, topRight: 'Point', bottomLeft: 'Point') -> bool:
#
# class Point:
# 	def __init__(self, x: int, y: int):
# 		self.x = x
# 		self.y = y


class Solution:
    def countShips(self, sea: "Sea", topRight: "Point", bottomLeft: "Point") -> int:
        def dfs(topRight, bottomLeft):
            x1, y1 = bottomLeft.x, bottomLeft.y
            x2, y2 = topRight.x, topRight.y
            if x1 > x2 or y1 > y2:
                return 0
            if not sea.hasShips(topRight, bottomLeft):
                return 0
            if x1 == x2 and y1 == y2:
                return 1
            midx = (x1 + x2) >> 1
            midy = (y1 + y2) >> 1
            a = dfs(topRight, Point(midx + 1, midy + 1))
            b = dfs(Point(midx, y2), Point(x1, midy + 1))
            c = dfs(Point(midx, midy), bottomLeft)
            d = dfs(Point(x2, midy), Point(midx + 1, y1))
            return a + b + c + d

        return dfs(topRight, bottomLeft)
```

#### Java

```java
/**
 * // This is Sea's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Sea {
 *     public boolean hasShips(int[] topRight, int[] bottomLeft);
 * }
 */

class Solution {
    public int countShips(Sea sea, int[] topRight, int[] bottomLeft) {
        int x1 = bottomLeft[0], y1 = bottomLeft[1];
        int x2 = topRight[0], y2 = topRight[1];
        if (x1 > x2 || y1 > y2) {
            return 0;
        }
        if (!sea.hasShips(topRight, bottomLeft)) {
            return 0;
        }
        if (x1 == x2 && y1 == y2) {
            return 1;
        }
        int midx = (x1 + x2) >> 1;
        int midy = (y1 + y2) >> 1;
        int a = countShips(sea, topRight, new int[] {midx + 1, midy + 1});
        int b = countShips(sea, new int[] {midx, y2}, new int[] {x1, midy + 1});
        int c = countShips(sea, new int[] {midx, midy}, bottomLeft);
        int d = countShips(sea, new int[] {x2, midy}, new int[] {midx + 1, y1});
        return a + b + c + d;
    }
}
```

#### C++

```cpp
/**
 * // This is Sea's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Sea {
 *   public:
 *     bool hasShips(vector<int> topRight, vector<int> bottomLeft);
 * };
 */

class Solution {
public:
    int countShips(Sea sea, vector<int> topRight, vector<int> bottomLeft) {
        int x1 = bottomLeft[0], y1 = bottomLeft[1];
        int x2 = topRight[0], y2 = topRight[1];
        if (x1 > x2 || y1 > y2) {
            return 0;
        }
        if (!sea.hasShips(topRight, bottomLeft)) {
            return 0;
        }
        if (x1 == x2 && y1 == y2) {
            return 1;
        }
        int midx = (x1 + x2) >> 1;
        int midy = (y1 + y2) >> 1;
        int a = countShips(sea, topRight, {midx + 1, midy + 1});
        int b = countShips(sea, {midx, y2}, {x1, midy + 1});
        int c = countShips(sea, {midx, midy}, bottomLeft);
        int d = countShips(sea, {x2, midy}, {midx + 1, y1});
        return a + b + c + d;
    }
};
```

#### Go

```go
/**
 * // This is Sea's API interface.
 * // You should not implement it, or speculate about its implementation
 * type Sea struct {
 *     func hasShips(topRight, bottomLeft []int) bool {}
 * }
 */

func countShips(sea Sea, topRight, bottomLeft []int) int {
	x1, y1 := bottomLeft[0], bottomLeft[1]
	x2, y2 := topRight[0], topRight[1]
	if x1 > x2 || y1 > y2 {
		return 0
	}
	if !sea.hasShips(topRight, bottomLeft) {
		return 0
	}
	if x1 == x2 && y1 == y2 {
		return 1
	}
	midx := (x1 + x2) >> 1
	midy := (y1 + y2) >> 1
	a := countShips(sea, topRight, []int{midx + 1, midy + 1})
	b := countShips(sea, []int{midx, y2}, []int{x1, midy + 1})
	c := countShips(sea, []int{midx, midy}, bottomLeft)
	d := countShips(sea, []int{x2, midy}, []int{midx + 1, y1})
	return a + b + c + d
}
```

#### TypeScript

```ts
/**
 * // This is the Sea's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Sea {
 *      hasShips(topRight: number[], bottomLeft: number[]): boolean {}
 * }
 */

function countShips(sea: Sea, topRight: number[], bottomLeft: number[]): number {
    const [x1, y1] = bottomLeft;
    const [x2, y2] = topRight;
    if (x1 > x2 || y1 > y2 || !sea.hasShips(topRight, bottomLeft)) {
        return 0;
    }
    if (x1 === x2 && y1 === y2) {
        return 1;
    }
    const midx = (x1 + x2) >> 1;
    const midy = (y1 + y2) >> 1;
    const a = countShips(sea, topRight, [midx + 1, midy + 1]);
    const b = countShips(sea, [midx, y2], [x1, midy + 1]);
    const c = countShips(sea, [midx, midy], bottomLeft);
    const d = countShips(sea, [x2, midy], [midx + 1, y1]);
    return a + b + c + d;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
