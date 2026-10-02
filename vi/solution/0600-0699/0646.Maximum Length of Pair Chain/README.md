---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Dynamic Programming
    - Sorting
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [646. Maximum Length of Pair Chain](https://leetcode.com/problems/maximum-length-of-pair-chain)

[中文文档](/solution/0600-0699/0646.Maximum%20Length%20of%20Pair%20Chain/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng gồm <code>n</code> cặp <code>pairs</code>, trong đó <code>pairs[i] = [left<sub>i</sub>, right<sub>i</sub>]</code> và <code>left<sub>i</sub> &lt; right<sub>i</sub></code>.</p>

<p>Cặp <code>p2 = [c, d]</code> <strong>nối tiếp</strong> cặp <code>p1 = [a, b]</code> nếu <code>b &lt; c</code>. Ta có thể tạo thành một <strong>chain</strong> gồm các cặp theo cách này.</p>

<p>Hãy trả về <em>độ dài của chain dài nhất có thể tạo thành</em>.</p>

<p>Bạn không cần dùng hết các khoảng đã cho. Có thể chọn các cặp theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> pairs = [[1,2],[2,3],[3,4]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chain dài nhất là [1,2] -&gt; [3,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> pairs = [[1,2],[7,8],[4,5]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chain dài nhất là [1,2] -&gt; [4,5] -&gt; [7,8].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == pairs.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>-1000 &lt;= left<sub>i</sub> &lt; right<sub>i</sub> &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting + Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Chain là một dãy cặp tăng nghiêm ngặt. DP kiểu LIS có độ phức tạp $O(n^2)$.
>
> Sắp xếp theo đầu mút phải rồi chọn mỗi cặp nếu nó nối tiếp được chain hiện tại. Đầu mút phải nhỏ hơn sẽ để lại nhiều khoảng trống hơn, nên chỉ cần một lượt greedy là tối ưu.

<!-- thinking:end -->

Ta sắp xếp các cặp theo số thứ hai tăng dần, đồng thời dùng biến $\textit{pre}$ để lưu giá trị lớn nhất của số thứ hai trong các cặp đã chọn.

Ta duyệt các cặp đã sắp xếp. Nếu số thứ nhất của cặp hiện tại lớn hơn $\textit{pre}$, ta chọn cặp này theo greedy, tăng đáp án thêm một và cập nhật $\textit{pre}$ thành số thứ hai của cặp hiện tại.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số cặp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLongestChain(self, pairs: List[List[int]]) -> int:
        pairs.sort(key=lambda x: x[1])
        ans, pre = 0, -inf
        for a, b in pairs:
            if pre < a:
                ans += 1
                pre = b
        return ans
```

#### Java

```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        Arrays.sort(pairs, (a, b) -> Integer.compare(a[1], b[1]));
        int ans = 0, pre = Integer.MIN_VALUE;
        for (var p : pairs) {
            if (pre < p[0]) {
                ++ans;
                pre = p[1];
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
    int findLongestChain(vector<vector<int>>& pairs) {
        ranges::sort(pairs, {}, [](const auto& p) { return p[1]; });
        int ans = 0, pre = INT_MIN;
        for (const auto& p : pairs) {
            if (pre < p[0]) {
                pre = p[1];
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLongestChain(pairs [][]int) (ans int) {
	sort.Slice(pairs, func(i, j int) bool { return pairs[i][1] < pairs[j][1] })
	pre := math.MinInt
	for _, p := range pairs {
		if pre < p[0] {
			ans++
			pre = p[1]
		}
	}
	return
}
```

#### TypeScript

```ts
function findLongestChain(pairs: number[][]): number {
    pairs.sort((a, b) => a[1] - b[1]);
    let [ans, pre] = [0, -Infinity];
    for (const [a, b] of pairs) {
        if (pre < a) {
            ++ans;
            pre = b;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_longest_chain(mut pairs: Vec<Vec<i32>>) -> i32 {
        pairs.sort_by_key(|pair| pair[1]);
        let mut ans = 0;
        let mut pre = i32::MIN;
        for pair in pairs {
            let (a, b) = (pair[0], pair[1]);
            if pre < a {
                ans += 1;
                pre = b;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
