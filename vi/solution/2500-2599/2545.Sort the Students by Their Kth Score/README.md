---
comments: true
difficulty: Medium
rating: 1294
source: Weekly Contest 329 Q2
tags:
    - Array
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [2545. Sort the Students by Their Kth Score](https://leetcode.com/problems/sort-the-students-by-their-kth-score)

[中文文档](/solution/2500-2599/2545.Sort%20the%20Students%20by%20Their%20Kth%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Có một lớp học gồm <code>m</code> học sinh và <code>n</code> bài thi. Bạn được cho một ma trận số nguyên <strong>đánh chỉ số từ 0</strong> <code>m x n</code> <code>score</code>, trong đó mỗi hàng biểu diễn một học sinh và <code>score[i][j]</code> là điểm số học sinh thứ <code>i<sup>th</sup></code> đạt được trong bài thi thứ <code>j<sup>th</sup></code>. Ma trận <code>score</code> chỉ chứa các số nguyên <strong>khác nhau</strong>.</p>

<p>Bạn cũng được cho một số nguyên <code>k</code>. Hãy sắp xếp các học sinh (tức là các hàng của ma trận) theo điểm số trong bài thi thứ <code>k<sup>th</sup></code>&nbsp;(<strong>đánh chỉ số từ 0</strong>) theo thứ tự từ cao xuống thấp.</p>

<p>Trả về <em>ma trận sau khi sắp xếp.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2545.Sort%20the%20Students%20by%20Their%20Kth%20Score/images/example1.png" style="width: 600px; height: 136px;" />
<pre>
<strong>Đầu vào:</strong> score = [[10,6,9,1],[7,5,11,2],[4,8,3,15]], k = 2
<strong>Đầu ra:</strong> [[7,5,11,2],[10,6,9,1],[4,8,3,15]]
<strong>Giải thích:</strong> Trong sơ đồ trên, S biểu thị học sinh, còn E biểu thị bài thi.
- Học sinh có chỉ số 1 đạt 11 điểm trong bài thi 2, là điểm cao nhất, nên xếp hạng nhất.
- Học sinh có chỉ số 0 đạt 9 điểm trong bài thi 2, là điểm cao thứ hai, nên xếp hạng nhì.
- Học sinh có chỉ số 2 đạt 3 điểm trong bài thi 2, là điểm thấp nhất, nên xếp hạng ba.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2545.Sort%20the%20Students%20by%20Their%20Kth%20Score/images/example2.png" style="width: 486px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> score = [[3,4],[5,6]], k = 0
<strong>Đầu ra:</strong> [[5,6],[3,4]]
<strong>Giải thích:</strong> Trong sơ đồ trên, S biểu thị học sinh, còn E biểu thị bài thi.
- Học sinh có chỉ số 1 đạt 5 điểm trong bài thi 0, là điểm cao nhất, nên xếp hạng nhất.
- Học sinh có chỉ số 0 đạt 3 điểm trong bài thi 0, là điểm thấp nhất, nên xếp hạng nhì.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == score.length</code></li>
	<li><code>n == score[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 250</code></li>
	<li><code>1 &lt;= score[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>score</code> chỉ gồm các số nguyên <strong>khác nhau</strong>.</li>
	<li><code>0 &lt;= k &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp học sinh theo điểm bài thi thứ $k$ giảm dần. Các hàng độc lập, vì vậy chỉ cần sắp xếp một lần với khóa $-x[k]$ là đủ.

<!-- thinking:end -->

Ta sắp xếp trực tiếp $\textit{score}$ theo thứ tự giảm dần dựa trên điểm số ở cột thứ $k$, sau đó trả về kết quả.

Độ phức tạp thời gian là $O(m \times \log m)$, độ phức tạp không gian là $O(\log m)$. Ở đây, $m$ là số hàng của $\textit{score}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortTheStudents(self, score: List[List[int]], k: int) -> List[List[int]]:
        return sorted(score, key=lambda x: -x[k])
```

#### Java

```java
class Solution {
    public int[][] sortTheStudents(int[][] score, int k) {
        Arrays.sort(score, (a, b) -> b[k] - a[k]);
        return score;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> sortTheStudents(vector<vector<int>>& score, int k) {
        ranges::sort(score, [k](const auto& a, const auto& b) { return a[k] > b[k]; });
        return score;
    }
};
```

#### Go

```go
func sortTheStudents(score [][]int, k int) [][]int {
	sort.Slice(score, func(i, j int) bool { return score[i][k] > score[j][k] })
	return score
}
```

#### TypeScript

```ts
function sortTheStudents(score: number[][], k: number): number[][] {
    return score.sort((a, b) => b[k] - a[k]);
}
```

#### Rust

```rust
impl Solution {
    pub fn sort_the_students(mut score: Vec<Vec<i32>>, k: i32) -> Vec<Vec<i32>> {
        let k = k as usize;
        score.sort_by(|a, b| b[k].cmp(&a[k]));
        score
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
