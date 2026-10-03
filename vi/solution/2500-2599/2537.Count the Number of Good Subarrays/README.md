---
comments: true
difficulty: Medium
rating: 1891
source: Weekly Contest 328 Q3
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2537. Count the Number of Good Subarrays](https://leetcode.com/problems/count-the-number-of-good-subarrays)

[中文文档](/solution/2500-2599/2537.Count%20the%20Number%20of%20Good%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, hãy trả về <em>số lượng <strong>mảng con tốt</strong> của</em> <code>nums</code>.</p>

<p>Một mảng con <code>arr</code> là <strong>tốt</strong> nếu có <strong>ít nhất </strong><code>k</code> cặp chỉ số <code>(i, j)</code> sao cho <code>i &lt; j</code> và <code>arr[i] == arr[j]</code>.</p>

<p>Một <strong>mảng con</strong> là một dãy phần tử liên tiếp và <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1,1], k = 10
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mảng con tốt duy nhất là chính mảng nums.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,4,3,2,2,4], k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 mảng con tốt khác nhau:
- [3,1,4,3,2,2] có 2 cặp.
- [3,1,4,3,2,2,4] có 3 cặp.
- [1,4,3,2,2,4] có 2 cặp.
- [4,3,2,2,4] có 2 cặp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con là tốt khi chứa ít nhất $k$ cặp phần tử bằng nhau. Việc kiểm tra mọi $[l,r]$ có độ phức tạp bậc hai hoặc cao hơn khi $n\le 10^5$.
>
> Số cặp tăng đơn điệu theo kích thước cửa sổ. Khi thêm $x$, số cặp tăng thêm $\textit{cnt}[x]$ cặp hiện có. Di chuyển con trỏ trái khi sau khi xóa phần tử, số cặp vẫn còn ít nhất $k$. Khi đó, mọi chỉ số trong $[0,i]$ đều là vị trí bắt đầu hợp lệ, nên cộng $i+1$ vào kết quả.

<!-- thinking:end -->

Nếu một mảng con chứa $k$ cặp phần tử giống nhau, thì mảng con đó phải chứa ít nhất $k$ cặp phần tử giống nhau.

Ta sử dụng một hash table $\textit{cnt}$ để đếm số lần xuất hiện của các phần tử trong sliding window, một biến $\textit{cur}$ để đếm số cặp phần tử giống nhau trong cửa sổ, và một con trỏ $i$ để duy trì biên trái của cửa sổ.

Khi duyệt qua mảng $\textit{nums}$, ta xem phần tử hiện tại $x$ là biên phải của cửa sổ. Số cặp phần tử giống nhau trong cửa sổ tăng thêm $\textit{cnt}[x]$, đồng thời tăng số lần xuất hiện của $x$ lên 1, tức là $\textit{cnt}[x] \leftarrow \textit{cnt}[x] + 1$. Tiếp theo, ta liên tục kiểm tra xem sau khi xóa phần tử ngoài cùng bên trái, số cặp phần tử giống nhau trong cửa sổ có lớn hơn hoặc bằng $k$ hay không. Nếu có, ta giảm số lần xuất hiện của phần tử ngoài cùng bên trái, tức là $\textit{cnt}[\textit{nums}[i]] \leftarrow \textit{cnt}[\textit{nums}[i]] - 1$, giảm số cặp phần tử giống nhau trong cửa sổ đi $\textit{cnt}[\textit{nums}[i]]$, tức là $\textit{cur} \leftarrow \textit{cur} - \textit{cnt}[\textit{nums}[i]]$, rồi dịch biên trái sang phải, tức là $i \leftarrow i + 1$. Lúc này, mọi phần tử ở bên trái và bao gồm cả biên trái đều có thể làm biên trái cho biên phải hiện tại, nên ta cộng $i + 1$ vào kết quả.

Cuối cùng, ta trả về kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGood(self, nums: List[int], k: int) -> int:
        cnt = Counter()
        ans = cur = 0
        i = 0
        for x in nums:
            cur += cnt[x]
            cnt[x] += 1
            while cur - cnt[nums[i]] + 1 >= k:
                cnt[nums[i]] -= 1
                cur -= cnt[nums[i]]
                i += 1
            if cur >= k:
                ans += i + 1
        return ans
```

#### Java

```java
class Solution {
    public long countGood(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        long ans = 0, cur = 0;
        int i = 0;
        for (int x : nums) {
            cur += cnt.merge(x, 1, Integer::sum) - 1;
            while (cur - cnt.get(nums[i]) + 1 >= k) {
                cur -= cnt.merge(nums[i++], -1, Integer::sum);
            }
            if (cur >= k) {
                ans += i + 1;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countGood(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        long long ans = 0;
        long long cur = 0;
        int i = 0;
        for (int& x : nums) {
            cur += cnt[x]++;
            while (cur - cnt[nums[i]] + 1 >= k) {
                cur -= --cnt[nums[i++]];
            }
            if (cur >= k) {
                ans += i + 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countGood(nums []int, k int) int64 {
	cnt := map[int]int{}
	ans, cur := 0, 0
	i := 0
	for _, x := range nums {
		cur += cnt[x]
		cnt[x]++
		for cur-cnt[nums[i]]+1 >= k {
			cnt[nums[i]]--
			cur -= cnt[nums[i]]
			i++
		}
		if cur >= k {
			ans += i + 1
		}
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function countGood(nums: number[], k: number): number {
    const cnt: Map<number, number> = new Map();
    let [ans, cur, i] = [0, 0, 0];

    for (const x of nums) {
        const count = cnt.get(x) || 0;
        cur += count;
        cnt.set(x, count + 1);

        while (cur - (cnt.get(nums[i])! - 1) >= k) {
            const countI = cnt.get(nums[i])!;
            cnt.set(nums[i], countI - 1);
            cur -= countI - 1;
            i += 1;
        }

        if (cur >= k) {
            ans += i + 1;
        }
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn count_good(nums: Vec<i32>, k: i32) -> i64 {
        let mut cnt = HashMap::new();
        let (mut ans, mut cur, mut i) = (0i64, 0i64, 0);

        for &x in &nums {
            cur += *cnt.get(&x).unwrap_or(&0);
            *cnt.entry(x).or_insert(0) += 1;

            while cur - (cnt[&nums[i]] - 1) >= k as i64 {
                *cnt.get_mut(&nums[i]).unwrap() -= 1;
                cur -= cnt[&nums[i]];
                i += 1;
            }

            if cur >= k as i64 {
                ans += (i + 1) as i64;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
