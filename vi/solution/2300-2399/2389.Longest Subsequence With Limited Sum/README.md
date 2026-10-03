---
comments: true
difficulty: Easy
rating: 1387
source: Weekly Contest 308 Q1
tags:
    - Greedy
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2389. Longest Subsequence With Limited Sum](https://leetcode.com/problems/longest-subsequence-with-limited-sum)

[中文文档](/solution/2300-2399/2389.Longest%20Subsequence%20With%20Limited%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng số nguyên <code>queries</code> có độ dài <code>m</code>.</p>

<p>Trả về <em>một mảng </em><code>answer</code><em> có độ dài </em><code>m</code><em>, trong đó </em><code>answer[i]</code><em> là kích thước <strong>lớn nhất</strong> của một <strong>dãy con</strong> có thể lấy từ </em><code>nums</code><em> sao cho <strong>tổng</strong> các phần tử của nó nhỏ hơn hoặc bằng </em><code>queries[i]</code>.</p>

<p><strong>Dãy con</strong> là một mảng có thể thu được từ một mảng khác bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,5,2,1], queries = [3,10,21]
<strong>Đầu ra:</strong> [2,3,4]
<strong>Giải thích:</strong> Ta trả lời các truy vấn như sau:
- Dãy con [2,1] có tổng nhỏ hơn hoặc bằng 3. Có thể chứng minh rằng 2 là kích thước lớn nhất của dãy con thỏa mãn điều kiện, nên answer[0] = 2.
- Dãy con [4,5,1] có tổng nhỏ hơn hoặc bằng 10. Có thể chứng minh rằng 3 là kích thước lớn nhất của dãy con thỏa mãn điều kiện, nên answer[1] = 3.
- Dãy con [4,5,2,1] có tổng nhỏ hơn hoặc bằng 21. Có thể chứng minh rằng 4 là kích thước lớn nhất của dãy con thỏa mãn điều kiện, nên answer[2] = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,4,5], queries = [1]
<strong>Đầu ra:</strong> [0]
<strong>Giải thích:</strong> Dãy con rỗng là dãy con duy nhất có tổng nhỏ hơn hoặc bằng 1, nên answer[0] = 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>m == queries.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i], queries[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tổng tiền tố + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con dài nhất dưới một giới hạn tổng sẽ sử dụng các phần tử nhỏ nhất. Với $n,m \le 1000$, chỉ cần sắp xếp và tính tổng tiền tố.
>
> Sau khi sắp xếp, tổng tiền tố $s[j]$ là tổng của $j+1$ phần tử nhỏ nhất. Ta tìm kiếm nhị phân tổng tiền tố đầu tiên lớn hơn truy vấn; chỉ số đó chính là độ dài cần tìm.

<!-- thinking:end -->

Theo mô tả đề bài, với mỗi $\textit{queries[i]}$, ta cần tìm một dãy con sao cho tổng các phần tử của nó không vượt quá $\textit{queries[i]}$ và độ dài dãy con là lớn nhất. Rõ ràng, ta nên chọn các phần tử nhỏ nhất có thể để tối đa hóa độ dài dãy con.

Do đó, trước tiên ta sắp xếp mảng $\textit{nums}$ theo thứ tự tăng dần, sau đó với mỗi $\textit{queries[i]}$, ta dùng tìm kiếm nhị phân để tìm chỉ số nhỏ nhất $j$ sao cho $\textit{nums}[0] + \textit{nums}[1] + \cdots + \textit{nums}[j] > \textit{queries[i]}$. Khi đó, $\textit{nums}[0] + \textit{nums}[1] + \cdots + \textit{nums}[j - 1]$ là tổng các phần tử của dãy con thỏa mãn điều kiện, và độ dài của dãy con này là $j$. Vì vậy, ta có thể thêm $j$ vào mảng kết quả.

Độ phức tạp thời gian là $O((n + m) \times \log n)$, độ phức tạp không gian là $O(n)$ hoặc $O(\log n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng $\textit{nums}$ và $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def answerQueries(self, nums: List[int], queries: List[int]) -> List[int]:
        nums.sort()
        s = list(accumulate(nums))
        return [bisect_right(s, q) for q in queries]
```

#### Java

```java
class Solution {
    public int[] answerQueries(int[] nums, int[] queries) {
        Arrays.sort(nums);
        for (int i = 1; i < nums.length; ++i) {
            nums[i] += nums[i - 1];
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int j = Arrays.binarySearch(nums, queries[i] + 1);
            ans[i] = j < 0 ? -j - 1 : j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) {
        ranges::sort(nums);
        for (int i = 1; i < nums.size(); i++) {
            nums[i] += nums[i - 1];
        }
        vector<int> ans;
        for (const auto& q : queries) {
            ans.emplace_back(upper_bound(nums.begin(), nums.end(), q) - nums.begin());
        }
        return ans;
    }
};
```

#### Go

```go
func answerQueries(nums []int, queries []int) (ans []int) {
	sort.Ints(nums)
	for i := 1; i < len(nums); i++ {
		nums[i] += nums[i-1]
	}
	for _, q := range queries {
		ans = append(ans, sort.SearchInts(nums, q+1))
	}
	return
}
```

#### TypeScript

```ts
function answerQueries(nums: number[], queries: number[]): number[] {
    nums.sort((a, b) => a - b);
    for (let i = 1; i < nums.length; i++) {
        nums[i] += nums[i - 1];
    }
    return queries.map(q => _.sortedIndex(nums, q + 1));
}
```

#### Rust

```rust
impl Solution {
    pub fn answer_queries(mut nums: Vec<i32>, queries: Vec<i32>) -> Vec<i32> {
        nums.sort();

        for i in 1..nums.len() {
            nums[i] += nums[i - 1];
        }

        queries.iter().map(|&q| {
            match nums.binary_search(&q) {
                Ok(idx) => idx as i32 + 1,
                Err(idx) => idx as i32,
            }
        }).collect()
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number[]} queries
 * @return {number[]}
 */
var answerQueries = function (nums, queries) {
    nums.sort((a, b) => a - b);
    for (let i = 1; i < nums.length; i++) {
        nums[i] += nums[i - 1];
    }
    return queries.map(q => _.sortedIndex(nums, q + 1));
};
```

#### C#

```cs
public class Solution {
    public int[] AnswerQueries(int[] nums, int[] queries) {
        Array.Sort(nums);
        for (int i = 1; i < nums.Length; ++i) {
            nums[i] += nums[i - 1];
        }
        int m = queries.Length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int j = Array.BinarySearch(nums, queries[i] + 1);
            ans[i] = j < 0 ? -j - 1 : j;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Truy vấn offline + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tìm kiếm từng truy vấn riêng lẻ. Sắp xếp các truy vấn offline cho phép hai con trỏ chỉ thêm các phần tử theo chiều tiến, nên sau khi sắp xếp, phần xử lý chỉ tuyến tính.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên ta sắp xếp mảng $nums$ theo thứ tự tăng dần.

Tiếp theo, ta định nghĩa một mảng chỉ số $idx$ có cùng độ dài với $queries$, trong đó $idx[i] = i$. Sau đó, ta sắp xếp mảng $idx$ theo thứ tự tăng dần dựa trên các giá trị trong $queries$. Nhờ vậy, ta có thể xử lý các phần tử trong $queries$ theo thứ tự tăng dần.

Ta dùng biến $s$ để lưu tổng các phần tử đang được chọn và biến $j$ để lưu số lượng phần tử đang được chọn. Ban đầu, $s = j = 0$.

Ta duyệt qua mảng chỉ số $idx$. Với mỗi chỉ số $i$, ta lần lượt thêm các phần tử từ mảng $nums$ vào dãy con hiện tại cho đến khi $s + nums[j] \gt queries[i]$. Khi đó, $j$ là độ dài của dãy con thỏa mãn điều kiện. Ta gán giá trị của $ans[i]$ bằng $j$, sau đó tiếp tục xử lý chỉ số tiếp theo.

Sau khi duyệt qua mảng chỉ số $idx$, ta thu được mảng kết quả $ans$, trong đó $ans[i]$ là độ dài của dãy con thỏa mãn $queries[i]$.

Độ phức tạp thời gian là $O(n \times \log n + m)$, độ phức tạp không gian là $O(m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng $nums$ và $queries$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def answerQueries(self, nums: List[int], queries: List[int]) -> List[int]:
        nums.sort()
        m = len(queries)
        ans = [0] * m
        idx = sorted(range(m), key=lambda i: queries[i])
        s = j = 0
        for i in idx:
            while j < len(nums) and s + nums[j] <= queries[i]:
                s += nums[j]
                j += 1
            ans[i] = j
        return ans
```

#### Java

```java
class Solution {
    public int[] answerQueries(int[] nums, int[] queries) {
        Arrays.sort(nums);
        int m = queries.length;
        Integer[] idx = new Integer[m];
        for (int i = 0; i < m; ++i) {
            idx[i] = i;
        }
        Arrays.sort(idx, (i, j) -> queries[i] - queries[j]);
        int[] ans = new int[m];
        int s = 0, j = 0;
        for (int i : idx) {
            while (j < nums.length && s + nums[j] <= queries[i]) {
                s += nums[j++];
            }
            ans[i] = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) {
        sort(nums.begin(), nums.end());
        int m = queries.size();
        vector<int> idx(m);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return queries[i] < queries[j];
        });
        vector<int> ans(m);
        int s = 0, j = 0;
        for (int i : idx) {
            while (j < nums.size() && s + nums[j] <= queries[i]) {
                s += nums[j++];
            }
            ans[i] = j;
        }
        return ans;
    }
};
```

#### Go

```go
func answerQueries(nums []int, queries []int) (ans []int) {
	sort.Ints(nums)
	m := len(queries)
	idx := make([]int, m)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return queries[idx[i]] < queries[idx[j]] })
	ans = make([]int, m)
	s, j := 0, 0
	for _, i := range idx {
		for j < len(nums) && s+nums[j] <= queries[i] {
			s += nums[j]
			j++
		}
		ans[i] = j
	}
	return
}
```

#### TypeScript

```ts
function answerQueries(nums: number[], queries: number[]): number[] {
    nums.sort((a, b) => a - b);
    const m = queries.length;
    const idx: number[] = new Array(m);
    for (let i = 0; i < m; i++) {
        idx[i] = i;
    }
    idx.sort((i, j) => queries[i] - queries[j]);
    const ans: number[] = new Array(m);
    let s = 0;
    let j = 0;
    for (const i of idx) {
        while (j < nums.length && s + nums[j] <= queries[i]) {
            s += nums[j++];
        }
        ans[i] = j;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
