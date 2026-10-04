---
comments: true
difficulty: Medium
rating: 1882
source: Weekly Contest 377 Q3
tags:
    - Graph
    - Array
    - String
    - Shortest Path
---

<!-- problem:start -->

# [2976. Minimum Cost to Convert String I](https://leetcode.com/problems/minimum-cost-to-convert-string-i)

[中文文档](/solution/2900-2999/2976.Minimum%20Cost%20to%20Convert%20String%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp hai chuỗi <code>source</code> và <code>target</code> được đánh chỉ số từ <strong>0</strong>, có cùng độ dài <code>n</code> và chỉ gồm các chữ cái tiếng Anh <strong>viết thường</strong>. Bạn cũng được cung cấp hai mảng ký tự <code>original</code> và <code>changed</code> được đánh chỉ số từ <strong>0</strong>, cùng một mảng số nguyên <code>cost</code>, trong đó <code>cost[i]</code> là chi phí đổi ký tự <code>original[i]</code> thành ký tự <code>changed[i]</code>.</p>

<p>Ban đầu, bạn có chuỗi <code>source</code>. Trong một thao tác, bạn có thể chọn một ký tự <code>x</code> trong chuỗi và đổi nó thành ký tự <code>y</code> với chi phí <code>z</code> <strong>nếu</strong> tồn tại <strong>ít nhất một</strong> chỉ số <code>j</code> sao cho <code>cost[j] == z</code>, <code>original[j] == x</code> và <code>changed[j] == y</code>.</p>

<p>Trả về <em>chi phí <strong>nhỏ nhất</strong> để chuyển đổi chuỗi </em><code>source</code><em> thành chuỗi </em><code>target</code><em> bằng cách sử dụng <strong>bất kỳ số lượng</strong> thao tác nào. Nếu không thể chuyển đổi</em> <code>source</code> <em>thành</em> <code>target</code>, <em>trả về</em> <code>-1</code>.</p>

<p><strong>Lưu ý</strong> rằng có thể tồn tại các chỉ số <code>i</code>, <code>j</code> sao cho <code>original[j] == original[i]</code> và <code>changed[j] == changed[i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;abcd&quot;, target = &quot;acbe&quot;, original = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;c&quot;,&quot;e&quot;,&quot;d&quot;], changed = [&quot;b&quot;,&quot;c&quot;,&quot;b&quot;,&quot;e&quot;,&quot;b&quot;,&quot;e&quot;], cost = [2,5,5,1,2,20]
<strong>Đầu ra:</strong> 28
<strong>Giải thích:</strong> Để chuyển đổi chuỗi &quot;abcd&quot; thành chuỗi &quot;acbe&quot;:
- Đổi giá trị tại chỉ số 1 từ &#39;b&#39; thành &#39;c&#39; với chi phí 5.
- Đổi giá trị tại chỉ số 2 từ &#39;c&#39; thành &#39;e&#39; với chi phí 1.
- Đổi giá trị tại chỉ số 2 từ &#39;e&#39; thành &#39;b&#39; với chi phí 2.
- Đổi giá trị tại chỉ số 3 từ &#39;d&#39; thành &#39;e&#39; với chi phí 20.
Tổng chi phí là 5 + 1 + 2 + 20 = 28.
Có thể chứng minh rằng đây là chi phí nhỏ nhất có thể.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;aaaa&quot;, target = &quot;bbbb&quot;, original = [&quot;a&quot;,&quot;c&quot;], changed = [&quot;c&quot;,&quot;b&quot;], cost = [1,2]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Để đổi ký tự &#39;a&#39; thành &#39;b&#39;, hãy đổi ký tự &#39;a&#39; thành &#39;c&#39; với chi phí 1, sau đó đổi ký tự &#39;c&#39; thành &#39;b&#39; với chi phí 2, tổng cộng là 1 + 2 = 3. Để đổi tất cả các lần xuất hiện của &#39;a&#39; thành &#39;b&#39;, tổng chi phí là 3 * 4 = 12.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;abcd&quot;, target = &quot;abce&quot;, original = [&quot;a&quot;], changed = [&quot;e&quot;], cost = [10000]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể chuyển đổi source thành target vì không thể đổi giá trị tại chỉ số 3 từ &#39;d&#39; thành &#39;e&#39;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= source.length == target.length &lt;= 10<sup>5</sup></code></li>
	<li><code>source</code>, <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= cost.length == original.length == changed.length &lt;= 2000</code></li>
	<li><code>original[i]</code>, <code>changed[i]</code> là các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= cost[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>original[i] != changed[i]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Floyd

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ cái có thể được đổi thành một chữ cái khác với một chi phí nhất định; ta cần tìm phép ánh xạ rẻ nhất từ $source$ sang $target$. Có $26$ chữ cái và các phép đổi có thể kết hợp với nhau, nên ta cần tìm đường đi ngắn nhất giữa mọi cặp đỉnh. Floyd hoàn tất việc tính $g[x][y]$ trong $26^3$.
>
> Ta cộng chi phí theo từng vị trí; nếu một cặp không thể đi tới nhau thì bài toán không có lời giải. Với các cạnh song song, ta giữ lại chi phí nhỏ hơn. Độ dài chuỗi có thể lên tới $10^5$, nên phần tiền xử lý được tách riêng khỏi quá trình duyệt chuỗi.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể coi mỗi chữ cái là một node, còn chi phí chuyển đổi giữa mỗi cặp chữ cái là một cạnh có hướng. Trước tiên, ta khởi tạo một mảng hai chiều kích thước $26 \times 26$ là $g$, trong đó $g[i][j]$ biểu diễn chi phí nhỏ nhất để đổi chữ cái $i$ thành chữ cái $j$. Ban đầu, $g[i][j] = \infty$, và nếu $i = j$ thì $g[i][j] = 0$.

Tiếp theo, ta duyệt các mảng $original$, $changed$ và $cost$. Với mỗi chỉ số $i$, ta cập nhật chi phí $cost[i]$ để đổi $original[i]$ thành $changed[i]$ vào $g[original[i]][changed[i]]$, lấy giá trị nhỏ nhất.

Sau đó, ta sử dụng thuật toán Floyd để tính chi phí nhỏ nhất giữa mọi cặp node trong $g$. Cuối cùng, ta duyệt hai chuỗi $source$ và $target$. Nếu $source[i] \neq target[i]$ và $g[source[i]][target[i]] \geq \infty$, điều đó có nghĩa là không thể hoàn tất phép chuyển đổi, nên ta trả về $-1$. Ngược lại, ta cộng $g[source[i]][target[i]]$ vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(m + n + |\Sigma|^3)$, còn độ phức tạp không gian là $O(|\Sigma|^2)$. Trong đó, $m$ và $n$ lần lượt là độ dài của các mảng $original$ và $source$; còn $|\Sigma|$ là kích thước của bảng chữ cái, tức là $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(
        self,
        source: str,
        target: str,
        original: List[str],
        changed: List[str],
        cost: List[int],
    ) -> int:
        g = [[inf] * 26 for _ in range(26)]
        for i in range(26):
            g[i][i] = 0
        for x, y, z in zip(original, changed, cost):
            x = ord(x) - ord('a')
            y = ord(y) - ord('a')
            g[x][y] = min(g[x][y], z)
        for k in range(26):
            for i in range(26):
                for j in range(26):
                    g[i][j] = min(g[i][j], g[i][k] + g[k][j])
        ans = 0
        for a, b in zip(source, target):
            if a != b:
                x, y = ord(a) - ord('a'), ord(b) - ord('a')
                if g[x][y] >= inf:
                    return -1
                ans += g[x][y]
        return ans
```

#### Java

```java
class Solution {
    public long minimumCost(
        String source, String target, char[] original, char[] changed, int[] cost) {
        final int inf = 1 << 29;
        int[][] g = new int[26][26];
        for (int i = 0; i < 26; ++i) {
            Arrays.fill(g[i], inf);
            g[i][i] = 0;
        }
        for (int i = 0; i < original.length; ++i) {
            int x = original[i] - 'a';
            int y = changed[i] - 'a';
            int z = cost[i];
            g[x][y] = Math.min(g[x][y], z);
        }
        for (int k = 0; k < 26; ++k) {
            for (int i = 0; i < 26; ++i) {
                for (int j = 0; j < 26; ++j) {
                    g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }
        long ans = 0;
        int n = source.length();
        for (int i = 0; i < n; ++i) {
            int x = source.charAt(i) - 'a';
            int y = target.charAt(i) - 'a';
            if (x != y) {
                if (g[x][y] >= inf) {
                    return -1;
                }
                ans += g[x][y];
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
    long long minimumCost(string source, string target, vector<char>& original, vector<char>& changed, vector<int>& cost) {
        const int inf = 1 << 29;
        int g[26][26];
        for (int i = 0; i < 26; ++i) {
            fill(begin(g[i]), end(g[i]), inf);
            g[i][i] = 0;
        }

        for (int i = 0; i < original.size(); ++i) {
            int x = original[i] - 'a';
            int y = changed[i] - 'a';
            int z = cost[i];
            g[x][y] = min(g[x][y], z);
        }

        for (int k = 0; k < 26; ++k) {
            for (int i = 0; i < 26; ++i) {
                for (int j = 0; j < 26; ++j) {
                    g[i][j] = min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }

        long long ans = 0;
        int n = source.length();
        for (int i = 0; i < n; ++i) {
            int x = source[i] - 'a';
            int y = target[i] - 'a';
            if (x != y) {
                if (g[x][y] >= inf) {
                    return -1;
                }
                ans += g[x][y];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumCost(source string, target string, original []byte, changed []byte, cost []int) (ans int64) {
	const inf = 1 << 29
	g := make([][]int, 26)
	for i := range g {
		g[i] = make([]int, 26)
		for j := range g[i] {
			if i == j {
				g[i][j] = 0
			} else {
				g[i][j] = inf
			}
		}
	}

	for i := 0; i < len(original); i++ {
		x := int(original[i] - 'a')
		y := int(changed[i] - 'a')
		z := cost[i]
		g[x][y] = min(g[x][y], z)
	}

	for k := 0; k < 26; k++ {
		for i := 0; i < 26; i++ {
			for j := 0; j < 26; j++ {
				g[i][j] = min(g[i][j], g[i][k]+g[k][j])
			}
		}
	}
	n := len(source)
	for i := 0; i < n; i++ {
		x := int(source[i] - 'a')
		y := int(target[i] - 'a')
		if x != y {
			if g[x][y] >= inf {
				return -1
			}
			ans += int64(g[x][y])
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumCost(
    source: string,
    target: string,
    original: string[],
    changed: string[],
    cost: number[],
): number {
    const [n, m, MAX] = [source.length, original.length, Number.POSITIVE_INFINITY];
    const g: number[][] = Array.from({ length: 26 }, () => Array(26).fill(MAX));
    const getIndex = (ch: string) => ch.charCodeAt(0) - 'a'.charCodeAt(0);

    for (let i = 0; i < 26; ++i) g[i][i] = 0;
    for (let i = 0; i < m; ++i) {
        const x = getIndex(original[i]);
        const y = getIndex(changed[i]);
        const z = cost[i];
        g[x][y] = Math.min(g[x][y], z);
    }

    for (let k = 0; k < 26; ++k) {
        for (let i = 0; i < 26; ++i) {
            for (let j = 0; g[i][k] < MAX && j < 26; j++) {
                if (g[k][j] < MAX) {
                    g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }
    }

    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const x = getIndex(source[i]);
        const y = getIndex(target[i]);
        if (x === y) continue;
        if (g[x][y] === MAX) return -1;
        ans += g[x][y];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_cost(
        source: String,
        target: String,
        original: Vec<char>,
        changed: Vec<char>,
        cost: Vec<i32>,
    ) -> i64 {
        let inf: i64 = i64::MAX / 4;
        let mut g = vec![vec![inf; 26]; 26];

        for i in 0..26 {
            g[i][i] = 0;
        }

        for i in 0..original.len() {
            let x = (original[i] as u8 - b'a') as usize;
            let y = (changed[i] as u8 - b'a') as usize;
            g[x][y] = g[x][y].min(cost[i] as i64);
        }

        for k in 0..26 {
            for i in 0..26 {
                for j in 0..26 {
                    let v = g[i][k] + g[k][j];
                    if v < g[i][j] {
                        g[i][j] = v;
                    }
                }
            }
        }

        let mut ans: i64 = 0;
        for (a, b) in source.bytes().zip(target.bytes()) {
            if a != b {
                let x = (a - b'a') as usize;
                let y = (b - b'a') as usize;
                if g[x][y] >= inf {
                    return -1;
                }
                ans += g[x][y];
            }
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} source
 * @param {string} target
 * @param {character[]} original
 * @param {character[]} changed
 * @param {number[]} cost
 * @return {number}
 */
var minimumCost = function (source, target, original, changed, cost) {
    const [n, m, MAX] = [source.length, original.length, Number.POSITIVE_INFINITY];
    const g = Array.from({ length: 26 }, () => Array(26).fill(MAX));
    const getIndex = ch => ch.charCodeAt(0) - 'a'.charCodeAt(0);

    for (let i = 0; i < 26; ++i) g[i][i] = 0;
    for (let i = 0; i < m; ++i) {
        const x = getIndex(original[i]);
        const y = getIndex(changed[i]);
        const z = cost[i];
        g[x][y] = Math.min(g[x][y], z);
    }

    for (let k = 0; k < 26; ++k) {
        for (let i = 0; i < 26; ++i) {
            for (let j = 0; g[i][k] < MAX && j < 26; j++) {
                if (g[k][j] < MAX) {
                    g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }
    }

    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const x = getIndex(source[i]);
        const y = getIndex(target[i]);
        if (x === y) continue;
        if (g[x][y] === MAX) return -1;
        ans += g[x][y];
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
