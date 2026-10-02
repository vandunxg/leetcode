---
comments: true
difficulty: Easy
rating: 1407
source: Weekly Contest 157 Q1
tags:
    - Greedy
    - Array
    - Math
---

<!-- problem:start -->

# [1217. Minimum Cost to Move Chips to The Same Position](https://leetcode.com/problems/minimum-cost-to-move-chips-to-the-same-position)

[中文文档](/solution/1200-1299/1217.Minimum%20Cost%20to%20Move%20Chips%20to%20The%20Same%20Position/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> chip, trong đó vị trí của chip thứ <code>i<sup>th</sup></code> là <code>position[i]</code>.</p>

<p>Ta cần đưa tất cả chip về <strong>cùng một vị trí</strong>. Trong một bước, có thể đổi vị trí của chip thứ <code>i<sup>th</sup></code> từ <code>position[i]</code> thành:</p>

<ul>
	<li><code>position[i] + 2</code> hoặc <code>position[i] - 2</code> với <code>cost = 0</code>.</li>
	<li><code>position[i] + 1</code> hoặc <code>position[i] - 1</code> với <code>cost = 1</code>.</li>
</ul>

<p>Trả về <em>chi phí nhỏ nhất</em> cần thiết để đưa tất cả chip về cùng một vị trí.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1217.Minimum%20Cost%20to%20Move%20Chips%20to%20The%20Same%20Position/images/chips_e1.jpg" style="width: 750px; height: 217px;" />
<pre>
<strong>Đầu vào:</strong> position = [1,2,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bước đầu tiên: Di chuyển chip ở vị trí 3 đến vị trí 1 với cost = 0.
Bước thứ hai: Di chuyển chip ở vị trí 2 đến vị trí 1 với cost = 1.
Tổng chi phí là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1217.Minimum%20Cost%20to%20Move%20Chips%20to%20The%20Same%20Position/images/chip_e2.jpg" style="width: 750px; height: 306px;" />
<pre>
<strong>Đầu vào:</strong> position = [2,2,2,3,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể di chuyển hai chip ở vị trí 3 đến vị trí 2. Mỗi lần di chuyển tốn cost = 1. Tổng chi phí = 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> position = [1,1000000000]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= position.length &lt;= 100</code></li>
	<li><code>1 &lt;= position[i] &lt;= 10^9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhận xét nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Di chuyển $2$ vị trí có chi phí $0$, nên có thể di chuyển miễn phí giữa mọi vị trí chẵn, và tương tự với các vị trí lẻ. Chỉ phải trả phí khi di chuyển $1$ vị trí để đổi giữa hai loại chẵn lẻ.
>
> Ta gom chip về một vị trí chẵn và một vị trí lẻ với chi phí $0$, sau đó chuyển nhóm ít chip hơn sang vị trí còn lại. Đáp án là giá trị nhỏ hơn giữa số chip ở vị trí lẻ và số chip ở vị trí chẵn.

<!-- thinking:end -->

Di chuyển tất cả chip ở vị trí chẵn về vị trí 0 và chip ở vị trí lẻ về vị trí 1, đều không tốn chi phí. Sau đó, chọn vị trí (0 hoặc 1) có ít chip hơn và chuyển số chip đó sang vị trí còn lại. Chi phí nhỏ nhất cần thiết chính là số chip ít hơn này.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là số chip.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCostToMoveChips(self, position: List[int]) -> int:
        a = sum(p % 2 for p in position)
        b = len(position) - a
        return min(a, b)
```

#### Java

```java
class Solution {
    public int minCostToMoveChips(int[] position) {
        int a = 0;
        for (int p : position) {
            a += p % 2;
        }
        int b = position.length - a;
        return Math.min(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCostToMoveChips(vector<int>& position) {
        int a = 0;
        for (auto& p : position) a += p & 1;
        int b = position.size() - a;
        return min(a, b);
    }
};
```

#### Go

```go
func minCostToMoveChips(position []int) int {
	a := 0
	for _, p := range position {
		a += p & 1
	}
	b := len(position) - a
	if a < b {
		return a
	}
	return b
}
```

#### JavaScript

```js
/**
 * @param {number[]} position
 * @return {number}
 */
var minCostToMoveChips = function (position) {
    let a = 0;
    for (let v of position) {
        a += v % 2;
    }
    let b = position.length - a;
    return Math.min(a, b);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
