---
comments: true
difficulty: Hard
rating: 2672
source: Weekly Contest 506 Q4
tags:
    - Greedy
    - Binary Indexed Tree
    - Array
    - Hash Table
    - Ordered Set
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3962. Maximum Subarray Sum After at Most K Swaps](https://leetcode.com/problems/maximum-subarray-sum-after-at-most-k-swaps)

[中文文档](/solution/3900-3999/3962.Maximum%20Subarray%20Sum%20After%20at%20Most%20K%20Swaps/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Bạn được phép thực hiện <strong>nhiều nhất</strong> <code>k</code> thao tác hoán đổi trên mảng.</p>

<p>Trong một thao tác hoán đổi, bạn có thể chọn hai chỉ số bất kỳ <code>i</code> và <code>j</code>, rồi hoán đổi <code>nums[i]</code> và <code>nums[j]</code>.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng <span data-keyword="subarray-nonempty">mảng con</span> lớn nhất</strong> có thể đạt được sau khi thực hiện các thao tác hoán đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-1,0,2], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta có thể hoán đổi tại các chỉ số 1 và 3, thu được mảng <code>[1, 2, 0, -1]</code>.</li>
	<li>Mảng con <code>[1, 2]</code> có tổng bằng 3, là tổng mảng con lớn nhất có thể đạt được sau nhiều nhất <code>k = 1</code>​​​​​​​ lần hoán đổi.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,2,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng mảng con lớn nhất có thể đạt được sau nhiều nhất <code>k = 2</code> lần hoán đổi là tổng của toàn bộ mảng, bằng 13.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,-2], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Cho phép <code>k = 0</code> lần hoán đổi.</li>
	<li>Các mảng con có thể là <code>[-1]</code>, <code>[-2]</code> và <code>[-1, -2]</code>, với tổng lần lượt là -1, -2 và -3.</li>
	<li>Trong các tổng này, giá trị lớn nhất là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1500</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt mảng con + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Một phép hoán đổi chỉ giúp mảng con đã chọn khi nó đưa vào một giá trị lớn hơn từ bên ngoài. Vì $n \le 1500$, ta có thể duyệt mọi đoạn; việc sắp xếp phần bên trong và bên ngoài của mỗi đoạn sẽ thêm $O(n \log n)$ và đẩy tổng độ phức tạp lên $O(n^3 \log n)$.
>
> Khi phần bên trong được sắp xếp tăng dần và phần bên ngoài được sắp xếp giảm dần, các mức tăng là đơn điệu. Nếu cặp thứ $t$ vẫn có giá trị nhỏ hơn ở bên trong, thì $t$ cặp đầu tiên đều đáng để hoán đổi; khi một cặp không làm tăng tổng, các cặp sau cũng không làm tăng. Có thể tìm số phép hoán đổi cần thực hiện bằng tìm kiếm nhị phân.
>
> Sau khi nén tọa độ, hai cây Fenwick lưu số lượng và tổng các giá trị ở bên trong và bên ngoài đoạn, nên có thể lấy giá trị nhỏ nhất hoặc lớn nhất theo thứ hạng. Khi cố định đầu trái, mở rộng đầu phải sẽ chuyển giá trị hiện tại từ cây bên ngoài vào cây bên trong.

<!-- thinking:end -->

Khử trùng lặp và sắp xếp các giá trị trong $\textit{nums}$, rồi dùng danh sách đó làm miền giá trị đã nén. Hai cây Fenwick lưu, tương ứng cho phần bên trong và bên ngoài, số lần xuất hiện của mỗi giá trị và tổng các lần xuất hiện đó. Mỗi cây có thể trả về giá trị nhỏ thứ $k$, hoặc tổng của một số giá trị nhỏ nhất hay lớn nhất, trong $O(\log n)$.

Duyệt đầu trái $l$. Ban đầu mọi phần tử đều nằm bên ngoài đoạn. Đầu phải $r$ chạy từ $l$ đến $n - 1$: chuyển $\textit{nums}[r]$ từ cây bên ngoài sang cây bên trong và cộng nó vào tổng đoạn $s$.

