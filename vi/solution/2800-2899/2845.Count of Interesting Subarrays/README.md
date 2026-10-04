---
comments: true
difficulty: Medium
rating: 2073
source: Weekly Contest 361 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [2845. Count of Interesting Subarrays](https://leetcode.com/problems/count-of-interesting-subarrays)

[中文文档](/solution/2800-2899/2845.Count%20of%20Interesting%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>, một số nguyên <code>modulo</code> và một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là tìm số lượng mảng con <strong>thú vị</strong>.</p>

<p>Một <strong>mảng con</strong> <code>nums[l..r]</code> là <strong>thú vị</strong> nếu thỏa mãn điều kiện sau:</p>

<ul>
	<li>Gọi <code>cnt</code> là số lượng chỉ số <code>i</code> trong khoảng <code>[l, r]</code> sao cho <code>nums[i] % modulo == k</code>. Khi đó, <code>cnt % modulo == k</code>.</li>
</ul>

<p>Trả về <em>một số nguyên biểu thị số lượng mảng con thú vị.</em></p>

<p><span><strong>Lưu ý:</strong> Mảng con là <em>một dãy phần tử liên tiếp không rỗng trong một mảng</em>.</span></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,4], modulo = 2, k = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong ví dụ này, các mảng con thú vị là:
Mảng con nums[0..0] là [3].
- Chỉ có một chỉ số, i = 0, trong khoảng [0, 0] thỏa mãn nums[i] % modulo == k.
- Do đó, cnt = 1 và cnt % modulo == k.
Mảng con nums[0..1] là [3,2].
- Chỉ có một chỉ số, i = 0, trong khoảng [0, 1] thỏa mãn nums[i] % modulo == k.
- Do đó, cnt = 1 và cnt % modulo == k.
Mảng con nums[0..2] là [3,2,4].
- Chỉ có một chỉ số, i = 0, trong khoảng [0, 2] thỏa mãn nums[i] % modulo == k.
- Do đó, cnt = 1 và cnt % modulo == k.
Có thể chứng minh rằng không có mảng con thú vị nào khác. Vậy đáp án là 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,9,6], modulo = 3, k = 0
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, các mảng con thú vị là:
Mảng con nums[0..3] là [3,1,9,6].
- Có ba chỉ số, i = 0, 2, 3, trong khoảng [0, 3] thỏa mãn nums[i] % modulo == k.
- Do đó, cnt = 3 và cnt % modulo == k.
Mảng con nums[1..1] là [1].
- Không có chỉ số nào, i, trong khoảng [1, 1] thỏa mãn nums[i] % modulo == k.
- Do đó, cnt = 0 và cnt % modulo == k.
Có thể chứng minh rằng không có mảng con thú vị nào khác. Vậy đáp án là 2.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5 </sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= modulo &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt; modulo</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Trên một mảng con, ta cần đếm số chỉ số thỏa mãn $nums[i]\bmod modulo=k$, và số lượng này phải đồng dư với $k$. Biến các chỉ số thỏa mãn thành $1$ giúp đưa điều kiện về dạng $(s_r-s_{l-1})\bmod modulo=k$. Một hash map lưu số lần xuất hiện của tổng tiền tố theo modulo $modulo$ sẽ đếm được các điểm bắt đầu phù hợp trong một lần duyệt.

<!-- thinking:end -->

Bài toán yêu cầu số lượng chỉ số $i$ trong một khoảng thỏa mãn $nums[i] \bmod modulo = k$. Ta có thể biến đổi mảng $nums$ thành một mảng $0-1$ là $arr$, trong đó $arr[i] = 1$ cho biết $nums[i] \bmod modulo = k$, ngược lại $arr[i] = 0$.

Với một khoảng $[l, r]$, ta có thể tính số lượng số $1$ trong $arr[l..r]$ bằng mảng tổng tiền tố $s$, cụ thể là $s[r] - s[l - 1]$, trong đó $s[0] = 0$.

Ta dùng một hash map $cnt$ để lưu số lần xuất hiện của tổng tiền tố $s \bmod modulo$, ban đầu $cnt[0]=1$.

Tiếp theo, ta duyệt qua mảng $arr$, tính tổng tiền tố $s$, cộng số lần xuất hiện của $(s-k) \bmod modulo$ vào đáp án, sau đó tăng số lần xuất hiện của $s \bmod modulo$ lên $1$.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countInterestingSubarrays(self, nums: List[int], modulo: int, k: int) -> int:
        arr = [int(x % modulo == k) for x in nums]
        cnt = Counter()
        cnt[0] = 1
        ans = s = 0
        for x in arr:
            s += x
            ans += cnt[(s - k) % modulo]
            cnt[s % modulo] += 1
        return ans
```

#### Java

```java
class Solution {
    public long countInterestingSubarrays(List<Integer> nums, int modulo, int k) {
        int n = nums.size();
        int[] arr = new int[n];
        for (int i = 0; i < n; ++i) {
            arr[i] = nums.get(i) % modulo == k ? 1 : 0;
        }
        Map<Integer, Integer> cnt = new HashMap<>();
        cnt.put(0, 1);
        long ans = 0;
        int s = 0;
        for (int x : arr) {
            s += x;
            ans += cnt.getOrDefault((s - k + modulo) % modulo, 0);
            cnt.merge(s % modulo, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countInterestingSubarrays(vector<int>& nums, int modulo, int k) {
        int n = nums.size();
        vector<int> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = int(nums[i] % modulo == k);
        }
        unordered_map<int, int> cnt;
        cnt[0] = 1;
        long long ans = 0;
        int s = 0;
        for (int x : arr) {
            s += x;
            ans += cnt[(s - k + modulo) % modulo];
            cnt[s % modulo]++;
        }
        return ans;
    }
};
```

#### Go

```go
func countInterestingSubarrays(nums []int, modulo int, k int) (ans int64) {
	arr := make([]int, len(nums))
	for i, x := range nums {
		if x%modulo == k {
			arr[i] = 1
		}
	}
	cnt := map[int]int{}
	cnt[0] = 1
	s := 0
	for _, x := range arr {
		s += x
		ans += int64(cnt[(s-k+modulo)%modulo])
		cnt[s%modulo]++
	}
	return
}
```

#### TypeScript

```ts
function countInterestingSubarrays(nums: number[], modulo: number, k: number): number {
    const arr: number[] = [];
    for (const x of nums) {
        arr.push(x % modulo === k ? 1 : 0);
    }
    const cnt: Map<number, number> = new Map();
    cnt.set(0, 1);
    let ans = 0;
    let s = 0;
    for (const x of arr) {
        s += x;
        ans += cnt.get((s - k + modulo) % modulo) || 0;
        cnt.set(s % modulo, (cnt.get(s % modulo) || 0) + 1);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn count_interesting_subarrays(nums: Vec<i32>, modulo: i32, k: i32) -> i64 {
        let mut arr: Vec<i32> = nums
            .iter()
            .map(|&x| if x % modulo == k { 1 } else { 0 })
            .collect();
        let mut cnt: HashMap<i32, i64> = HashMap::new();
        cnt.insert(0, 1);

        let mut ans: i64 = 0;
        let mut s: i32 = 0;

        for x in arr {
            s += x;
            let key = (s - k).rem_euclid(modulo);
            ans += *cnt.get(&key).unwrap_or(&0);
            *cnt.entry(s % modulo).or_insert(0) += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
