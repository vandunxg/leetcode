---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [874. Walking Robot Simulation](https://leetcode.com/problems/walking-robot-simulation)

[中文文档](/solution/0800-0899/0874.Walking%20Robot%20Simulation/README.md)

## Mô tả

<!-- description:start -->

<p>Robot trên mặt phẳng XY vô hạn bắt đầu tại điểm <code>(0, 0)</code> và hướng về phía bắc. Robot nhận mảng số nguyên <code>commands</code>, biểu diễn chuỗi các bước cần thực hiện. Robot chỉ có thể nhận ba loại lệnh sau:</p>

<ul>
	<li><code>-2</code>: Quay trái <code>90</code> độ.</li>
	<li><code>-1</code>: Quay phải <code>90</code> độ.</li>
	<li><code>1 &lt;= k &lt;= 9</code>: Đi thẳng <code>k</code> đơn vị, mỗi lần một đơn vị.</li>
</ul>

<p>Một số ô lưới là <code>obstacles</code>. Chướng ngại vật thứ <code>i<sup>th</sup></code> nằm tại điểm <code>obstacles[i] = (x<sub>i</sub>, y<sub>i</sub>)</code>. Nếu robot gặp chướng ngại vật, nó sẽ đứng nguyên tại vị trí hiện tại (ô kề với chướng ngại vật) rồi chuyển sang lệnh tiếp theo.</p>

<p>Trả về <strong>bình phương khoảng cách Euclid lớn nhất</strong> mà robot đạt được tại bất kỳ thời điểm nào trên đường đi (ví dụ, nếu khoảng cách là <code>5</code> thì trả về <code>25</code>).</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Có thể có chướng ngại vật tại <code>(0, 0)</code>. Nếu vậy, robot sẽ bỏ qua chướng ngại vật này cho đến khi rời khỏi gốc tọa độ. Tuy nhiên, do chướng ngại vật nên robot không thể quay lại <code>(0, 0)</code>.</li>
	<li>Hướng bắc là chiều +Y.</li>
	<li>Hướng đông là chiều +X.</li>
	<li>Hướng nam là chiều -Y.</li>
	<li>Hướng tây là chiều -X.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">commands = [4,-1,3], obstacles = []</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích: </strong></p>

<p>Robot bắt đầu tại <code>(0, 0)</code>:</p>

<ol>
	<li>Đi về phía bắc 4 đơn vị đến <code>(0, 4)</code>.</li>
	<li>Quay phải.</li>
	<li>Đi về phía đông 3 đơn vị đến <code>(3, 4)</code>.</li>
</ol>

<p>Điểm xa gốc tọa độ nhất mà robot đến được là <code>(3, 4)</code>, có bình phương khoảng cách bằng <code>3<sup>2</sup> + 4<sup>2 </sup>= 25</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">commands = [4,-1,4,-2,4], obstacles = [[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">65</span></p>

<p><strong>Giải thích:</strong></p>

<p>Robot bắt đầu tại <code>(0, 0)</code>:</p>

<ol>
	<li>Đi về phía bắc 4 đơn vị đến <code>(0, 4)</code>.</li>
	<li>Quay phải.</li>
	<li>Đi về phía đông 1 đơn vị thì bị chặn bởi chướng ngại vật tại <code>(2, 4)</code>; robot dừng ở <code>(1, 4)</code>.</li>
	<li>Quay trái.</li>
	<li>Đi về phía bắc 4 đơn vị đến <code>(1, 8)</code>.</li>
</ol>

<p>Điểm xa gốc tọa độ nhất mà robot đến được là <code>(1, 8)</code>, có bình phương khoảng cách bằng <code>1<sup>2</sup> + 8<sup>2</sup> = 65</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">commands = [6,-1,-1,6], obstacles = [[0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">36</span></p>

<p><strong>Giải thích:</strong></p>

<p>Robot bắt đầu tại <code>(0, 0)</code>:</p>

<ol>
	<li>Đi về phía bắc 6 đơn vị đến <code>(0, 6)</code>.</li>
	<li>Quay phải.</li>
	<li>Quay phải.</li>
	<li>Đi về phía nam 5 đơn vị thì bị chặn bởi chướng ngại vật tại <code>(0,0)</code>; robot dừng ở <code>(0, 1)</code>.</li>
</ol>

<p>Điểm xa gốc tọa độ nhất mà robot đến được là <code>(0, 6)</code>, có bình phương khoảng cách bằng <code>6<sup>2</sup> = 36</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= commands.length &lt;= 10<sup>4</sup></code></li>
	<li><code>commands[i]</code> là <code>-2</code>, <code>-1</code> hoặc một số nguyên trong phạm vi <code>[1, 9]</code>.</li>
	<li><code>0 &lt;= obstacles.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-3 * 10<sup>4</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 3 * 10<sup>4</sup></code></li>
	<li>Đảm bảo đáp án nhỏ hơn <code>2<sup>31</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Robot quay hoặc di chuyển; chướng ngại vật sẽ chặn bước đi. Vì số lệnh và số chướng ngại vật đều $\le 10^4$, có thể mô phỏng trực tiếp nếu kiểm tra chướng ngại vật trong $O(1)$.
>
> Lưu chướng ngại vật trong set và dùng mảng hướng tuần hoàn. Ở mỗi bước, kiểm tra ô kế tiếp; đáp án là bình phương khoảng cách Euclid lớn nhất.

<!-- thinking:end -->

Ta định nghĩa mảng hướng $dirs = [0, 1, 0, -1, 0]$ có độ dài $5$, trong đó mỗi cặp phần tử liên tiếp biểu diễn một hướng. Cụ thể, $(dirs[0], dirs[1])$ là hướng bắc, $(dirs[1], dirs[2])$ là hướng đông, v.v.

Ta dùng hash table $s$ để lưu tọa độ mọi chướng ngại vật, nhờ đó có thể kiểm tra trong $O(1)$ liệu bước tiếp theo có gặp chướng ngại vật hay không.

Ngoài ra, dùng hai biến $x$ và $y$ biểu diễn tọa độ hiện tại của robot, ban đầu $x = y = 0$. Biến $k$ biểu diễn hướng hiện tại, còn $ans$ lưu bình phương khoảng cách Euclid lớn nhất tính từ gốc tọa độ.

Tiếp theo, duyệt từng phần tử $c$ trong mảng $commands$:

- Nếu $c = -2$, robot quay trái $90$ độ, tức $k = (k + 3) \bmod 4$;
- Nếu $c = -1$, robot quay phải $90$ độ, tức $k = (k + 1) \bmod 4$;
- Nếu không, robot đi thẳng $c$ đơn vị. Kết hợp hướng hiện tại $k$ với mảng hướng $dirs$ để lấy độ tăng theo trục $x$ và $y$. Cộng từng bước vào $x$ và $y$, rồi kiểm tra tọa độ mới $(nx, ny)$ có nằm trong tập chướng ngại vật hay không. Nếu không, cập nhật $ans$; nếu có, dừng di chuyển và chuyển sang lệnh tiếp theo.

Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(C \times n + m)$ và độ phức tạp không gian là $O(m)$, trong đó $C$ là số bước tối đa cho mỗi lệnh di chuyển, còn $n$ và $m$ lần lượt là độ dài của các mảng $commands$ và $obstacles$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def robotSim(self, commands: List[int], obstacles: List[List[int]]) -> int:
        dirs = (0, 1, 0, -1, 0)
        s = {(x, y) for x, y in obstacles}
        ans = k = 0
        x = y = 0
        for c in commands:
            if c == -2:
                k = (k + 3) % 4
            elif c == -1:
                k = (k + 1) % 4
            else:
                for _ in range(c):
                    nx, ny = x + dirs[k], y + dirs[k + 1]
                    if (nx, ny) in s:
                        break
                    x, y = nx, ny
                    ans = max(ans, x * x + y * y)
        return ans
```

#### Java

```java
class Solution {
    public int robotSim(int[] commands, int[][] obstacles) {
        int[] dirs = {0, 1, 0, -1, 0};
        Set<Integer> s = new HashSet<>(obstacles.length);
        for (var e : obstacles) {
            s.add(f(e[0], e[1]));
        }
        int ans = 0, k = 0;
        int x = 0, y = 0;
        for (int c : commands) {
            if (c == -2) {
                k = (k + 3) % 4;
            } else if (c == -1) {
                k = (k + 1) % 4;
            } else {
                while (c-- > 0) {
                    int nx = x + dirs[k], ny = y + dirs[k + 1];
                    if (s.contains(f(nx, ny))) {
                        break;
                    }
                    x = nx;
                    y = ny;
                    ans = Math.max(ans, x * x + y * y);
                }
            }
        }
        return ans;
    }

    private int f(int x, int y) {
        return x * 60010 + y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int robotSim(vector<int>& commands, vector<vector<int>>& obstacles) {
        int dirs[5] = {0, 1, 0, -1, 0};
        auto f = [](int x, int y) {
            return x * 60010 + y;
        };
        unordered_set<int> s;
        for (auto& e : obstacles) {
            s.insert(f(e[0], e[1]));
        }
        int ans = 0, k = 0;
        int x = 0, y = 0;
        for (int c : commands) {
            if (c == -2) {
                k = (k + 3) % 4;
            } else if (c == -1) {
                k = (k + 1) % 4;
            } else {
                while (c--) {
                    int nx = x + dirs[k], ny = y + dirs[k + 1];
                    if (s.count(f(nx, ny))) {
                        break;
                    }
                    x = nx;
                    y = ny;
                    ans = max(ans, x * x + y * y);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func robotSim(commands []int, obstacles [][]int) (ans int) {
	dirs := [5]int{0, 1, 0, -1, 0}
	type pair struct{ x, y int }
	s := map[pair]bool{}
	for _, e := range obstacles {
		s[pair{e[0], e[1]}] = true
	}
	var x, y, k int
	for _, c := range commands {
		if c == -2 {
			k = (k + 3) % 4
		} else if c == -1 {
			k = (k + 1) % 4
		} else {
			for ; c > 0 && !s[pair{x + dirs[k], y + dirs[k+1]}]; c-- {
				x += dirs[k]
				y += dirs[k+1]
				ans = max(ans, x*x+y*y)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function robotSim(commands: number[], obstacles: number[][]): number {
    const dirs = [0, 1, 0, -1, 0];
    const s: Set<number> = new Set();
    const f = (x: number, y: number) => x * 60010 + y;
    for (const [x, y] of obstacles) {
        s.add(f(x, y));
    }
    let [ans, x, y, k] = [0, 0, 0, 0];
    for (let c of commands) {
        if (c === -2) {
            k = (k + 3) % 4;
        } else if (c === -1) {
            k = (k + 1) % 4;
        } else {
            while (c-- > 0) {
                const [nx, ny] = [x + dirs[k], y + dirs[k + 1]];
                if (s.has(f(nx, ny))) {
                    break;
                }
                [x, y] = [nx, ny];
                ans = Math.max(ans, x * x + y * y);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn robot_sim(commands: Vec<i32>, obstacles: Vec<Vec<i32>>) -> i32 {
        let dirs: [i32; 5] = [0, 1, 0, -1, 0];
        let mut s: HashSet<(i32, i32)> = HashSet::new();

        for o in obstacles {
            s.insert((o[0], o[1]));
        }

        let mut ans: i32 = 0;
        let mut k: i32 = 0;
        let mut x: i32 = 0;
        let mut y: i32 = 0;

        for c in commands {
            if c == -2 {
                k = (k + 3) % 4;
            } else if c == -1 {
                k = (k + 1) % 4;
            } else {
                for _ in 0..c {
                    let nx: i32 = x + dirs[k as usize];
                    let ny: i32 = y + dirs[k as usize + 1];

                    if s.contains(&(nx, ny)) {
                        break;
                    }

                    x = nx;
                    y = ny;
                    ans = ans.max(x * x + y * y);
                }
            }
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} commands
 * @param {number[][]} obstacles
 * @return {number}
 */
var robotSim = function (commands, obstacles) {
    const dirs = [0, 1, 0, -1, 0];
    const s = new Set();

    const f = (x, y) => x * 60010 + y;

    for (const [x, y] of obstacles) {
        s.add(f(x, y));
    }

    let x = 0,
        y = 0,
        k = 0;
    let ans = 0;

    for (let c of commands) {
        if (c === -2) {
            k = (k + 3) % 4;
        } else if (c === -1) {
            k = (k + 1) % 4;
        } else {
            while (c-- > 0) {
                const nx = x + dirs[k];
                const ny = y + dirs[k + 1];
                if (s.has(f(nx, ny))) {
                    break;
                }
                x = nx;
                y = ny;
                ans = Math.max(ans, x * x + y * y);
            }
        }
    }

    return ans;
};
```

#### C#

```cs
public class Solution {
    public int RobotSim(int[] commands, int[][] obstacles) {
        int[] dirs = {0, 1, 0, -1, 0};
        HashSet<int> s = new HashSet<int>();

        int F(int x, int y) => x * 60010 + y;

        foreach (var o in obstacles) {
            s.Add(F(o[0], o[1]));
        }

        int x = 0, y = 0, k = 0;
        int ans = 0;

        foreach (int c0 in commands) {
            int c = c0;
            if (c == -2) {
                k = (k + 3) % 4;
            } else if (c == -1) {
                k = (k + 1) % 4;
            } else {
                while (c-- > 0) {
                    int nx = x + dirs[k];
                    int ny = y + dirs[k + 1];
                    if (s.Contains(F(nx, ny))) {
                        break;
                    }
                    x = nx;
                    y = ny;
                    ans = Math.Max(ans, x * x + y * y);
                }
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
