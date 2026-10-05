---
comments: true
difficulty: Medium
rating: 1387
source: Weekly Contest 481 Q2
tags:
    - Array
    - Hash Table
    - String
    - Enumeration
---

<!-- problem:start -->

# [3784. Minimum Deletion Cost to Make All Characters Equal](https://leetcode.com/problems/minimum-deletion-cost-to-make-all-characters-equal)

[中文文档](/solution/3700-3799/3784.Minimum%20Deletion%20Cost%20to%20Make%20All%20Characters%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> có độ dài <code>n</code> và một mảng số nguyên <code>cost</code> có cùng độ dài, trong đó <code>cost[i]</code> là chi phí để <strong>xóa</strong> ký tự thứ <code>i<sup>th</sup></code> của <code>s</code>.</p>

<p>Bạn có thể xóa bất kỳ số lượng ký tự nào khỏi <code>s</code> (có thể không xóa), sao cho chuỗi kết quả <strong>không rỗng</strong> và chỉ gồm các ký tự <strong>giống nhau</strong>.</p>

<p>Trả về một số nguyên biểu thị tổng chi phí xóa <strong>nhỏ nhất</strong> cần thiết.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabaac&quot;, cost = [1,2,3,4,1,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa các ký tự tại các chỉ số 0, 1, 2, 3, 4 sẽ thu được chuỗi <code>&quot;c&quot;</code>, chỉ gồm các ký tự giống nhau, và tổng chi phí là <code>cost[0] + cost[1] + cost[2] + cost[3] + cost[4] = 1 + 2 + 3 + 4 + 1 = 11</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;, cost = [10,5,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa các ký tự tại các chỉ số 1 và 2 sẽ thu được chuỗi <code>&quot;a&quot;</code>, chỉ gồm các ký tự giống nhau, và tổng chi phí là <code>cost[1] + cost[2] = 5 + 8 = 13</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zzzzz&quot;, cost = [67,67,67,67,67]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả ký tự trong <code>s</code> đều giống nhau, nên chi phí xóa là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s.length == cost.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= cost[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi còn lại phải không rỗng và chỉ gồm một loại ký tự, tức là ta xóa mọi ký tự khác. Chi phí giữ lại chữ cái $c$ bằng tổng chi phí trừ đi chi phí của tất cả các ký tự $c$; ta lấy giá trị nhỏ nhất trên mọi $c$.

<!-- thinking:end -->

Ta tính tổng chi phí xóa của từng ký tự trong chuỗi và lưu vào một bảng băm $g$, trong đó key là ký tự còn value là tổng chi phí xóa tương ứng. Đồng thời, ta tính tổng chi phí $\textit{tot}$ khi xóa tất cả ký tự.

Tiếp theo, ta duyệt qua bảng băm $g$. Với mỗi ký tự $c$, ta tính chi phí xóa nhỏ nhất cần thiết để giữ lại ký tự đó, bằng $\textit{tot} - g[c]$. Đáp án cuối cùng là giá trị nhỏ nhất trong các chi phí xóa nhỏ nhất tương ứng với từng ký tự.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài của chuỗi $s$, còn $\Sigma$ là tập các ký tự phân biệt xuất hiện trong chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, s: str, cost: List[int]) -> int:
        tot = 0
        g = defaultdict(int)
        for c, v in zip(s, cost):
            tot += v
            g[c] += v
        return min(tot - x for x in g.values())
```

#### Java

```java
class Solution {
    public long minCost(String s, int[] cost) {
        long tot = 0;
        Map<Character, Long> g = new HashMap<>(26);
        for (int i = 0; i < cost.length; ++i) {
            tot += cost[i];
            g.merge(s.charAt(i), (long) cost[i], Long::sum);
        }
        long ans = tot;
        for (long v : g.values()) {
            ans = Math.min(ans, tot - v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCost(string s, vector<int>& cost) {
        long long tot = 0;
        unordered_map<char, long long> g;
        for (int i = 0; i < cost.size(); ++i) {
            tot += cost[i];
            g[s[i]] += cost[i];
        }
        long long ans = tot;
        for (auto [_, v] : g) {
            ans = min(ans, tot - v);
        }
        return ans;
    }
};
```

#### Go

```go
func minCost(s string, cost []int) int64 {
	tot := int64(0)
	g := map[byte]int64{}
	for i, v := range cost {
		tot += int64(v)
		g[s[i]] += int64(v)
	}
	ans := tot
	for _, x := range g {
		ans = min(ans, tot-x)
	}
	return ans
}
```

#### TypeScript

```ts
function minCost(s: string, cost: number[]): number {
    let tot = 0;
    const g: Map<string, number> = new Map();
    for (let i = 0; i < s.length; i++) {
        const c = s[i];
        const v = cost[i];
        tot += v;
        g.set(c, (g.get(c) ?? 0) + v);
    }
    let ans = tot;
    for (const x of g.values()) {
        ans = Math.min(ans, tot - x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
