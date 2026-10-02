---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [789. Escape The Ghosts](https://leetcode.com/problems/escape-the-ghosts)

[中文文档](/solution/0700-0799/0789.Escape%20The%20Ghosts/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi một phiên bản đơn giản hóa của PAC-MAN trên lưới 2D vô hạn. Bạn bắt đầu tại điểm <code>[0, 0]</code> và được cho điểm đích <code>target = [x<sub>target</sub>, y<sub>target</sub>]</code> cần đến. Trên bản đồ có một số bóng ma, vị trí xuất phát của chúng được cho trong mảng 2 chiều <code>ghosts</code>, trong đó <code>ghosts[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> là vị trí xuất phát của bóng ma thứ <code>i<sup>th</sup></code>. Tất cả đầu vào đều là <strong>tọa độ nguyên</strong>.</p>

<p>Mỗi lượt, bạn và tất cả bóng ma có thể độc lập chọn <strong>di chuyển 1 đơn vị</strong> theo một trong bốn hướng chính: bắc, đông, nam hoặc tây, hoặc <strong>đứng yên</strong>. Mọi hành động diễn ra <strong>đồng thời</strong>.</p>

<p>Bạn thoát được khi và chỉ khi đến đích <strong>trước</strong> khi bất kỳ bóng ma nào bắt kịp bạn. Nếu bạn đến một ô bất kỳ (kể cả đích) <strong>cùng lúc</strong> với bóng ma thì <strong>không</strong> được tính là thoát.</p>

<p>Trả về <code>true</code><em> nếu bạn có thể thoát bất kể các bóng ma di chuyển thế nào; nếu không, trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ghosts = [[1,0],[0,3]], target = [0,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bạn có thể đến đích (0, 1) sau 1 lượt, trong khi các bóng ma ở (1, 0) và (0, 3) không thể bắt kịp bạn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ghosts = [[1,0]], target = [2,0]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Bạn cần đến đích (2, 0), nhưng bóng ma ở (1, 0) nằm giữa bạn và đích.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> ghosts = [[2,0]], target = [1,0]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Bóng ma có thể đến đích cùng lúc với bạn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= ghosts.length &lt;= 100</code></li>
	<li><code>ghosts[i].length == 2</code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>Có thể có <strong>nhiều bóng ma</strong> ở cùng một vị trí.</li>
	<li><code>target.length == 2</code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sub>target</sub>, y<sub>target</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bạn và các bóng ma di chuyển đồng thời. Nếu khoảng cách Manhattan từ một bóng ma đến đích không lớn hơn khoảng cách của bạn, nó có thể chặn bạn.
>
> Bạn thoát được khi và chỉ khi khoảng cách từ mọi bóng ma đến đích đều lớn hơn nghiêm ngặt khoảng cách từ điểm xuất phát của bạn đến đích.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def escapeGhosts(self, ghosts: List[List[int]], target: List[int]) -> bool:
        tx, ty = target
        return all(abs(tx - x) + abs(ty - y) > abs(tx) + abs(ty) for x, y in ghosts)
```

#### Java

```java
class Solution {
    public boolean escapeGhosts(int[][] ghosts, int[] target) {
        int tx = target[0], ty = target[1];
        for (var g : ghosts) {
            int x = g[0], y = g[1];
            if (Math.abs(tx - x) + Math.abs(ty - y) <= Math.abs(tx) + Math.abs(ty)) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool escapeGhosts(vector<vector<int>>& ghosts, vector<int>& target) {
        int tx = target[0], ty = target[1];
        for (auto& g : ghosts) {
            int x = g[0], y = g[1];
            if (abs(tx - x) + abs(ty - y) <= abs(tx) + abs(ty)) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func escapeGhosts(ghosts [][]int, target []int) bool {
	tx, ty := target[0], target[1]
	for _, g := range ghosts {
		x, y := g[0], g[1]
		if abs(tx-x)+abs(ty-y) <= abs(tx)+abs(ty) {
			return false
		}
	}
	return true
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
function escapeGhosts(ghosts: number[][], target: number[]): boolean {
    const [tx, ty] = target;
    for (const [x, y] of ghosts) {
        if (Math.abs(tx - x) + Math.abs(ty - y) <= Math.abs(tx) + Math.abs(ty)) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
