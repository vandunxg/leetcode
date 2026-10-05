---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [4061. Minimum Queen Moves to Reach Target](https://leetcode.com/problems/minimum-queen-moves-to-reach-target)

[中文文档](/solution/4000-4099/4061.Minimum%20Queen%20Moves%20to%20Reach%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bàn cờ trống <code>8 x 8</code> với các hàng và cột được đánh số từ <strong>1</strong>.</p>

<p>Bạn được cho một mảng <code>source = [sr, sc]</code> biểu diễn vị trí bắt đầu của một <strong>hậu</strong>, và một mảng <code>target = [tr, tc]</code> biểu diễn vị trí đích.</p>

<p>Trong một nước đi, hậu di chuyển một hoặc nhiều ô theo một <strong>hàng</strong>, <strong>cột</strong> hoặc <strong>đường chéo</strong> duy nhất, đồng thời vẫn nằm trong bàn cờ.</p>

<p>Trả về số nước đi <strong>nhỏ nhất</strong> để hậu đáp <strong>chính xác</strong> tại <code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [8,1], target = [1,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong>​​​​​​​<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4061.Minimum%20Queen%20Moves%20to%20Reach%20Target/images/111.png" style="width: 300px; height: 303px;" />​​​​​​​</strong></p>

<p>Một nước đi theo đường chéo đưa hậu đi thẳng từ <code>(8, 1)</code> đến <code>(1, 8)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [4,2], target = [1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4061.Minimum%20Queen%20Moves%20to%20Reach%20Target/images/1e602ed4-c525-4be4-a6a9-6804cb7d2a55.png" style="width: 300px; height: 305px;" />​​​​​​​</p>

<p>Hậu di chuyển từ <code>(4, 2)</code> đến <code>(4, 3)</code>, sau đó từ <code>(4, 3)</code> đến <code>(1, 3)</code>, đến đích sau 2 nước đi.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [1,1], target = [1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hậu đã ở vị trí đích, nên không cần thực hiện nước đi nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong>​​​​​​​</p>

<ul>
	<li><code>source == [sr, sc]</code></li>
	<li><code>target == [tr, tc]</code></li>
	<li><code>1 &lt;= sr, sc, tr, tc &lt;= 8</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Bàn cờ chỉ có kích thước $8\times 8$, nên BFS từ ô nguồn cũng tìm được đường đi ngắn nhất. Mỗi nước đi của hậu có thể đến bất kỳ ô nào trên cùng hàng, cột hoặc đường chéo, và khoảng cách chỉ có thể là $0$, $1$ hoặc $2$.
>
> Khi hai ô khác nhau, đi theo hàng đến $(s_r, t_c)$ rồi theo cột đến $(t_r, t_c)$. Hai nước đi luôn đến được đích, vì bàn cờ trống.
>
> Nếu ô trung gian trùng với một trong hai đầu mút, thì ô nguồn và ô đích đã nằm trên cùng một hàng hoặc cột, nên chỉ cần một nước đi. Vì vậy, chỉ cần kiểm tra hai ô có trùng nhau, có chung hàng hoặc cột, hay có chung đường chéo hay không.

<!-- thinking:end -->

Đáp án là $0$ khi $(s_r, s_c)$ và $(t_r, t_c)$ là cùng một ô.

Hậu di chuyển được bất kỳ số ô nào trên cùng một hàng, một cột hoặc một đường chéo. Đáp án là $1$ khi $s_r=t_r$, $s_c=t_c$ hoặc $|s_r-t_r|=|s_c-t_c|$.

Mọi cặp ô còn lại cần hai nước đi. Đi từ $(s_r, s_c)$ đến $(s_r, t_c)$, sau đó từ $(s_r, t_c)$ đến $(t_r, t_c)$. Các ô có hàng và cột khác nhau, nên ô trung gian khác với cả hai đầu mút. Bàn cờ trống, vì vậy cả hai nước đi đều hợp lệ. Đáp án là $2$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minQueenMoves(self, source: list[int], target: list[int]) -> int:
        sr, sc = source
        tr, tc = target
        if sr == tr and sc == tc:
            return 0
        if sr == tr or sc == tc or abs(sr - tr) == abs(sc - tc):
            return 1
        return 2
```

#### Java

```java
class Solution {
    public int minQueenMoves(int[] source, int[] target) {
        int sr = source[0], sc = source[1];
        int tr = target[0], tc = target[1];
        if (sr == tr && sc == tc) {
            return 0;
        }
        if (sr == tr || sc == tc || Math.abs(sr - tr) == Math.abs(sc - tc)) {
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
    int minQueenMoves(vector<int>& source, vector<int>& target) {
        int sr = source[0], sc = source[1];
        int tr = target[0], tc = target[1];
        if (sr == tr && sc == tc) {
            return 0;
        }
        if (sr == tr || sc == tc || abs(sr - tr) == abs(sc - tc)) {
            return 1;
        }
        return 2;
    }
};
```

#### Go

```go
func minQueenMoves(source []int, target []int) int {
	sr, sc := source[0], source[1]
	tr, tc := target[0], target[1]
	if sr == tr && sc == tc {
		return 0
	}
	if sr == tr || sc == tc || abs(sr-tr) == abs(sc-tc) {
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
function minQueenMoves(source: number[], target: number[]): number {
    const [sr, sc] = source;
    const [tr, tc] = target;
    if (sr === tr && sc === tc) {
        return 0;
    }
    if (sr === tr || sc === tc || Math.abs(sr - tr) === Math.abs(sc - tc)) {
        return 1;
    }
    return 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
