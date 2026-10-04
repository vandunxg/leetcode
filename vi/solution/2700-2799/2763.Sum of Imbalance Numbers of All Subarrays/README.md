---
comments: true
difficulty: Hard
rating: 2277
source: Weekly Contest 352 Q4
tags:
    - Array
    - Hash Table
    - Enumeration
---

<!-- problem:start -->

# [2763. Sum of Imbalance Numbers of All Subarrays](https://leetcode.com/problems/sum-of-imbalance-numbers-of-all-subarrays)

[中文文档](/solution/2700-2799/2763.Sum%20of%20Imbalance%20Numbers%20of%20All%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Độ mất cân bằng</strong> của một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>arr</code> có độ dài <code>n</code> được định nghĩa là số chỉ số trong <code>sarr = sorted(arr)</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt; n - 1</code>, và</li>
	<li><code>sarr[i+1] - sarr[i] &gt; 1</code></li>
</ul>

<p>Ở đây, <code>sorted(arr)</code> là hàm trả về phiên bản đã được sắp xếp của <code>arr</code>.</p>

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>, hãy trả về <em><strong>tổng độ mất cân bằng</strong> của tất cả các <strong>mảng con</strong> của nó</em>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử <strong>liên tiếp, không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,1,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 mảng con có độ mất cân bằng khác<strong> </strong>0:
- Mảng con [3, 1] có độ mất cân bằng bằng 1.
- Mảng con [3, 1, 4] có độ mất cân bằng bằng 1.
- Mảng con [1, 4] có độ mất cân bằng bằng 1.
Độ mất cân bằng của tất cả các mảng con khác đều bằng 0. Vì vậy, tổng độ mất cân bằng của tất cả các mảng con của nums là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,3,3,5]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Có 7 mảng con có độ mất cân bằng khác 0:
- Mảng con [1, 3] có độ mất cân bằng bằng 1.
- Mảng con [1, 3, 3] có độ mất cân bằng bằng 1.
- Mảng con [1, 3, 3, 3] có độ mất cân bằng bằng 1.
- Mảng con [1, 3, 3, 3, 5] có độ mất cân bằng bằng 2.
- Mảng con [3, 3, 3, 5] có độ mất cân bằng bằng 1.
- Mảng con [3, 3, 5] có độ mất cân bằng bằng 1.
- Mảng con [3, 5] có độ mất cân bằng bằng 1.
Độ mất cân bằng của tất cả các mảng con khác đều bằng 0. Vì vậy, tổng độ mất cân bằng của tất cả các mảng con của nums là 8.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Độ mất cân bằng của một mảng con là số khoảng cách kề nhau lớn hơn $1$ sau khi sắp xếp các giá trị phân biệt; ta cần tính tổng này trên tất cả các mảng con. Việc sắp xếp từng mảng con sẽ khó đáp ứng với $n\le 1000$ nếu không duy trì các khoảng cách một cách tăng dần.
>
> Cố định đầu trái và chèn phần tử ở đầu phải vào một danh sách đã sắp xếp. So sánh phần tử đó với phần tử liền trước và liền sau để xác định các khoảng cách mới lớn hơn $1$ xuất hiện hay một khoảng cách cũ bị chia tách. Biến $cnt$ theo dõi độ mất cân bằng và được cộng vào đáp án.

<!-- thinking:end -->

Trước tiên, ta có thể liệt kê đầu trái $i$ của mảng con. Với mỗi $i$, ta liệt kê đầu phải $j$ của mảng con theo thứ tự tăng dần, đồng thời duy trì tất cả phần tử trong mảng con hiện tại bằng một danh sách có thứ tự. Ta cũng sử dụng biến $cnt$ để duy trì độ mất cân bằng của mảng con hiện tại.

Với mỗi số $nums[j]$, ta tìm phần tử đầu tiên $nums[k]$ trong danh sách có thứ tự lớn hơn hoặc bằng $nums[j]$, và phần tử cuối cùng $nums[h]$ nhỏ hơn $nums[j]$:

- Nếu $nums[k]$ tồn tại và hiệu giữa $nums[k]$ và $nums[j]$ lớn hơn $1$, độ mất cân bằng tăng $1$;
- Nếu $nums[h]$ tồn tại và hiệu giữa $nums[j]$ và $nums[h]$ lớn hơn $1$, độ mất cân bằng tăng $1$;
- Nếu cả $nums[k]$ và $nums[h]$ đều tồn tại, việc chèn phần tử $nums[j]$ vào giữa $nums[h]$ và $nums[k]$ sẽ làm độ mất cân bằng giảm $1$.

Sau đó, ta cộng độ mất cân bằng của mảng con hiện tại vào đáp án và tiếp tục lặp cho đến khi duyệt xong tất cả các mảng con.

Độ phức tạp thời gian là $O(n^2 \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumImbalanceNumbers(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 0
        for i in range(n):
            sl = SortedList()
            cnt = 0
            for j in range(i, n):
                k = sl.bisect_left(nums[j])
                h = k - 1
                if h >= 0 and nums[j] - sl[h] > 1:
                    cnt += 1
                if k < len(sl) and sl[k] - nums[j] > 1:
                    cnt += 1
                if h >= 0 and k < len(sl) and sl[k] - sl[h] > 1:
                    cnt -= 1
                sl.add(nums[j])
                ans += cnt
        return ans
```

#### Java

```java
class Solution {
    public int sumImbalanceNumbers(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            TreeMap<Integer, Integer> tm = new TreeMap<>();
            int cnt = 0;
            for (int j = i; j < n; ++j) {
                Integer k = tm.ceilingKey(nums[j]);
                if (k != null && k - nums[j] > 1) {
                    ++cnt;
                }
                Integer h = tm.floorKey(nums[j]);
                if (h != null && nums[j] - h > 1) {
                    ++cnt;
                }
                if (h != null && k != null && k - h > 1) {
                    --cnt;
                }
                tm.merge(nums[j], 1, Integer::sum);
                ans += cnt;
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
    int sumImbalanceNumbers(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            multiset<int> s;
            int cnt = 0;
            for (int j = i; j < n; ++j) {
                auto it = s.lower_bound(nums[j]);
                if (it != s.end() && *it - nums[j] > 1) {
                    ++cnt;
                }
                if (it != s.begin() && nums[j] - *prev(it) > 1) {
                    ++cnt;
                }
                if (it != s.end() && it != s.begin() && *it - *prev(it) > 1) {
                    --cnt;
                }
                s.insert(nums[j]);
                ans += cnt;
            }
        }
        return ans;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
