---
comments: true
difficulty: Medium
rating: 1793
source: Weekly Contest 340 Q2
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [2615. Sum of Distances](https://leetcode.com/problems/sum-of-distances)

[中文文档](/solution/2600-2699/2615.Sum%20of%20Distances/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>.</p>

<p>Tồn tại một mảng <code>arr</code> có độ dài bằng <code>nums.length</code>, trong đó <code>arr[i]</code> là tổng của <code>|i - j|</code> trên mọi <code>j</code> sao cho <code>nums[j] == nums[i]</code> và <code>j != i</code>. Nếu không có <code>j</code> nào như vậy, đặt <code>arr[i]</code> bằng 0.</p>

<p>Trả về mảng <code>arr</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,1,1,2]
<strong>Đầu ra:</strong> [5,0,3,4,0]
<strong>Giải thích:</strong>
Với i = 0, nums[0] == nums[2] và nums[0] == nums[3]. Do đó, arr[0] = |0 - 2| + |0 - 3| = 5.
Với i = 1, arr[1] = 0 vì không có chỉ số nào khác có giá trị 3.
Với i = 2, nums[2] == nums[0] và nums[2] == nums[3]. Do đó, arr[2] = |2 - 0| + |2 - 3| = 3.
Với i = 3, nums[3] == nums[0] và nums[3] == nums[2]. Do đó, arr[3] = |3 - 0| + |3 - 2| = 4.
Với i = 4, arr[4] = 0 vì không có chỉ số nào khác có giá trị 2.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,5,3]
<strong>Đầu ra:</strong> [0,0,0]
<strong>Giải thích:</strong> Vì mỗi phần tử trong nums đều khác nhau, arr[i] = 0 với mọi i.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài toán này giống với <a href="https://leetcode.com/problems/intervals-between-identical-elements/description/" target="_blank"> 2121: Intervals Between Identical Elements.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash map + tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi chỉ số, ta cần tính tổng khoảng cách đến tất cả các phần tử có cùng giá trị. Nếu duyệt qua mọi phần tử cùng giá trị từ từng vị trí, độ phức tạp sẽ là bậc hai khi $n \le 10^5$.
>
> Các chỉ số của cùng một giá trị đã được sắp xếp. Khi chuyển sang chỉ số tiếp theo, phần đóng góp bên trái tăng thêm bằng số lượng phần tử bên trái nhân với khoảng cách, còn phần đóng góp bên phải giảm đi bằng số lượng phần tử bên phải nhân với khoảng cách; hai tổng được cập nhật giúp duyệt mỗi nhóm trong thời gian tuyến tính.
>
> Gom các chỉ số theo giá trị, sau đó áp dụng cách chuyển tổng tiền tố này cho từng nhóm.

<!-- thinking:end -->

Đầu tiên, dùng một hash map $d$ để ghi lại danh sách các chỉ số của mỗi phần tử trong mảng $nums$, nghĩa là $d[x]$ biểu diễn danh sách tất cả chỉ số trong $nums$ có giá trị là $x$.

Với mỗi danh sách chỉ số $idx$ trong hash map $d$, ta có thể tính giá trị của $arr[i]$ cho từng chỉ số $i$ trong $idx$. Với chỉ số đầu tiên $idx[0]$, tổng khoảng cách đến tất cả các chỉ số bên phải là $right = \sum_{i=0}^{m-1} - idx[0] \times m$. Sau đó, ta duyệt qua $idx$; ở mỗi lần lặp, tính $ans[idx[i]] = left + right$, rồi cập nhật $left$ và $right$ như sau: $left = left + (idx[i+1] - idx[i]) \times (i+1)$ và $right = right - (idx[i+1] - idx[i]) \times (m-i-1)$.

