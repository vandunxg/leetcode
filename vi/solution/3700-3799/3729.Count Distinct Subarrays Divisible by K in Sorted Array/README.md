---
comments: true
difficulty: Hard
rating: 2248
source: Weekly Contest 473 Q4
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [3729. Count Distinct Subarrays Divisible by K in Sorted Array](https://leetcode.com/problems/count-distinct-subarrays-divisible-by-k-in-sorted-array)

[中文文档](/solution/3700-3799/3729.Count%20Distinct%20Subarrays%20Divisible%20by%20K%20in%20Sorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> đã được <strong>sắp xếp</strong> theo thứ tự <strong>không giảm</strong> và một số nguyên dương <code>k</code>.</p>

<p>Một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> của <code>nums</code> là <strong>tốt</strong> nếu tổng các phần tử của nó <strong>chia hết</strong> cho <code>k</code>.</p>

<p>Trả về một số nguyên biểu thị số lượng mảng con <strong>tốt</strong> <strong>phân biệt</strong> của <code>nums</code>.</p>

<p>Các mảng con được xem là <strong>phân biệt</strong> nếu chuỗi giá trị của chúng khác nhau. Ví dụ, có 3 mảng con <strong>phân biệt</strong> trong <code>[1, 1, 1]</code>, đó là <code>[1]</code>, <code>[1, 1]</code> và <code>[1, 1, 1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con tốt là <code>[1, 2]</code>, <code>[3]</code> và <code>[1, 2, 3]</code>. Ví dụ, <code>[1, 2, 3]</code> là mảng con tốt vì tổng các phần tử của nó là <code>1 + 2 + 3 = 6</code>, và <code>6 % k = 6 % 3 = 0</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,2,2,2,2], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con tốt là <code>[2, 2, 2]</code> và <code>[2, 2, 2, 2, 2, 2]</code>. Ví dụ, <code>[2, 2, 2]</code> là mảng con tốt vì tổng các phần tử của nó là <code>2 + 2 + 2 = 6</code>, và <code>6 % k = 6 % 6 = 0</code>.</p>

<p>Lưu ý rằng <code>[2, 2, 2]</code> chỉ được đếm một lần.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự không giảm.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng được sắp xếp không giảm và $n\le 10^5$. Prefix sum modulo $k$ đếm mọi mảng con có tổng chia hết cho $k$, nhưng sẽ đếm thừa các chuỗi giá trị giống nhau. Sau đó, ta duyệt qua các đoạn gồm những giá trị bằng nhau và trừ đi các bản sao thừa có độ dài khiến tổng của đoạn chia hết cho $k$, từ đó thu được các chuỗi phân biệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numGoodSubarrays(self, nums: List[int], k: int) -> int:
        cnt = Counter({0: 1})
        ans = s = 0
        for x in nums:
            s = (s + x) % k
            ans += cnt[s]
            cnt[s] += 1
        n = len(nums)
        i = 0
        while i < n:
            j = i + 1
            while j < n and nums[j] == nums[i]:
                j += 1
            m = j - i
            for h in range(1, m + 1):
                if (h * nums[i]) % k == 0:
                    ans -= m - h
            i = j
        return ans
```

#### Java

```java
class Solution {
    public long numGoodSubarrays(int[] nums, int k) {
        long ans = 0;
        int s = 0;
        Map<Integer, Integer> cnt = new HashMap<>();
        cnt.put(0, 1);
        for (int x : nums) {
            s = (s + x) % k;
            ans += cnt.getOrDefault(s, 0);
            cnt.merge(s, 1, Integer::sum);
        }
        int n = nums.length;
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && nums[j] == nums[i]) {
                ++j;
            }
            int m = j - i;
            for (int h = 1; h <= m; ++h) {
                if (1L * nums[i] * h % k == 0) {
                    ans -= (m - h);
                }
            }
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numGoodSubarrays(vector<int>& nums, int k) {
        long long ans = 0;
        int s = 0;
        unordered_map<int, int> cnt;
        cnt[0] = 1;
        for (int x : nums) {
            s = (s + x) % k;
            ans += cnt[s]++;
        }
        int n = nums.size();
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && nums[j] == nums[i]) {
                ++j;
            }
            int m = j - i;
            for (int h = 1; h <= m; ++h) {
                if (1LL * nums[i] * h % k == 0) {
                    ans -= (m - h);
                }
            }
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func numGoodSubarrays(nums []int, k int) (ans int64) {
    s := 0
    cnt := map[int]int{0: 1}
    for _, x := range nums {
        s = (s + x) % k
        ans += int64(cnt[s])
        cnt[s]++
    }

    n := len(nums)
    for i := 0; i < n; {
        j := i + 1
        for j < n && nums[j] == nums[i] {
            j++
        }
        m := j - i
        for h := 1; h <= m; h++ {
            if int64(nums[i])*int64(h)%int64(k) == 0 {
                ans -= int64(m - h)
            }
        }
        i = j
    }
    return
}
```

#### TypeScript

```ts
function numGoodSubarrays(nums: number[], k: number): number {
    let ans = 0;
    let s = 0;
    const cnt = new Map<number, number>();
    cnt.set(0, 1);

    for (const x of nums) {
        s = (s + x) % k;
        ans += cnt.get(s) ?? 0;
        cnt.set(s, (cnt.get(s) ?? 0) + 1);
    }

    const n = nums.length;
    for (let i = 0; i < n;) {
        let j = i + 1;
        while (j < n && nums[j] === nums[i]) ++j;
        const m = j - i;
        for (let h = 1; h <= m; ++h) {
            if ((nums[i] * h) % k === 0) {
                ans -= m - h;
            }
        }
        i = j;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
