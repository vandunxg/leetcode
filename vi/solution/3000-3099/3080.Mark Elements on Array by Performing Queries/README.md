---
comments: true
difficulty: Medium
rating: 1607
source: Biweekly Contest 126 Q2
tags:
    - Array
    - Hash Table
    - Sorting
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3080. Mark Elements on Array by Performing Queries](https://leetcode.com/problems/mark-elements-on-array-by-performing-queries)

[中文文档](/solution/3000-3099/3080.Mark%20Elements%20on%20Array%20by%20Performing%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> có kích thước <code>n</code>, gồm các số nguyên dương.</p>

<p>Bạn cũng được cho một mảng 2 chiều <code>queries</code> có kích thước <code>m</code>, trong đó <code>queries[i] = [index<sub>i</sub>, k<sub>i</sub>]</code>.</p>

<p>Ban đầu, mọi phần tử của mảng đều <strong>chưa được đánh dấu</strong>.</p>

<p>Bạn cần thực hiện lần lượt <code>m</code> truy vấn trên mảng. Với truy vấn thứ <code>i<sup>th</sup></code>, bạn thực hiện như sau:</p>

<ul>
	<li>Đánh dấu phần tử tại chỉ số <code>index<sub>i</sub></code> nếu phần tử đó chưa được đánh dấu.</li>
	<li>Sau đó, đánh dấu <code>k<sub>i</sub></code> phần tử chưa được đánh dấu có giá trị <strong>nhỏ nhất</strong> trong mảng. Nếu có nhiều phần tử như vậy, hãy đánh dấu các phần tử có chỉ số nhỏ nhất. Nếu số phần tử chưa được đánh dấu nhỏ hơn <code>k<sub>i</sub></code>, hãy đánh dấu tất cả các phần tử đó.</li>
</ul>

<p>Trả về <em>một mảng answer có kích thước </em><code>m</code><em>, trong đó </em><code>answer[i]</code><em> là <strong>tổng</strong> các phần tử chưa được đánh dấu trong mảng sau </em><code>i<sup>th</sup></code><em> truy vấn.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">nums = [1,2,2,1,2,3,1], queries = [[1,2],[3,3],[4,2]]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">[8,3,0]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta thực hiện các truy vấn sau trên mảng:</p>

<ul>
	<li>Đánh dấu phần tử tại chỉ số <code>1</code>, sau đó đánh dấu <code>2</code> phần tử chưa được đánh dấu nhỏ nhất có chỉ số nhỏ nhất nếu chúng tồn tại. Các phần tử đã được đánh dấu lúc này là <code>nums = [<strong><u>1</u></strong>,<u><strong>2</strong></u>,2,<u><strong>1</strong></u>,2,3,1]</code>. Tổng các phần tử chưa được đánh dấu là <code>2 + 2 + 3 + 1 = 8</code>.</li>
	<li>Đánh dấu phần tử tại chỉ số <code>3</code>; vì phần tử này đã được đánh dấu nên ta bỏ qua. Sau đó, ta đánh dấu <code>3</code> phần tử chưa được đánh dấu nhỏ nhất có chỉ số nhỏ nhất. Các phần tử đã được đánh dấu lúc này là <code>nums = [<strong><u>1</u></strong>,<u><strong>2</strong></u>,<u><strong>2</strong></u>,<u><strong>1</strong></u>,<u><strong>2</strong></u>,3,<strong><u>1</u></strong>]</code>. Tổng các phần tử chưa được đánh dấu là <code>3</code>.</li>
	<li>Đánh dấu phần tử tại chỉ số <code>4</code>; vì phần tử này đã được đánh dấu nên ta bỏ qua. Sau đó, ta đánh dấu <code>2</code> phần tử chưa được đánh dấu nhỏ nhất có chỉ số nhỏ nhất nếu chúng tồn tại. Các phần tử đã được đánh dấu lúc này là <code>nums = [<strong><u>1</u></strong>,<u><strong>2</strong></u>,<u><strong>2</strong></u>,<u><strong>1</strong></u>,<u><strong>2</strong></u>,<strong><u>3</u></strong>,<u><strong>1</strong></u>]</code>. Tổng các phần tử chưa được đánh dấu là <code>0</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">nums = [1,4,2,3], queries = [[0,1]]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">[7]</span></p>

<p><strong>Giải thích: </strong>Ta thực hiện một truy vấn: đánh dấu phần tử tại chỉ số <code>0</code> và đánh dấu phần tử nhỏ nhất trong số các phần tử chưa được đánh dấu. Các phần tử đã được đánh dấu là <code>nums = [<strong><u>1</u></strong>,4,<u><strong>2</strong></u>,3]</code>, nên tổng các phần tử chưa được đánh dấu là <code>4 + 3 = 7</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>m == queries.length</code></li>
	<li><code>1 &lt;= m &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= index<sub>i</sub>, k<sub>i</sub> &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn đánh dấu một chỉ số cho trước, sau đó đánh dấu $k$ giá trị nhỏ nhất hiện chưa được đánh dấu, ưu tiên chỉ số nhỏ hơn khi bằng nhau. Vì $n \le 10^5$, ta không thể tìm lại các giá trị nhỏ nhất sau mỗi truy vấn.
>
> Thứ tự các phần tử nhỏ nhất chưa được đánh dấu là cố định, nên ta có thể sắp xếp trước theo $(\textit{value},\textit{index})$ và dùng một con trỏ chỉ tiến về phía trước.
>
> Ta duy trì tổng các phần tử và một mảng đánh dấu. Với mỗi truy vấn, ta đánh dấu chỉ số được chỉ định, sau đó lấy thêm $k$ phần tử chưa được đánh dấu từ danh sách đã sắp xếp.

<!-- thinking:end -->

Đầu tiên, ta tính tổng $s$ của mảng $nums$. Ta định nghĩa một mảng $mark$ để cho biết các phần tử trong mảng đã được đánh dấu hay chưa, và khởi tạo tất cả phần tử là chưa được đánh dấu.

Sau đó, ta tạo một mảng $arr$, trong đó mỗi phần tử là một tuple $(x, i)$, biểu thị phần tử thứ $i$ trong mảng có giá trị $x$. Ta sắp xếp mảng $arr$ theo giá trị của các phần tử. Nếu các giá trị bằng nhau, ta sắp xếp chúng theo thứ tự tăng dần của chỉ số.

Tiếp theo, ta duyệt mảng $queries$. Với mỗi truy vấn $[index, k]$, trước tiên ta kiểm tra phần tử tại chỉ số $index$ đã được đánh dấu hay chưa. Nếu chưa, ta đánh dấu phần tử đó và trừ giá trị của phần tử tại chỉ số $index$ khỏi $s$. Sau đó, ta duyệt mảng $arr$. Với mỗi phần tử $(x, i)$, nếu phần tử $i$ chưa được đánh dấu, ta đánh dấu nó và trừ giá trị $x$ tương ứng với phần tử $i$ khỏi $s$, cho đến khi $k$ bằng $0$ hoặc đã duyệt hết mảng $arr$. Cuối cùng, ta thêm $s$ vào mảng kết quả.

Sau khi duyệt qua tất cả truy vấn, ta thu được mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def unmarkedSumArray(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        n = len(nums)
        s = sum(nums)
        mark = [False] * n
        arr = sorted((x, i) for i, x in enumerate(nums))
        j = 0
        ans = []
        for index, k in queries:
            if not mark[index]:
                mark[index] = True
                s -= nums[index]
            while k and j < n:
                if not mark[arr[j][1]]:
                    mark[arr[j][1]] = True
                    s -= arr[j][0]
                    k -= 1
                j += 1
            ans.append(s)
        return ans
```

#### Java

```java
class Solution {
    public long[] unmarkedSumArray(int[] nums, int[][] queries) {
        int n = nums.length;
        long s = Arrays.stream(nums).asLongStream().sum();
        boolean[] mark = new boolean[n];
        int[][] arr = new int[n][0];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[]{nums[i], i};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int m = queries.length;
        long[] ans = new long[m];
        for (int i = 0, j = 0; i < m; ++i) {
            int index = queries[i][0], k = queries[i][1];
            if (!mark[index]) {
                mark[index] = true;
                s -= nums[index];
            }
            for (; k > 0 && j < n; ++j) {
                if (!mark[arr[j][1]]) {
                    mark[arr[j][1]] = true;
                    s -= arr[j][0];
                    --k;
                }
            }
            ans[i] = s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> unmarkedSumArray(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        long long s = accumulate(nums.begin(), nums.end(), 0LL);
        vector<bool> mark(n);
        vector<pair<int, int>> arr;
        for (int i = 0; i < n; ++i) {
            arr.emplace_back(nums[i], i);
        }
        sort(arr.begin(), arr.end());
        vector<long long> ans;
        int m = queries.size();
        for (int i = 0, j = 0; i < m; ++i) {
            int index = queries[i][0], k = queries[i][1];
            if (!mark[index]) {
                mark[index] = true;
                s -= nums[index];
            }
            for (; k && j < n; ++j) {
                if (!mark[arr[j].second]) {
                    mark[arr[j].second] = true;
                    s -= arr[j].first;
                    --k;
                }
            }
            ans.push_back(s);
        }
        return ans;
    }
};
```

#### Go

```go
func unmarkedSumArray(nums []int, queries [][]int) []int64 {
	n := len(nums)
	var s int64
	for _, x := range nums {
		s += int64(x)
	}
	mark := make([]bool, n)
	arr := make([][2]int, 0, n)
	for i, x := range nums {
		arr = append(arr, [2]int{x, i})
	}
	sort.Slice(arr, func(i, j int) bool {
		if arr[i][0] == arr[j][0] {
			return arr[i][1] < arr[j][1]
		}
		return arr[i][0] < arr[j][0]
	})
	ans := make([]int64, len(queries))
	j := 0
	for i, q := range queries {
		index, k := q[0], q[1]
		if !mark[index] {
			mark[index] = true
			s -= int64(nums[index])
		}
		for ; k > 0 && j < n; j++ {
			if !mark[arr[j][1]] {
				mark[arr[j][1]] = true
				s -= int64(arr[j][0])
				k--
			}
		}
		ans[i] = s
	}
	return ans
}
```

#### TypeScript

```ts
function unmarkedSumArray(nums: number[], queries: number[][]): number[] {
    const n = nums.length;
    let s = nums.reduce((acc, x) => acc + x, 0);
    const mark: boolean[] = Array(n).fill(false);
    const arr = nums.map((x, i) => [x, i]);
    arr.sort((a, b) => (a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]));
    let j = 0;
    const ans: number[] = [];
    for (let [index, k] of queries) {
        if (!mark[index]) {
            mark[index] = true;
            s -= nums[index];
        }
        for (; k && j < n; ++j) {
            if (!mark[arr[j][1]]) {
                mark[arr[j][1]] = true;
                s -= arr[j][0];
                --k;
            }
        }
        ans.push(s);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
