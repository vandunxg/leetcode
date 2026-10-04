---
comments: true
difficulty: Medium
rating: 1696
source: Biweekly Contest 130 Q2
tags:
    - Array
    - Hash Table
    - String
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [3143. Maximum Points Inside the Square](https://leetcode.com/problems/maximum-points-inside-the-square)

[中文文档](/solution/3100-3199/3143.Maximum%20Points%20Inside%20the%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2D<strong> </strong><code>points</code> và một chuỗi <code>s</code>, trong đó <code>points[i]</code> biểu diễn tọa độ của điểm <code>i</code>, còn <code>s[i]</code> biểu diễn <strong>tag</strong> của điểm <code>i</code>.</p>

<p>Một hình vuông <strong>hợp lệ</strong> là hình vuông có tâm tại gốc tọa độ <code>(0, 0)</code>, các cạnh song song với các trục tọa độ và <strong>không</strong> chứa hai điểm có cùng tag.</p>

<p>Hãy trả về số lượng điểm <strong>lớn nhất</strong> nằm trong một hình vuông <strong>hợp lệ</strong>.</p>

<p>Lưu ý:</p>

<ul>
	<li>Một điểm được xem là nằm bên trong hình vuông nếu nó nằm trên hoặc bên trong các biên của hình vuông.</li>
	<li>Độ dài cạnh của hình vuông có thể bằng 0.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3143.Maximum%20Points%20Inside%20the%20Square/images/3708-tc1.png" style="width: 303px; height: 303px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[2,2],[-1,-2],[-4,4],[-3,1],[3,-3]], s = &quot;abdca&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hình vuông có độ dài cạnh bằng 4 bao phủ hai điểm <code>points[0]</code> và <code>points[1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3143.Maximum%20Points%20Inside%20the%20Square/images/3708-tc2.png" style="width: 302px; height: 302px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,1],[-2,-2],[-2,2]], s = &quot;abb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hình vuông có độ dài cạnh bằng 2 bao phủ một điểm, đó là <code>points[0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,1],[-1,-1],[2,-2]], s = &quot;ccd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể tạo một hình vuông hợp lệ nào có tâm tại gốc tọa độ sao cho nó chỉ bao phủ một trong hai điểm <code>points[0]</code> và <code>points[1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, points.length &lt;= 10<sup>5</sup></code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>-10<sup>9</sup> &lt;= points[i][0], points[i][1] &lt;= 10<sup>9</sup></code></li>
	<li><code>s.length == points.length</code></li>
	<li><code>points</code> gồm các tọa độ phân biệt.</li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Một hình vuông có các cạnh song song với các trục tọa độ và tâm tại gốc tọa độ phải chứa các tag phân biệt. Việc thử từng độ dài cạnh rồi kiểm tra lại các tag sẽ phụ thuộc vào khoảng tọa độ.
>
> Điểm $(x,y)$ nằm trong hình vuông có nửa cạnh $d$ khi và chỉ khi $\max(|x|,|y|)\le d$. Khi thêm các điểm theo $d$ tăng dần, ta phải dừng lại ở tag trùng đầu tiên.
>
> Nhóm các chỉ số theo $d$ và duyệt các nhóm theo thứ tự. Nếu một tag trong lớp hiện tại đã xuất hiện, trả về số lượng trước đó; nếu không, chấp nhận toàn bộ lớp hiện tại.

<!-- thinking:end -->

Với một điểm $(x, y)$, ta có thể ánh xạ nó vào góc phần tư thứ nhất với gốc tọa độ làm tâm, tức là $(\max(|x|, |y|), \max(|x|, |y|))$. Nhờ đó, ta có thể ánh xạ tất cả các điểm vào góc phần tư thứ nhất rồi sắp xếp chúng theo khoảng cách từ điểm đến gốc tọa độ.

Ta có thể dùng bảng băm $g$ để lưu khoảng cách từ tất cả các điểm đến gốc tọa độ, sau đó sắp xếp chúng theo khoảng cách. Với mỗi khoảng cách $d$, ta nhóm tất cả các điểm có khoảng cách bằng $d$ lại với nhau rồi duyệt qua các điểm này. Nếu có hai điểm có cùng tag, hình vuông đó không hợp lệ và ta trả về đáp án ngay. Nếu không, ta cộng các điểm này vào đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng điểm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPointsInsideSquare(self, points: List[List[int]], s: str) -> int:
        g = defaultdict(list)
        for i, (x, y) in enumerate(points):
            g[max(abs(x), abs(y))].append(i)
        vis = set()
        ans = 0
        for d in sorted(g):
            idx = g[d]
            for i in idx:
                if s[i] in vis:
                    return ans
                vis.add(s[i])
            ans += len(idx)
        return ans
```

#### Java

```java
class Solution {
    public int maxPointsInsideSquare(int[][] points, String s) {
        TreeMap<Integer, List<Integer>> g = new TreeMap<>();
        for (int i = 0; i < points.length; ++i) {
            int x = points[i][0], y = points[i][1];
            int key = Math.max(Math.abs(x), Math.abs(y));
            g.computeIfAbsent(key, k -> new ArrayList<>()).add(i);
        }
        boolean[] vis = new boolean[26];
        int ans = 0;
        for (var idx : g.values()) {
            for (int i : idx) {
                int j = s.charAt(i) - 'a';
                if (vis[j]) {
                    return ans;
                }
                vis[j] = true;
            }
            ans += idx.size();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPointsInsideSquare(vector<vector<int>>& points, string s) {
        map<int, vector<int>> g;
        for (int i = 0; i < points.size(); ++i) {
            auto& p = points[i];
            int key = max(abs(p[0]), abs(p[1]));
            g[key].push_back(i);
        }
        bool vis[26]{};
        int ans = 0;
        for (auto& [_, idx] : g) {
            for (int i : idx) {
                int j = s[i] - 'a';
                if (vis[j]) {
                    return ans;
                }
                vis[j] = true;
            }
            ans += idx.size();
        }
        return ans;
    }
};
```

#### Go

```go
func maxPointsInsideSquare(points [][]int, s string) (ans int) {
	g := map[int][]int{}
	for i, p := range points {
		key := max(p[0], -p[0], p[1], -p[1])
		g[key] = append(g[key], i)
	}
	vis := [26]bool{}
	keys := []int{}
	for k := range g {
		keys = append(keys, k)
	}
	sort.Ints(keys)
	for _, k := range keys {
		idx := g[k]
		for _, i := range idx {
			j := s[i] - 'a'
			if vis[j] {
				return
			}
			vis[j] = true
		}
		ans += len(idx)
	}
	return
}
```

#### TypeScript

```ts
function maxPointsInsideSquare(points: number[][], s: string): number {
    const n = points.length;
    const g: Map<number, number[]> = new Map();
    for (let i = 0; i < n; ++i) {
        const [x, y] = points[i];
        const key = Math.max(Math.abs(x), Math.abs(y));
        if (!g.has(key)) {
            g.set(key, []);
        }
        g.get(key)!.push(i);
    }
    const keys = Array.from(g.keys()).sort((a, b) => a - b);
    const vis: boolean[] = Array(26).fill(false);
    let ans = 0;
    for (const key of keys) {
        const idx = g.get(key)!;
        for (const i of idx) {
            const j = s.charCodeAt(i) - 'a'.charCodeAt(0);
            if (vis[j]) {
                return ans;
            }
            vis[j] = true;
        }
        ans += idx.length;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
