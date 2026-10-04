---
comments: true
difficulty: Hard
rating: 2598
source: Weekly Contest 419 Q4
tags:
    - Array
    - Hash Table
    - Sliding Window
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3321. Find X-Sum of All K-Long Subarrays II](https://leetcode.com/problems/find-x-sum-of-all-k-long-subarrays-ii)

[中文文档](/solution/3300-3399/3321.Find%20X-Sum%20of%20All%20K-Long%20Subarrays%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có <code>n</code> phần tử, cùng hai số nguyên <code>k</code> và <code>x</code>.</p>

<p><strong>x-sum</strong> của một mảng được tính theo quy trình sau:</p>

<ul>
    <li>Đếm số lần xuất hiện của tất cả phần tử trong mảng.</li>
    <li>Chỉ giữ lại các lần xuất hiện của <code>x</code> phần tử có tần suất cao nhất. Nếu hai phần tử có cùng số lần xuất hiện, phần tử có giá trị <strong>lớn hơn</strong> được xem là có tần suất cao hơn.</li>
    <li>Tính tổng của mảng sau khi lọc.</li>
</ul>

<p><strong>Lưu ý</strong> rằng nếu một mảng có ít hơn <code>x</code> phần tử phân biệt, <strong>x-sum</strong> của mảng là tổng của mảng đó.</p>

<p>Trả về một mảng số nguyên <code>answer</code> có độ dài <code>n - k + 1</code>, trong đó <code>answer[i]</code> là <strong>x-sum</strong> của <span data-keyword="subarray-nonempty">mảng con</span> <code>nums[i..i + k - 1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,2,3,4,2,3], k = 6, x = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,10,12]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Với mảng con <code>[1, 1, 2, 2, 3, 4]</code>, chỉ các phần tử 1 và 2 được giữ lại trong mảng kết quả. Do đó, <code>answer[0] = 1 + 1 + 2 + 2</code>.</li>
    <li>Với mảng con <code>[1, 2, 2, 3, 4, 2]</code>, chỉ các phần tử 2 và 4 được giữ lại trong mảng kết quả. Do đó, <code>answer[1] = 2 + 2 + 2 + 4</code>. Lưu ý rằng 4 được giữ lại vì nó lớn hơn 3 và 1, là các phần tử xuất hiện cùng số lần.</li>
    <li>Với mảng con <code>[2, 2, 3, 4, 2, 3]</code>, chỉ các phần tử 2 và 3 được giữ lại. Do đó, <code>answer[2] = 2 + 2 + 2 + 3 + 3</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,8,7,8,7,5], k = 2, x = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[11,15,15,15,12]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>k == x</code>, <code>answer[i]</code> bằng tổng của mảng con <code>nums[i..i + k - 1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>nums.length == n</code></li>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= x &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Tập hợp có thứ tự

<!-- thinking:start -->

> **Tư duy**
>
> Định nghĩa giống với phần I, nhưng $n \le 10^5$, nên việc xây dựng lại từng cửa sổ sẽ quá chậm. Tập hợp top-$x$ cần được cập nhật trong $O(\log n)$ cho mỗi lần dịch cửa sổ.
>
> Hai tập hợp có thứ tự lưu các cặp top-$x$ hiện tại và phần còn lại; một map lưu tần suất. Mỗi lần cập nhật vẫn cần xóa cặp cũ trước khi chèn cặp mới.
>
> $s$ lưu tổng có trọng số của tập hợp top-$x$. Sau mỗi lần thêm và xóa, chúng ta cân bằng cho đến khi $|l|=x$, nhờ đó xử lý mọi cửa sổ chỉ trong một lượt duyệt.

<!-- thinking:end -->

Chúng ta sử dụng một bảng băm $\textit{cnt}$ để đếm số lần xuất hiện của mỗi phần tử trong cửa sổ, một tập hợp có thứ tự $\textit{l}$ để lưu $x$ phần tử có số lần xuất hiện nhiều nhất trong cửa sổ, và một tập hợp có thứ tự khác $\textit{r}$ để lưu các phần tử còn lại.

Chúng ta duy trì biến $\textit{s}$ để biểu diễn tổng các phần tử trong $\textit{l}$. Ban đầu, chúng ta thêm $k$ phần tử đầu tiên vào cửa sổ, cập nhật hai tập hợp có thứ tự $\textit{l}$ và $\textit{r}$, rồi tính giá trị của $\textit{s}$. Nếu kích thước của $\textit{l}$ nhỏ hơn $x$ và $\textit{r}$ không rỗng, chúng ta liên tục chuyển phần tử lớn nhất từ $\textit{r}$ sang $\textit{l}$ cho đến khi kích thước của $\textit{l}$ bằng $x$, đồng thời cập nhật $\textit{s}$. Nếu kích thước của $\textit{l}$ lớn hơn $x$, chúng ta liên tục chuyển phần tử nhỏ nhất từ $\textit{l}$ sang $\textit{r}$ cho đến khi kích thước của $\textit{l}$ bằng $x$, đồng thời cập nhật $\textit{s}$. Khi đó, chúng ta có thể tính $\textit{x-sum}$ của cửa sổ hiện tại và thêm nó vào mảng kết quả. Sau đó, chúng ta xóa phần tử ở biên trái của cửa sổ, cập nhật $\textit{cnt}$, các tập hợp có thứ tự $\textit{l}$ và $\textit{r}$, cũng như giá trị của $\textit{s}$. Tiếp tục duyệt mảng cho đến khi hoàn tất.

Độ phức tạp thời gian là $O(n \times \log k)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

Các bài tương tự:

- [3013. Divide an Array Into Subarrays With Minimum Cost II](/solution/3000-3099/3013.Divide%20an%20Array%20Into%20Subarrays%20With%20Minimum%20Cost%20II/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findXSum(self, nums: List[int], k: int, x: int) -> List[int]:
        def add(v: int):
            if cnt[v] == 0:
                return
            p = (cnt[v], v)
            if l and p > l[0]:
                nonlocal s
                s += p[0] * p[1]
                l.add(p)
            else:
                r.add(p)

        def remove(v: int):
            if cnt[v] == 0:
                return
            p = (cnt[v], v)
            if p in l:
                nonlocal s
                s -= p[0] * p[1]
                l.remove(p)
            else:
                r.remove(p)

        l = SortedList()
        r = SortedList()
        cnt = Counter()
        s = 0
        n = len(nums)
        ans = [0] * (n - k + 1)
        for i, v in enumerate(nums):
            remove(v)
            cnt[v] += 1
            add(v)
            j = i - k + 1
            if j < 0:
                continue
            while r and len(l) < x:
                p = r.pop()
                l.add(p)
                s += p[0] * p[1]
            while len(l) > x:
                p = l.pop(0)
                s -= p[0] * p[1]
                r.add(p)
            ans[j] = s

            remove(nums[j])
            cnt[nums[j]] -= 1
            add(nums[j])
        return ans
```

#### Java

```java
class Solution {
    private TreeSet<int[]> l = new TreeSet<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
    private TreeSet<int[]> r = new TreeSet<>(l.comparator());
    private Map<Integer, Integer> cnt = new HashMap<>();
    private long s;

    public long[] findXSum(int[] nums, int k, int x) {
        int n = nums.length;
        long[] ans = new long[n - k + 1];
        for (int i = 0; i < n; ++i) {
            int v = nums[i];
            remove(v);
            cnt.merge(v, 1, Integer::sum);
            add(v);
            int j = i - k + 1;
            if (j < 0) {
                continue;
            }
            while (!r.isEmpty() && l.size() < x) {
                var p = r.pollLast();
                s += 1L * p[0] * p[1];
                l.add(p);
            }
            while (l.size() > x) {
                var p = l.pollFirst();
                s -= 1L * p[0] * p[1];
                r.add(p);
            }
            ans[j] = s;

            remove(nums[j]);
            cnt.merge(nums[j], -1, Integer::sum);
            add(nums[j]);
        }
        return ans;
    }

    private void remove(int v) {
        if (!cnt.containsKey(v)) {
            return;
        }
        var p = new int[] {cnt.get(v), v};
        if (l.contains(p)) {
            l.remove(p);
            s -= 1L * p[0] * p[1];
        } else {
            r.remove(p);
        }
    }

    private void add(int v) {
        if (!cnt.containsKey(v)) {
            return;
        }
        var p = new int[] {cnt.get(v), v};
        if (!l.isEmpty() && l.comparator().compare(l.first(), p) < 0) {
            l.add(p);
            s += 1L * p[0] * p[1];
        } else {
            r.add(p);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> findXSum(vector<int>& nums, int k, int x) {
        using pii = pair<int, int>;
        set<pii> l, r;
        long long s = 0;
        unordered_map<int, int> cnt;
        auto add = [&](int v) {
            if (cnt[v] == 0) {
                return;
            }
            pii p = {cnt[v], v};
            if (!l.empty() && p > *l.begin()) {
                s += 1LL * p.first * p.second;
                l.insert(p);
            } else {
                r.insert(p);
            }
        };
        auto remove = [&](int v) {
            if (cnt[v] == 0) {
                return;
            }
            pii p = {cnt[v], v};
            auto it = l.find(p);
            if (it != l.end()) {
                s -= 1LL * p.first * p.second;
                l.erase(it);
            } else {
                r.erase(p);
            }
        };
        vector<long long> ans;
        for (int i = 0; i < nums.size(); ++i) {
            remove(nums[i]);
            ++cnt[nums[i]];
            add(nums[i]);

            int j = i - k + 1;
            if (j < 0) {
                continue;
            }

            while (!r.empty() && l.size() < x) {
                pii p = *r.rbegin();
                s += 1LL * p.first * p.second;
                r.erase(p);
                l.insert(p);
            }
            while (l.size() > x) {
                pii p = *l.begin();
                s -= 1LL * p.first * p.second;
                l.erase(p);
                r.insert(p);
            }
            ans.push_back(s);

            remove(nums[j]);
            --cnt[nums[j]];
            add(nums[j]);
        }
        return ans;
    }
};
```

#### Go

```go
func findXSum(nums []int, k int, x int) []int64 {
    type pair struct{ cnt, v int }
    cmpFn := func(a, b pair) int { return cmp.Or(a.cnt-b.cnt, a.v-b.v) }
    l := redblacktree.NewWith[pair, struct{}](cmpFn)
    r := redblacktree.NewWith[pair, struct{}](cmpFn)
    cnt := map[int]int{}
    var s int64

    add := func(v int) {
        if cnt[v] == 0 {
            return
        }
        p := pair{cnt[v], v}
        if !l.Empty() && cmpFn(l.Left().Key, p) < 0 {
            s += int64(p.cnt) * int64(p.v)
            l.Put(p, struct{}{})
        } else {
            r.Put(p, struct{}{})
        }
    }
    remove := func(v int) {
        if cnt[v] == 0 {
            return
        }
        p := pair{cnt[v], v}
        if _, ok := l.Get(p); ok {
            s -= int64(p.cnt) * int64(p.v)
            l.Remove(p)
        } else {
            r.Remove(p)
        }
    }

    n := len(nums)
    ans := make([]int64, 0, n-k+1)
    for i, v := range nums {
        remove(v)
        cnt[v]++
        add(v)
        j := i - k + 1
        if j < 0 {
            continue
        }
        for !r.Empty() && l.Size() < x {
            p := r.Right().Key
            s += int64(p.cnt) * int64(p.v)
            r.Remove(p)
            l.Put(p, struct{}{})
        }
        for l.Size() > x {
            p := l.Left().Key
            s -= int64(p.cnt) * int64(p.v)
            l.Remove(p)
            r.Put(p, struct{}{})
        }
        ans = append(ans, s)
        remove(nums[j])
        cnt[nums[j]]--
        add(nums[j])
    }
    return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
