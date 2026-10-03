---
comments: true
difficulty: Medium
rating: 1651
source: Weekly Contest 302 Q3
tags:
    - Array
    - String
    - Divide and Conquer
    - Quickselect
    - Radix Sort
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2343. Query Kth Smallest Trimmed Number](https://leetcode.com/problems/query-kth-smallest-trimmed-number)

[中文文档](/solution/2300-2399/2343.Query%20Kth%20Smallest%20Trimmed%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>nums</code> được đánh chỉ số từ <strong>0</strong>, trong đó mọi chuỗi có <strong>cùng độ dài</strong> và chỉ gồm các chữ số.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code> được đánh chỉ số từ <strong>0</strong>, trong đó <code>queries[i] = [k<sub>i</sub>, trim<sub>i</sub>]</code>. Với mỗi <code>queries[i]</code>, bạn cần:</p>

<ul>
	<li><strong>Cắt</strong> mỗi số trong <code>nums</code> để chỉ giữ lại <strong><code>trim<sub>i</sub></code> chữ số ngoài cùng bên phải</strong>.</li>
	<li>Xác định <strong>chỉ số</strong> của số sau khi cắt nhỏ thứ <code>k<sub>i</sub><sup>th</sup></code> trong <code>nums</code>. Nếu hai số sau khi cắt bằng nhau, số có <strong>chỉ số</strong> nhỏ hơn được xem là nhỏ hơn.</li>
	<li>Đưa mỗi số trong <code>nums</code> về độ dài ban đầu.</li>
</ul>

<p>Trả về <em>một mảng </em><code>answer</code><em> có cùng độ dài với </em><code>queries</code><em>, trong đó </em><code>answer[i]</code><em> là đáp án của truy vấn thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li>Cắt để chỉ còn <code>x</code> chữ số ngoài cùng bên phải nghĩa là liên tục xóa chữ số ngoài cùng bên trái cho đến khi chỉ còn <code>x</code> chữ số.</li>
	<li>Các chuỗi trong <code>nums</code> có thể chứa các số 0 ở đầu.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;102&quot;,&quot;473&quot;,&quot;251&quot;,&quot;814&quot;], queries = [[1,1],[2,3],[4,2],[1,2]]
<strong>Đầu ra:</strong> [2,2,1,0]
<strong>Giải thích:</strong>
1. Sau khi cắt còn chữ số cuối cùng, nums = [&quot;2&quot;,&quot;3&quot;,&quot;1&quot;,&quot;4&quot;]. Số nhỏ nhất là 1 ở chỉ số 2.
2. Sau khi cắt còn 3 chữ số cuối cùng, nums không thay đổi. Số nhỏ thứ 2<sup>nd</sup> là 251 ở chỉ số 2.
3. Sau khi cắt còn 2 chữ số cuối cùng, nums = [&quot;02&quot;,&quot;73&quot;,&quot;51&quot;,&quot;14&quot;]. Số nhỏ thứ 4<sup>th</sup> là 73.
4. Sau khi cắt còn 2 chữ số cuối cùng, số nhỏ nhất là 2 ở chỉ số 0.
   Lưu ý rằng số sau khi cắt &quot;02&quot; được xem là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;24&quot;,&quot;37&quot;,&quot;96&quot;,&quot;04&quot;], queries = [[2,1],[2,2]]
<strong>Đầu ra:</strong> [3,0]
<strong>Giải thích:</strong>
1. Sau khi cắt còn chữ số cuối cùng, nums = [&quot;4&quot;,&quot;7&quot;,&quot;6&quot;,&quot;4&quot;]. Số nhỏ thứ 2<sup>nd</sup> là 4 ở chỉ số 3.
   Có hai số 4, nhưng số ở chỉ số 0 được xem là nhỏ hơn số ở chỉ số 3.
2. Sau khi cắt còn 2 chữ số cuối cùng, nums không thay đổi. Số nhỏ thứ 2<sup>nd</sup> là 24.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i].length &lt;= 100</code></li>
	<li><code>nums[i]</code> chỉ gồm các chữ số.</li>
	<li>Mọi <code>nums[i].length</code> đều <strong>bằng nhau</strong>.</li>
	<li><code>1 &lt;= queries.length &lt;= 100</code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>1 &lt;= k<sub>i</sub> &lt;= nums.length</code></li>
	<li><code>1 &lt;= trim<sub>i</sub> &lt;= nums[i].length</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể dùng <strong>thuật toán Radix Sort</strong> để giải bài toán này không? Độ phức tạp của lời giải đó là bao nhiêu?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn giữ lại $trim$ chữ số ngoài cùng bên phải và yêu cầu chỉ số ban đầu đứng thứ $k$ theo thứ tự tăng dần. Kích thước đầu vào đủ nhỏ để sắp xếp lại cho từng truy vấn.
>
> Với $(k, trim)$, ta sắp xếp các hậu tố kèm chỉ số; đáp án là chỉ số thứ $k$. Thứ tự từ điển của chuỗi tự xử lý các số 0 ở đầu.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể mô phỏng quá trình cắt, sau đó sắp xếp các chuỗi đã cắt và cuối cùng tìm số tương ứng với chỉ số.

Độ phức tạp thời gian là $O(m \times n \times \log n \times s)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $m$ và $n$ lần lượt là độ dài của mảng $\textit{nums}$ và $\textit{queries}$, còn $s$ là độ dài của chuỗi $\textit{nums}[i]$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestTrimmedNumbers(
        self, nums: List[str], queries: List[List[int]]
    ) -> List[int]:
        ans = []
        for k, trim in queries:
            t = sorted((v[-trim:], i) for i, v in enumerate(nums))
            ans.append(t[k - 1][1])
        return ans
```

#### Java

```java
class Solution {
    public int[] smallestTrimmedNumbers(String[] nums, int[][] queries) {
        int n = nums.length;
        int m = queries.length;
        int[] ans = new int[m];
        String[][] t = new String[n][2];
        for (int i = 0; i < m; ++i) {
            int k = queries[i][0], trim = queries[i][1];
            for (int j = 0; j < n; ++j) {
                t[j] = new String[] {nums[j].substring(nums[j].length() - trim), String.valueOf(j)};
            }
            Arrays.sort(t, (a, b) -> {
                int x = a[0].compareTo(b[0]);
                return x == 0 ? Long.compare(Integer.valueOf(a[1]), Integer.valueOf(b[1])) : x;
            });
            ans[i] = Integer.valueOf(t[k - 1][1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallestTrimmedNumbers(vector<string>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        vector<pair<string, int>> t(n);
        vector<int> ans;
        for (auto& q : queries) {
            int k = q[0], trim = q[1];
            for (int j = 0; j < n; ++j) {
                t[j] = {nums[j].substr(nums[j].size() - trim), j};
            }
            sort(t.begin(), t.end());
            ans.push_back(t[k - 1].second);
        }
        return ans;
    }
};
```

#### Go

```go
func smallestTrimmedNumbers(nums []string, queries [][]int) []int {
	type pair struct {
		s string
		i int
	}
	ans := make([]int, len(queries))
	t := make([]pair, len(nums))
	for i, q := range queries {
		for j, s := range nums {
			t[j] = pair{s[len(s)-q[1]:], j}
		}
		sort.Slice(t, func(i, j int) bool { a, b := t[i], t[j]; return a.s < b.s || a.s == b.s && a.i < b.i })
		ans[i] = t[q[0]-1].i
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
