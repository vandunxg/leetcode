---
comments: true
difficulty: Medium
rating: 1244
source: Biweekly Contest 190 Q1
---

<!-- problem:start -->

# [4034. Minimum Bishop Moves to Reach Target](https://leetcode.com/problems/minimum-bishop-moves-to-reach-target)

[中文文档](/solution/4000-4099/4034.Minimum%20Bishop%20Moves%20to%20Reach%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bàn cờ trống <code>8 x 8</code> với các hàng và cột được đánh số từ <strong>1</strong>.</p>

<p>Bạn được cho một mảng <code>source = [sr, sc]</code> biểu diễn vị trí ban đầu của một <strong>tượng</strong>, và một mảng <code>target = [tr, tc]</code> biểu diễn vị trí đích.</p>

<p>Trong một nước đi, tượng di chuyển một hoặc nhiều ô theo một hướng <strong>đường chéo</strong> duy nhất và luôn nằm trong bàn cờ.</p>

<p>Trả về số nước đi <strong>nhỏ nhất</strong> để tượng đáp <strong>chính xác</strong> tại <code>target</code>. Nếu không thể đến <code>target</code>, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [8,1], target = [1,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong>​​​​​​​</strong><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4034.Minimum%20Bishop%20Moves%20to%20Reach%20Target/images/image.png" style="width: 300px; height: 307px;" /></p>

<p>Một nước đi theo đường chéo đưa tượng đi thẳng từ <code>(8, 1)</code> đến <code>(1, 8)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [4,2], target = [1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4034.Minimum%20Bishop%20Moves%20to%20Reach%20Target/images/screenshot-2026-07-23-at-23625am.png" style="width: 300px; height: 305px;" /></p>

<p>Tượng di chuyển từ <code>(4, 2)</code> đến <code>(3, 1)</code>, sau đó từ <code>(3, 1)</code> đến <code>(1, 3)</code>, đến đích sau 2 nước đi.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [1,1], target = [3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dù thực hiện bao nhiêu nước đi theo đường chéo, tượng bắt đầu tại <code>(1, 1)</code> cũng không thể đáp tại <code>(3, 4)</code>. Vì vậy, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong>​​​​​​​</p>

<ul>
	<li><code>source.length == target.length == 2</code></li>
	<li><code>1 &lt;= sr, sc, tr, tc &lt;= 8</code></li>
	<li><code>source != target</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Tượng luôn đi trên các đường chéo, nên mỗi nước đi thay đổi hàng và cột cùng một lượng, đồng thời $(r+c)\bmod 2$ không đổi. Hai ô khác màu không thể đến được với nhau, nên không cần tìm kiếm.
>
> Với hai ô cùng màu, một nước đi là đủ nếu chúng đã nằm trên cùng một đường chéo; nếu không, mọi cặp ô cùng màu trên bàn cờ $8\times 8$ đều có một ô trung gian chung, nên khoảng cách là $2$.
>
> Phân tích các trường hợp chỉ cho kết quả $-1$, $1$ hoặc $2$, với thời gian hằng số.

<!-- thinking:end -->

Tượng chỉ di chuyển theo các đường chéo, và mỗi nước đi thay đổi hàng và cột cùng một lượng, nên $(r + c) \bmod 2$ không bao giờ thay đổi. Nói cách khác, tượng chỉ có thể đứng trên các ô cùng màu với ô bắt đầu. Nếu $(sr + sc)$ và $(tr + tc)$ có tính chẵn lẻ khác nhau, tượng không thể đến đích, nên ta trả về $-1$.

Ngược lại, nếu ô nguồn và ô đích nằm trên cùng một đường chéo, tức là $|sr - tr| = |sc - tc|$, thì chỉ cần một nước đi, nên ta trả về $1$.

Trong mọi trường hợp còn lại, hai ô có cùng màu nhưng không nằm trên một đường chéo chung. Vì đảm bảo $\textit{source} \neq \textit{target}$ và mọi cặp ô cùng màu trên bàn cờ $8 \times 8$ đều có thể nối với nhau qua một ô trung gian, đáp án là $2$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minBishopMoves(self, source: List[int], target: List[int]) -> int:
        sr, sc = source
        tr, tc = target
        if (sr + sc) % 2 != (tr + tc) % 2:
            return -1
        if abs(sr - tr) == abs(sc - tc):
            return 1
        return 2
```

#### Java

```java
class Solution {
    public int minBishopMoves(int[] source, int[] target) {
        int sr = source[0], sc = source[1];
        int tr = target[0], tc = target[1];
        if ((sr + sc) % 2 != (tr + tc) % 2) {
            return -1;
        }
        if (Math.abs(sr - tr) == Math.abs(sc - tc)) {
            return 1;
        }
        return 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minBishopMoves(vector<int>& source, vector<int>& target) {
        int sr = source[0], sc = source[1];
        int tr = target[0], tc = target[1];
        if ((sr + sc) % 2 != (tr + tc) % 2) {
            return -1;
        }
        if (abs(sr - tr) == abs(sc - tc)) {
            return 1;
        }
        return 2;
    }
};
```

#### Go

```go
func minBishopMoves(source []int, target []int) int {
	sr, sc := source[0], source[1]
	tr, tc := target[0], target[1]
	if (sr+sc)%2 != (tr+tc)%2 {
		return -1
	}
	if abs(sr-tr) == abs(sc-tc) {
		return 1
	}
	return 2
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minBishopMoves(source: number[], target: number[]): number {
    const [sr, sc] = source;
    const [tr, tc] = target;
    if ((sr + sc) % 2 !== (tr + tc) % 2) {
        return -1;
    }
    if (Math.abs(sr - tr) === Math.abs(sc - tc)) {
        return 1;
    }
    return 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
