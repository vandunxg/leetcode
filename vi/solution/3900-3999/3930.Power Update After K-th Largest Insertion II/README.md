---
comments: true
difficulty: Hard
tags:
    - Segment Tree
    - Array
    - Hash Table
    - Math
    - Sorting
---

<!-- problem:start -->

# [3930. Power Update After K-th Largest Insertion II 🔒](https://leetcode.com/problems/power-update-after-k-th-largest-insertion-ii)

[中文文档](/solution/3900-3999/3930.Power%20Update%20After%20K-th%20Largest%20Insertion%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>p</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code>, trong đó mỗi <code>queries[i] = [val<sub>i</sub>, k<sub>i</sub>]</code>.</p>

<p>Với mỗi truy vấn:</p>

<ul>
	<li>Chèn <code>val<sub>i</sub></code> vào <code>nums</code>.</li>
	<li>Gọi <code>x</code> là phần tử <code>k<sub>i</sub><sup>th</sup></code> <strong>lớn nhất</strong> trong <code>nums</code> hiện tại.</li>
	<li><strong>Cập nhật</strong> <code>p</code> thành <code>p<sup>x</sup> % (10<sup>9</sup> + 7)</code>.</li>
</ul>

<p>Trả về một mảng <code>ans</code>, trong đó <code>ans[i]</code> biểu diễn giá trị của <code>p</code> sau khi xử lý truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2], p = 4, queries = [[3,1],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[64,4096]</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0">
	<thead>
		<tr>
			<th><code>i</code></th>
			<th><code>val<sub>i</sub></code></th>
			<th><code>nums</code><br />hiện tại</th>
			<th><code>k<sub>i</sub></code></th>
			<th><code>k<sub>i</sub><sup>th</sup></code><br />lớn nhất</th>
			<th>p</th>
			<th><code>p = p<sup>k</sup> % (10<sup>9</sup> + 7)</code> mới</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>0</td>
			<td>3</td>
			<td>[2, 3]</td>
			<td>1</td>
			<td>3</td>
			<td>4</td>
			<td>4<sup>3</sup> % (10<sup>9</sup> + 7) = 64</td>
		</tr>
		<tr>
			<td>1</td>
			<td>1</td>
			<td>[2, 3, 1]</td>
			<td>2</td>
			<td>2</td>
			<td>64</td>
			<td>64<sup>2</sup> % (10<sup>9</sup> + 7) = 4096</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, <code>ans = [64, 4096]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,5], p = 6, queries = [[4,3],[7,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1296,220296870]</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0">
	<thead>
		<tr>
			<th><code>i</code></th>
			<th><code>val<sub>i</sub></code></th>
			<th><code>nums</code><br />hiện tại</th>
			<th><code>k<sub>i</sub></code></th>
			<th><code>k<sub>i</sub><sup>th</sup></code><br />lớn nhất</th>
			<th><code>p</code></th>
			<th><code>p = p<sup>k</sup> % (10<sup>9</sup> + 7)</code> mới</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>0</td>
			<td>4</td>
			<td>[7, 5, 4]</td>
			<td>3</td>
			<td>4</td>
			<td>6</td>
			<td>6<sup>4</sup> % (10<sup>9</sup> + 7) = 1296</td>
		</tr>
		<tr>
			<td>1</td>
			<td>7</td>
			<td>[7, 5, 4, 7]</td>
			<td>2</td>
			<td>7</td>
			<td>1296</td>
			<td>1296<sup>7</sup> % (10<sup>9</sup> + 7) = 220296870</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, <code>ans = [1296, 220296870]</code></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>​​​​​​​1 &lt;= p &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code><sup>​​​​​​​</sup>1 &lt;= val<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k<sub>i</sub> &lt;= n + i + 1</code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Danh sách đã sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp lại sau mỗi lần chèn để lấy phần tử lớn thứ $k$ sẽ tốn $O(n\log n)$ cho mỗi truy vấn và không phù hợp khi tổng độ dài lên đến $2\times 10^4$. Ta cần một cấu trúc hỗ trợ chèn rồi chọn phần tử theo thứ hạng.
>
> Một danh sách đã sắp xếp lưu mọi giá trị hiện tại; sau khi chèn $val$, phần tử $sl[-k]$ là phần tử lớn thứ $k$, rồi $p$ được cập nhật thành $p^x\bmod(10^9+7)$.
>
> Phép chèn và phép lũy thừa modulo đều có độ phức tạp logarit, nên tổng độ phức tạp là $O((n+m)\log(n+m))$.

<!-- thinking:end -->

Ta dùng một danh sách đã sắp xếp $\textit{sl}$ để duy trì mảng hiện tại $nums$. Với mỗi truy vấn, ta chèn $val_i$ vào $\textit{sl}$, sau đó tìm phần tử lớn thứ $k_i$, ký hiệu là $x$, trong $\textit{sl}$. Dùng phép lũy thừa nhanh, ta cập nhật $p$ thành $p^x \bmod (10^9 + 7)$ rồi thêm $p$ mới vào mảng kết quả.

Độ phức tạp thời gian là $O((n + m) \log (n + m))$, độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của $\textit{nums}$ và $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def powerUpdate(
        self, nums: list[int], p: int, queries: list[list[int]]
    ) -> list[int]:
        ans = []
        sl = SortedList(nums)
        mod = 10**9 + 7
        for val, k in queries:
            sl.add(val)
            p = pow(p, sl[-k], mod)
            ans.append(p)
        return ans
```

#### C++

```cpp
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
#include <vector>

using namespace std;
using namespace __gnu_pbds;

template <typename T>
using ordered_multiset = tree<pair<T, int>, null_type, less<pair<T, int>>,
    rb_tree_tag, tree_order_statistics_node_update>;

class Solution {
public:
    vector<int> powerUpdate(vector<int>& nums, int p, vector<vector<int>>& queries) {
        vector<int> ans;
        ordered_multiset<int> sl;
        const int mod = 1e9 + 7;

        for (int i = 0; i < nums.size(); i++) {
            sl.insert({nums[i], i});
        }

        int next_id = nums.size();

        auto mod_pow = [&](long long base, long long exp) -> long long {
            long long result = 1;
            base %= mod;
            while (exp > 0) {
                if (exp & 1) result = (result * base) % mod;
                base = (base * base) % mod;
                exp >>= 1;
            }
            return result;
        };

        for (const auto& query : queries) {
            int val = query[0];
            int k = query[1];

            sl.insert({val, next_id++});

            auto it = sl.find_by_order(sl.size() - k);
            int kth_largest = it->first;

            p = mod_pow(p, kth_largest);
            ans.push_back(p);
        }

        return ans;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
