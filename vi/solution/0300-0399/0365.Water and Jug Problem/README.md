---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Math
    - Greatest Common Divisor
    - Euclidean Algorithm
    - Extended Euclidean Algorithm
    - Bézout's Identity
---

<!-- problem:start -->

# [365. Water and Jug Problem](https://leetcode.com/problems/water-and-jug-problem)

[中文文档](/solution/0300-0399/0365.Water%20and%20Jug%20Problem/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai bình có dung tích lần lượt là <code>x</code> lít và <code>y</code> lít, cùng nguồn nước vô hạn. Hãy xác định liệu có thể đong được tổng lượng nước trong hai bình bằng <code>target</code> hay không, bằng các thao tác sau:</p>

<ul>
	<li>Đổ đầy nước vào một trong hai bình.</li>
	<li>Đổ hết nước khỏi một trong hai bình.</li>
	<li>Rót nước từ bình này sang bình kia cho đến khi bình nhận đầy hoặc bình rót cạn.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> x = 3, y = 5, target = 4 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> true </span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện các bước sau để đong được tổng cộng 4 lít nước:</p>

<ol>
	<li>Đổ đầy bình 5 lít (0, 5).</li>
	<li>Rót nước từ bình 5 lít sang bình 3 lít, để lại 2 lít trong bình 5 lít (3, 2).</li>
	<li>Đổ hết nước khỏi bình 3 lít (0, 2).</li>
	<li>Chuyển 2 lít nước từ bình 5 lít sang bình 3 lít (2, 0).</li>
	<li>Đổ đầy bình 5 lít một lần nữa (2, 5).</li>
	<li>Rót nước từ bình 5 lít sang bình 3 lít cho đến khi bình 3 lít đầy. Khi đó bình 5 lít còn 4 lít (3, 4).</li>
	<li>Đổ hết nước khỏi bình 3 lít. Lúc này, bình 5 lít còn đúng 4 lít (0, 4).</li>
</ol>

<p>Tham khảo: ví dụ trong phim <a href="https://www.youtube.com/watch?v=BVtQNK_ZUJg&amp;ab_channel=notnek01" target="_blank">Die Hard</a>.</p>
</div>

<p><strong class="example">Ví dụ 2: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> x = 2, y = 6, target = 5 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> false </span></p>
</div>

<p><strong class="example">Ví dụ 3: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> x = 1, y = 2, target = 3 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> true </span></p>

<p><strong>Giải thích:</strong> Đổ đầy cả hai bình. Khi đó tổng lượng nước trong hai bình bằng 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= x, y, target&nbsp;&lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Hai bình có dung tích $x,y$; liệu có thể đong được $z$ lít không? Đẳng thức Bézout cho biết $z$ phải là bội số của $\gcd(x,y)$. Có thể xác định điều này bằng cách duyệt các trạng thái. Có $O(xy)$ trạng thái.
>
> $dfs(i,j)$ biểu diễn lượng nước hiện có trong hai bình. Nếu trạng thái đã được thăm thì trả về thất bại; nếu lượng nước trong một bình hoặc tổng lượng nước bằng $z$ thì thành công. Nếu không, thực hiện thao tác đổ đầy, đổ cạn hoặc rót nước. Bắt đầu từ $(0,0)$.

<!-- thinking:end -->

Ta ký hiệu $jug1Capacity$ là $x$, $jug2Capacity$ là $y$, còn $targetCapacity$ là $z$.

Tiếp theo, định nghĩa hàm $dfs(i, j)$ để xác định liệu có thể đong được $z$ lít nước khi bình $jug1$ có $i$ lít và bình $jug2$ có $j$ lít hay không.

Hàm $dfs(i, j)$ thực hiện như sau:

- Nếu $(i, j)$ đã được thăm, trả về $false$.
- Nếu $i = z$, $j = z$ hoặc $i + j = z$, trả về $true$.
- Nếu có thể đong được $z$ lít bằng cách đổ đầy hoặc đổ cạn $jug1$ hay $jug2$, trả về $true$.
- Nếu có thể đong được $z$ lít bằng cách rót nước từ $jug1$ sang $jug2$ hoặc từ $jug2$ sang $jug1$, trả về $true$.

Đáp án là $dfs(0, 0)$.

Độ phức tạp thời gian và không gian đều là $O(x + y)$, trong đó $x$ và $y$ lần lượt là dung tích của $jug1Capacity$ và $jug2Capacity$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canMeasureWater(self, x: int, y: int, z: int) -> bool:
        def dfs(i: int, j: int) -> bool:
            if (i, j) in vis:
                return False
            vis.add((i, j))
            if i == z or j == z or i + j == z:
                return True
            if dfs(x, j) or dfs(i, y) or dfs(0, j) or dfs(i, 0):
                return True
            a = min(i, y - j)
            b = min(j, x - i)
            return dfs(i - a, j + a) or dfs(i + b, j - b)

        vis = set()
        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Set<Long> vis = new HashSet<>();
    private int x, y, z;

    public boolean canMeasureWater(int jug1Capacity, int jug2Capacity, int targetCapacity) {
        x = jug1Capacity;
        y = jug2Capacity;
        z = targetCapacity;
        return dfs(0, 0);
    }

    private boolean dfs(int i, int j) {
        long st = f(i, j);
        if (!vis.add(st)) {
            return false;
        }
        if (i == z || j == z || i + j == z) {
            return true;
        }
        if (dfs(x, j) || dfs(i, y) || dfs(0, j) || dfs(i, 0)) {
            return true;
        }
        int a = Math.min(i, y - j);
        int b = Math.min(j, x - i);
        return dfs(i - a, j + a) || dfs(i + b, j - b);
    }

    private long f(int i, int j) {
        return i * 1000000L + j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canMeasureWater(int x, int y, int z) {
        using pii = pair<int, int>;
        stack<pii> stk;
        stk.emplace(0, 0);
        auto hash_function = [](const pii& o) { return hash<int>()(o.first) ^ hash<int>()(o.second); };
        unordered_set<pii, decltype(hash_function)> vis(0, hash_function);
        while (stk.size()) {
            auto st = stk.top();
            stk.pop();
            if (vis.count(st)) {
                continue;
            }
            vis.emplace(st);
            auto [i, j] = st;
            if (i == z || j == z || i + j == z) {
                return true;
            }
            stk.emplace(x, j);
            stk.emplace(i, y);
            stk.emplace(0, j);
            stk.emplace(i, 0);
            int a = min(i, y - j);
            int b = min(j, x - i);
            stk.emplace(i - a, j + a);
            stk.emplace(i + b, j - b);
        }
        return false;
    }
};
```

#### Go

```go
func canMeasureWater(x int, y int, z int) bool {
	type pair struct{ x, y int }
	vis := map[pair]bool{}
	var dfs func(int, int) bool
	dfs = func(i, j int) bool {
		st := pair{i, j}
		if vis[st] {
			return false
		}
		vis[st] = true
		if i == z || j == z || i+j == z {
			return true
		}
		if dfs(x, j) || dfs(i, y) || dfs(0, j) || dfs(i, 0) {
			return true
		}
		a := min(i, y-j)
		b := min(j, x-i)
		return dfs(i-a, j+a) || dfs(i+b, j-b)
	}
	return dfs(0, 0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
