---
comments: true
difficulty: Easy
rating: 1140
source: Weekly Contest 442 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3492. Maximum Containers on a Ship](https://leetcode.com/problems/maximum-containers-on-a-ship)

[中文文档](/solution/3400-3499/3492.Maximum%20Containers%20on%20a%20Ship/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>, biểu diễn một boong chở hàng kích thước <code>n x n</code> trên một con tàu. Mỗi ô trên boong có thể chứa một container có trọng lượng <strong>đúng bằng</strong> <code>w</code>.</p>

<p>Tuy nhiên, tổng trọng lượng của tất cả container khi được xếp lên boong không được vượt quá sức chứa tối đa của con tàu là <code>maxWeight</code>.</p>

<p>Trả về số lượng container <strong>lớn nhất</strong> có thể xếp lên tàu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, w = 3, maxWeight = 15</span></p>

<p><strong>Đầu ra:</strong> 4</p>

<p><strong>Giải thích: </strong></p>

<p>Boong tàu có 4 ô và mỗi container nặng 3. Tổng trọng lượng khi xếp tất cả container là 12, không vượt quá <code>maxWeight</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, w = 5, maxWeight = 20</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích: </strong></p>

<p>Boong tàu có 9 ô và mỗi container nặng 5. Số container lớn nhất có thể xếp lên tàu mà không vượt quá <code>maxWeight</code> là 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= w &lt;= 1000</code></li>
	<li><code>1 &lt;= maxWeight &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Boong tàu có $n^2$ ô, mỗi ô chứa một container có trọng lượng $w$, với tổng trọng lượng không vượt quá $\textit{maxWeight}$.
>
> Số lượng bị giới hạn bởi cả số ô và trọng lượng, do đó $\min(n^2,\lfloor \textit{maxWeight}/w\rfloor)=\lfloor\min(n^2 w,\textit{maxWeight})/w\rfloor$.
>
> Chỉ cần phép lấy min và phép chia nguyên trong thời gian O(1); không cần mô phỏng việc xếp container.

<!-- thinking:end -->

Trước tiên, ta tính trọng lượng tối đa mà con tàu có thể chở, là $n \times n \times w$. Sau đó, ta lấy giá trị nhỏ hơn giữa giá trị này và $\text{maxWeight}$, rồi chia cho $w$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxContainers(self, n: int, w: int, maxWeight: int) -> int:
        return min(n * n * w, maxWeight) // w
```

#### Java

```java
class Solution {
    public int maxContainers(int n, int w, int maxWeight) {
        return Math.min(n * n * w, maxWeight) / w;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxContainers(int n, int w, int maxWeight) {
        return min(n * n * w, maxWeight) / w;
    }
};
```

#### Go

```go
func maxContainers(n int, w int, maxWeight int) int {
	return min(n*n*w, maxWeight) / w
}
```

#### TypeScript

```ts
function maxContainers(n: number, w: number, maxWeight: number): number {
    return (Math.min(n * n * w, maxWeight) / w) | 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
