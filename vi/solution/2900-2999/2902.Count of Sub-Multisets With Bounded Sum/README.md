---
comments: true
difficulty: Hard
rating: 2758
source: Biweekly Contest 115 Q4
tags:
    - Array
    - Hash Table
    - Dynamic Programming
    - Knapsack
    - Sliding Window
    - Bounded Knapsack
---

<!-- problem:start -->

# [2902. Count of Sub-Multisets With Bounded Sum](https://leetcode.com/problems/count-of-sub-multisets-with-bounded-sum)

[中文文档](/solution/2900-2999/2902.Count%20of%20Sub-Multisets%20With%20Bounded%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên không âm <code>nums</code> được đánh chỉ số từ <strong>0</strong>, cùng hai số nguyên <code>l</code> và <code>r</code>.</p>

<p>Hãy trả về <em><strong>số lượng sub-multiset</strong> trong</em> <code>nums</code> <em>sao cho tổng các phần tử trong mỗi sub-multiset nằm trong đoạn đóng</em> <code>[l, r]</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả theo modulo <code>10<sup>9 </sup>+ 7</code>.</p>

<p>Một <strong>sub-multiset</strong> là một tập hợp <strong>không có thứ tự</strong> gồm các phần tử của mảng, trong đó một giá trị <code>x</code> có thể xuất hiện <code>0, 1, ..., occ[x]</code> lần, với <code>occ[x]</code> là số lần xuất hiện của <code>x</code> trong mảng.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li>Hai <strong>sub-multiset</strong> được xem là giống nhau nếu việc sắp xếp cả hai sub-multiset cho ra các multiset giống hệt nhau.</li>
	<li>Tổng của một multiset <strong>rỗng</strong> là <code>0</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2,3], l = 6, r = 6
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Tập con duy nhất của nums có tổng bằng 6 là {1, 2, 3}.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,4,2,7], l = 1, r = 5
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các tập con của nums có tổng nằm trong đoạn [1, 5] là {1}, {2}, {4}, {2, 2}, {1, 2}, {1, 4} và {1, 2, 2}.
</pre>

<p><strong>Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,3,5,2], l = 3, r = 5
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Các tập con của nums có tổng nằm trong đoạn [3, 5] là {3}, {5}, {1, 2}, {1, 3}, {2, 2}, {2, 3}, {1, 1, 2}, {1, 1, 3} và {1, 2, 2}.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 2 * 10<sup>4</sup></code></li>
	<li>Tổng của <code>nums</code> không vượt quá <code>2 * 10<sup>4</sup></code>.</li>
	<li><code>0 &lt;= l &lt;= r &lt;= 2 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm các sub-multiset có tổng nằm trong $[l,r]$, với tổng của toàn bộ các phần tử không vượt quá $2 \times 10^4$. Nếu dùng knapsack 0-1 thông thường cho từng phần tử, chi phí sẽ vào khoảng $n \cdot r$ và khá sát giới hạn; các giá trị lặp lại nên được xử lý bằng bounded knapsack.
>
> Với mỗi $num$ khác 0 có tần suất $freq$, một prefix theo bước $stride[i]=dp[i]+dp[i-num]+\cdots$ cho phép ta loại các số hạng sử dụng nhiều hơn $freq$ bản sao. Các số 0 không làm thay đổi tổng, nên đáp án được nhân với $zeros+1$ (bao gồm cả trường hợp không chọn số nào).

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubMultisets(self, nums: List[int], l: int, r: int) -> int:
        kMod = 1_000_000_007
        # dp[i] := # of submultisets of nums with sum i
        dp = [1] + [0] * r
        count = collections.Counter(nums)
        zeros = count.pop(0, 0)

        for num, freq in count.items():
            # stride[i] := dp[i] + dp[i - num] + dp[i - 2 * num] + ...
            stride = dp.copy()
            for i in range(num, r + 1):
                stride[i] += stride[i - num]
            for i in range(r, 0, -1):
                if i >= num * (freq + 1):
                    # dp[i] + dp[i - num] + dp[i - freq * num]
                    dp[i] = stride[i] - stride[i - num * (freq + 1)]
                else:
                    dp[i] = stride[i]

        return (zeros + 1) * sum(dp[l : r + 1]) % kMod
