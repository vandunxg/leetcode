---
comments: true
difficulty: Hard
rating: 2398
source: Weekly Contest 483 Q4
tags:
    - Bit Manipulation
    - Array
    - Two Pointers
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [3801. Minimum Cost to Merge Sorted Lists](https://leetcode.com/problems/minimum-cost-to-merge-sorted-lists)

[中文文档](/solution/3800-3899/3801.Minimum%20Cost%20to%20Merge%20Sorted%20Lists/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>lists</code>, trong đó mỗi <code>lists[i]</code> là một mảng số nguyên không rỗng <strong>được sắp xếp</strong> theo thứ tự <strong>không giảm</strong>.</p>

<p>Bạn có thể <strong>lặp lại</strong> việc chọn hai list <code>a = lists[i]</code> và <code>b = lists[j]</code>, trong đó <code>i != j</code>, rồi merge chúng. <strong>Chi phí</strong> để merge <code>a</code> và <code>b</code> là:</p>

<p><code>len(a) + len(b) + abs(median(a) - median(b))</code>, trong đó <code>len</code> và <code>median</code> lần lượt biểu thị độ dài và median của list.</p>

<p>Sau khi merge <code>a</code> và <code>b</code>, xóa cả <code>a</code> và <code>b</code> khỏi <code>lists</code>, rồi chèn list <strong>đã sắp xếp</strong> mới vào <strong>bất kỳ</strong> vị trí nào. Tiếp tục merge cho đến khi chỉ còn <strong>một</strong> list.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng chi phí nhỏ nhất</strong> cần thiết để merge tất cả các list thành một list đã sắp xếp duy nhất.</p>

<p><strong>Median</strong> của một mảng là phần tử ở giữa sau khi sắp xếp mảng theo thứ tự không giảm. Nếu mảng có số phần tử chẵn, median là phần tử ở giữa bên trái.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lists = [[1,3,5],[2,4],[6,7,8]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">18</span></p>

<p><strong>Giải thích:</strong></p>

<p>Merge <code>a = [1, 3, 5]</code> và <code>b = [2, 4]</code>:</p>

<ul>
	<li><code>len(a) = 3</code> và <code>len(b) = 2</code></li>
	<li><code>median(a) = 3</code> và <code>median(b) = 2</code></li>
	<li><code>cost = len(a) + len(b) + abs(median(a) - median(b)) = 3 + 2 + abs(3 - 2) = 6</code></li>
</ul>

<p>Vậy <code>lists</code> trở thành <code>[[1, 2, 3, 4, 5], [6, 7, 8]]</code>.</p>

<p>Merge <code>a = [1, 2, 3, 4, 5]</code> và <code>b = [6, 7, 8]</code>:</p>

<ul>
	<li><code>len(a) = 5</code> và <code>len(b) = 3</code></li>
	<li><code>median(a) = 3</code> và <code>median(b) = 7</code></li>
	<li><code>cost = len(a) + len(b) + abs(median(a) - median(b)) = 5 + 3 + abs(3 - 7) = 12</code></li>
</ul>

<p>Vậy <code>lists</code> trở thành <code>[[1, 2, 3, 4, 5, 6, 7, 8]]</code>, và tổng chi phí là <code>6 + 12 = 18</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lists = [[1,1,5],[1,4,7,8]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Merge <code>a = [1, 1, 5]</code> và <code>b = [1, 4, 7, 8]</code>:</p>

<ul>
	<li><code>len(a) = 3</code> và <code>len(b) = 4</code></li>
	<li><code>median(a) = 1</code> và <code>median(b) = 4</code></li>
	<li><code>cost = len(a) + len(b) + abs(median(a) - median(b)) = 3 + 4 + abs(1 - 4) = 10</code></li>
</ul>

<p>Vậy <code>lists</code> trở thành <code>[[1, 1, 1, 4, 5, 7, 8]]</code>, và tổng chi phí là 10.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lists = [[1],[3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Merge <code>a = [1]</code> và <code>b = [3]</code>:</p>

<ul>
	<li><code>len(a) = 1</code> và <code>len(b) = 1</code></li>
	<li><code>median(a) = 1</code> và <code>median(b) = 3</code></li>
	<li><code>cost = len(a) + len(b) + abs(median(a) - median(b)) = 1 + 1 + abs(1 - 3) = 4</code></li>
</ul>

<p>Vậy <code>lists</code> trở thành <code>[[1, 3]]</code>, và tổng chi phí là 4.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lists = [[1],[1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng chi phí là <code>len(a) + len(b) + abs(median(a) - median(b)) = 1 + 1 + abs(1 - 1) = 2</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= lists.length &lt;= 12</code></li>
	<li><code>1 &lt;= lists[i].length &lt;= 500</code></li>
	<li><code>-10<sup>9</sup> &lt;= lists[i][j] &lt;= 10<sup>9</sup></code></li>
	<li><code>lists[i]</code> được sắp xếp theo thứ tự không giảm.</li>
	<li><strong>Tổng</strong> <code>lists[i].length</code> không vượt quá 2000.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất $n \le 12$ list, nên việc liệt kê thứ tự merge dưới dạng các cây Catalan sẽ lặp lại cùng một tập con nhiều lần. Tổng độ dài không lớn, nhưng không thể tìm kiếm trực tiếp theo thứ tự merge.
>
> Độ dài và median của một phép merge chỉ phụ thuộc vào multiset các giá trị, không phụ thuộc vào chuỗi merge trung gian. Vì vậy, mỗi tập con có một độ dài và median duy nhất.
>
> Ta biểu diễn các list chưa dùng bằng bit mask, tiền xử lý số lượng phần tử và median trái của mỗi tập con khác rỗng, sau đó thực hiện DP bằng cách tách một tập hợp thành hai tập con khác rỗng, cộng khoảng cách giữa hai median và tổng độ dài.
>
> DP trên $2^n$ tập con bao phủ mọi collection; đáp án là chi phí của full mask.

<!-- thinking:end -->

Số lượng list thỏa mãn $n \le 12$, vì vậy ta có thể dùng bitmask để biểu diễn mọi tập con của các list.

Merge hai list đã sắp xếp tạo ra union đã sắp xếp của các phần tử của chúng, nên độ dài và median của một tập hợp các list chỉ phụ thuộc vào chính tập hợp đó, không phụ thuộc vào thứ tự merge. Median là phần tử nhỏ thứ $\lfloor (len + 1)/2 \rfloor$ sau khi sắp xếp, tức phần tử ở giữa bên trái.

Với mỗi tập con khác rỗng $i$, ta tiền xử lý:

- $\textit{cnt}[i]$: số phần tử trong tập con;
- $\textit{med}[i]$: median của tập con. Dùng binary search trên các giá trị phân biệt và đếm số phần tử trong tập con không lớn hơn $\textit{mid}$.

Gọi $f[i]$ là chi phí nhỏ nhất để merge tất cả các list trong tập con $i$ thành một list. Nếu $i$ chỉ chứa một list, $f[i] = 0$. Ngược lại, ta liệt kê một tập con thực sự khác rỗng $j$ của $i$ và đặt $k = i \oplus j$:

$$
f[i] = \min_{j \subset i} \big(f[j] + f[k] + |\textit{med}[j] - \textit{med}[k]|\big) + \textit{cnt}[i]
$$

Phần độ dài của phép merge cuối cùng luôn là $\textit{cnt}[i]$. Đáp án là $f[2^n - 1]$.

Độ phức tạp thời gian là $O(3^n + 2^n \times n \times \log V \times \log L)$, còn độ phức tạp không gian là $O(2^n)$, trong đó $n$ là số lượng list, $V$ là số giá trị phân biệt và $L$ là độ dài lớn nhất của một list.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMergeCost(self, lists: List[List[int]]) -> int:
        n = len(lists)
        vals = sorted({x for v in lists for x in v})
        cnt = [0] * (1 << n)
        med = [0] * (1 << n)
        for i in range(1, 1 << n):
            for j, v in enumerate(lists):
                if i >> j & 1:
                    cnt[i] += len(v)
            need = (cnt[i] + 1) // 2
            l, r = 0, len(vals) - 1
            while l < r:
                mid = (l + r) >> 1
                le = 0
                b = i
                while b:
                    t = (b & -b).bit_length() - 1
                    le += bisect_right(lists[t], vals[mid])
                    if le >= need:
                        break
                    b &= b - 1
                if le >= need:
                    r = mid
                else:
                    l = mid + 1
            med[i] = vals[l]

        f = [inf] * (1 << n)
        for i in range(1, 1 << n):
            if i.bit_count() == 1:
                f[i] = 0
                continue
            j = (i - 1) & i
            while j:
                k = i ^ j
                if j <= k:
                    f[i] = min(f[i], f[j] + f[k] + abs(med[j] - med[k]))
                j = (j - 1) & i
            f[i] += cnt[i]
        return f[-1]
```

#### Java

```java
class Solution {
    public long minMergeCost(int[][] lists) {
        int n = lists.length;
        int tot = 0;
        for (int[] v : lists) {
            tot += v.length;
        }
        int[] vals = new int[tot];
        int p = 0;
        for (int[] v : lists) {
            for (int x : v) {
                vals[p++] = x;
            }
        }
        Arrays.sort(vals);
        int m = 0;
        for (int i = 0; i < tot; ++i) {
            if (m == 0 || vals[i] != vals[m - 1]) {
                vals[m++] = vals[i];
            }
        }
        int[] cnt = new int[1 << n];
        int[] med = new int[1 << n];
        for (int i = 1; i < 1 << n; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    cnt[i] += lists[j].length;
                }
            }
            int need = (cnt[i] + 1) / 2;
            int l = 0, r = m - 1;
            while (l < r) {
                int mid = (l + r) >> 1;
                int le = 0;
                for (int b = i; b > 0; b &= b - 1) {
                    int id = Integer.numberOfTrailingZeros(b);
                    le += upperBound(lists[id], vals[mid]);
                    if (le >= need) {
                        break;
                    }
                }
                if (le >= need) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            med[i] = vals[l];
        }

        long[] f = new long[1 << n];
        Arrays.fill(f, Long.MAX_VALUE / 4);
        for (int i = 1; i < 1 << n; ++i) {
            if (Integer.bitCount(i) == 1) {
                f[i] = 0;
                continue;
            }
            for (int j = (i - 1) & i; j > 0; j = (j - 1) & i) {
                int k = i ^ j;
                if (j <= k) {
                    f[i] = Math.min(f[i], f[j] + f[k] + Math.abs(med[j] - med[k]));
                }
            }
            f[i] += cnt[i];
        }
        return f[(1 << n) - 1];
    }

    private int upperBound(int[] a, int x) {
        int l = 0, r = a.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (a[mid] <= x) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minMergeCost(vector<vector<int>>& lists) {
        int n = lists.size();
        vector<int> vals;
        for (auto& v : lists) {
            vals.insert(vals.end(), v.begin(), v.end());
        }
        sort(vals.begin(), vals.end());
        vals.erase(unique(vals.begin(), vals.end()), vals.end());

        vector<int> cnt(1 << n);
        vector<int> med(1 << n);
        for (int i = 1; i < 1 << n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    cnt[i] += lists[j].size();
                }
            }
            int need = (cnt[i] + 1) / 2;
            int l = 0, r = vals.size() - 1;
            while (l < r) {
                int mid = (l + r) >> 1;
                int le = 0;
                for (int b = i; b; b &= b - 1) {
                    int id = __builtin_ctz(b);
                    le += upper_bound(lists[id].begin(), lists[id].end(), vals[mid]) - lists[id].begin();
                    if (le >= need) {
                        break;
                    }
                }
                if (le >= need) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            med[i] = vals[l];
        }

        vector<long long> f(1 << n, 1e18);
        for (int i = 1; i < 1 << n; ++i) {
            if (__builtin_popcount(i) == 1) {
                f[i] = 0;
                continue;
            }
            for (int j = (i - 1) & i; j; j = (j - 1) & i) {
                int k = i ^ j;
                if (j <= k) {
                    f[i] = min(f[i], f[j] + f[k] + abs(med[j] - med[k]));
                }
            }
            f[i] += cnt[i];
        }
        return f[(1 << n) - 1];
    }
};
```

#### Go

```go
func minMergeCost(lists [][]int) int64 {
	n := len(lists)
	set := map[int]struct{}{}
	for _, v := range lists {
		for _, x := range v {
			set[x] = struct{}{}
		}
	}
	vals := make([]int, 0, len(set))
	for x := range set {
		vals = append(vals, x)
	}
	sort.Ints(vals)

	cnt := make([]int, 1<<n)
	med := make([]int, 1<<n)
	for i := 1; i < 1<<n; i++ {
		for j, v := range lists {
			if i>>j&1 == 1 {
				cnt[i] += len(v)
			}
		}
		need := (cnt[i] + 1) / 2
		l, r := 0, len(vals)-1
		for l < r {
			mid := (l + r) >> 1
			le := 0
			for b := i; b > 0; b &= b - 1 {
				id := bits.TrailingZeros(uint(b))
				le += sort.Search(len(lists[id]), func(p int) bool { return lists[id][p] > vals[mid] })
				if le >= need {
					break
				}
			}
			if le >= need {
				r = mid
			} else {
				l = mid + 1
			}
		}
		med[i] = vals[l]
	}

	f := make([]int64, 1<<n)
	for i := range f {
		f[i] = 1e18
	}
	for i := 1; i < 1<<n; i++ {
		if bits.OnesCount(uint(i)) == 1 {
			f[i] = 0
			continue
		}
		for j := (i - 1) & i; j > 0; j = (j - 1) & i {
			k := i ^ j
			if j <= k {
				d := med[j] - med[k]
				if d < 0 {
					d = -d
				}
				f[i] = min(f[i], f[j]+f[k]+int64(d))
			}
		}
		f[i] += int64(cnt[i])
	}
	return f[1<<n-1]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
