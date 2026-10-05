---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3763. Maximum Total Sum with Threshold Constraints 🔒](https://leetcode.com/problems/maximum-total-sum-with-threshold-constraints)

[中文文档](/solution/3700-3799/3763.Maximum%20Total%20Sum%20with%20Threshold%20Constraints/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums</code> và <code>threshold</code>, cả hai đều có độ dài <code>n</code>.</p>

<p>Bắt đầu với <code>step = 1</code>, bạn lặp lại các bước sau:</p>

<ul>
    <li>Chọn một chỉ số <code>i</code> <strong>chưa được sử dụng</strong> sao cho <code>threshold[i] &lt;= step</code>.

    <ul>
        <li>Nếu không tồn tại chỉ số nào như vậy, quá trình kết thúc.</li>
    </ul>
    </li>
    <li>Cộng <code>nums[i]</code> vào tổng đang tính.</li>
    <li>Đánh dấu chỉ số <code>i</code> là đã sử dụng và tăng <code>step</code> thêm 1.</li>

</ul>

<p>Hãy trả về <strong>tổng</strong> <strong>lớn nhất</strong> có thể đạt được bằng cách chọn các chỉ số một cách tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,10,4,2,1,6], threshold = [5,1,5,5,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">17</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Ở <code>step = 1</code>, chọn <code>i = 1</code> vì <code>threshold[1] &lt;= step</code>. Tổng trở thành 10. Đánh dấu chỉ số 1.</li>
    <li>Ở <code>step = 2</code>, chọn <code>i = 4</code> vì <code>threshold[4] &lt;= step</code>. Tổng trở thành 11. Đánh dấu chỉ số 4.</li>
    <li>Ở <code>step = 3</code>, chọn <code>i = 5</code> vì <code>threshold[5] &lt;= step</code>. Tổng trở thành 17. Đánh dấu chỉ số 5.</li>
    <li>Ở <code>step = 4</code>, không thể chọn các chỉ số 0, 2 hoặc 3 vì threshold của chúng <code>&gt; 4</code>, nên quá trình kết thúc.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1,5,2,3], threshold = [3,3,2,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ở <code>step = 1</code>, không có chỉ số <code>i</code> nào thỏa mãn <code>threshold[i] &lt;= 1</code>, nên quá trình kết thúc ngay lập tức. Do đó, tổng bằng 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,6,10,13], threshold = [2,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">31</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Ở <code>step = 1</code>, chọn <code>i = 3</code> vì <code>threshold[3] &lt;= step</code>. Tổng trở thành 13. Đánh dấu chỉ số 3.</li>
    <li>Ở <code>step = 2</code>, chọn <code>i = 2</code> vì <code>threshold[2] &lt;= step</code>. Tổng trở thành 23. Đánh dấu chỉ số 2.</li>
    <li>Ở <code>step = 3</code>, chọn <code>i = 1</code> vì <code>threshold[1] &lt;= step</code>. Tổng trở thành 29. Đánh dấu chỉ số 1.</li>
    <li>Ở <code>step = 4</code>, chọn <code>i = 0</code> vì <code>threshold[0] &lt;= step</code>. Tổng trở thành 31. Đánh dấu chỉ số 0.</li>
    <li>Sau <code>step = 4</code>, tất cả các chỉ số đã được chọn, nên quá trình kết thúc.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>n == nums.length == threshold.length</code></li>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= threshold[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ở bước $t$, ta có thể chọn một chỉ số chưa được sử dụng có threshold không vượt quá $t$, và muốn chọn $nums[i]$ lớn nhất. Sắp xếp các chỉ số theo threshold, ta đưa các giá trị vừa được mở khóa vào một sorted set khi $t$ tăng dần, rồi luôn chọn giá trị lớn nhất hiện tại; nếu set rỗng thì quá trình kết thúc.

<!-- thinking:end -->

Ta nhận thấy ở mỗi bước, ta muốn chọn số lớn nhất trong số những số thỏa mãn điều kiện để cộng vào tổng. Vì vậy, ta có thể dùng greedy để giải bài toán này.

Trước tiên, ta sắp xếp mảng chỉ số $\textit{idx}$ có độ dài $n$ theo thứ tự tăng dần của threshold tương ứng. Sau đó, ta dùng sorted set hoặc priority queue (max heap) để lưu các số hiện đang thỏa mãn điều kiện. Ở mỗi bước, ta thêm tất cả các số có threshold nhỏ hơn hoặc bằng số bước hiện tại vào sorted set hoặc priority queue, rồi chọn số lớn nhất trong đó để cộng vào tổng. Nếu sorted set hoặc priority queue rỗng tại thời điểm này, nghĩa là không còn số nào thỏa mãn điều kiện, và ta kết thúc quá trình.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, nums: List[int], threshold: List[int]) -> int:
        n = len(nums)
        idx = sorted(range(n), key=lambda i: threshold[i])
        sl = SortedList()
        step = 1
        ans = i = 0
        while True:
            while i < n and threshold[idx[i]] <= step:
                sl.add(nums[idx[i]])
                i += 1
            if not sl:
                break
            ans += sl.pop()
            step += 1
        return ans
```

#### Java

```java
class Solution {
    public long maxSum(int[] nums, int[] threshold) {
        int n = nums.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, Comparator.comparingInt(i -> threshold[i]));
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        long ans = 0;
        for (int i = 0, step = 1;; ++step) {
            while (i < n && threshold[idx[i]] <= step) {
                tm.merge(nums[idx[i]], 1, Integer::sum);
                ++i;
            }
            if (tm.isEmpty()) {
                break;
            }
            int x = tm.lastKey();
            ans += x;
            if (tm.merge(x, -1, Integer::sum) == 0) {
                tm.remove(x);
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
    long long maxSum(vector<int>& nums, vector<int>& threshold) {
        int n = nums.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int a, int b) { return threshold[a] < threshold[b]; });

        multiset<int> ms;
        long long ans = 0;
        int i = 0;

        for (int step = 1;; ++step) {
            while (i < n && threshold[idx[i]] <= step) {
                ms.insert(nums[idx[i]]);
                ++i;
            }
            if (ms.empty()) {
                break;
            }

            auto it = prev(ms.end());
            ans += *it;
            ms.erase(it);
        }
        return ans;
    }
};
```

#### Go

```go
func maxSum(nums []int, threshold []int) int64 {
    n := len(nums)
    idx := make([]int, n)
    for i := 0; i < n; i++ {
        idx[i] = i
    }
    sort.Slice(idx, func(a, b int) bool {
        return threshold[idx[a]] < threshold[idx[b]]
    })

    tree := redblacktree.NewWithIntComparator()
    var ans int64
    i := 0

    for step := 1; ; step++ {
        for i < n && threshold[idx[i]] <= step {
            val := nums[idx[i]]
            if cnt, found := tree.Get(val); found {
                tree.Put(val, cnt.(int)+1)
            } else {
                tree.Put(val, 1)
            }
            i++
        }
        if tree.Empty() {
            break
        }

        node := tree.Right()
        key := node.Key.(int)
        cnt := node.Value.(int)

        ans += int64(key)
        if cnt == 1 {
            tree.Remove(key)
        } else {
            tree.Put(key, cnt-1)
        }
    }

    return ans
}
```

#### TypeScript

```ts
function maxSum(nums: number[], threshold: number[]): number {
    const n = nums.length;
    const idx = Array.from({ length: n }, (_, i) => i).sort((a, b) => threshold[a] - threshold[b]);
    const pq = new MaxPriorityQueue<number>();
    let ans = 0;
    for (let i = 0, step = 1; ; ++step) {
        while (i < n && threshold[idx[i]] <= step) {
            pq.enqueue(nums[idx[i]]);
            ++i;
        }
        if (pq.isEmpty()) {
            break;
        }
        ans += pq.dequeue();
    }
    return ans;
}
```

#### Rust

```rust
use std::cmp::Reverse;
use std::collections::BinaryHeap;

impl Solution {
    pub fn max_sum(nums: Vec<i32>, threshold: Vec<i32>) -> i64 {
        let n = nums.len();
        let mut idx: Vec<usize> = (0..n).collect();
        idx.sort_by_key(|&i| threshold[i]);

        let mut pq = BinaryHeap::new();
        let mut ans: i64 = 0;
        let mut i = 0;
        let mut step = 1;

        loop {
            while i < n && threshold[idx[i]] <= step {
                pq.push(nums[idx[i]]);
                i += 1;
            }
            match pq.pop() {
                Some(x) => {
                    ans += x as i64;
                    step += 1;
                }
                None => break,
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
