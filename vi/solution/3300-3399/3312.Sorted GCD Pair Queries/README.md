---
comments: true
difficulty: Hard
rating: 2532
source: Weekly Contest 418 Q4
tags:
    - Array
    - Hash Table
    - Math
    - Binary Search
    - Combinatorics
    - Counting
    - Greatest Common Divisor
    - Number Theory
    - Prefix Sum
    - Euclidean Algorithm
---

<!-- problem:start -->

# [3312. Sorted GCD Pair Queries](https://leetcode.com/problems/sorted-gcd-pair-queries)

[中文文档](/solution/3300-3399/3312.Sorted%20GCD%20Pair%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng số nguyên <code>queries</code>.</p>

<p>Gọi <code>gcdPairs</code> là mảng thu được bằng cách tính <span data-keyword="gcd-function">GCD</span> của mọi cặp <code>(nums[i], nums[j])</code> có thể, trong đó <code>0 &lt;= i &lt; j &lt; n</code>, sau đó sắp xếp các giá trị này theo <strong>thứ tự tăng dần</strong>.</p>

<p>Với mỗi truy vấn <code>queries[i]</code>, bạn cần tìm phần tử tại chỉ số <code>queries[i]</code> trong <code>gcdPairs</code>.</p>

<p>Trả về một mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là giá trị tại <code>gcdPairs[queries[i]]</code> tương ứng với mỗi truy vấn.</p>

<p>Thuật ngữ <code>gcd(a, b)</code> chỉ <strong>ước chung lớn nhất</strong> của <code>a</code> và <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,4], queries = [0,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>gcdPairs = [gcd(nums[0], nums[1]), gcd(nums[0], nums[2]), gcd(nums[1], nums[2])] = [1, 2, 1]</code>.</p>

<p>Sau khi sắp xếp theo thứ tự tăng dần, <code>gcdPairs = [1, 1, 2]</code>.</p>

<p>Vì vậy, đáp án là <code>[gcdPairs[queries[0]], gcdPairs[queries[1]], gcdPairs[queries[2]]] = [1, 2, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4,2,1], queries = [5,3,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,2,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>gcdPairs</code> sau khi sắp xếp theo thứ tự tăng dần là <code>[1, 1, 1, 2, 2, 4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2], queries = [0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>gcdPairs = [2]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= queries[i] &lt; n * (n - 1) / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Tổng tiền tố + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu GCD thứ $q$ trong danh sách đã sắp xếp của tất cả các cặp không thứ tự. Với $n \le 10^5$, ta không thể liệt kê tất cả các cặp.
>
> Đếm số cặp có GCD là bội của $i$ bằng $v(v-1)/2$, sau đó trừ $\textit{cntG}[2i],\textit{cntG}[3i],\ldots$ khỏi các giá trị từ $i$ lớn đến $i$ nhỏ để tách ra các cặp có GCD đúng bằng $i$.
>
> Tổng tiền tố của $\textit{cntG}$ cho phép mỗi truy vấn tìm kiếm nhị phân chỉ số đầu tiên lớn hơn $q$.

<!-- thinking:end -->

Ta có thể tiền xử lý để thu được số lần xuất hiện của ước chung lớn nhất (GCD) của mọi cặp trong mảng $\textit{nums}$, được lưu trong mảng $\textit{cntG}$. Sau đó, ta tính tổng tiền tố của mảng $\textit{cntG}$. Cuối cùng, với mỗi truy vấn, ta có thể dùng tìm kiếm nhị phân để tìm chỉ số của phần tử đầu tiên trong mảng $\textit{cntG}$ lớn hơn $\textit{queries}[i]$, đây chính là đáp án.

Gọi $\textit{mx}$ là giá trị lớn nhất trong mảng $\textit{nums}$, và dùng $\textit{cnt}$ để lưu số lần xuất hiện của mỗi số trong mảng $\textit{nums}$. Gọi $\textit{cntG}[i]$ là số cặp trong mảng $\textit{nums}$ có GCD bằng $i$. Để tính $\textit{cntG}[i]$, ta thực hiện các bước sau:

1. Tính số lần xuất hiện $v$ của các bội của $i$ trong mảng $\textit{nums}$. Khi đó, số cặp tạo bởi hai phần tử bất kỳ trong các bội này chắc chắn có GCD là bội của $i$, tức là cần tăng $\textit{cntG}[i]$ thêm $v \times (v - 1) / 2$;
2. Loại bỏ các cặp có GCD là bội của $i$ và lớn hơn $i$. Do đó, với các bội $j$ của $i$, ta cần trừ $\textit{cntG}[j]$.

Các bước trên yêu cầu duyệt $i$ từ lớn đến nhỏ để khi tính $\textit{cntG}[i]$, ta đã tính xong mọi $\textit{cntG}[j]$.

Cuối cùng, ta tính tổng tiền tố của mảng $\textit{cntG}$. Với mỗi truy vấn, ta có thể dùng tìm kiếm nhị phân để tìm chỉ số của phần tử đầu tiên trong mảng $\textit{cntG}$ lớn hơn $\textit{queries}[i]$, đây chính là đáp án.

Độ phức tạp thời gian là $O(n + (M + q) \times \log M)$, còn độ phức tạp không gian là $O(M)$. Ở đây, $n$ và $M$ lần lượt là độ dài và giá trị lớn nhất của mảng $\textit{nums}$, còn $q$ là số lượng truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gcdValues(self, nums: List[int], queries: List[int]) -> List[int]:
        mx = max(nums)
        cnt = Counter(nums)
        cnt_g = [0] * (mx + 1)
        for i in range(mx, 0, -1):
            v = 0
            for j in range(i, mx + 1, i):
                v += cnt[j]
                cnt_g[i] -= cnt_g[j]
            cnt_g[i] += v * (v - 1) // 2
        s = list(accumulate(cnt_g))
        return [bisect_right(s, q) for q in queries]
```

#### Java

```java
class Solution {
    public int[] gcdValues(int[] nums, long[] queries) {
        int mx = Arrays.stream(nums).max().getAsInt();
        int[] cnt = new int[mx + 1];
        long[] cntG = new long[mx + 1];
        for (int x : nums) {
            ++cnt[x];
        }
        for (int i = mx; i > 0; --i) {
            int v = 0;
            for (int j = i; j <= mx; j += i) {
                v += cnt[j];
                cntG[i] -= cntG[j];
            }
            cntG[i] += 1L * v * (v - 1) / 2;
        }
        for (int i = 2; i <= mx; ++i) {
            cntG[i] += cntG[i - 1];
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            ans[i] = search(cntG, queries[i]);
        }
        return ans;
    }

    private int search(long[] nums, long x) {
        int n = nums.length;
        int l = 0, r = n;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> gcdValues(vector<int>& nums, vector<long long>& queries) {
        int mx = ranges::max(nums);
        vector<int> cnt(mx + 1);
        vector<long long> cntG(mx + 1);
        for (int x : nums) {
            ++cnt[x];
        }
        for (int i = mx; i; --i) {
            long long v = 0;
            for (int j = i; j <= mx; j += i) {
                v += cnt[j];
                cntG[i] -= cntG[j];
            }
            cntG[i] += 1LL * v * (v - 1) / 2;
        }
        for (int i = 2; i <= mx; ++i) {
            cntG[i] += cntG[i - 1];
        }
        vector<int> ans;
        for (auto&& q : queries) {
            ans.push_back(upper_bound(cntG.begin(), cntG.end(), q) - cntG.begin());
        }
        return ans;
    }
};
```

#### Go

```go
func gcdValues(nums []int, queries []int64) (ans []int) {
	mx := slices.Max(nums)
	cnt := make([]int, mx+1)
	cntG := make([]int, mx+1)
	for _, x := range nums {
		cnt[x]++
	}
	for i := mx; i > 0; i-- {
		var v int
		for j := i; j <= mx; j += i {
			v += cnt[j]
			cntG[i] -= cntG[j]
		}
		cntG[i] += v * (v - 1) / 2
	}
	for i := 2; i <= mx; i++ {
		cntG[i] += cntG[i-1]
	}
	for _, q := range queries {
		ans = append(ans, sort.SearchInts(cntG, int(q)+1))
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
