---
comments: true
difficulty: Hard
rating: 2050
source: Biweekly Contest 134 Q4
tags:
    - Bit Manipulation
    - Segment Tree
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3209. Number of Subarrays With AND Value of K](https://leetcode.com/problems/number-of-subarrays-with-and-value-of-k)

[中文文档](/solution/3200-3299/3209.Number%20of%20Subarrays%20With%20AND%20Value%20of%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, hãy trả về số lượng <span data-keyword="subarray-nonempty">mảng con không rỗng</span> của <code>nums</code> sao cho phép <code>AND</code> theo bit của các phần tử trong mảng con bằng <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các mảng con đều chỉ chứa các số 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con có giá trị <code>AND</code> bằng 1 là: <code>[<u><strong>1</strong></u>,1,2]</code>, <code>[1,<u><strong>1</strong></u>,2]</code>, <code>[<u><strong>1,1</strong></u>,2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con có giá trị <code>AND</code> bằng 2 là: <code>[1,<b><u>2</u></b>,3]</code>, <code>[1,<u><strong>2,3</strong></u>]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i], k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 10^5$, việc liệt kê phép AND của mọi mảng con sẽ có độ phức tạp $O(n^2)$ và quá chậm. Khi cố định đầu phải, việc di chuyển đầu trái chỉ có thể làm giảm giá trị AND, còn các giá trị không vượt quá $10^9$, nên có nhiều nhất khoảng $30$ giá trị AND khác nhau xuất hiện.
>
> Một counter lưu “AND kết thúc tại chỉ số trước đó $\to$ tần suất”. Với $x$, ta thực hiện AND từng key cũ với $x$ để tạo một map mới, thêm singleton $x$, rồi cộng số lượng của key $k$ vào đáp án. Mỗi đầu phải chỉ tác động đến một số lượng key có bậc logarit.

<!-- thinking:end -->

Theo mô tả bài toán, cần tìm kết quả của phép AND theo bit của các phần tử từ chỉ số $l$ đến $r$ trong mảng $\textit{nums}$, tức là $\textit{nums}[l] \land \textit{nums}[l + 1] \land \cdots \land \textit{nums}[r]$, trong đó $\land$ biểu diễn phép AND theo bit.

Nếu cố định đầu phải $r$, thì phạm vi của đầu trái $l$ là $[0, r]$. Vì giá trị AND theo bit giảm đơn điệu khi $l$ giảm, đồng thời giá trị của $nums[i]$ không vượt quá $10^9$, đoạn $[0, r]$ có thể có nhiều nhất $30$ giá trị khác nhau. Do đó, ta có thể dùng một set để lưu tất cả các giá trị của $\textit{nums}[l] \land \textit{nums}[l + 1] \land \cdots \land \textit{nums}[r]$ và số lần xuất hiện của các giá trị này.

Khi duyệt từ $r$ đến $r+1$, các giá trị có $r+1$ làm đầu phải là kết quả của việc thực hiện phép AND theo bit giữa từng giá trị trong set với $nums[r + 1]$, cộng thêm chính $\textit{nums}[r + 1]$.

Do đó, chỉ cần liệt kê từng giá trị trong set và thực hiện phép AND theo bit với $\textit{nums[r]}$ để lấy tất cả các giá trị cùng số lần xuất hiện tương ứng khi $r$ là đầu phải. Sau đó, cộng thêm số lần xuất hiện của $\textit{nums[r]}$. Lúc này, cộng số lần xuất hiện của giá trị $k$ vào đáp án. Tiếp tục duyệt $r$ cho đến khi duyệt hết mảng.

Độ phức tạp thời gian là $O(n \times \log M)$, độ phức tạp không gian là $O(\log M)$. Trong đó, $n$ và $M$ lần lượt là độ dài của mảng $\textit{nums}$ và giá trị lớn nhất trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        ans = 0
        pre = Counter()
        for x in nums:
            cur = Counter()
            for y, v in pre.items():
                cur[x & y] += v
            cur[x] += 1
            ans += cur[k]
            pre = cur
        return ans
```

#### Java

```java
class Solution {
    public long countSubarrays(int[] nums, int k) {
        long ans = 0;
        Map<Integer, Integer> pre = new HashMap<>();
        for (int x : nums) {
            Map<Integer, Integer> cur = new HashMap<>();
            for (var e : pre.entrySet()) {
                int y = e.getKey(), v = e.getValue();
                cur.merge(x & y, v, Integer::sum);
            }
            cur.merge(x, 1, Integer::sum);
            ans += cur.getOrDefault(k, 0);
            pre = cur;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countSubarrays(vector<int>& nums, int k) {
        long long ans = 0;
        unordered_map<int, int> pre;
        for (int x : nums) {
            unordered_map<int, int> cur;
            for (auto& [y, v] : pre) {
                cur[x & y] += v;
            }
            cur[x]++;
            ans += cur[k];
            pre = cur;
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int, k int) (ans int64) {
    pre := map[int]int{}
    for _, x := range nums {
        cur := map[int]int{}
        for y, v := range pre {
            cur[x&y] += v
        }
        cur[x]++
        ans += int64(cur[k])
        pre = cur
    }
    return
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[], k: number): number {
    let ans = 0;
    let pre = new Map<number, number>();
    for (const x of nums) {
        const cur = new Map<number, number>();
        for (const [y, v] of pre) {
            const z = x & y;
            cur.set(z, (cur.get(z) || 0) + v);
        }
        cur.set(x, (cur.get(x) || 0) + 1);
        ans += cur.get(k) || 0;
        pre = cur;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
