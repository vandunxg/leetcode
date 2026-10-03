---
comments: true
difficulty: Hard
rating: 2304
source: Biweekly Contest 76 Q4
tags:
    - Graph
    - Array
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [2242. Maximum Score of a Node Sequence](https://leetcode.com/problems/maximum-score-of-a-node-sequence)

[中文文档](/solution/2200-2299/2242.Maximum%20Score%20of%20a%20Node%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị <strong>vô hướng</strong> gồm <code>n</code> đỉnh, được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn được cung cấp một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>scores</code> có độ dài <code>n</code>, trong đó <code>scores[i]</code> là điểm số của đỉnh <code>i</code>. Bạn cũng được cung cấp một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết tồn tại một cạnh <strong>vô hướng</strong> nối đỉnh <code>a<sub>i</sub></code> và đỉnh <code>b<sub>i</sub></code>.</p>

<p>Một dãy đỉnh là <b>hợp lệ</b> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Có một cạnh nối mọi cặp đỉnh <strong>liền kề</strong> trong dãy.</li>
	<li>Không có đỉnh nào xuất hiện quá một lần trong dãy.</li>
</ul>

<p>Điểm số của một dãy đỉnh được định nghĩa là <strong>tổng</strong> điểm số của các đỉnh trong dãy.</p>

<p>Hãy trả về <em><strong>điểm số lớn nhất</strong> của một dãy đỉnh hợp lệ có độ dài </em><code>4</code><em>. </em>Nếu không tồn tại dãy như vậy, trả về<em> </em><code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2242.Maximum%20Score%20of%20a%20Node%20Sequence/images/ex1new3.png" style="width: 290px; height: 215px;" />
<pre>
<strong>Đầu vào:</strong> scores = [5,2,9,8,4], edges = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong> Hình trên cho thấy đồ thị và dãy đỉnh được chọn [0,1,2,3].
Điểm số của dãy đỉnh là 5 + 2 + 9 + 8 = 24.
Có thể chứng minh rằng không có dãy đỉnh nào khác có điểm số lớn hơn 24.
Lưu ý rằng các dãy [3,1,2,0] và [1,0,2,3] cũng hợp lệ và có điểm số bằng 24.
Dãy [0,3,2,4] không hợp lệ vì không có cạnh nào nối đỉnh 0 và đỉnh 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2242.Maximum%20Score%20of%20a%20Node%20Sequence/images/ex2.png" style="width: 333px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> scores = [9,20,6,4,11,12], edges = [[0,3],[5,3],[2,4],[1,3]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Hình trên cho thấy đồ thị.
Không có dãy đỉnh hợp lệ nào có độ dài 4, nên ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == scores.length</code></li>
	<li><code>4 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= scores[i] &lt;= 10<sup>8</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta muốn tìm bốn đỉnh liên tiếp phân biệt có tổng điểm số lớn nhất. $n \le 5\times 10^4$ khiến việc liệt kê mọi đường đi có độ dài $3$ là không thể. Phần giữa của đường đi là một cạnh $(a,b)$; hai đầu mút $c$ và $d$ lần lượt là các đỉnh kề với $a$ và $b$, đồng thời tất cả đều phân biệt.
>
> Chỉ giữ lại ba đỉnh kề có điểm số cao nhất của mỗi đỉnh. Khi liệt kê một cạnh, ta chỉ cần thử một số không đổi các cặp $(c,d)$ sau khi loại bỏ những xung đột với $a$ và $b$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, scores: List[int], edges: List[List[int]]) -> int:
        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        for k in g.keys():
            g[k] = nlargest(3, g[k], key=lambda x: scores[x])
        ans = -1
        for a, b in edges:
            for c in g[a]:
                for d in g[b]:
                    if b != c != d != a:
                        t = scores[a] + scores[b] + scores[c] + scores[d]
                        ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public int maximumScore(int[] scores, int[][] edges) {
        int n = scores.length;
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        for (int i = 0; i < n; ++i) {
            g[i].sort((a, b) -> scores[b] - scores[a]);
            g[i] = g[i].subList(0, Math.min(3, g[i].size()));
        }
        int ans = -1;
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            for (int c : g[a]) {
                for (int d : g[b]) {
                    if (c != b && c != d && a != d) {
                        int t = scores[a] + scores[b] + scores[c] + scores[d];
                        ans = Math.max(ans, t);
                    }
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
