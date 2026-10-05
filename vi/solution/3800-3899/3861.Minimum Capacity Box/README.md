---
comments: true
difficulty: Easy
rating: 1154
source: Weekly Contest 492 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3861. Minimum Capacity Box](https://leetcode.com/problems/minimum-capacity-box)

[中文文档](/solution/3800-3899/3861.Minimum%20Capacity%20Box/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>capacity</code>, trong đó <code>capacity[i]</code> biểu thị sức chứa của hộp thứ <code>i<sup>th</sup></code>, và một số nguyên <code>itemSize</code> biểu thị kích thước của một món đồ.</p>

<p>Hộp thứ <code>i<sup>th</sup></code> có thể chứa món đồ nếu <code>capacity[i] &gt;= itemSize</code>.</p>

<p>Hãy trả về một số nguyên biểu thị chỉ số của hộp có sức chứa <strong>nhỏ nhất</strong> nhưng vẫn có thể chứa món đồ. Nếu có nhiều hộp như vậy, trả về <strong>chỉ số nhỏ nhất</strong>.</p>

<p>Nếu không có hộp nào có thể chứa món đồ, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">capacity = [1,5,3,7], itemSize = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hộp ở chỉ số 2 có sức chứa bằng 3, là sức chứa nhỏ nhất có thể chứa món đồ. Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">capacity = [3,5,4,3], itemSize = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sức chứa nhỏ nhất có thể chứa món đồ là 3, và giá trị này xuất hiện ở các chỉ số 0 và 3. Vì vậy, đáp án là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">capacity = [4], itemSize = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có hộp nào có đủ sức chứa để chứa món đồ, nên đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= capacity.length &lt;= 100</code></li>
	<li><code>1 &lt;= capacity[i] &lt;= 100</code></li>
	<li><code>1 &lt;= itemSize &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Trong số các hộp có sức chứa $\ge \textit{itemSize}$, ta chọn hộp có sức chứa nhỏ nhất, nếu bằng nhau thì chọn chỉ số nhỏ nhất. Vì độ dài mảng $\le 100$, ta chỉ cần duyệt một lần.
>
> Không cần sắp xếp: chỉ cần lưu chỉ số tốt nhất hiện tại.
>
> Cập nhật khi $x \ge \textit{itemSize}$ và (chưa chọn hộp nào hoặc $x$ nhỏ hơn). Việc duyệt từ trái sang phải đã đảm bảo ưu tiên chỉ số nhỏ hơn khi sức chứa bằng nhau.
>
> Nếu không chọn được hộp nào, trả về $-1$.

<!-- thinking:end -->

Ta khởi tạo biến $\textit{ans}$ để lưu chỉ số của hộp có sức chứa nhỏ nhất nhưng vẫn có thể chứa món đồ, với giá trị ban đầu là $-1$. Ta duyệt mảng $\textit{capacity}$ và với mỗi hộp, nếu sức chứa của hộp lớn hơn hoặc bằng $\textit{itemSize}$ thì hộp đó có thể chứa món đồ. Khi đó, ta kiểm tra xem đây có phải là hộp có sức chứa nhỏ nhất trong số các hộp đã gặp hay không; nếu đúng, ta cập nhật $\textit{ans}$. Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{capacity}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumIndex(self, capacity: list[int], itemSize: int) -> int:
        ans = -1
        for i, x in enumerate(capacity):
            if x >= itemSize and (ans == -1 or x < capacity[ans]):
                ans = i
        return ans
```

#### Java

```java
class Solution {
    public int minimumIndex(int[] capacity, int itemSize) {
        int ans = -1;
        for (int i = 0; i < capacity.length; ++i) {
            int x = capacity[i];
            if (x >= itemSize && (ans == -1 || x < capacity[ans])) {
                ans = i;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumIndex(vector<int>& capacity, int itemSize) {
        int ans = -1;
        for (int i = 0; i < capacity.size(); ++i) {
            int x = capacity[i];
            if (x >= itemSize && (ans == -1 || x < capacity[ans])) {
                ans = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumIndex(capacity []int, itemSize int) int {
	ans := -1
	for i, x := range capacity {
		if x >= itemSize && (ans == -1 || x < capacity[ans]) {
			ans = i
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumIndex(capacity: number[], itemSize: number): number {
    let ans = -1;
    for (let i = 0; i < capacity.length; ++i) {
        const x = capacity[i];
        if (x >= itemSize && (ans === -1 || x < capacity[ans])) {
            ans = i;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
