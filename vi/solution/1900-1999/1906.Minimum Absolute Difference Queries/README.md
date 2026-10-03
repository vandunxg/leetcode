---
comments: true
difficulty: Medium
rating: 2146
source: Weekly Contest 246 Q4
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1906. Minimum Absolute Difference Queries](https://leetcode.com/problems/minimum-absolute-difference-queries)

[中文文档](/solution/1900-1999/1906.Minimum%20Absolute%20Difference%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Hiệu tuyệt đối nhỏ nhất</strong> của một mảng <code>a</code> được định nghĩa là <strong>giá trị nhỏ nhất</strong> của <code>|a[i] - a[j]|</code>, trong đó <code>0 &lt;= i &lt; j &lt; a.length</code> và <code>a[i] != a[j]</code>. Nếu tất cả phần tử của <code>a</code> đều <strong>giống nhau</strong>, hiệu tuyệt đối nhỏ nhất là <code>-1</code>.</p>

<ul>
	<li>Ví dụ, hiệu tuyệt đối nhỏ nhất của mảng <code>[5,<u>2</u>,<u>3</u>,7,2]</code> là <code>|2 - 3| = 1</code>. Lưu ý rằng nó không phải là <code>0</code> vì <code>a[i]</code> và <code>a[j]</code> phải khác nhau.</li>
</ul>

<p>Cho một mảng số nguyên <code>nums</code> và mảng <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>. Với mỗi truy vấn <code>i</code>, hãy tính <strong>hiệu tuyệt đối nhỏ nhất</strong> của <strong>mảng con</strong> <code>nums[l<sub>i</sub>...r<sub>i</sub>]</code> chứa các phần tử của <code>nums</code> nằm giữa hai chỉ số <strong>đánh số từ 0</strong> <code>l<sub>i</sub></code> và <code>r<sub>i</sub></code> (<strong>bao gồm cả hai đầu</strong>).</p>

<p>Trả về <em>một <strong>mảng</strong> </em><code>ans</code> <em>trong đó</em> <code>ans[i]</code> <em>là đáp án của</em> <code>i<sup>th</sup></code> <em>truy vấn</em>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp trong một mảng.</p>

<p>Giá trị của <code>|x|</code> được định nghĩa như sau:</p>

<ul>
	<li><code>x</code> nếu <code>x &gt;= 0</code>.</li>
	<li><code>-x</code> nếu <code>x &lt; 0</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,4,8], queries = [[0,1],[1,2],[2,3],[0,3]]
<strong>Đầu ra:</strong> [2,1,4,1]
<strong>Giải thích:</strong> Các truy vấn được xử lý như sau:
- queries[0] = [0,1]: Mảng con là [<u>1</u>,<u>3</u>] và hiệu tuyệt đối nhỏ nhất là |1-3| = 2.
- queries[1] = [1,2]: Mảng con là [<u>3</u>,<u>4</u>] và hiệu tuyệt đối nhỏ nhất là |3-4| = 1.
- queries[2] = [2,3]: Mảng con là [<u>4</u>,<u>8</u>] và hiệu tuyệt đối nhỏ nhất là |4-8| = 4.
- queries[3] = [0,3]: Mảng con là [1,<u>3</u>,<u>4</u>,8] và hiệu tuyệt đối nhỏ nhất là |3-4| = 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,5,2,2,7,10], queries = [[2,3],[0,2],[0,5],[3,5]]
<strong>Đầu ra:</strong> [-1,1,1,3]
<strong>Giải thích: </strong>Các truy vấn được xử lý như sau:
- queries[0] = [2,3]: Mảng con là [2,2] và hiệu tuyệt đối nhỏ nhất là -1 vì tất cả
  phần tử đều giống nhau.
