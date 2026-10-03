---
comments: true
difficulty: Medium
rating: 1379
source: Weekly Contest 273 Q2
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [2120. Execution of All Suffix Instructions Staying in a Grid](https://leetcode.com/problems/execution-of-all-suffix-instructions-staying-in-a-grid)

[Tài liệu tiếng Trung](/solution/2100-2199/2120.Execution%20of%20All%20Suffix%20Instructions%20Staying%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Có một lưới <code>n x n</code>, trong đó ô ở góc trên bên trái là <code>(0, 0)</code> và ô ở góc dưới bên phải là <code>(n - 1, n - 1)</code>. Cho số nguyên <code>n</code> và một mảng số nguyên <code>startPos</code>, trong đó <code>startPos = [start<sub>row</sub>, start<sub>col</sub>]</code> cho biết robot ban đầu ở ô <code>(start<sub>row</sub>, start<sub>col</sub>)</code>.</p>

<p>Đồng thời, cho chuỗi <code>s</code> có độ dài <code>m</code> và được đánh <strong>chỉ số từ 0</strong>, trong đó <code>s[i]</code> là chỉ dẫn thứ <code>i<sup>th</sup></code> của robot: <code>&#39;L&#39;</code> (di chuyển sang trái), <code>&#39;R&#39;</code> (di chuyển sang phải), <code>&#39;U&#39;</code> (di chuyển lên) và <code>&#39;D&#39;</code> (di chuyển xuống).</p>

<p>Robot có thể bắt đầu thực hiện từ chỉ dẫn thứ <code>i<sup>th</sup></code> bất kỳ trong <code>s</code>. Robot thực hiện lần lượt các chỉ dẫn về phía cuối <code>s</code>, nhưng sẽ dừng lại nếu thỏa mãn một trong hai điều kiện sau:</p>

<ul>
	<li>Chỉ dẫn tiếp theo sẽ đưa robot ra khỏi lưới.</li>
	<li>Không còn chỉ dẫn nào để thực hiện.</li>
</ul>

<p>Trả về <em>một mảng</em> <code>answer</code> <em>có độ dài</em> <code>m</code> <em>trong đó</em> <code>answer[i]</code> <em>là <strong>số chỉ dẫn</strong> mà robot có thể thực hiện nếu robot <strong>bắt đầu thực hiện từ</strong></em> <code>i<sup>th</sup></code> <em>chỉ dẫn trong</em> <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2120.Execution%20of%20All%20Suffix%20Instructions%20Staying%20in%20a%20Grid/images/1.png" style="width: 145px; height: 142px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, startPos = [0,1], s = &quot;RRDDLU&quot;
<strong>Đầu ra:</strong> [1,5,4,3,1,0]
<strong>Giải thích:</strong> Bắt đầu từ startPos và bắt đầu thực hiện từ chỉ dẫn thứ i<sup>th</sup>:
- 0<sup>th</sup>: &quot;<u><strong>R</strong></u>RDDLU&quot;. Chỉ có một chỉ dẫn &quot;R&quot; có thể được thực hiện trước khi robot đi ra khỏi lưới.
- 1<sup>st</sup>:  &quot;<u><strong>RDDLU</strong></u>&quot;. Có thể thực hiện cả năm chỉ dẫn mà robot vẫn ở trong lưới và kết thúc tại (1, 1).
- 2<sup>nd</sup>:   &quot;<u><strong>DDLU</strong></u>&quot;. Có thể thực hiện cả bốn chỉ dẫn mà robot vẫn ở trong lưới và kết thúc tại (1, 0).
- 3<sup>rd</sup>:    &quot;<u><strong>DLU</strong></u>&quot;. Có thể thực hiện cả ba chỉ dẫn mà robot vẫn ở trong lưới và kết thúc tại (0, 0).
- 4<sup>th</sup>:     &quot;<u><strong>L</strong></u>U&quot;. Chỉ có một chỉ dẫn &quot;L&quot; có thể được thực hiện trước khi robot đi ra khỏi lưới.
- 5<sup>th</sup>:      &quot;U&quot;. Nếu di chuyển lên, robot sẽ đi ra khỏi lưới.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2120.Execution%20of%20All%20Suffix%20Instructions%20Staying%20in%20a%20Grid/images/2.png" style="width: 106px; height: 103px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, startPos = [1,1], s = &quot;LURD&quot;
<strong>Đầu ra:</strong> [4,1,0,0]
<strong>Giải thích:</strong>
- 0<sup>th</sup>: &quot;<u><strong>LURD</strong></u>&quot;.
- 1<sup>st</sup>:  &quot;<u><strong>U</strong></u>RD&quot;.
- 2<sup>nd</sup>:   &quot;RD&quot;.
- 3<sup>rd</sup>:    &quot;D&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2120.Execution%20of%20All%20Suffix%20Instructions%20Staying%20in%20a%20Grid/images/3.png" style="width: 67px; height: 64px;" />
<pre>
<strong>Đầu vào:</strong> n = 1, startPos = [0,0], s = &quot;LRUD&quot;
<strong>Đầu ra:</strong> [0,0,0,0]
<strong>Giải thích:</strong> Bất kể robot bắt đầu thực hiện từ chỉ dẫn nào, nó đều sẽ đi ra khỏi lưới.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == s.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 500</code></li>
	<li><code>startPos.length == 2</code></li>
	<li><code>0 &lt;= start<sub>row</sub>, start<sub>col</sub> &lt; n</code></li>
	<li><code>s</code> chỉ gồm các ký tự <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code>, <code>&#39;U&#39;</code> và <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi chỉ số bắt đầu $i$, ta thực hiện hậu tố $s[i:]$ từ $\textit{startPos}$ cho đến khi rời khỏi lưới hoặc hết chỉ dẫn. Vì $m\le 500$, việc mô phỏng mọi hậu tố có độ phức tạp $O(m^2)$ và vẫn đáp ứng yêu cầu.
>
> Nước đi được quyết định bởi ký tự tiếp theo; khi rời khỏi bàn cờ $n\times n$, quá trình di chuyển dừng lại. Các hậu tố có chung ô bắt đầu nhưng không chung đường đi, nên hầu như không có trạng thái dùng lại được.
>
> Vòng lặp bên ngoài chọn $i$, còn vòng lặp bên trong di chuyển từ $i$ đến cuối chuỗi và đếm số bước vẫn nằm trong lưới.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def executeInstructions(self, n: int, startPos: List[int], s: str) -> List[int]:
        ans = []
        m = len(s)
        mp = {"L": [0, -1], "R": [0, 1], "U": [-1, 0], "D": [1, 0]}
        for i in range(m):
            x, y = startPos
            t = 0
            for j in range(i, m):
                a, b = mp[s[j]]
                if 0 <= x + a < n and 0 <= y + b < n:
                    x, y, t = x + a, y + b, t + 1
                else:
                    break
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    public int[] executeInstructions(int n, int[] startPos, String s) {
        int m = s.length();
        int[] ans = new int[m];
        Map<Character, int[]> mp = new HashMap<>(4);
        mp.put('L', new int[] {0, -1});
        mp.put('R', new int[] {0, 1});
        mp.put('U', new int[] {-1, 0});
        mp.put('D', new int[] {1, 0});
        for (int i = 0; i < m; ++i) {
            int x = startPos[0], y = startPos[1];
            int t = 0;
            for (int j = i; j < m; ++j) {
                char c = s.charAt(j);
                int a = mp.get(c)[0], b = mp.get(c)[1];
                if (0 <= x + a && x + a < n && 0 <= y + b && y + b < n) {
                    x += a;
                    y += b;
                    ++t;
                } else {
                    break;
                }
            }
            ans[i] = t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> executeInstructions(int n, vector<int>& startPos, string s) {
        int m = s.size();
        vector<int> ans(m);
        unordered_map<char, vector<int>> mp;
        mp['L'] = {0, -1};
        mp['R'] = {0, 1};
        mp['U'] = {-1, 0};
        mp['D'] = {1, 0};
        for (int i = 0; i < m; ++i) {
            int x = startPos[0], y = startPos[1];
            int t = 0;
            for (int j = i; j < m; ++j) {
                int a = mp[s[j]][0], b = mp[s[j]][1];
                if (0 <= x + a && x + a < n && 0 <= y + b && y + b < n) {
                    x += a;
                    y += b;
                    ++t;
                } else
                    break;
            }
            ans[i] = t;
        }
        return ans;
    }
};
```

#### Go

```go
func executeInstructions(n int, startPos []int, s string) []int {
	m := len(s)
	mp := make(map[byte][]int)
	mp['L'] = []int{0, -1}
	mp['R'] = []int{0, 1}
	mp['U'] = []int{-1, 0}
	mp['D'] = []int{1, 0}
	ans := make([]int, m)
	for i := 0; i < m; i++ {
		x, y := startPos[0], startPos[1]
		t := 0
		for j := i; j < m; j++ {
			a, b := mp[s[j]][0], mp[s[j]][1]
			if 0 <= x+a && x+a < n && 0 <= y+b && y+b < n {
				x += a
				y += b
				t++
			} else {
				break
			}
		}
		ans[i] = t
	}
	return ans
}
```

#### TypeScript

```ts
function executeInstructions(n: number, startPos: number[], s: string): number[] {
    const m = s.length;
    const ans = new Array(m);
    for (let i = 0; i < m; i++) {
        let [y, x] = startPos;
        let j: number;
        for (j = i; j < m; j++) {
            const c = s[j];
            if (c === 'U') {
                y--;
            } else if (c === 'D') {
                y++;
            } else if (c === 'L') {
                x--;
            } else {
                x++;
            }
            if (y === -1 || y === n || x === -1 || x === n) {
                break;
            }
        }
        ans[i] = j - i;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn execute_instructions(n: i32, start_pos: Vec<i32>, s: String) -> Vec<i32> {
        let s = s.as_bytes();
        let m = s.len();
        let mut ans = vec![0; m];
        for i in 0..m {
            let mut y = start_pos[0];
            let mut x = start_pos[1];
            let mut j = i;
            while j < m {
                match s[j] {
                    b'U' => {
                        y -= 1;
                    }
                    b'D' => {
                        y += 1;
                    }
                    b'L' => {
                        x -= 1;
                    }
                    _ => {
                        x += 1;
                    }
                }
                if y == -1 || y == n || x == -1 || x == n {
                    break;
                }
                j += 1;
            }
            ans[i] = (j - i) as i32;
        }
        ans
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* executeInstructions(int n, int* startPos, int startPosSize, char* s, int* returnSize) {
    int m = strlen(s);
    int* ans = malloc(sizeof(int) * m);
    for (int i = 0; i < m; i++) {
        int y = startPos[0];
        int x = startPos[1];
        int j = i;
        for (j = i; j < m; j++) {
            if (s[j] == 'U') {
                y--;
            } else if (s[j] == 'D') {
                y++;
            } else if (s[j] == 'L') {
                x--;
            } else {
                x++;
            }
            if (y == -1 || y == n || x == -1 || x == n) {
                break;
            }
        }
        ans[i] = j - i;
    }
    *returnSize = m;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
