---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
    - Sorting
---

<!-- problem:start -->

# [2638. Count the Number of K-Free Subsets 🔒](https://leetcode.com/problems/count-the-number-of-k-free-subsets)

[中文文档](/solution/2600-2699/2638.Count%20the%20Number%20of%20K-Free%20Subsets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>, trong đó các phần tử <strong>khác nhau</strong>, và một số nguyên <code>k</code>.</p>

<p>Một tập con được gọi là tập con <strong>k-Free</strong> nếu không chứa <strong>hai</strong> phần tử nào có hiệu tuyệt đối bằng <code>k</code>. Lưu ý rằng tập rỗng là một tập con <strong>k-Free</strong>.</p>

<p>Trả về <em>số lượng tập con <strong>k-Free</strong> của </em><code>nums</code>.</p>

<p><b>Tập con</b> của một mảng là cách chọn các phần tử (có thể không chọn phần tử nào) từ mảng đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,6], k = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có 5 tập con hợp lệ: {}, {5}, {4}, {6} và {4, 6}.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,5,8], k = 5
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Có 12 tập con hợp lệ: {}, {2}, {3}, {5}, {8}, {2, 3}, {2, 3, 5}, {2, 5}, {2, 5, 8}, {2, 8}, {3, 5} và {5, 8}.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,5,9,11], k = 20
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Mọi tập con đều hợp lệ. Vì tổng số tập con là 2<sup>4 </sup>= 16 nên đáp án là 16.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một tập con không được chứa hai giá trị có hiệu bằng $k$. Việc liệt kê $2^n$ tập con không phù hợp với $n \le 50$. Các giá trị có hiệu bằng $k$ có cùng phần dư khi chia cho $k$, nên các lớp phần dư khác nhau là độc lập.
>
> Trong một lớp đã sắp xếp, chỉ có khoảng cách $k$ giữa hai phần tử kề nhau mới cấm chọn đồng thời, nên ta có thể dùng DP tuyến tính: $f[i]=f[i-1]+f[i-2]$ khi có xung đột, ngược lại $f[i]=2f[i-1]$.
>
> Nhân kết quả của các lớp; $f[0]=1$ để tính cả tập con rỗng.

<!-- thinking:end -->

Trước tiên, sắp xếp mảng $nums$ theo thứ tự tăng dần, sau đó nhóm các phần tử trong mảng theo phần dư khi chia cho $k$, tức là các phần tử $nums[i] \bmod k$ có cùng phần dư sẽ thuộc cùng một nhóm. Khi đó, hai phần tử bất kỳ thuộc hai nhóm khác nhau sẽ không có hiệu tuyệt đối bằng $k$. Vì vậy, ta có thể tính số tập con trong từng nhóm, rồi nhân số tập con của các nhóm để thu được đáp án.

Với mỗi nhóm $arr$, ta có thể dùng quy hoạch động để tính số tập con. Gọi $f[i]$ là số tập con của $i$ phần tử đầu tiên, ban đầu $f[0] = 1$ và $f[1]=2$. Khi $i \geq 2$, nếu $arr[i-1]-arr[i-2]=k$, nếu chọn $arr[i-1]$ thì $f[i]=f[i-2]$; nếu không chọn $arr[i-1]$ thì $f[i]=f[i-1]$. Do đó, khi $arr[i-1]-arr[i-2]=k$, ta có $f[i]=f[i-1]+f[i-2]$; ngược lại, $f[i] = f[i - 1] \times 2$. Số tập con của nhóm này là $f[m]$, trong đó $m$ là độ dài của mảng $arr$.

Cuối cùng, nhân số tập con của từng nhóm để thu được đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTheNumOfKFreeSubsets(self, nums: List[int], k: int) -> int:
        nums.sort()
        g = defaultdict(list)
        for x in nums:
            g[x % k].append(x)
        ans = 1
        for arr in g.values():
            m = len(arr)
            f = [0] * (m + 1)
            f[0] = 1
            f[1] = 2
            for i in range(2, m + 1):
                if arr[i - 1] - arr[i - 2] == k:
                    f[i] = f[i - 1] + f[i - 2]
                else:
                    f[i] = f[i - 1] * 2
            ans *= f[m]
        return ans
```

#### Java

```java
class Solution {
    public long countTheNumOfKFreeSubsets(int[] nums, int k) {
        Arrays.sort(nums);
        Map<Integer, List<Integer>> g = new HashMap<>();
        for (int i = 0; i < nums.length; ++i) {
            g.computeIfAbsent(nums[i] % k, x -> new ArrayList<>()).add(nums[i]);
        }
        long ans = 1;
        for (var arr : g.values()) {
            int m = arr.size();
            long[] f = new long[m + 1];
            f[0] = 1;
            f[1] = 2;
            for (int i = 2; i <= m; ++i) {
                if (arr.get(i - 1) - arr.get(i - 2) == k) {
                    f[i] = f[i - 1] + f[i - 2];
                } else {
                    f[i] = f[i - 1] * 2;
                }
            }
            ans *= f[m];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countTheNumOfKFreeSubsets(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        unordered_map<int, vector<int>> g;
        for (int i = 0; i < nums.size(); ++i) {
            g[nums[i] % k].push_back(nums[i]);
        }
        long long ans = 1;
        for (auto& [_, arr] : g) {
            int m = arr.size();
            long long f[m + 1];
            f[0] = 1;
            f[1] = 2;
            for (int i = 2; i <= m; ++i) {
                if (arr[i - 1] - arr[i - 2] == k) {
                    f[i] = f[i - 1] + f[i - 2];
                } else {
                    f[i] = f[i - 1] * 2;
                }
            }
            ans *= f[m];
        }
        return ans;
    }
};
```

#### Go

```go
func countTheNumOfKFreeSubsets(nums []int, k int) int64 {
	sort.Ints(nums)
	g := map[int][]int{}
	for _, x := range nums {
		g[x%k] = append(g[x%k], x)
	}
	ans := int64(1)
	for _, arr := range g {
		m := len(arr)
		f := make([]int64, m+1)
		f[0] = 1
		f[1] = 2
		for i := 2; i <= m; i++ {
			if arr[i-1]-arr[i-2] == k {
				f[i] = f[i-1] + f[i-2]
			} else {
				f[i] = f[i-1] * 2
			}
		}
		ans *= f[m]
	}
	return ans
}
```

#### TypeScript

```ts
function countTheNumOfKFreeSubsets(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    const g: Map<number, number[]> = new Map();
    for (const x of nums) {
        const y = x % k;
        if (!g.has(y)) {
            g.set(y, []);
        }
        g.get(y)!.push(x);
    }
    let ans: number = 1;
    for (const [_, arr] of g) {
        const m = arr.length;
        const f: number[] = new Array(m + 1).fill(1);
        f[1] = 2;
        for (let i = 2; i <= m; ++i) {
            if (arr[i - 1] - arr[i - 2] === k) {
                f[i] = f[i - 1] + f[i - 2];
            } else {
                f[i] = f[i - 1] * 2;
            }
        }
        ans *= f[m];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