Gọi $c$ là số phần tử bên trong. Có thể thực hiện nhiều nhất $t = \min(k, c, n - c)$ phép hoán đổi. Tìm kiếm nhị phân $mid$ lớn nhất trong $[1, t]$ sao cho giá trị nhỏ thứ $mid$ ở bên trong nhỏ hơn nghiêm ngặt giá trị lớn thứ $mid$ ở bên ngoài, và gọi nó là $best$. Nếu $best > 0$, một ứng viên là $s$ cộng tổng $best$ giá trị lớn nhất bên ngoài, rồi trừ tổng $best$ giá trị nhỏ nhất bên trong. Đáp án là ứng viên lớn nhất trên mọi đoạn.

Độ phức tạp thời gian là $O(n^2 \log^2 n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python

```

#### Java

```java
class Fenwick {
    int n;
    int[] count;
    long[] sum;

    Fenwick(int n) {
        this.n = n;
        count = new int[n + 1];
        sum = new long[n + 1];
    }

    void add(int idx, int cnt, long val) {
        ++idx;
        while (idx <= n) {
            count[idx] += cnt;
            sum[idx] += val;
            idx += idx & -idx;
        }
    }

    int prefixCount(int idx) {
        int res = 0;
        while (idx > 0) {
            res += count[idx];
            idx -= idx & -idx;
        }
        return res;
    }

    long prefixSum(int idx) {
        long res = 0;
        while (idx > 0) {
            res += sum[idx];
            idx -= idx & -idx;
        }
        return res;
    }

    int kth(int k) {
        int idx = 0;
        for (int bit = Integer.highestOneBit(n); bit > 0; bit >>= 1) {
            int next = idx + bit;
            if (next <= n && count[next] < k) {
                idx = next;
                k -= count[next];
            }
        }
        return idx;
    }

    long sumSmallest(int k, int[] values) {
        if (k <= 0) {
            return 0;
        }
        int pos = kth(k);
        int before = prefixCount(pos);
        return prefixSum(pos) + (long) (k - before) * values[pos];
    }

    long sumLargest(int k, int[] values) {
        int total = prefixCount(n);
        if (k <= 0) {
            return 0;
        }
        if (k >= total) {
            return prefixSum(n);
        }
        return prefixSum(n) - sumSmallest(total - k, values);
    }
}

class Solution {
    public long maxSum(int[] nums, int k) {
        int n = nums.length;
        int[] sorted = nums.clone();
        Arrays.sort(sorted);
        int m = 0;
        for (int i = 0; i < n; ++i) {
            if (i == 0 || sorted[i] != sorted[i - 1]) {
                sorted[m++] = sorted[i];
            }
        }
        int[] values = Arrays.copyOf(sorted, m);
        int[] idx = new int[n];
        for (int i = 0; i < n; ++i) {
            idx[i] = Arrays.binarySearch(values, nums[i]);
        }

        long ans = Long.MIN_VALUE;
        for (int l = 0; l < n; ++l) {
            Fenwick inside = new Fenwick(m);
            Fenwick outside = new Fenwick(m);
            for (int i = 0; i < n; ++i) {
                outside.add(idx[i], 1, nums[i]);
            }
            long window = 0;
            for (int r = l; r < n; ++r) {
                outside.add(idx[r], -1, -nums[r]);
                inside.add(idx[r], 1, nums[r]);
                window += nums[r];
                int inCnt = r - l + 1;
                int outCnt = n - inCnt;
                int limit = Math.min(k, Math.min(inCnt, outCnt));
                if (limit == 0) {
                    ans = Math.max(ans, window);
                    continue;
                }
                int lo = 1;
                int hi = limit;
                int best = 0;
                while (lo <= hi) {
                    int mid = (lo + hi) >>> 1;
                    int small = inside.kth(mid);
                    int large = outside.kth(outCnt - mid + 1);
                    if (values[small] < values[large]) {
                        best = mid;
                        lo = mid + 1;
                    } else {
                        hi = mid - 1;
                    }
                }
                if (best == 0) {
                    ans = Math.max(ans, window);
                } else {
                    long cand = window + outside.sumLargest(best, values)
                        - inside.sumSmallest(best, values);
                    ans = Math.max(ans, cand);
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp

```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
