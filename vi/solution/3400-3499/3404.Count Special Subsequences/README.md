---
comments: true
difficulty: Medium
rating: 2445
source: Weekly Contest 430 Q3
tags:
    - Array
    - Hash Table
    - Math
    - Enumeration
---

<!-- problem:start -->

# [3404. Count Special Subsequences](https://leetcode.com/problems/count-special-subsequences)

[中文文档](/solution/3400-3499/3404.Count%20Special%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên dương.</p>

<p>Một <strong>dãy con đặc biệt</strong> được định nghĩa là một <span data-keyword="subsequence-array">dãy con</span> có độ dài 4, được biểu diễn bởi các chỉ số <code>(p, q, r, s)</code>, trong đó <code>p &lt; q &lt; r &lt; s</code>. Dãy con này <strong>phải</strong> thỏa mãn các điều kiện sau:</p>

<ul>
    <li><code>nums[p] * nums[r] == nums[q] * nums[s]</code></li>
    <li>Phải có <em>ít nhất</em> <strong>một</strong> phần tử giữa mỗi cặp chỉ số. Nói cách khác, <code>q - p &gt; 1</code>, <code>r - q &gt; 1</code> và <code>s - r &gt; 1</code>.</li>
</ul>

<p>Trả về <em>số lượng</em> các <strong>dãy con</strong> <strong>đặc biệt</strong> khác nhau trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,3,6,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có một dãy con đặc biệt trong <code>nums</code>.</p>

<ul>
    <li><code>(p, q, r, s) = (0, 2, 4, 6)</code>:

    <ul>
        <li>Dãy con này tương ứng với các phần tử <code>(1, 3, 3, 1)</code>.</li>
        <li><code>nums[p] * nums[r] = nums[0] * nums[4] = 1 * 3 = 3</code></li>
        <li><code>nums[q] * nums[s] = nums[2] * nums[6] = 3 * 1 = 3</code></li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,4,3,4,3,4,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có ba dãy con đặc biệt trong <code>nums</code>.</p>

<ul>
    <li><code>(p, q, r, s) = (0, 2, 4, 6)</code>:

    <ul>
        <li>Dãy con này tương ứng với các phần tử <code>(3, 3, 3, 3)</code>.</li>
        <li><code>nums[p] * nums[r] = nums[0] * nums[4] = 3 * 3 = 9</code></li>
        <li><code>nums[q] * nums[s] = nums[2] * nums[6] = 3 * 3 = 9</code></li>
    </ul>
    </li>
    <li><code>(p, q, r, s) = (1, 3, 5, 7)</code>:
    <ul>
        <li>Dãy con này tương ứng với các phần tử <code>(4, 4, 4, 4)</code>.</li>
        <li><code>nums[p] * nums[r] = nums[1] * nums[5] = 4 * 4 = 16</code></li>
        <li><code>nums[q] * nums[s] = nums[3] * nums[7] = 4 * 4 = 16</code></li>
    </ul>
    </li>
    <li><code>(p, q, r, s) = (0, 2, 5, 7)</code>:
    <ul>
        <li>Dãy con này tương ứng với các phần tử <code>(3, 3, 4, 4)</code>.</li>
        <li><code>nums[p] * nums[r] = nums[0] * nums[5] = 3 * 4 = 12</code></li>
        <li><code>nums[q] * nums[s] = nums[2] * nums[7] = 3 * 4 = 12</code></li>
    </ul>
    </li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>7 &lt;= nums.length &lt;= 1000</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con đặc biệt cần các chỉ số cách nhau và $a/b=c/d$. Liệt kê bốn chỉ số có độ phức tạp $O(n^4)$ với $n\le 1000$. Ngay cả khi ghép từng cặp hai đầu hai lần, ta vẫn cần một phép kiểm tra bằng nhau nhanh cho các tỉ số.
>
> Các tỉ số bằng nhau khi các cặp có thứ tự sau khi rút gọn trùng nhau. Nếu lập chỉ mục cho mọi cặp bên phải $(c,d)$ bằng key đã rút gọn, ta có thể truy vấn một cặp bên trái $(a,b)$ trong $O(1)$.
>
> Trước tiên, ta đếm mọi $(r,s)$ hợp lệ theo key $(\lfloor d/g\rfloor,\lfloor c/g\rfloor)$. Sau đó, ta duyệt $q$ từ trái sang phải: trả lời các truy vấn sử dụng $q$ làm phần tử thứ hai, đồng thời loại bỏ các cặp $(c,d)$ vi phạm $p<q<r<s$ sau khi $q$ tăng lên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubsequences(self, nums: List[int]) -> int:
        n = len(nums)
        cnt = defaultdict(int)
        for r in range(4, n - 2):
            c = nums[r]
            for s in range(r + 2, n):
                d = nums[s]
                g = gcd(c, d)
                cnt[(d // g, c // g)] += 1
        ans = 0
        for q in range(2, n - 4):
            b = nums[q]
            for p in range(q - 1):
                a = nums[p]
                g = gcd(a, b)
                ans += cnt[(a // g, b // g)]
            c = nums[q + 2]
            for s in range(q + 4, n):
                d = nums[s]
                g = gcd(c, d)
                cnt[(d // g, c // g)] -= 1
        return ans
```

#### Java

```java
class Solution {
    public long numberOfSubsequences(int[] nums) {
        int n = nums.length;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int r = 4; r < n - 2; ++r) {
            int c = nums[r];
            for (int s = r + 2; s < n; ++s) {
                int d = nums[s];
                int g = gcd(c, d);
                cnt.merge(((d / g) << 12) | (c / g), 1, Integer::sum);
            }
        }
        long ans = 0;
        for (int q = 2; q < n - 4; ++q) {
            int b = nums[q];
            for (int p = 0; p < q - 1; ++p) {
                int a = nums[p];
                int g = gcd(a, b);
                ans += cnt.getOrDefault(((a / g) << 12) | (b / g), 0);
            }
            int c = nums[q + 2];
            for (int s = q + 4; s < n; ++s) {
                int d = nums[s];
                int g = gcd(c, d);
                cnt.merge(((d / g) << 12) | (c / g), -1, Integer::sum);
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numberOfSubsequences(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int, int> cnt;

        for (int r = 4; r < n - 2; ++r) {
            int c = nums[r];
            for (int s = r + 2; s < n; ++s) {
                int d = nums[s];
                int g = gcd(c, d);
                cnt[((d / g) << 12) | (c / g)]++;
            }
        }

        long long ans = 0;
        for (int q = 2; q < n - 4; ++q) {
            int b = nums[q];
            for (int p = 0; p < q - 1; ++p) {
                int a = nums[p];
                int g = gcd(a, b);
                ans += cnt[((a / g) << 12) | (b / g)];
            }
            int c = nums[q + 2];
            for (int s = q + 4; s < n; ++s) {
                int d = nums[s];
                int g = gcd(c, d);
                cnt[((d / g) << 12) | (c / g)]--;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubsequences(nums []int) (ans int64) {
    n := len(nums)
    cnt := make(map[int]int)
    gcd := func(a, b int) int {
        for b != 0 {
            a, b = b, a%b
        }
        return a
    }
    for r := 4; r < n-2; r++ {
        c := nums[r]
        for s := r + 2; s < n; s++ {
            d := nums[s]
            g := gcd(c, d)
            key := ((d / g) << 12) | (c / g)
            cnt[key]++
        }
    }
    for q := 2; q < n-4; q++ {
        b := nums[q]
        for p := 0; p < q-1; p++ {
            a := nums[p]
            g := gcd(a, b)
            key := ((a / g) << 12) | (b / g)
            ans += int64(cnt[key])
        }
        c := nums[q+2]
        for s := q + 4; s < n; s++ {
            d := nums[s]
            g := gcd(c, d)
            key := ((d / g) << 12) | (c / g)
            cnt[key]--
        }
    }
    return
}
```

#### TypeScript

```ts
function numberOfSubsequences(nums: number[]): number {
    const n = nums.length;
    const cnt = new Map<number, number>();

    function gcd(a: number, b: number): number {
        while (b !== 0) {
            [a, b] = [b, a % b];
        }
        return a;
    }

    for (let r = 4; r < n - 2; r++) {
        const c = nums[r];
        for (let s = r + 2; s < n; s++) {
            const d = nums[s];
            const g = gcd(c, d);
            const key = ((d / g) << 12) | (c / g);
            cnt.set(key, (cnt.get(key) || 0) + 1);
        }
    }

    let ans = 0;
    for (let q = 2; q < n - 4; q++) {
        const b = nums[q];
        for (let p = 0; p < q - 1; p++) {
            const a = nums[p];
            const g = gcd(a, b);
            const key = ((a / g) << 12) | (b / g);
            ans += cnt.get(key) || 0;
        }
        const c = nums[q + 2];
        for (let s = q + 4; s < n; s++) {
            const d = nums[s];
            const g = gcd(c, d);
            const key = ((d / g) << 12) | (c / g);
            cnt.set(key, (cnt.get(key) || 0) - 1);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