- queries[1] = [0,2]: Mảng con là [<u>4</u>,<u>5</u>,2] và hiệu tuyệt đối nhỏ nhất là |4-5| = 1.
- queries[2] = [0,5]: Mảng con là [<u>4</u>,<u>5</u>,2,2,7,10] và hiệu tuyệt đối nhỏ nhất là |4-5| = 1.
- queries[3] = [3,5]: Mảng con là [2,<u>7</u>,<u>10</u>] và hiệu tuyệt đối nhỏ nhất là |7-10| = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= queries.length &lt;= 2&nbsp;* 10<sup>4</sup></code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt; r<sub>i</sub> &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp một khoảng truy vấn để duyệt các khoảng cách giữa những phần tử kề nhau tốn $O((r-l)\log(r-l))$. Với $n\le 10^5$ và $q\le 2\times 10^4$, cách này quá chậm.
>
> Các giá trị nằm trong $[1,100]$, nên khoảng cách nhỏ nhất giữa hai giá trị phân biệt là hiệu của hai giá trị liên tiếp thực sự xuất hiện trong $[l,r]$. Ta chỉ cần biết mỗi một trong $100$ giá trị có xuất hiện hay không.
>
> Tổng tiền tố của từng giá trị cho phép kiểm tra sự xuất hiện trong $O(1)$ với mỗi giá trị; sau đó mỗi truy vấn duyệt qua $1\ldots 100$ và ghi nhận khoảng cách nhỏ nhất giữa các giá trị liên tiếp có xuất hiện.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDifference(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        m, n = len(nums), len(queries)
        pre_sum = [[0] * 101 for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, 101):
                t = 1 if nums[i - 1] == j else 0
                pre_sum[i][j] = pre_sum[i - 1][j] + t

        ans = []
        for i in range(n):
            left, right = queries[i][0], queries[i][1] + 1
            t = inf
            last = -1
            for j in range(1, 101):
                if pre_sum[right][j] - pre_sum[left][j] > 0:
                    if last != -1:
                        t = min(t, j - last)
                    last = j
            if t == inf:
                t = -1
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    public int[] minDifference(int[] nums, int[][] queries) {
        int m = nums.length, n = queries.length;
        int[][] preSum = new int[m + 1][101];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= 100; ++j) {
                int t = nums[i - 1] == j ? 1 : 0;
                preSum[i][j] = preSum[i - 1][j] + t;
            }
        }

        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int left = queries[i][0], right = queries[i][1] + 1;
            int t = Integer.MAX_VALUE;
            int last = -1;
            for (int j = 1; j <= 100; ++j) {
                if (preSum[right][j] > preSum[left][j]) {
                    if (last != -1) {
                        t = Math.min(t, j - last);
                    }
                    last = j;
                }
            }
            if (t == Integer.MAX_VALUE) {
                t = -1;
            }
            ans[i] = t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minDifference(vector<int>& nums, vector<vector<int>>& queries) {
        int m = nums.size(), n = queries.size();
        int preSum[m + 1][101];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= 100; ++j) {
                int t = nums[i - 1] == j ? 1 : 0;
                preSum[i][j] = preSum[i - 1][j] + t;
            }
        }

        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            int left = queries[i][0], right = queries[i][1] + 1;
            int t = 101;
            int last = -1;
            for (int j = 1; j <= 100; ++j) {
                if (preSum[right][j] > preSum[left][j]) {
                    if (last != -1) {
                        t = min(t, j - last);
                    }
                    last = j;
                }
            }
            if (t == 101) {
                t = -1;
            }
            ans[i] = t;
        }
        return ans;
    }
};
```

#### Go

```go
func minDifference(nums []int, queries [][]int) []int {
	m, n := len(nums), len(queries)
	preSum := make([][101]int, m+1)
	for i := 1; i <= m; i++ {
		for j := 1; j <= 100; j++ {
			t := 0
			if nums[i-1] == j {
				t = 1
			}
			preSum[i][j] = preSum[i-1][j] + t
		}
	}

	ans := make([]int, n)
	for i := 0; i < n; i++ {
		left, right := queries[i][0], queries[i][1]+1
		t, last := 101, -1
		for j := 1; j <= 100; j++ {
			if preSum[right][j]-preSum[left][j] > 0 {
				if last != -1 {
					if t > j-last {
						t = j - last
					}
				}
				last = j
			}
		}
		if t == 101 {
			t = -1
		}
		ans[i] = t
	}
	return ans
}
```

#### TypeScript

```ts
function minDifference(nums: number[], queries: number[][]): number[] {
    let m = nums.length,
        n = queries.length;
    let max = 100;
    // let max = Math.max(...nums);
    let pre: number[][] = [];
    pre.push(new Array(max + 1).fill(0));
    for (let i = 0; i < m; ++i) {
        let num = nums[i];
        pre.push(pre[i].slice());
        pre[i + 1][num] += 1;
    }

    let ans = [];
    for (let [left, right] of queries) {
        let last = -1;
        let min = Infinity;
        for (let j = 1; j < max + 1; ++j) {
            if (pre[left][j] < pre[right + 1][j]) {
                if (last != -1) {
                    min = Math.min(min, j - last);
                }
                last = j;
            }
        }
        ans.push(min == Infinity ? -1 : min);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
