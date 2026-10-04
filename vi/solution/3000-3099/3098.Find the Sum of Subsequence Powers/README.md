---
comments: true
difficulty: Hard
rating: 2552
source: Biweekly Contest 127 Q4
tags:
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3098. Find the Sum of Subsequence Powers](https://leetcode.com/problems/find-the-sum-of-subsequence-powers)

[中文文档](/solution/3000-3099/3098.Find%20the%20Sum%20of%20Subsequence%20Powers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p><strong>Power</strong> của một <span data-keyword="subsequence-array">dãy con</span> được định nghĩa là <strong>hiệu tuyệt đối nhỏ nhất</strong> giữa <strong>bất kỳ</strong> hai phần tử nào trong dãy con.</p>

<p>Hãy trả về <em><strong>tổng</strong> các <strong>power</strong> của <strong>tất cả</strong> các dãy con của </em><code>nums</code><em> có độ dài</em> <strong><em>bằng</em></strong> <code>k</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 4 dãy con trong <code>nums</code> có độ dài 3: <code>[1,2,3]</code>, <code>[1,3,4]</code>, <code>[1,2,4]</code> và <code>[2,3,4]</code>. Tổng các power là <code>|2 - 3| + |3 - 4| + |2 - 1| + |3 - 4| = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con duy nhất trong <code>nums</code> có độ dài 2 là&nbsp;<code>[2,2]</code>. Tổng các power là <code>|2 - 2| = 0</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,-1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 3 dãy con trong <code>nums</code> có độ dài 2: <code>[4,3]</code>, <code>[4,-1]</code> và <code>[3,-1]</code>. Tổng các power là <code>|4 - 3| + |4 - (-1)| + |3 - (-1)| = 10</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 50</code></li>
	<li><code>-10<sup>8</sup> &lt;= nums[i] &lt;= 10<sup>8</sup> </code></li>
	<li><code>2 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Power của một dãy con là hiệu nhỏ nhất giữa từng cặp phần tử; ta cần tính tổng giá trị này trên các dãy con có độ dài $k$. $n \le 50$.
>
> Sau khi sắp xếp, chỉ các hiệu giữa những giá trị được chọn liên tiếp mới có thể là nhỏ nhất, nên trạng thái cần lưu là chỉ số hiện tại, chỉ số phần tử được chọn cuối cùng, số phần tử còn phải chọn và giá trị nhỏ nhất hiện tại.
>
> Hàm $\textit{dfs}(i,j,k,\textit{mi})$ có memoization sẽ bỏ qua $i$ hoặc chọn nó rồi cập nhật giá trị nhỏ nhất bằng $\textit{nums}[i]-\textit{nums}[j]$. Việc sắp xếp bảo đảm các hiệu này không âm.

<!-- thinking:end -->

Vì bài toán liên quan đến hiệu nhỏ nhất giữa các phần tử của một dãy con, ta có thể sắp xếp mảng $\textit{nums}$, giúp việc tính hiệu nhỏ nhất giữa các phần tử trong dãy con trở nên thuận tiện hơn.

Tiếp theo, ta xây dựng hàm $dfs(i, j, k, mi)$, biểu diễn tổng power khi đang xử lý phần tử thứ $i$, phần tử được chọn cuối cùng là phần tử thứ $j$, cần chọn thêm $k$ phần tử và hiệu nhỏ nhất hiện tại là $mi$. Do đó, đáp án là $dfs(0, n, k, +\infty)$ (nếu phần tử được chọn cuối cùng là phần tử thứ $n$, điều đó có nghĩa là trước đó chưa chọn phần tử nào).

Quá trình thực thi hàm $dfs(i, j, k, mi)$ như sau:

- Nếu $i \geq n$, nghĩa là đã xử lý tất cả phần tử. Nếu $k = 0$, trả về $mi$; ngược lại, trả về $0$.
- Nếu số phần tử còn lại $n - i$ nhỏ hơn $k$, trả về $0$.
- Nếu không chọn phần tử thứ $i$, tổng power nhận được là $dfs(i + 1, j, k, mi)$.
- Ta cũng có thể chọn phần tử thứ $i$. Nếu $j = n$, nghĩa là trước đó chưa chọn phần tử nào, tổng power nhận được là $dfs(i + 1, i, k - 1, mi)$; ngược lại, tổng power nhận được là $dfs(i + 1, i, k - 1, \min(mi, \textit{nums}[i] - \textit{nums}[j]))$.
- Cộng các kết quả trên và trả về kết quả theo modulo $10^9 + 7$.

Để tránh tính toán lặp lại, ta có thể dùng memoization để lưu các kết quả đã tính.

Độ phức tạp thời gian là $O(n^4 \times k)$, và độ phức tạp không gian là $O(n^4 \times k)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfPowers(self, nums: List[int], k: int) -> int:
        @cache
        def dfs(i: int, j: int, k: int, mi: int) -> int:
            if i >= n:
                return mi if k == 0 else 0
            if n - i < k:
                return 0
            ans = dfs(i + 1, j, k, mi)
            if j == n:
                ans += dfs(i + 1, i, k - 1, mi)
            else:
                ans += dfs(i + 1, i, k - 1, min(mi, nums[i] - nums[j]))
            ans %= mod
            return ans

        mod = 10**9 + 7
        n = len(nums)
        nums.sort()
        return dfs(0, n, k, inf)
```

#### Java

```java
class Solution {
    private Map<Long, Integer> f = new HashMap<>();
    private final int mod = (int) 1e9 + 7;
    private int[] nums;

    public int sumOfPowers(int[] nums, int k) {
        Arrays.sort(nums);
        this.nums = nums;
        return dfs(0, nums.length, k, Integer.MAX_VALUE);
    }

    private int dfs(int i, int j, int k, int mi) {
        if (i >= nums.length) {
            return k == 0 ? mi : 0;
        }
        if (nums.length - i < k) {
            return 0;
        }
        long key = (1L * mi) << 18 | (i << 12) | (j << 6) | k;
        if (f.containsKey(key)) {
            return f.get(key);
        }
        int ans = dfs(i + 1, j, k, mi);
        if (j == nums.length) {
            ans += dfs(i + 1, i, k - 1, mi);
        } else {
            ans += dfs(i + 1, i, k - 1, Math.min(mi, nums[i] - nums[j]));
        }
        ans %= mod;
        f.put(key, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfPowers(vector<int>& nums, int k) {
        unordered_map<long long, int> f;
        const int mod = 1e9 + 7;
        int n = nums.size();
        sort(nums.begin(), nums.end());
        auto dfs = [&](this auto&& dfs, int i, int j, int k, int mi) -> int {
            if (i >= n) {
                return k == 0 ? mi : 0;
            }
            if (n - i < k) {
                return 0;
            }
            long long key = (1LL * mi) << 18 | (i << 12) | (j << 6) | k;
            if (f.contains(key)) {
                return f[key];
            }
            long long ans = dfs(i + 1, j, k, mi);
            if (j == n) {
                ans += dfs(i + 1, i, k - 1, mi);
            } else {
                ans += dfs(i + 1, i, k - 1, min(mi, nums[i] - nums[j]));
            }
            ans %= mod;
            f[key] = ans;
            return f[key];
        };
        return dfs(0, n, k, INT_MAX);
    }
};
```

#### Go

```go
func sumOfPowers(nums []int, k int) int {
	const mod int = 1e9 + 7
	sort.Ints(nums)
	n := len(nums)
	f := map[int]int{}
	var dfs func(i, j, k, mi int) int
	dfs = func(i, j, k, mi int) int {
		if i >= n {
			if k == 0 {
				return mi
			}
			return 0
		}
		if n-i < k {
			return 0
		}
		key := mi<<18 | (i << 12) | (j << 6) | k
		if v, ok := f[key]; ok {
			return v
		}
		ans := dfs(i+1, j, k, mi)
		if j == n {
			ans += dfs(i+1, i, k-1, mi)
		} else {
			ans += dfs(i+1, i, k-1, min(mi, nums[i]-nums[j]))
		}
		ans %= mod
		f[key] = ans
		return ans
	}
	return dfs(0, n, k, math.MaxInt)
}
```

#### TypeScript

```ts
function sumOfPowers(nums: number[], k: number): number {
    const mod = BigInt(1e9 + 7);
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const f: Map<bigint, bigint> = new Map();
    function dfs(i: number, j: number, k: number, mi: number): bigint {
        if (i >= n) {
            if (k === 0) {
                return BigInt(mi);
            }
            return BigInt(0);
        }
        if (n - i < k) {
            return BigInt(0);
        }
        const key =
            (BigInt(mi) << BigInt(18)) |
            (BigInt(i) << BigInt(12)) |
            (BigInt(j) << BigInt(6)) |
            BigInt(k);
        if (f.has(key)) {
            return f.get(key)!;
        }
        let ans = dfs(i + 1, j, k, mi);
        if (j === n) {
            ans += dfs(i + 1, i, k - 1, mi);
        } else {
            ans += dfs(i + 1, i, k - 1, Math.min(mi, nums[i] - nums[j]));
        }
        ans %= mod;
        f.set(key, ans);
        return ans;
    }

    return Number(dfs(0, n, k, Number.MAX_SAFE_INTEGER));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
