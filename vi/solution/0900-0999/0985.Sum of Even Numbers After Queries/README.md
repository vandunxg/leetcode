---
comments: true
difficulty: Medium
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [985. Sum of Even Numbers After Queries](https://leetcode.com/problems/sum-of-even-numbers-after-queries)

[中文文档](/solution/0900-0999/0985.Sum%20of%20Even%20Numbers%20After%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và mảng <code>queries</code>, trong đó <code>queries[i] = [val<sub>i</sub>, index<sub>i</sub>]</code>.</p>

<p>Với mỗi query <code>i</code>, trước tiên cập nhật <code>nums[index<sub>i</sub>] = nums[index<sub>i</sub>] + val<sub>i</sub></code>, sau đó tính tổng các giá trị chẵn trong <code>nums</code>.</p>

<p>Trả về mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là kết quả của query thứ <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], queries = [[1,0],[-3,1],[-4,0],[2,3]]
<strong>Đầu ra:</strong> [8,6,2,4]
<strong>Giải thích:</strong> Ban đầu, mảng là [1,2,3,4].
Sau khi cộng 1 vào nums[0], mảng trở thành [2,2,3,4], tổng các giá trị chẵn là 2 + 2 + 4 = 8.
Sau khi cộng -3 vào nums[1], mảng trở thành [2,-1,3,4], tổng các giá trị chẵn là 2 + 4 = 6.
Sau khi cộng -4 vào nums[0], mảng trở thành [-2,-1,3,4], tổng các giá trị chẵn là -2 + 4 = 2.
Sau khi cộng 2 vào nums[3], mảng trở thành [-2,-1,3,6], tổng các giá trị chẵn là -2 + 6 = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1], queries = [[4,0]]
<strong>Đầu ra:</strong> [0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= val<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= index<sub>i</sub> &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi cộng $v$ vào một phần tử, cần trả về tổng các số chẵn. Quét lại toàn bộ mảng sau mỗi lần cập nhật sẽ quá chậm. Duy trì tổng các số chẵn $s$: trừ giá trị cũ nếu nó chẵn, cập nhật phần tử, rồi cộng giá trị mới nếu nó chẵn.

<!-- thinking:end -->

Dùng biến số nguyên $\textit{s}$ để lưu tổng các số chẵn trong mảng $\textit{nums}$. Ban đầu, $\textit{s}$ bằng tổng các số chẵn trong $\textit{nums}$.

Với mỗi query $(v, i)$, trước tiên kiểm tra xem $\textit{nums}[i]$ có chẵn không. Nếu có, trừ $\textit{nums}[i]$ khỏi $\textit{s}$. Sau đó cộng $v$ vào $\textit{nums}[i]$. Nếu giá trị mới của $\textit{nums}[i]$ là số chẵn, cộng nó vào $\textit{s}$, rồi thêm $\textit{s}$ vào mảng kết quả.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của hai mảng $\textit{nums}$ và $\textit{queries}$. Không tính bộ nhớ dành cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumEvenAfterQueries(
        self, nums: List[int], queries: List[List[int]]
    ) -> List[int]:
        s = sum(x for x in nums if x % 2 == 0)
        ans = []
        for v, i in queries:
            if nums[i] % 2 == 0:
                s -= nums[i]
            nums[i] += v
            if nums[i] % 2 == 0:
                s += nums[i]
            ans.append(s)
        return ans
```

#### Java

```java
class Solution {
    public int[] sumEvenAfterQueries(int[] nums, int[][] queries) {
        int s = 0;
        for (int x : nums) {
            if (x % 2 == 0) {
                s += x;
            }
        }
        int m = queries.length;
        int[] ans = new int[m];
        int k = 0;
        for (var q : queries) {
            int v = q[0], i = q[1];
            if (nums[i] % 2 == 0) {
                s -= nums[i];
            }
            nums[i] += v;
            if (nums[i] % 2 == 0) {
                s += nums[i];
            }
            ans[k++] = s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sumEvenAfterQueries(vector<int>& nums, vector<vector<int>>& queries) {
        int s = 0;
        for (int x : nums) {
            if (x % 2 == 0) {
                s += x;
            }
        }
        vector<int> ans;
        for (auto& q : queries) {
            int v = q[0], i = q[1];
            if (nums[i] % 2 == 0) {
                s -= nums[i];
            }
            nums[i] += v;
            if (nums[i] % 2 == 0) {
                s += nums[i];
            }
            ans.push_back(s);
        }
        return ans;
    }
};
```

#### Go

```go
func sumEvenAfterQueries(nums []int, queries [][]int) (ans []int) {
	s := 0
	for _, x := range nums {
		if x%2 == 0 {
			s += x
		}
	}
	for _, q := range queries {
		v, i := q[0], q[1]
		if nums[i]%2 == 0 {
			s -= nums[i]
		}
		nums[i] += v
		if nums[i]%2 == 0 {
			s += nums[i]
		}
		ans = append(ans, s)
	}
	return
}
```

#### TypeScript

```ts
function sumEvenAfterQueries(nums: number[], queries: number[][]): number[] {
    let s = nums.reduce((acc, x) => acc + (x % 2 === 0 ? x : 0), 0);
    const ans: number[] = [];
    for (const [v, i] of queries) {
        if (nums[i] % 2 === 0) {
            s -= nums[i];
        }
        nums[i] += v;
        if (nums[i] % 2 === 0) {
            s += nums[i];
        }
        ans.push(s);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_even_after_queries(mut nums: Vec<i32>, queries: Vec<Vec<i32>>) -> Vec<i32> {
        let mut s: i32 = nums.iter().filter(|&x| x % 2 == 0).sum();
        let mut ans = Vec::with_capacity(queries.len());

        for query in queries {
            let (v, i) = (query[0], query[1] as usize);
            if nums[i] % 2 == 0 {
                s -= nums[i];
            }
            nums[i] += v;
            if nums[i] % 2 == 0 {
                s += nums[i];
            }
            ans.push(s);
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number[][]} queries
 * @return {number[]}
 */
var sumEvenAfterQueries = function (nums, queries) {
    let s = nums.reduce((acc, cur) => acc + (cur % 2 === 0 ? cur : 0), 0);
    const ans = [];
    for (const [v, i] of queries) {
        if (nums[i] % 2 === 0) {
            s -= nums[i];
        }
        nums[i] += v;
        if (nums[i] % 2 === 0) {
            s += nums[i];
        }
        ans.push(s);
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int[] SumEvenAfterQueries(int[] nums, int[][] queries) {
        int s = nums.Where(x => x % 2 == 0).Sum();
        int[] ans = new int[queries.Length];

        for (int j = 0; j < queries.Length; j++) {
            int v = queries[j][0], i = queries[j][1];
            if (nums[i] % 2 == 0) {
                s -= nums[i];
            }
            nums[i] += v;
            if (nums[i] % 2 == 0) {
                s += nums[i];
            }
            ans[j] = s;
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
