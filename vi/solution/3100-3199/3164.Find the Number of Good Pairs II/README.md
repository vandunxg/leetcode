---
comments: true
difficulty: Medium
rating: 1777
source: Weekly Contest 399 Q3
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3164. Find the Number of Good Pairs II](https://leetcode.com/problems/find-the-number-of-good-pairs-ii)

[中文文档](/solution/3100-3199/3164.Find%20the%20Number%20of%20Good%20Pairs%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có độ dài lần lượt là <code>n</code> và <code>m</code>. Đồng thời, bạn được cho một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Một cặp <code>(i, j)</code> được gọi là <strong>tốt</strong> nếu <code>nums1[i]</code> chia hết cho <code>nums2[j] * k</code> (<code>0 &lt;= i &lt;= n - 1</code>, <code>0 &lt;= j &lt;= m - 1</code>).</p>

<p>Trả về tổng số cặp <strong>tốt</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,3,4], nums2 = [1,3,4], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>
5 cặp tốt là <code>(0, 0)</code>, <code>(1, 0)</code>, <code>(1, 1)</code>, <code>(2, 0)</code> và <code>(2, 2)</code>.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,2,4,12], nums2 = [2,4], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>2 cặp tốt là <code>(3, 0)</code> và <code>(3, 1)</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums1[i], nums2[j] &lt;= 10<sup>6</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê bội số

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện giống phần I, nhưng độ dài có thể lên tới $10^5$, nên duyệt hai vòng lặp sẽ không đáp ứng được.
>
> $x$ ghép cặp với $y$ khi và chỉ khi $x$ chia hết cho $k$ và $x/k$ là bội của $y$. Sau khi đếm các thương, ta duyệt các bội của từng $y$.
>
> Xây dựng $cnt1$ từ các giá trị trong $nums1$ chia hết cho $k$. Với mỗi $x$ trong $cnt2$, duyệt từ $x$ đến $mx$ và cộng $cnt1[y]\cdot v$. Nếu $cnt1$ rỗng thì kết quả là $0$.

<!-- thinking:end -->

Ta sử dụng một hash table `cnt1` để ghi lại số lần xuất hiện của mỗi số sau khi chia cho $k$ trong mảng `nums1`, và một hash table `cnt2` để ghi lại số lần xuất hiện của mỗi số trong mảng `nums2`.

Tiếp theo, ta duyệt từng số $x$ trong mảng `nums2`. Với mỗi số $x$, ta duyệt các bội của nó là $y$, trong đó phạm vi của $y$ là $[x, \textit{mx}]$, với `mx` là giá trị khóa lớn nhất trong `cnt1`. Sau đó, ta tính tổng `cnt1[y]`, ký hiệu là $s$. Cuối cùng, ta cộng $s \times v$ vào đáp án, trong đó $v$ là `cnt2[x]`.

Độ phức tạp thời gian là $O(n + m + (M / k) \times \log m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó $n$ và $m$ lần lượt là độ dài của các mảng `nums1` và `nums2`, còn $M$ là giá trị lớn nhất trong mảng `nums1`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPairs(self, nums1: List[int], nums2: List[int], k: int) -> int:
        cnt1 = Counter(x // k for x in nums1 if x % k == 0)
        if not cnt1:
            return 0
        cnt2 = Counter(nums2)
        ans = 0
        mx = max(cnt1)
        for x, v in cnt2.items():
            s = sum(cnt1[y] for y in range(x, mx + 1, x))
            ans += s * v
        return ans
```

#### Java

```java
class Solution {
    public long numberOfPairs(int[] nums1, int[] nums2, int k) {
        Map<Integer, Integer> cnt1 = new HashMap<>();
        for (int x : nums1) {
            if (x % k == 0) {
                cnt1.merge(x / k, 1, Integer::sum);
            }
        }
        if (cnt1.isEmpty()) {
            return 0;
        }
        Map<Integer, Integer> cnt2 = new HashMap<>();
        for (int x : nums2) {
            cnt2.merge(x, 1, Integer::sum);
        }
        long ans = 0;
        int mx = Collections.max(cnt1.keySet());
        for (var e : cnt2.entrySet()) {
            int x = e.getKey(), v = e.getValue();
            int s = 0;
            for (int y = x; y <= mx; y += x) {
                s += cnt1.getOrDefault(y, 0);
            }
            ans += 1L * s * v;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numberOfPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        unordered_map<int, int> cnt1;
        for (int x : nums1) {
            if (x % k == 0) {
                cnt1[x / k]++;
            }
        }
        if (cnt1.empty()) {
            return 0;
        }
        unordered_map<int, int> cnt2;
        for (int x : nums2) {
            ++cnt2[x];
        }
        int mx = 0;
        for (auto& [x, _] : cnt1) {
            mx = max(mx, x);
        }
        long long ans = 0;
        for (auto& [x, v] : cnt2) {
            long long s = 0;
            for (int y = x; y <= mx; y += x) {
                s += cnt1[y];
            }
            ans += s * v;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfPairs(nums1 []int, nums2 []int, k int) (ans int64) {
    cnt1 := map[int]int{}
    for _, x := range nums1 {
        if x%k == 0 {
            cnt1[x/k]++
        }
    }
    if len(cnt1) == 0 {
        return 0
    }
    cnt2 := map[int]int{}
    for _, x := range nums2 {
        cnt2[x]++
    }
    mx := 0
    for x := range cnt1 {
        mx = max(mx, x)
    }
    for x, v := range cnt2 {
        s := 0
        for y := x; y <= mx; y += x {
            s += cnt1[y]
        }
        ans += int64(s) * int64(v)
    }
    return
}
```

#### TypeScript

```ts
function numberOfPairs(nums1: number[], nums2: number[], k: number): number {
    const cnt1: Map<number, number> = new Map();
    for (const x of nums1) {
        if (x % k === 0) {
            cnt1.set((x / k) | 0, (cnt1.get((x / k) | 0) || 0) + 1);
        }
    }
    if (cnt1.size === 0) {
        return 0;
    }
    const cnt2: Map<number, number> = new Map();
    for (const x of nums2) {
        cnt2.set(x, (cnt2.get(x) || 0) + 1);
    }
    const mx = Math.max(...cnt1.keys());
    let ans = 0;
    for (const [x, v] of cnt2) {
        let s = 0;
        for (let y = x; y <= mx; y += x) {
            s += cnt1.get(y) || 0;
        }
        ans += s * v;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