```

#### Java

```java
class Solution {
    static final int MOD = 1_000_000_007;
    public int countSubMultisets(List<Integer> nums, int l, int r) {
        Map<Integer, Integer> count = new HashMap<>();
        int total = 0;
        for (int num : nums) {
            total += num;
            if (num <= r) {
                count.merge(num, 1, Integer::sum);
            }
        }
        if (total < l) {
            return 0;
        }
        r = Math.min(r, total);
        int[] dp = new int[r + 1];
        dp[0] = count.getOrDefault(0, 0) + 1;
        count.remove(Integer.valueOf(0));
        int sum = 0;
        for (Map.Entry<Integer, Integer> e : count.entrySet()) {
            int num = e.getKey();
            int c = e.getValue();
            sum = Math.min(sum + c * num, r);
            // prefix part
            // dp[i] = dp[i] + dp[i - num] + ... + dp[i - c*num] + dp[i-(c+1)*num] + ... + dp[i %
            // num]
            for (int i = num; i <= sum; i++) {
                dp[i] = (dp[i] + dp[i - num]) % MOD;
            }
            int temp = (c + 1) * num;
            // correction part
            // subtract dp[i - (freq + 1) * num] to the end part.
            // leves dp[i] = dp[i] + dp[i-num] +...+ dp[i - c*num];
            for (int i = sum; i >= temp; i--) {
                dp[i] = (dp[i] - dp[i - temp] + MOD) % MOD;
            }
        }
        int ans = 0;
        for (int i = l; i <= r; i++) {
            ans += dp[i];
            ans %= MOD;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSubMultisets(const vector<int> &nums, int l, int r) {
        int cnt[20001] = {};
        int memo[20001] = {};
        const int mod = 1000000007;
        for (int n : nums) {
            ++cnt[n];
        }
        fill_n(memo, cnt[1] + 1, 1);
        for (int n = 2, total = cnt[1]; n <= r; ++n) {
            if (!cnt[n]) {
                continue;
            }
            int top = (cnt[n] + 1) * n;
            total += n * cnt[n];
            for (int i = n, ii = min(total, r); i <= ii; ++i) {
                memo[i] = (memo[i] + memo[i - n]) % mod;
            }
            for (int i = min(total, r); i >= top; --i) {
                memo[i] = (mod + memo[i] - memo[i - top]) % mod;
            }
        }
        return accumulate(memo + l, memo + r + 1, 0LL) * (cnt[0] + 1) % mod;
    }
};
```

#### Go

```go
func countSubMultisets(nums []int, l int, r int) int {
	multiset := make(map[int]int)
	for _, num := range nums {
		multiset[num]++
	}
	mem := make([]int, r+1)
	mem[0] = 1
	prefix := make([]int, len(mem))
	for num, occ := range multiset {
		copy(prefix, mem)
		for sum := num; sum <= r; sum++ {
			prefix[sum] = (prefix[sum] + prefix[sum-num]) % mod
		}
		for sum := r; sum >= 0; sum-- {
			if num > 0 {
				mem[sum] = prefix[sum]
				if sum >= num*(occ+1) {
					mem[sum] = (mem[sum] - prefix[sum-num*(occ+1)] + mod) % mod
				}
			} else {
				mem[sum] = (mem[sum] * (occ + 1)) % mod
			}
		}
	}
	var result int
	for sum := l; sum <= r; sum++ {
		result = (result + mem[sum]) % mod
	}
	return result
}
var mod int = 1e9 + 7
```

#### TypeScript

```ts
function countSubMultisets(nums: number[], l: number, r: number): number {
    const cnt: number[] = Array(20001).fill(0);
    const memo: number[] = Array(20001).fill(0);
    const mod: number = 1000000007;
    for (const n of nums) {
        cnt[n]++;
    }
    memo.fill(1, 0, cnt[1] + 1);
    let total: number = cnt[1];
    for (let n = 2; n <= r; ++n) {
        if (!cnt[n]) {
            continue;
        }
        const top: number = (cnt[n] + 1) * n;
        total += n * cnt[n];
        for (let i = n, ii = Math.min(total, r); i <= ii; ++i) {
            memo[i] = (memo[i] + memo[i - n]) % mod;
        }
        for (let i = Math.min(total, r); i >= top; --i) {
            memo[i] = (mod + memo[i] - memo[i - top]) % mod;
        }
    }
    let result: number = 0;
    for (let i = l; i <= r; i++) {
        result = (result + memo[i]) % mod;
    }
    return (result * (cnt[0] + 1)) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
