---
comments: true
difficulty: Easy
rating: 1168
source: Weekly Contest 399 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3162. Find the Number of Good Pairs I](https://leetcode.com/problems/find-the-number-of-good-pairs-i)

[中文文档](/solution/3100-3199/3162.Find%20the%20Number%20of%20Good%20Pairs%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có độ dài lần lượt là <code>n</code> và <code>m</code>. Ngoài ra, bạn được cho một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Một cặp <code>(i, j)</code> được gọi là <strong>tốt</strong> nếu <code>nums1[i]</code> chia hết cho <code>nums2[j] * k</code> (<code>0 &lt;= i &lt;= n - 1</code>, <code>0 &lt;= j &lt;= m - 1</code>).</p>

<p>Trả về tổng số cặp <strong>tốt</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,3,4], nums2 = [1,3,4], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>
Có 5 cặp tốt là <code>(0, 0)</code>, <code>(1, 0)</code>, <code>(1, 1)</code>, <code>(2, 0)</code> và <code>(2, 2)</code>.</div>

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
    <li><code>1 &lt;= n, m &lt;= 50</code></li>
    <li><code>1 &lt;= nums1[i], nums2[j] &lt;= 50</code></li>
    <li><code>1 &lt;= k &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp là cặp tốt khi $nums1[i]$ chia hết cho $nums2[j]\cdot k$. Độ dài của cả hai mảng không quá $50$, nên có thể dùng hai vòng lặp lồng nhau.
>
> Không cần sàng. Với mỗi cặp, kiểm tra $x\bmod(y\cdot k)=0$.
>
> Tính tổng vị từ trên tích Descartes của hai mảng trong $O(mn)$.

<!-- thinking:end -->

Ta duyệt trực tiếp tất cả các cặp phần tử $(x, y)$ và kiểm tra xem $x \bmod (y \times k) = 0$ hay không. Nếu thỏa mãn điều kiện, tăng đáp án lên một.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là độ dài của các mảng $\textit{nums1}$ và $\textit{nums2}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPairs(self, nums1: List[int], nums2: List[int], k: int) -> int:
        return sum(x % (y * k) == 0 for x in nums1 for y in nums2)
```

#### Java

```java
class Solution {
    public int numberOfPairs(int[] nums1, int[] nums2, int k) {
        int ans = 0;
        for (int x : nums1) {
            for (int y : nums2) {
                if (x % (y * k) == 0) {
                    ++ans;
                }
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
    int numberOfPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        int ans = 0;
        for (int x : nums1) {
            for (int y : nums2) {
                if (x % (y * k) == 0) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfPairs(nums1 []int, nums2 []int, k int) (ans int) {
    for _, x := range nums1 {
        for _, y := range nums2 {
            if x%(y*k) == 0 {
                ans++
            }
        }
    }
    return
}
```

#### TypeScript

```ts
function numberOfPairs(nums1: number[], nums2: number[], k: number): number {
    let ans = 0;
    for (const x of nums1) {
        for (const y of nums2) {
            if (x % (y * k) === 0) {
                ++ans;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Bảng băm + Duyệt các bội số

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 dựa vào việc $mn$ rất nhỏ. Phần II cần một cách gom nhóm nhanh hơn.
>
> Đếm các thương $x/k$ của những giá trị trong $nums1$ chia hết cho $k$, sau đó với mỗi $x$ phân biệt trong $nums2$, duyệt qua các bội số $y$ của nó và cộng $cnt1[y]$.
>
> Sau khi băm, vòng lặp bên trong tăng từng bước $x$ cho đến $mx$ và nhân với tần suất của $x$.

<!-- thinking:end -->

Ta sử dụng một bảng băm `cnt1` để ghi nhận số lần xuất hiện của mỗi số sau khi chia cho $k$ trong mảng `nums1`, và một bảng băm `cnt2` để ghi nhận số lần xuất hiện của mỗi số trong mảng `nums2`.

Tiếp theo, ta duyệt từng số $x$ trong mảng `nums2`. Với mỗi số $x$, ta duyệt các bội số $y$ của nó, trong đó phạm vi của $y$ là $[x, \textit{mx}]$, với `mx` là giá trị khóa lớn nhất trong `cnt1`. Sau đó, ta tính tổng `cnt1[y]`, ký hiệu là $s$. Cuối cùng, ta cộng $s \times v$ vào đáp án, trong đó $v$ là `cnt2[x]`.

Độ phức tạp thời gian là $O(n + m + (M / k) \times \log m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng `nums1` và `nums2`, còn $M$ là giá trị lớn nhất trong mảng `nums1`.

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
    public int numberOfPairs(int[] nums1, int[] nums2, int k) {
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
        int ans = 0;
        int mx = Collections.max(cnt1.keySet());
        for (var e : cnt2.entrySet()) {
            int x = e.getKey(), v = e.getValue();
            int s = 0;
            for (int y = x; y <= mx; y += x) {
                s += cnt1.getOrDefault(y, 0);
            }
            ans += s * v;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfPairs(vector<int>& nums1, vector<int>& nums2, int k) {
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
        int ans = 0;
        for (auto& [x, v] : cnt2) {
            int s = 0;
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
func numberOfPairs(nums1 []int, nums2 []int, k int) (ans int) {
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
        ans += s * v
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