Sau khi duyệt xong, ta thu được mảng $arr$ tương ứng với từng phần tử trong $nums$, được lưu trong $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distance(self, nums: List[int]) -> List[int]:
        d = defaultdict(list)
        for i, x in enumerate(nums):
            d[x].append(i)
        ans = [0] * len(nums)
        for idx in d.values():
            left, right = 0, sum(idx) - len(idx) * idx[0]
            for i in range(len(idx)):
                ans[idx[i]] = left + right
                if i + 1 < len(idx):
                    left += (idx[i + 1] - idx[i]) * (i + 1)
                    right -= (idx[i + 1] - idx[i]) * (len(idx) - i - 1)
        return ans
```

#### Java

```java
class Solution {
    public long[] distance(int[] nums) {
        int n = nums.length;
        long[] ans = new long[n];
        Map<Integer, List<Integer>> d = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            d.computeIfAbsent(nums[i], k -> new ArrayList<>()).add(i);
        }
        for (var idx : d.values()) {
            int m = idx.size();
            long left = 0;
            long right = -1L * m * idx.get(0);
            for (int i : idx) {
                right += i;
            }
            for (int i = 0; i < m; ++i) {
                ans[idx.get(i)] = left + right;
                if (i + 1 < m) {
                    left += (idx.get(i + 1) - idx.get(i)) * (i + 1L);
                    right -= (idx.get(i + 1) - idx.get(i)) * (m - i - 1L);
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
    vector<long long> distance(vector<int>& nums) {
        int n = nums.size();
        vector<long long> ans(n);
        unordered_map<int, vector<int>> d;
        for (int i = 0; i < n; ++i) {
            d[nums[i]].push_back(i);
        }
        for (auto& [_, idx] : d) {
            int m = idx.size();
            long long left = 0;
            long long right = -1LL * m * idx[0];
            for (int i : idx) {
                right += i;
            }
            for (int i = 0; i < m; ++i) {
                ans[idx[i]] = left + right;
                if (i + 1 < m) {
                    left += (idx[i + 1] - idx[i]) * (i + 1);
                    right -= (idx[i + 1] - idx[i]) * (m - i - 1);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func distance(nums []int) []int64 {
	n := len(nums)
	ans := make([]int64, n)
	d := map[int][]int{}
	for i, x := range nums {
		d[x] = append(d[x], i)
	}
	for _, idx := range d {
		m := len(idx)
		left, right := 0, -m*idx[0]
		for _, i := range idx {
			right += i
		}
		for i := range idx {
			ans[idx[i]] = int64(left + right)
			if i+1 < m {
				left += (idx[i+1] - idx[i]) * (i + 1)
				right -= (idx[i+1] - idx[i]) * (m - i - 1)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function distance(nums: number[]): number[] {
    const n = nums.length;
    const ans = new Array(n).fill(0);
    const d = new Map<number, number[]>();
    for (let i = 0; i < n; ++i) {
        if (!d.has(nums[i])) {
            d.set(nums[i], []);
        }
        d.get(nums[i])!.push(i);
    }
    for (const idx of d.values()) {
        const m = idx.length;
        let left = 0;
        let right = -1 * m * idx[0];
        for (const i of idx) {
            right += i;
        }
        for (let i = 0; i < m; ++i) {
            ans[idx[i]] = left + right;
            if (i + 1 < m) {
                left += (idx[i + 1] - idx[i]) * (i + 1);
                right -= (idx[i + 1] - idx[i]) * (m - i - 1);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn distance(nums: Vec<i32>) -> Vec<i64> {
        let n = nums.len();
        let mut ans = vec![0i64; n];
        let mut d: HashMap<i32, Vec<usize>> = HashMap::new();
        for i in 0..n {
            d.entry(nums[i]).or_insert(Vec::new()).push(i);
        }
        for idx in d.values() {
            let m = idx.len();
            let mut left = 0i64;
            let mut right = 0i64;
            for &i in idx {
                right += i as i64;
            }
            right -= m as i64 * idx[0] as i64;
            for i in 0..m {
                ans[idx[i]] = left + right;
                if i + 1 < m {
                    let diff = (idx[i + 1] - idx[i]) as i64;
                    left += diff * (i + 1) as i64;
                    right -= diff * (m - i - 1) as i64;
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
