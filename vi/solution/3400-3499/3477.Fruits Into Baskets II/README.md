---
comments: true
difficulty: Easy
rating: 1295
source: Weekly Contest 440 Q1
tags:
    - Segment Tree
    - Array
    - Binary Search
    - Ordered Set
    - Simulation
---

<!-- problem:start -->

# [3477. Fruits Into Baskets II](https://leetcode.com/problems/fruits-into-baskets-ii)

[中文文档](/solution/3400-3499/3477.Fruits%20Into%20Baskets%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>fruits</code> và <code>baskets</code>, mỗi mảng có độ dài <code>n</code>, trong đó <code>fruits[i]</code> biểu thị <strong>số lượng</strong> của loại trái cây thứ <code>i<sup>th</sup></code>, còn <code>baskets[j]</code> biểu thị <strong>sức chứa</strong> của giỏ thứ <code>j<sup>th</sup></code>.</p>

<p>Từ trái sang phải, hãy xếp trái cây theo các quy tắc sau:</p>

<ul>
	<li>Mỗi loại trái cây phải được xếp vào <strong>giỏ còn trống ở vị trí ngoài cùng bên trái</strong> có sức chứa <strong>lớn hơn hoặc bằng</strong> số lượng của loại trái cây đó.</li>
	<li>Mỗi giỏ chỉ có thể chứa <b>một</b> loại trái cây.</li>
	<li>Nếu một loại trái cây <b>không thể được xếp</b> vào bất kỳ giỏ nào, loại trái cây đó vẫn <b>chưa được xếp</b>.</li>
</ul>

<p>Trả về số loại trái cây vẫn chưa được xếp sau khi hoàn tất mọi lần phân bổ có thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">fruits = [4,2,5], baskets = [3,5,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>fruits[0] = 4</code> được xếp vào <code>baskets[1] = 5</code>.</li>
	<li><code>fruits[1] = 2</code> được xếp vào <code>baskets[0] = 3</code>.</li>
	<li><code>fruits[2] = 5</code> không thể được xếp vào <code>baskets[2] = 4</code>.</li>
</ul>

<p>Vì còn một loại trái cây chưa được xếp, ta trả về 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">fruits = [3,6,1], baskets = [6,4,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>fruits[0] = 3</code> được xếp vào <code>baskets[0] = 6</code>.</li>
	<li><code>fruits[1] = 6</code> không thể được xếp vào <code>baskets[1] = 4</code> (không đủ sức chứa), nhưng có thể được xếp vào giỏ còn trống tiếp theo là <code>baskets[2] = 7</code>.</li>
	<li><code>fruits[2] = 1</code> được xếp vào <code>baskets[1] = 4</code>.</li>
</ul>

<p>Vì tất cả trái cây đều được xếp thành công, ta trả về 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == fruits.length == baskets.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= fruits[i], baskets[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi loại trái cây được xếp vào giỏ chưa sử dụng ở vị trí ngoài cùng bên trái có thể chứa loại trái cây đó. Với các giới hạn đã cho, ta có thể duyệt qua mọi giỏ cho từng loại trái cây.
>
> Các giỏ đã sử dụng phải được đánh dấu, nếu không cùng một giỏ sẽ bị sử dụng lại.
>
> Một mảng boolean $\textit{vis}$ ghi nhận trạng thái sử dụng của các giỏ. Ta thử trái cây và các giỏ đều từ trái sang phải; nếu không tìm được giỏ phù hợp, ta tăng số lượng chưa được xếp.

<!-- thinking:end -->

Ta dùng một mảng boolean $\textit{vis}$ có độ dài $n$ để ghi nhận các giỏ đã được sử dụng, cùng một biến $\textit{ans}$ để ghi nhận số trái cây chưa được xếp, ban đầu $\textit{ans} = n$.

Tiếp theo, ta duyệt qua từng loại trái cây $x$. Với loại trái cây hiện tại, ta duyệt qua tất cả các giỏ để tìm giỏ chưa được sử dụng đầu tiên $i$ có sức chứa lớn hơn hoặc bằng $x$. Nếu tìm thấy, ta giảm $\textit{ans}$ đi $1$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{fruits}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfUnplacedFruits(self, fruits: List[int], baskets: List[int]) -> int:
        n = len(fruits)
        vis = [False] * n
        ans = n
        for x in fruits:
            for i, y in enumerate(baskets):
                if y >= x and not vis[i]:
                    vis[i] = True
                    ans -= 1
                    break
        return ans
```

#### Java

```java
class Solution {
    public int numOfUnplacedFruits(int[] fruits, int[] baskets) {
        int n = fruits.length;
        boolean[] vis = new boolean[n];
        int ans = n;
        for (int x : fruits) {
            for (int i = 0; i < n; ++i) {
                if (baskets[i] >= x && !vis[i]) {
                    vis[i] = true;
                    --ans;
                    break;
                }
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
    int numOfUnplacedFruits(vector<int>& fruits, vector<int>& baskets) {
        int n = fruits.size();
        vector<bool> vis(n);
        int ans = n;
        for (int x : fruits) {
            for (int i = 0; i < n; ++i) {
                if (baskets[i] >= x && !vis[i]) {
                    vis[i] = true;
                    --ans;
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numOfUnplacedFruits(fruits []int, baskets []int) int {
	n := len(fruits)
	ans := n
	vis := make([]bool, n)
	for _, x := range fruits {
		for i, y := range baskets {
			if y >= x && !vis[i] {
				vis[i] = true
				ans--
				break
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function numOfUnplacedFruits(fruits: number[], baskets: number[]): number {
    const n = fruits.length;
    const vis: boolean[] = Array(n).fill(false);
    let ans = n;
    for (const x of fruits) {
        for (let i = 0; i < n; ++i) {
            if (baskets[i] >= x && !vis[i]) {
                vis[i] = true;
                --ans;
                break;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
