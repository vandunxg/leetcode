---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3846. Total Distance to Type a String Using One Finger 🔒](https://leetcode.com/problems/total-distance-to-type-a-string-using-one-finger)

[中文文档](/solution/3800-3899/3846.Total%20Distance%20to%20Type%20a%20String%20Using%20One%20Finger/README.md)

## Mô tả

<!-- description:start -->

Có một bàn phím đặc biệt, trong đó các phím được sắp xếp trên một lưới hình chữ nhật như sau.
<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<td style="border: 1px solid black;">q</td>
			<td style="border: 1px solid black;">w</td>
			<td style="border: 1px solid black;">e</td>
			<td style="border: 1px solid black;">r</td>
			<td style="border: 1px solid black;">t</td>
			<td style="border: 1px solid black;">y</td>
			<td style="border: 1px solid black;">u</td>
			<td style="border: 1px solid black;">i</td>
			<td style="border: 1px solid black;">o</td>
			<td style="border: 1px solid black;">p</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">a</td>
			<td style="border: 1px solid black;">s</td>
			<td style="border: 1px solid black;">d</td>
			<td style="border: 1px solid black;">f</td>
			<td style="border: 1px solid black;">g</td>
			<td style="border: 1px solid black;">h</td>
			<td style="border: 1px solid black;">j</td>
			<td style="border: 1px solid black;">k</td>
			<td style="border: 1px solid black;">l</td>
			<td style="border: 1px solid black;">&nbsp;</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">z</td>
			<td style="border: 1px solid black;">x</td>
			<td style="border: 1px solid black;">c</td>
			<td style="border: 1px solid black;">v</td>
			<td style="border: 1px solid black;">b</td>
			<td style="border: 1px solid black;">n</td>
			<td style="border: 1px solid black;">m</td>
			<td style="border: 1px solid black;">&nbsp;</td>
			<td style="border: 1px solid black;">&nbsp;</td>
			<td style="border: 1px solid black;">&nbsp;</td>
		</tr>
	</tbody>
</table>

<p>Cho chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường. Hãy trả về một số nguyên biểu thị tổng <strong>khoảng cách</strong> cần di chuyển để gõ <code>s</code> chỉ bằng một ngón tay. Ngón tay của bạn bắt đầu ở phím <code>&#39;a&#39;</code>.</p>

<p><strong>Khoảng cách</strong> giữa hai phím tại <code>(r1, c1)</code> và <code>(r2, c2)</code> là <code>|r1 - r2| + |c1 - c2|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;hello&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">17</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ngón tay của bạn bắt đầu ở phím <code>&#39;a&#39;</code>, có tọa độ <code>(1, 0)</code>.</li>
	<li>Di chuyển đến phím <code>&#39;h&#39;</code>, có tọa độ <code>(1, 5)</code>. Khoảng cách là <code>|1 - 1| + |0 - 5| = 5</code>.</li>
	<li>Di chuyển đến phím <code>&#39;e&#39;</code>, có tọa độ <code>(0, 2)</code>. Khoảng cách là <code>|1 - 0| + |5 - 2| = 4</code>.</li>
	<li>Di chuyển đến phím <code>&#39;l&#39;</code>, có tọa độ <code>(1, 8)</code>. Khoảng cách là <code>|0 - 1| + |2 - 8| = 7</code>.</li>
	<li>Di chuyển đến phím <code>&#39;l&#39;</code>, có tọa độ <code>(1, 8)</code>. Khoảng cách là <code>|1 - 1| + |8 - 8| = 0</code>.</li>
	<li>Di chuyển đến phím <code>&#39;o&#39;</code>, có tọa độ <code>(0, 8)</code>. Khoảng cách là <code>|1 - 0| + |8 - 8| = 1</code>.</li>
	<li>Tổng khoảng cách là <code>5 + 4 + 7 + 0 + 1 = 17</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;a&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ngón tay của bạn bắt đầu ở phím <code>&#39;a&#39;</code>, có tọa độ <code>(1, 0)</code>.</li>
	<li>Di chuyển đến phím <code>&#39;a&#39;</code>, có tọa độ <code>(1, 0)</code>. Khoảng cách là <code>|1 - 1| + |0 - 0| = 0</code>.</li>
	<li>Tổng khoảng cách là 0.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một ngón tay bắt đầu tại $\texttt{a}$ và gõ $s$ theo thứ tự, với chi phí là khoảng cách Manhattan trên lưới bàn phím. Vì $|s| \le 10^4$ và bố cục bàn phím là cố định nên ta có thể duyệt trực tiếp.
>
> Khoảng cách giữa hai phím liên tiếp chỉ phụ thuộc vào tọa độ của chúng.
>
> Ta tiền xử lý vị trí của các phím trong ba hàng, sau đó cộng dồn khoảng cách Manhattan giữa các phím liên tiếp, bắt đầu từ vị trí ngầm định của $\texttt{a}$.
>
> Một lần duyệt là đủ để tính tổng quãng đường di chuyển.

<!-- thinking:end -->

Ta định nghĩa một hash table $\textit{pos}$ để lưu vị trí của mỗi ký tự trên bàn phím. Với mỗi ký tự trong chuỗi $s$, ta tính khoảng cách từ ký tự trước đó đến ký tự hiện tại rồi cộng vào đáp án. Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự, ở đây gồm 26 chữ cái tiếng Anh viết thường.

<!-- tabs:start -->

#### Python3

```python
pos = {}
keys = ['qwertyuiop', 'asdfghjkl', 'zxcvbnm']
for (i, row) in enumerate(keys):
    for (j, key) in enumerate(row):
        pos[key] = (i, j)


class Solution:
    def totalDistance(self, s: str) -> int:
        pre = 'a'
        ans = 0
        for cur in s:
            x1, y1 = pos[pre]
            x2, y2 = pos[cur]
            dist = abs(x1 - x2) + abs(y1 - y2)
            ans += dist
            pre = cur
        return ans
```

#### Java

```java
class Solution {
    private static final Map<Character, int[]> pos = new HashMap<>();

    static {
        String[] keys = {"qwertyuiop", "asdfghjkl", "zxcvbnm"};
        for (int i = 0; i < keys.length; i++) {
            for (int j = 0; j < keys[i].length(); j++) {
                pos.put(keys[i].charAt(j), new int[] {i, j});
            }
        }
    }

    public int totalDistance(String s) {
        char pre = 'a';
        int ans = 0;

        for (char cur : s.toCharArray()) {
            int[] p1 = pos.get(pre);
            int[] p2 = pos.get(cur);
            ans += Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1]);
            pre = cur;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalDistance(string s) {
        static unordered_map<char, pair<int, int>> pos = [] {
            unordered_map<char, pair<int, int>> m;
            vector<string> keys = {"qwertyuiop", "asdfghjkl", "zxcvbnm"};
            for (int i = 0; i < keys.size(); ++i) {
                for (int j = 0; j < keys[i].size(); ++j) {
                    m[keys[i][j]] = {i, j};
                }
            }
            return m;
        }();

        char pre = 'a';
        int ans = 0;

        for (char cur : s) {
            auto [x1, y1] = pos[pre];
            auto [x2, y2] = pos[cur];
            ans += abs(x1 - x2) + abs(y1 - y2);
            pre = cur;
        }

        return ans;
    }
};
```

#### Go

```go
var pos map[byte][2]int

func init() {
	pos = make(map[byte][2]int)
	keys := []string{"qwertyuiop", "asdfghjkl", "zxcvbnm"}
	for i, row := range keys {
		for j := 0; j < len(row); j++ {
			pos[row[j]] = [2]int{i, j}
		}
	}
}

func totalDistance(s string) int {
	pre := byte('a')
	ans := 0

	for i := 0; i < len(s); i++ {
		cur := s[i]
		p1 := pos[pre]
		p2 := pos[cur]
		ans += abs(p1[0]-p2[0]) + abs(p1[1]-p2[1])
		pre = cur
	}

	return ans
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
const pos: Record<string, [number, number]> = {};

const keys = ['qwertyuiop', 'asdfghjkl', 'zxcvbnm'];
keys.forEach((row, i) => {
    [...row].forEach((key, j) => {
        pos[key] = [i, j];
    });
});

function totalDistance(s: string): number {
    let pre = 'a';
    let ans = 0;

    for (const cur of s) {
        const [x1, y1] = pos[pre];
        const [x2, y2] = pos[cur];
        ans += Math.abs(x1 - x2) + Math.abs(y1 - y2);
        pre = cur;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
