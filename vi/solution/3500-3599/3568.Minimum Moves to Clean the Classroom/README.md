---
comments: true
difficulty: Medium
rating: 2143
source: Weekly Contest 452 Q3
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [3568. Minimum Moves to Clean the Classroom](https://leetcode.com/problems/minimum-moves-to-clean-the-classroom)

[Tài liệu tiếng Trung](/solution/3500-3599/3568.Minimum%20Moves%20to%20Clean%20the%20Classroom/README.md)

## Mô tả

<!-- description:start -->

<p data-end="324" data-start="147">Cho một lưới <code>m x n</code> là <code>classroom</code>, trong đó một học sinh tình nguyện có nhiệm vụ dọn rác rải rác trong phòng. Mỗi ô trong lưới thuộc một trong các loại sau:</p>

<ul>
    <li><code>&#39;S&#39;</code>: Vị trí bắt đầu của học sinh</li>
    <li><code>&#39;L&#39;</code>: Rác cần được nhặt (sau khi được nhặt, ô trở nên trống)</li>
    <li><code>&#39;R&#39;</code>: Khu vực reset, khôi phục năng lượng của học sinh về đầy, bất kể mức năng lượng hiện tại (có thể sử dụng nhiều lần)</li>
    <li><code>&#39;X&#39;</code>: Chướng ngại vật mà học sinh không thể đi qua</li>
    <li><code>&#39;.&#39;</code>: Ô trống</li>
</ul>

<p>Bạn cũng được cho một số nguyên <code>energy</code>, biểu thị mức năng lượng tối đa của học sinh. Học sinh bắt đầu tại vị trí <code>&#39;S&#39;</code> với mức năng lượng này.</p>

<p>Mỗi lần di chuyển đến một ô kề (lên, xuống, trái hoặc phải) tiêu tốn 1 đơn vị năng lượng. Nếu năng lượng giảm về 0, học sinh chỉ có thể tiếp tục khi đang ở một khu vực reset <code>&#39;R&#39;</code>, nơi năng lượng được khôi phục về <strong>mức tối đa</strong> <code>energy</code>.</p>

<p>Trả về số bước di chuyển <strong>ít nhất</strong> cần thiết để nhặt toàn bộ rác, hoặc <code>-1</code> nếu không thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">classroom = [&quot;S.&quot;, &quot;XL&quot;], energy = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Học sinh bắt đầu tại ô <code data-end="262" data-start="254">(0, 0)</code> với 2 đơn vị năng lượng.</li>
    <li>Vì ô <code>(1, 0)</code> chứa chướng ngại vật &#39;X&#39;, học sinh không thể đi thẳng xuống dưới.</li>
    <li>Một chuỗi di chuyển hợp lệ để nhặt toàn bộ rác là:
    <ul>
        <li>Bước 1: Từ <code>(0, 0)</code> &rarr; <code>(0, 1)</code>, tiêu tốn 1 đơn vị năng lượng và còn lại 1 đơn vị.</li>
        <li>Bước 2: Từ <code>(0, 1)</code> &rarr; <code>(1, 1)</code> để nhặt rác <code>&#39;L&#39;</code>.</li>
    </ul>
    </li>
    <li>Học sinh nhặt toàn bộ rác sau 2 bước di chuyển. Do đó, kết quả là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">classroom = [&quot;LS&quot;, &quot;RL&quot;], energy = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Học sinh bắt đầu tại ô <code data-end="262" data-start="254">(0, 1)</code> với 4 đơn vị năng lượng.</li>
    <li>Một chuỗi di chuyển hợp lệ để nhặt toàn bộ rác là:
    <ul>
        <li>Bước 1: Từ <code>(0, 1)</code> &rarr; <code>(0, 0)</code> để nhặt rác đầu tiên <code>&#39;L&#39;</code>, tiêu tốn 1 đơn vị năng lượng và còn lại 3 đơn vị.</li>
        <li>Bước 2: Từ <code>(0, 0)</code> &rarr; <code>(1, 0)</code> đến <code>&#39;R&#39;</code> để reset và khôi phục năng lượng về 4.</li>
        <li>Bước 3: Từ <code>(1, 0)</code> &rarr; <code>(1, 1)</code> để nhặt rác thứ hai <code data-end="1068" data-start="1063">&#39;L&#39;</code>.</li>
    </ul>
    </li>
    <li>Học sinh nhặt toàn bộ rác sau 3 bước di chuyển. Do đó, kết quả là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">classroom = [&quot;L.S&quot;, &quot;RXL&quot;], energy = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có đường đi hợp lệ nào nhặt được toàn bộ <code>&#39;L&#39;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == classroom.length &lt;= 20</code></li>
    <li><code>1 &lt;= n == classroom[i].length &lt;= 20</code></li>
    <li><code>classroom[i][j]</code> là một trong các ký tự <code>&#39;S&#39;</code>, <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code>, <code>&#39;X&#39;</code> hoặc <code>&#39;.&#39;</code></li>
    <li><code>1 &lt;= energy &lt;= 50</code></li>
    <li>Có chính xác <strong>một</strong> <code>&#39;S&#39;</code> trong lưới.</li>
    <li>Có <strong>tối đa</strong> 10 ô <code>&#39;L&#39;</code> trong lưới.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Lưới có kích thước nhỏ và số ô rác ít, nên một trạng thái là (vị trí, năng lượng còn lại, mask của những ô rác chưa nhặt). Năng lượng bằng $0$ sẽ ngăn việc di chuyển; ô $R$ sẽ nạp lại năng lượng ban đầu.
>
> BFS theo từng level làm tăng số bước; mask bằng $0$ chính là đáp án. Mỗi trạng thái chỉ cần được thăm một lần.

<!-- thinking:end -->

Ta có thể dùng Breadth-First Search (BFS) để giải bài toán này. Trước tiên, ta cần tìm vị trí bắt đầu của học sinh và ghi lại vị trí của tất cả rác. Sau đó, ta dùng BFS để khám phá mọi đường đi có thể bắt đầu từ vị trí ban đầu, đồng thời theo dõi năng lượng hiện tại và rác đã nhặt.

Trong BFS, ta cần duy trì một trạng thái gồm vị trí hiện tại, năng lượng còn lại và một bitmask biểu diễn rác đã nhặt. Ta có thể dùng một queue để lưu các trạng thái này và một set để ghi lại các trạng thái đã thăm, tránh thăm lại chúng.

Ta bắt đầu từ vị trí ban đầu và thử di chuyển theo bốn hướng. Nếu di chuyển đến một ô có rác, ta cập nhật bitmask rác đã nhặt. Nếu di chuyển đến một khu vực reset, ta khôi phục năng lượng về giá trị tối đa. Mỗi bước di chuyển tiêu tốn 1 đơn vị năng lượng.

Nếu tìm thấy trong BFS một trạng thái có bitmask rác bằng 0 (nghĩa là đã nhặt toàn bộ rác), ta trả về số bước hiện tại. Nếu BFS kết thúc mà không tìm thấy trạng thái như vậy, ta trả về -1.

Độ phức tạp thời gian là $O(m \times n \times \textit{energy} \times 2^{\textit{count}})$, và độ phức tạp không gian là $O(m \times n \times \textit{energy} \times 2^{\textit{count}})$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của lưới, còn $\textit{count}$ là số ô rác.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, classroom: List[str], energy: int) -> int:
        m, n = len(classroom), len(classroom[0])
        d = [[0] * n for _ in range(m)]
        x = y = cnt = 0
        for i, row in enumerate(classroom):
            for j, c in enumerate(row):
                if c == "S":
                    x, y = i, j
                elif c == "L":
                    d[i][j] = cnt
                    cnt += 1
        if cnt == 0:
            return 0
        vis = [
            [[[False] * (1 << cnt) for _ in range(energy + 1)] for _ in range(n)]
            for _ in range(m)
        ]
        q = [(x, y, energy, (1 << cnt) - 1)]
        vis[x][y][energy][(1 << cnt) - 1] = True
        dirs = (-1, 0, 1, 0, -1)
        ans = 0
        while q:
            t = q
            q = []
            for i, j, cur_energy, mask in t:
                if mask == 0:
                    return ans
                if cur_energy <= 0:
                    continue
                for k in range(4):
                    x, y = i + dirs[k], j + dirs[k + 1]
                    if 0 <= x < m and 0 <= y < n and classroom[x][y] != "X":
                        nxt_energy = (
                            energy if classroom[x][y] == "R" else cur_energy - 1
                        )
                        nxt_mask = mask
                        if classroom[x][y] == "L":
                            nxt_mask &= ~(1 << d[x][y])
                        if not vis[x][y][nxt_energy][nxt_mask]:
                            vis[x][y][nxt_energy][nxt_mask] = True
                            q.append((x, y, nxt_energy, nxt_mask))
            ans += 1
        return -1
```

#### Java

```java
class Solution {
    public int minMoves(String[] classroom, int energy) {
        int m = classroom.length, n = classroom[0].length();
        int[][] d = new int[m][n];
        int x = 0, y = 0, cnt = 0;
        for (int i = 0; i < m; i++) {
            String row = classroom[i];
            for (int j = 0; j < n; j++) {
                char c = row.charAt(j);
                if (c == 'S') {
                    x = i;
                    y = j;
                } else if (c == 'L') {
                    d[i][j] = cnt;
                    cnt++;
                }
            }
        }
        if (cnt == 0) {
            return 0;
        }
        boolean[][][][] vis = new boolean[m][n][energy + 1][1 << cnt];
        List<int[]> q = new ArrayList<>();
        q.add(new int[] {x, y, energy, (1 << cnt) - 1});
        vis[x][y][energy][(1 << cnt) - 1] = true;
        int[] dirs = {-1, 0, 1, 0, -1};
        int ans = 0;
        while (!q.isEmpty()) {
            List<int[]> t = q;
            q = new ArrayList<>();
            for (int[] state : t) {
                int i = state[0], j = state[1], curEnergy = state[2], mask = state[3];
                if (mask == 0) {
                    return ans;
                }
                if (curEnergy <= 0) {
                    continue;
                }
                for (int k = 0; k < 4; k++) {
                    int nx = i + dirs[k], ny = j + dirs[k + 1];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && classroom[nx].charAt(ny) != 'X') {
                        int nxtEnergy = classroom[nx].charAt(ny) == 'R' ? energy : curEnergy - 1;
                        int nxtMask = mask;
                        if (classroom[nx].charAt(ny) == 'L') {
                            nxtMask &= ~(1 << d[nx][ny]);
                        }
                        if (!vis[nx][ny][nxtEnergy][nxtMask]) {
                            vis[nx][ny][nxtEnergy][nxtMask] = true;
                            q.add(new int[] {nx, ny, nxtEnergy, nxtMask});
                        }
                    }
                }
            }
            ans++;
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(vector<string>& classroom, int energy) {
        int m = classroom.size(), n = classroom[0].size();
        vector<vector<int>> d(m, vector<int>(n, 0));
        int x = 0, y = 0, cnt = 0;
        for (int i = 0; i < m; ++i) {
            string& row = classroom[i];
            for (int j = 0; j < n; ++j) {
                char c = row[j];
                if (c == 'S') {
                    x = i;
                    y = j;
                } else if (c == 'L') {
                    d[i][j] = cnt;
                    cnt++;
                }
            }
        }
        if (cnt == 0) {
            return 0;
        }
        vector<vector<vector<vector<bool>>>> vis(m, vector<vector<vector<bool>>>(n, vector<vector<bool>>(energy + 1, vector<bool>(1 << cnt, false))));
        queue<tuple<int, int, int, int>> q;
        q.emplace(x, y, energy, (1 << cnt) - 1);
        vis[x][y][energy][(1 << cnt) - 1] = true;
        vector<int> dirs = {-1, 0, 1, 0, -1};
        int ans = 0;
        while (!q.empty()) {
            int sz = q.size();
            while (sz--) {
                auto [i, j, cur_energy, mask] = q.front();
                q.pop();
                if (mask == 0) {
                    return ans;
                }
                if (cur_energy <= 0) {
                    continue;
                }
                for (int k = 0; k < 4; ++k) {
                    int nx = i + dirs[k], ny = j + dirs[k + 1];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && classroom[nx][ny] != 'X') {
                        int nxt_energy = classroom[nx][ny] == 'R' ? energy : cur_energy - 1;
                        int nxt_mask = mask;
                        if (classroom[nx][ny] == 'L') {
                            nxt_mask &= ~(1 << d[nx][ny]);
                        }
                        if (!vis[nx][ny][nxt_energy][nxt_mask]) {
                            vis[nx][ny][nxt_energy][nxt_mask] = true;
                            q.emplace(nx, ny, nxt_energy, nxt_mask);
                        }
                    }
                }
            }
            ans++;
        }
        return -1;
    }
};
```

#### Go

```go
func minMoves(classroom []string, energy int) int {
    m, n := len(classroom), len(classroom[0])
    d := make([][]int, m)
    for i := range d {
        d[i] = make([]int, n)
    }
    x, y, cnt := 0, 0, 0
    for i := 0; i < m; i++ {
        row := classroom[i]
        for j := 0; j < n; j++ {
            c := row[j]
            if c == 'S' {
                x, y = i, j
            } else if c == 'L' {
                d[i][j] = cnt
                cnt++
            }
        }
    }
    if cnt == 0 {
        return 0
    }

    vis := make([][][][]bool, m)
    for i := range vis {
        vis[i] = make([][][]bool, n)
        for j := range vis[i] {
            vis[i][j] = make([][]bool, energy+1)
            for e := range vis[i][j] {
                vis[i][j][e] = make([]bool, 1<<cnt)
            }
        }
    }
    type state struct {
        i, j, curEnergy, mask int
    }
    q := []state{{x, y, energy, (1 << cnt) - 1}}
    vis[x][y][energy][(1<<cnt)-1] = true
    dirs := []int{-1, 0, 1, 0, -1}
    ans := 0

    for len(q) > 0 {
        t := q
        q = []state{}
        for _, s := range t {
            i, j, curEnergy, mask := s.i, s.j, s.curEnergy, s.mask
            if mask == 0 {
                return ans
            }
            if curEnergy <= 0 {
                continue
            }
            for k := 0; k < 4; k++ {
                nx, ny := i+dirs[k], j+dirs[k+1]
                if nx >= 0 && nx < m && ny >= 0 && ny < n && classroom[nx][ny] != 'X' {
                    var nxtEnergy int
                    if classroom[nx][ny] == 'R' {
                        nxtEnergy = energy
                    } else {
                        nxtEnergy = curEnergy - 1
                    }
                    nxtMask := mask
                    if classroom[nx][ny] == 'L' {
                        nxtMask &= ^(1 << d[nx][ny])
                    }
                    if !vis[nx][ny][nxtEnergy][nxtMask] {
                        vis[nx][ny][nxtEnergy][nxtMask] = true
                        q = append(q, state{nx, ny, nxtEnergy, nxtMask})
                    }
                }
            }
        }
        ans++
    }
    return -1
}
```

#### TypeScript

```ts
function minMoves(classroom: string[], energy: number): number {
    const m = classroom.length;
    const n = classroom[0].length;
    const d: number[][] = Array.from({ length: m }, () => Array(n).fill(0));
    let x = 0;
    let y = 0;
    let cnt = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            const c = classroom[i][j];
            if (c === 'S') {
                x = i;
                y = j;
            } else if (c === 'L') {
                d[i][j] = cnt++;
            }
        }
    }
    if (cnt === 0) {
        return 0;
    }
    const vis = Array.from({ length: m }, () =>
        Array.from({ length: n }, () =>
            Array.from({ length: energy + 1 }, () => new Uint8Array(1 << cnt)),
        ),
    );
    let q: number[][] = [[x, y, energy, (1 << cnt) - 1]];
    vis[x][y][energy][(1 << cnt) - 1] = 1;
    const dirs = [-1, 0, 1, 0, -1];
    let ans = 0;
    while (q.length) {
        const t = q;
        q = [];
        for (const [i, j, curEnergy, mask] of t) {
            if (mask === 0) {
                return ans;
            }
            if (curEnergy <= 0) {
                continue;
            }
            for (let k = 0; k < 4; ++k) {
                const nx = i + dirs[k];
                const ny = j + dirs[k + 1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && classroom[nx][ny] !== 'X') {
                    const nxtEnergy = classroom[nx][ny] === 'R' ? energy : curEnergy - 1;
                    let nxtMask = mask;
                    if (classroom[nx][ny] === 'L') {
                        nxtMask &= ~(1 << d[nx][ny]);
                    }
                    if (!vis[nx][ny][nxtEnergy][nxtMask]) {
                        vis[nx][ny][nxtEnergy][nxtMask] = 1;
                        q.push([nx, ny, nxtEnergy, nxtMask]);
                    }
                }
            }
        }
        ++ans;
    }
    return -1;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn min_moves(classroom: Vec<String>, energy: i32) -> i32 {
        let m = classroom.len();
        let n = classroom[0].len();
        let e = energy as usize;
        let mut d = vec![vec![0; n]; m];
        let mut x = 0;
        let mut y = 0;
        let mut cnt = 0;
        for i in 0..m {
            let row = classroom[i].as_bytes();
            for j in 0..n {
                if row[j] == b'S' {
                    x = i;
                    y = j;
                } else if row[j] == b'L' {
                    d[i][j] = cnt;
                    cnt += 1;
                }
            }
        }
        if cnt == 0 {
            return 0;
        }
        let masks = 1usize << cnt;
        let mut vis = vec![false; m * n * (e + 1) * masks];
        let id = |i: usize, j: usize, en: usize, mask: usize| {
            ((i * n + j) * (e + 1) + en) * masks + mask
        };
        let full = masks - 1;
        vis[id(x, y, e, full)] = true;
        let mut q = VecDeque::from([(x, y, e, full)]);
        let dirs = [-1, 0, 1, 0, -1];
        let mut ans = 0;
        while !q.is_empty() {
            for _ in 0..q.len() {
                let (i, j, cur, mask) = q.pop_front().unwrap();
                if mask == 0 {
                    return ans;
                }
                if cur == 0 {
                    continue;
                }
                for k in 0..4 {
                    let nx = i as i32 + dirs[k];
                    let ny = j as i32 + dirs[k + 1];
                    if nx < 0 || ny < 0 {
                        continue;
                    }
                    let nx = nx as usize;
                    let ny = ny as usize;
                    if nx >= m || ny >= n {
                        continue;
                    }
                    let c = classroom[nx].as_bytes()[ny];
                    if c == b'X' {
                        continue;
                    }
                    let nxt_e = if c == b'R' { e } else { cur - 1 };
                    let mut nxt_mask = mask;
                    if c == b'L' {
                        nxt_mask &= !(1 << d[nx][ny]);
                    }
                    let idx = id(nx, ny, nxt_e, nxt_mask);
                    if !vis[idx] {
                        vis[idx] = true;
                        q.push_back((nx, ny, nxt_e, nxt_mask));
                    }
                }
            }
            ans += 1;
        }
        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
